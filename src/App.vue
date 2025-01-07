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
        <button
            :class="['nav-button', { active: currentPage === 'advancements' }]"
            @click="currentPage = 'advancements'"
        >
          <img src="/src/assets/new-5-svgrepo-com.svg" style="width: 20px;height: 20px"/>
          Advancements
        </button>
        <button
            :class="['nav-button', { active: currentPage === 'dataset-description' }]"
            @click="currentPage = 'dataset-description'"
        >
          <svg width="20" height="20" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path stroke="currentColor"
                  d="M11 19.913c-4.966-.084-8-1.388-8-2.334v-6.248C4.643 12.429 8.082 13 11.5 13s6.857-.57 8.5-1.67V14h1V5c0-1.97-4.78-3-9.5-3S2 3.03 2 5v12.58c0 2.116 4.448 3.255 9 3.333zM11.5 3C17 3 20 4.321 20 5s-3 2-8.5 2S3 5.679 3 5s3-2 8.5-2zM3 6.411C4.643 7.457 8.082 8 11.5 8s6.857-.543 8.5-1.589v3.437C20 10.726 16.689 12 11.5 12S3 10.726 3 9.848zM18 15l-1-1h-3l-1 1h-1v8h11v-8zm4 7h-9v-4h9zm-9-5v-1h.414l1-1h2.172l1 1H22v1z"/>
            <path fill="none" d="M0 0h24v24H0z"/>
          </svg>
          Dataset Description
        </button>
        <button
            :class="['nav-button', { active: currentPage === 'problems' }]"
            @click="currentPage = 'problems'"
        >
          <svg fill="currentColor" stroke="currentColor" width="20" height="20" viewBox="0 0 1920 1920"
               xmlns="http://www.w3.org/2000/svg">
            <path
                d="M961 1C431.645 1 1 431.645 1 961s430.645 960 960 960 960-430.645 960-960S1490.355 1 961 1m0 1807.059c-467.125 0-847.059-379.934-847.059-847.059 0-467.125 379.934-847.059 847.059-847.059 467.125 0 847.059 379.934 847.059 847.059 0 467.125-379.934 847.059-847.059 847.059m319.465-1246.374-319.51 319.51-319.398-319.51-79.85 79.85 319.398 319.51-319.397 319.398 79.85 79.85 319.397-319.398 319.51 319.397 79.85-79.85-319.51-319.397 319.51-319.51-79.85-79.85Z"
                fill-rule="evenodd"/>
          </svg>
          Problems
        </button>
      </div>

    </nav>
    <div class="container">
      <AudioProcessor v-if="currentPage === 'home'"/>
      <ModelArchitecture v-if="currentPage === 'architecture'"/>
      <DatasetDescription v-if="currentPage === 'dataset-description'"/>
      <Problems v-if="currentPage === 'problems'"/>
      <Advancements v-if="currentPage === 'advancements'"/>
    </div>
  </div>
</template>

<script>
import AudioProcessor from './components/AudioProcessor.vue'
import ModelArchitecture from './components/ModelArchitecture.vue'
import DatasetDescription from "@/components/DatasetDescription.vue";
import Problems from "@/components/Problems.vue";
import Advancements from "@/components/Advancements.vue";

export default {
  name: 'App',
  components: {
    Advancements,
    Problems,
    DatasetDescription,
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