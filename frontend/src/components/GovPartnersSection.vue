<script setup>
import { ref } from 'vue'

const props = defineProps({
  govLinks: {
    type: Array,
    required: true
  },
  usefulLinks: {
    type: Array,
    required: true
  }
})

const activeTab = ref('gov') // 'gov' or 'useful'
</script>

<template>
  <section class="home-section partners-section">
    <div class="container">
      <div class="section-header-row">
        <div>
          <h2 class="section-title">Davlat organlari va foydali havolalar</h2>
          <p class="section-subtitle">O‘zbekiston Respublikasi davlat hokimiyati organlari va soha portallari</p>
        </div>

        <!-- Toggle Tabs -->
        <div class="partners-tabs">
          <button 
            class="tab-btn" 
            :class="{ active: activeTab === 'gov' }"
            @click="activeTab = 'gov'"
          >
            Davlat tashkilotlari
          </button>
          <button 
            class="tab-btn" 
            :class="{ active: activeTab === 'useful' }"
            @click="activeTab = 'useful'"
          >
            Foydali havolalar
          </button>
        </div>
      </div>

      <!-- Partners Grid -->
      <div v-if="activeTab === 'gov'" class="partners-grid">
        <a 
          v-for="item in govLinks" 
          :key="item.id" 
          :href="item.url" 
          target="_blank" 
          rel="noopener"
          class="partner-card"
        >
          <div class="partner-emblem-box">
            <img 
              v-if="item.icon" 
              :src="item.icon" 
              :alt="item.title" 
              class="partner-icon" 
              loading="lazy"
            />
            <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>
            </svg>
          </div>

          <h3 class="partner-title">{{ item.title }}</h3>

          <div class="partner-external-icon">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/>
              <polyline points="15 3 21 3 21 9"/>
              <line x1="10" y1="14" x2="21" y2="3"/>
            </svg>
          </div>
        </a>
      </div>

      <!-- Useful Links Grid -->
      <div v-else class="partners-grid">
        <a 
          v-for="item in usefulLinks" 
          :key="item.id" 
          :href="item.url" 
          target="_blank" 
          rel="noopener"
          class="partner-card"
        >
          <div class="partner-emblem-box useful-icon-box">
            <img 
              v-if="item.icon" 
              :src="item.icon" 
              :alt="item.title" 
              class="partner-icon" 
              loading="lazy"
            />
            <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="12" cy="12" r="10"/>
              <line x1="2" y1="12" x2="22" y2="12"/>
              <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
            </svg>
          </div>

          <h3 class="partner-title">{{ item.title }}</h3>

          <div class="partner-external-icon">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/>
              <polyline points="15 3 21 3 21 9"/>
              <line x1="10" y1="14" x2="21" y2="3"/>
            </svg>
          </div>
        </a>
      </div>
    </div>
  </section>
</template>

<style scoped>
.partners-section {
  padding-top: 36px;
  padding-bottom: 36px;
}

.partners-tabs {
  display: flex;
  align-items: center;
  gap: 8px;
  background-color: var(--color-gray-100);
  padding: 4px;
  border-radius: var(--radius-full);
}

.tab-btn {
  padding: 8px 18px;
  border-radius: var(--radius-full);
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--color-gray-600);
  transition: all var(--transition-fast);
}

.tab-btn.active {
  background-color: var(--color-primary);
  color: #ffffff;
  box-shadow: 0 2px 4px rgba(1, 61, 140, 0.2);
}

.partners-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}

.partner-card {
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: 16px 18px;
  display: flex;
  align-items: center;
  gap: 14px;
  box-shadow: var(--shadow-sm);
  transition: all var(--transition-fast);
  position: relative;
}

.partner-card:hover {
  transform: translateY(-3px);
  border-color: var(--color-primary);
  box-shadow: var(--shadow-md);
}

.partner-emblem-box {
  width: 48px;
  height: 48px;
  border-radius: var(--radius-md);
  background-color: var(--color-primary-50);
  color: var(--color-primary);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  overflow: hidden;
  padding: 6px;
}

.partner-icon {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.partner-emblem-box svg {
  width: 22px;
  height: 22px;
}

.partner-title {
  font-size: 0.88rem;
  font-weight: 700;
  color: var(--color-gray-800);
  line-height: 1.35;
  flex: 1;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.partner-card:hover .partner-title {
  color: var(--color-primary);
}

.partner-external-icon {
  color: var(--color-gray-400);
  transition: all var(--transition-fast);
  flex-shrink: 0;
}

.partner-external-icon svg {
  width: 16px;
  height: 16px;
}

.partner-card:hover .partner-external-icon {
  color: var(--color-primary);
  transform: translate(2px, -2px);
}

@media (max-width: 1024px) {
  .partners-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .partners-grid {
    grid-template-columns: 1fr;
  }
}
</style>
