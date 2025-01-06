<template>
  <div>
    <h1>Model Architecture: Audio Source Separation Network</h1>

    <div class="architecture-container">
      <div class="architecture-section">
        <h2>Demonstrative Video</h2>
        <p>Simple video that goes over what the model does under the hood</p>
        <video
            class="architecture-video"
            autoplay
            src="../assets/video/separation_video.mp4"
            controls
            style="width: 100%;"
        ></video>
      </div>

      <div class="architecture-section">
        <h2>Network Architecture Overview</h2>
        <vue-mermaid-string :value="diagram" />
      </div>

      <NetworkComponents/>
      <ModelDetails/>
    </div>
  </div>
</template>

<script>
import mermaid from 'mermaid'
import NetworkComponents from './NetworkDetails.vue'
import ModelDetails from './ModelDetails.vue'
import VueMermaidString from 'vue-mermaid-string'

export default {
  name: 'ModelArchitecture',
  components: {
    NetworkComponents,
    ModelDetails,
    VueMermaidString
  },
  mounted() {
    // this.initMermaid()
  },
  computed: {
    // equals graph TD\n  A --> B
    diagram: () => `
        graph TD
          A[Input Audio Spectrogram] --> B[Encoder Block 1]
          B --> C[Encoder Block 2]
          C --> D[Encoder Block 3]
          D --> E[Encoder Block 4]
          E --> F[Self-Attention Layer]
          F --> G[Bi-LSTM Bridge]
          G --> H[Decoder Block 4]
          H --> I[Decoder Block 3]
          I --> J[Decoder Block 2]
          J --> K[Decoder Block 1]
          K --> L[Output Layer]

          D -.-> H
          C -.-> I
          B -.-> J

          style F fill:#f9f,stroke:#333
          style G fill:#ff9,stroke:#333
      `,
  },

}
</script>
