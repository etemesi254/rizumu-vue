// components/AudioSamples.vue
<template>
  <div class="samples-section">
    <div class="samples-header">
      <h2>Samples</h2>
      <select v-model="selectedCategory" class="samples-dropdown">
        <option
            v-for="sample in samples"
            :key="sample.category"
            :value="sample.category"
        >
          {{ sample.category }}
        </option>
      </select>
    </div>

    <div v-if="currentSample" class="sample-content">
      <p>Description: <p v-html="currentSample.description"></p></p>
      <div v-if="currentSample.mix">
        <h3>Mix</h3>
        <audio v-if="currentSample.mix" controls :src="currentSample.mix"></audio>
      </div>
      <div v-if="currentSample.speech">
        <h3>Speech</h3>
        <audio controls :src="currentSample.speech"></audio>
      </div>
      <div v-if="currentSample.output">
        <h3>Model Output</h3>
        <audio v-if="currentSample.output" controls :src="currentSample.output"></audio>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AudioSamples',
  data() {
    return {
      selectedCategory: 'Sample 1 Test Data',
      samples: [
        {
          category: "Sample 1 Test Data",
          description: "A Sample derived from test Data",
          mix: 'public/samples/mix.mp3',
          speech: 'public/samples/speech.mp3',
          output: 'public/samples/cleaned.mp3'
        },
        {
          category: "Sauti Sol Nenda Lote",
          description: "Sauti Sol Nenda Lote Mama Song from <a href='https://www.youtube.com/watch?v=YCsg_V-AYOw'> Youtube </a>  first 1 minute of the song",
          mix: 'public/samples/sauti-sol/mix.mp3',
          output: 'public/samples/sauti-sol/output.mp3',
        },
        {
          category: "Rick Astley Never Gonna Give You Up",
          description: "Rick Astley Never Give You Up 1 minute, shows the model weakness to music files",
          mix: 'public/samples/rick-astley/mix.mp3',
          output: 'public/samples/rick-astley/output.mp3',
        },
        {
          category: "DnR folder 312",
          description: "Sample from Divide and Remaster Dataset 312, lifted mix and speech files",
          mix: "public/samples/sample-2/mix.mp3",
          speech: 'public/samples/sample-2/speech.mp3',
          output: 'public/samples/sample-2/model-output.mp3',
        },
        {
          category: "Kidum-Mapenzi",
          description: "Kidum Mapenzi mix, again, show model weakness to music files since they were not present in training dataset",
          mix: 'public/samples/rick-astley/mix.mp3',
          output: 'public/samples/rick-astley/output.mp3',
        },
      ]
    }
  },
  computed: {
    currentSample() {
      return this.samples.find(sample => sample.category === this.selectedCategory)
    }
  }
}
</script>