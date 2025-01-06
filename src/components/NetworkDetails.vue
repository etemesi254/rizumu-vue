// components/NetworkComponents.vue
<template>
  <div class="architecture-section">
    <h2>Network Components</h2>

    <div class="network-layer" v-for="(layer, index) in networkLayers" :key="index">
      <h3>{{ layer.title }}</h3>
      <p>{{ layer.description }}</p>
      <div class="layer-details">
        {{ layer.details }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NetworkComponents',
  data() {
    return {
      networkLayers: [
        {
          title: '1. Encoder Blocks (×4)',
          description: 'Sequential downsampling blocks that process and compress input features:',
          details: 'Input → Conv2D(k=3, p=1) → ReLU → Conv2D(k=3, p=1) → ReLU → MaxPool2D(k=2, s=2)\nChannel progression: 1 → 64 → 128 → 256 → 512'
        },
        {
          title: '2. Self-Attention Mechanism',
          description: 'Learns global dependencies in the feature space using scaled dot-product attention:',
          details: 'Query = Conv2D(hidden_size=512, hidden_size//8, k=1)\nKey = Conv2D(hidden_size, hidden_size//8, k=1)\nValue = Conv2D(hidden_size, hidden_size, k=1)\nAttention = softmax(Q × K^T) × V'
        },
        {
          title: '3. Bidirectional LSTM Bridge',
          description: 'Processes temporal dependencies in both directions:',
          details: 'Input → Conv2D(k=1) → BiLSTM(hidden_size=512) → Linear → Conv2D(k=1) → ReLU'
        },
        {
          title: '4. Decoder Blocks (×4)',
          description: 'Upsampling blocks with skip connections from corresponding encoders:',
          details: 'Input → ConvTranspose2D(k=2, s=2) → Concatenate(skip) →\nConv2D(k=3, p=1) → ReLU → Conv2D(k=3, p=1) → ReLU\nChannel progression: 512 → 256 → 128 → 64 → 32'
        },
        {
          title: '5. Final Output Layer',
          description: 'Converts processed features to output spectrogram:',
          details: 'Input → ConvTranspose2D(64→32, k=2, s=2) → Conv2D(32→1, k=1)'
        }
      ]
    }
  }
}
</script>
