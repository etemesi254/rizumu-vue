<template>
  <div class="audio-processor">
    <h1>Rizumu</h1>
    <p>Rizumu is a speech extractor/isolator of audio files.
      Given an audio file, it isolates speech from the file.</p>

    <!-- Settings Panel -->
    <div class="settings-panel">
      <div class="processing-mode">
        <label>
          Processing Mode:
          <select v-model="processingMode">
            <option value="server">Server</option>
            <option value="onnx">ONNX (Browser)</option>
          </select>
        </label>
      </div>
      <div v-if="processingMode === 'onnx'" id="onnxSettings">
        <label>
          Execution Provider:
          <select v-model="executionProvider" @change="initModel">
            <option value="wasm">WebAssembly</option>
            <option value="webgl">WebGL</option>
            <option value="wasm-simd">WebAssembly SIMD</option>
          </select>
        </label>
      </div>
    </div>

    <!-- File Input -->
    <div class="file-input-wrapper">
      <input
          type="file"
          id="audioInput"
          ref="audioInput"
          accept="audio/*"
          style="display: none;"
          @change="handleFileChange"
      >
      <label
          for="audioInput"
          class="file-input-label"
          @dragover.prevent
          @drop.prevent="handleFileDrop"
      >
        Drop audio file here or click to upload
      </label>
      <div v-if="fileName" class="file-name">{{ fileName }}</div>
    </div>

    <!-- Process Button -->
    <button
        :disabled="!selectedFile || isProcessing"
        @click="processAudio"
        class="process-button"
        id="processBtn"
    >
      {{ isProcessing ? 'Processing...' : 'Process Audio' }}
    </button>

    <!-- Progress Indicator -->
    <div v-if="isProcessing" class="progress-wrapper">
      <div class="progress-bar">
        <div :style="{ width: `${progress}%` }"></div>
      </div>
      <div class="status-text">{{ currentStatus }}</div>
      <div class="processing-steps">
        <div
            v-for="step in processingSteps"
            :key="step.id"
            :class="['processing-step', {
            active: step.status === 'active',
            done: step.status === 'done'
          }]"
        >
          <div class="step-indicator"></div>
          {{ step.text }}
        </div>
      </div>
    </div>

    <!-- Results -->
    <div v-if="error" class="error-message">
      {{ error }}
    </div>

    <div v-if="processedAudioUrl" class="result-section">
      <h3>Processed Audio</h3>
      <audio
          controls
          :src="processedAudioUrl"
          class="audio-player"
      ></audio>
      <button
          @click="downloadProcessedAudio"
          class="download-button"
      >
        Download Processed Audio
      </button>
    </div>

    <!-- Samples Section -->
    <AudioSamples/>
  </div>
</template>

<script>
import * as ort from 'onnxruntime-web'
import AudioSamples from './AudioSamples.vue'

