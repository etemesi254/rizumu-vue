<template>
  <div>
    <nav class="nav">
      <div class="nav-content">
        <button
            :class="['nav-button', { active: currentPage === 'home' }]"
            @click="currentPage = 'home'"
        >
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
            <polyline points="9 22 9 12 15 12 15 22"></polyline>
          </svg>
          Home
        </button>
        <button
            :class="['nav-button', { active: currentPage === 'architecture' }]"
            @click="currentPage = 'architecture'"
        >
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="2" y="3" width="20" height="18" rx="2" ry="2"></rect>
            <line x1="8" y1="12" x2="16" y2="12"></line>
            <line x1="8" y1="16" x2="16" y2="16"></line>
            <line x1="8" y1="8" x2="16" y2="8"></line>
          </svg>
          Model Architecture
        </button>
      </div>
    </nav>
    <div class="container">
      <AudioProcessor v-if="currentPage === 'home'" />
      <ModelArchitecture v-if="currentPage === 'architecture'" />
    </div>
  </div>
</template>

<script>
import AudioProcessor from './components/AudioProcessor.vue'
import ModelArchitecture from './components/ModelArchitecture.vue'

export default {
  name: 'App',
  components: {
    AudioProcessor,
    ModelArchitecture
  },
  data() {
    return {
      currentPage: 'home'
    }
  }
}
</script>