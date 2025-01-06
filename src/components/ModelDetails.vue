<template>
  <div class="architecture-section">
    <h2>Model Details</h2>
    <table class="parameter-table">
      <thead>
      <tr>
        <th>Parameter</th>
        <th>Value</th>
        <th>Description</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(param, index) in modelParameters" :key="index">
        <td>{{ param.name }}</td>
        <td>{{ param.value }}</td>
        <td>{{ param.description }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'ModelDetails',
  data() {
    return {
      modelParameters: [
        {
          name: 'Input Channels',
          value: '1',
          description: 'Mono audio spectrogram input'
        },
        {
          name: 'Output Channels',
          value: '1',
          description: 'Separated source spectrogram'
        },
        {
          name: 'Network Depth',
          value: '4',
          description: 'Number of encoder/decoder stages'
        },
        {
          name: 'LSTM Hidden Size',
          value: '512',
          description: 'Bidirectional LSTM feature dimension'
        },
        {
          name: 'Max Feature Channels',
          value: '512',
          description: 'Maximum number of feature channels in bottleneck'
        },
        {
          name: 'Model Parameters',
          value: '19.8 million',
          description: 'Model weights'
        },
        {
          name: 'Model Size',
          value: '98 Mb',
          description: 'Size of an exported model'
        }
      ]
    }
  }
}
</script>

<style scoped>
.parameter-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1rem;
}

.parameter-table th,
.parameter-table td {
  border: 1px solid #ddd;
  padding: 0.75rem;
  text-align: left;
}

.parameter-table th {
  background-color: #f8f9fa;
}

.parameter-table tr:nth-child(even) {
  background-color: #f8f9fa;
}
</style>