export default {
  name: 'AudioProcessor',

  components: {
    AudioSamples
  },

  data() {
    return {
      processingMode: 'server',
      executionProvider: 'wasm',
      selectedFile: null,
      fileName: '',
      isProcessing: false,
      progress: 0,
      currentStatus: '',
      error: null,
      processedAudioUrl: null,
      session: null,
      processingSteps: [
        {id: 'load', text: 'Loading audio file', status: ''},
        {id: 'process', text: 'Running through model', status: ''},
        {id: 'convert', text: 'Converting output', status: ''}
      ]
    }
  },

  async mounted() {
    if (this.processingMode === 'onnx') {
      await this.initModel()
    }
  },

  methods: {
    async initModel() {
      try {
        const modelUrl = 'assets/model/rizumu_onnx.onnx'
        const options = {
          executionProviders: [this.executionProvider === 'webgl' ? 'webgl' : 'wasm'],
          enableWasmSIMD: this.executionProvider === 'wasm-simd'
        }

        if (this.session) {
          await this.session.release()
        }

        this.session = await ort.InferenceSession.create(modelUrl, options)
        console.log('Model initialized with:', this.executionProvider)
      } catch (e) {
        this.error = `Failed to initialize model: ${e.message}`
        console.error('Model initialization error:', e)
      }
    },

    handleFileChange(event) {
      const file = event.target.files[0]
      if (file) {
        this.selectedFile = file
        this.fileName = file.name
        this.error = null
        this.processedAudioUrl = null
      }
    },

    handleFileDrop(event) {
      const file = event.dataTransfer.files[0]
      if (file && file.type.startsWith('audio/')) {
        this.selectedFile = file
        this.fileName = file.name
        this.error = null
        this.processedAudioUrl = null
      }
    },

    updateProcessingStep(step, status) {
      const stepIndex = this.processingSteps.findIndex(s => s.id === step)
      if (stepIndex !== -1) {
        this.processingSteps[stepIndex].status = status
      }
    },

    async processAudio() {
      if (!this.selectedFile) return

      this.isProcessing = true
      this.error = null
      this.processedAudioUrl = null
      this.progress = 0

      try {
        // Step 1: Load audio
        this.updateProcessingStep('load', 'active')
        const audioBuffer = await this.loadAudioFile(this.selectedFile)
        this.updateProcessingStep('load', 'done')
        this.progress = 33

        // Step 2: Process
        this.updateProcessingStep('process', 'active')
        let processedData
        if (this.processingMode === 'onnx') {
          processedData = await this.processWithOnnx(audioBuffer)
        } else {
          processedData = await this.processWithServer(this.selectedFile)
        }
        this.updateProcessingStep('process', 'done')
        this.progress = 66

        // Step 3: Convert
        this.updateProcessingStep('convert', 'active')
        let audioBlob;
        if (this.processingMode === "onnx") {
          audioBlob = await this.createAudioBlob(processedData)
        } else {
          audioBlob = new Blob([processedData], {type: "audio/wav"})
        }
        this.processedAudioUrl = URL.createObjectURL(audioBlob)
        this.updateProcessingStep('convert', 'done')
        this.progress = 100

      } catch (e) {
        this.error = `Processing failed: ${e.message}`
        console.error('Processing error:', e)
      } finally {
        this.isProcessing = false
      }
    },

    async loadAudioFile(file) {
      const audioContext = new AudioContext({sampleRate: 44100})
      const arrayBuffer = await file.arrayBuffer()
      const audioBuffer = await audioContext.decodeAudioData(arrayBuffer)
      return audioBuffer.getChannelData(0)
    },

    async processWithOnnx(audioData) {
      const tensor = new ort.Tensor('float32', audioData, [1, audioData.length])
      const feeds = {input: tensor}
      const results = await this.session.run(feeds)
      return results.output.data
    },

    async processWithServer(file) {
      const formData = new FormData()
      formData.append('audio_file', file)

      const response = await fetch('https://rizumu.eps-raerex.org/extract-speech', {
        method: 'POST',
        body: formData
      })

      if (!response.ok) {
        throw new Error(`Server error: ${response.statusText}`)
      }

      return response.arrayBuffer()
    },

    async createAudioBlob(audioData) {
      const sampleRate = 44100
      const writer = this.createWaveFileWriter(audioData, sampleRate)
      return new Blob([writer.buffer], {type: 'audio/wav'})
    },

    createWaveFileWriter(audioData, sampleRate) {
      const bytesPerSample = 4
      const numberOfChannels = 1
      const byteRate = sampleRate * numberOfChannels * bytesPerSample
      const wavDataBytes = audioData.length * bytesPerSample
      const headerBytes = 44
      const totalBytes = headerBytes + wavDataBytes
      const buffer = new ArrayBuffer(totalBytes)
      const view = new DataView(buffer)
      let pos = 0

      // Write WAV header
      this.writeString(view, pos, 'RIFF');
      pos += 4
      view.setUint32(pos, totalBytes, true);
      pos += 4
      this.writeString(view, pos, 'WAVE');
      pos += 4
      this.writeString(view, pos, 'fmt ');
      pos += 4
      view.setUint32(pos, 16, true);
      pos += 4
      view.setUint16(pos, 3, true);
      pos += 2
      view.setUint16(pos, numberOfChannels, true);
      pos += 2
      view.setUint32(pos, sampleRate, true);
      pos += 4
      view.setUint32(pos, byteRate, true);
      pos += 4
      view.setUint16(pos, bytesPerSample, true);
      pos += 2
      view.setUint16(pos, bytesPerSample * 8, true);
      pos += 2
      this.writeString(view, pos, 'data');
      pos += 4
      view.setUint32(pos, wavDataBytes, true);
      pos += 4

      // Write audio data
      for (let i = 0; i < audioData.length; i++) {
        view.setFloat32(pos, audioData[i], true)
        pos += 4
      }

      return {buffer, view}
    },

    writeString(view, offset, string) {
      for (let i = 0; i < string.length; i++) {
        view.setUint8(offset + i, string.charCodeAt(i))
      }
    },

    downloadProcessedAudio() {
      if (this.processedAudioUrl) {
        const a = document.createElement('a')
        a.href = this.processedAudioUrl
        a.download = `processed_${this.fileName || 'audio'}.wav`
        a.click()
      }
    }
  }
}
</script>