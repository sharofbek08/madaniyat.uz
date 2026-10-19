<script setup>
import { ref } from 'vue'

const props = defineProps({
  siteInfo: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['open-symbols', 'open-accessibility'])

const selectedLang = ref('oz')
const isLangOpen = ref(false)

const languages = [
  { code: 'oz', label: 'O‘zbekcha' },
  { code: 'uz', label: 'Ўзбекча' },
  { code: 'ru', label: 'Русский' },
  { code: 'en', label: 'English' },
  { code: 'qr', label: 'Qaraqalpaqsha' }
]

const setLang = (code) => {
  selectedLang.value = code
  isLangOpen.value = false
}

const toggleLang = () => {
  isLangOpen.value = !isLangOpen.value
}
</script>

<template>
  <div class="top-bar">
    <div class="container top-bar-container">
      <!-- Left side: State symbols & Weather & Currency -->
      <div class="top-bar-left">
        <!-- State Symbols trigger -->
        <button class="top-btn state-symbols-btn" @click="emit('open-symbols')" title="Davlat ramzlari">
          <svg class="top-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M4 15s1-1 4-1 5 2 8 2 4-1 4-1V3s-1 1-4 1-5-2-8-2-4 1-4 1z"/>
            <line x1="4" y1="22" x2="4" y2="15"/>
          </svg>
          <span class="btn-text">Davlat ramzlari</span>
        </button>

        <span class="divider"></span>

        <!-- Weather -->
        <div class="weather-widget" :title="`Ob-havo: ${siteInfo.weather?.condition}`">
          <svg class="weather-icon" viewBox="0 0 24 24" fill="none" stroke="#f59e0b" stroke-width="2">
            <circle cx="12" cy="12" r="4"/>
            <path d="M12 2v2M12 20v2M4.93 4.93l1.41 1.41M17.66 17.66l1.41 1.41M2 12h2M20 12h2M6.34 17.66l-1.41 1.41M19.07 4.93l-1.41 1.41"/>
          </svg>
          <span class="weather-city">{{ siteInfo.weather?.city }}</span>
          <span class="weather-temp">{{ siteInfo.weather?.temp }}</span>
        </div>

        <span class="divider"></span>

        <!-- Currency -->
        <div class="currency-widget" title="Markaziy Bank valyuta kurslari">
          <span class="curr-title">MB kursi:</span>
          <div class="curr-items">
            <span v-for="c in siteInfo.currency" :key="c.code" class="curr-item">
              <strong>{{ c.code }}:</strong> {{ c.rate }}
            </span>
          </div>
        </div>
      </div>

      <!-- Right side: Accessibility & Language & Login -->
      <div class="top-bar-right">
        <!-- Accessibility Modal Trigger -->
        <button class="top-btn accessibility-btn" @click="emit('open-accessibility')" title="Maxsus imkoniyatlar (zaif ko‘ruvchilar uchun)">
          <svg class="top-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="12" cy="12" r="3"/>
            <path d="M2 12s3-7 10-7 10 7 10 7-3 7-10 7-10-7-10-7z"/>
          </svg>
          <span class="btn-text">Maxsus imkoniyatlar</span>
        </button>

        <span class="divider"></span>

        <!-- Language Selector -->
        <div class="lang-dropdown-wrapper">
          <button class="top-btn lang-btn" @click="toggleLang" aria-label="Tilni tanlash">
            <svg class="top-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="12" cy="12" r="10"/>
              <line x1="2" y1="12" x2="22" y2="12"/>
              <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
            </svg>
            <span class="lang-text">{{ languages.find(l => l.code === selectedLang)?.label }}</span>
            <svg class="chevron-icon" :class="{ open: isLangOpen }" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="6 9 12 15 18 9"/>
            </svg>
          </button>

          <transition name="fade">
            <div v-if="isLangOpen" class="lang-menu">
              <button 
                v-for="l in languages" 
                :key="l.code"
                class="lang-option"
                :class="{ active: l.code === selectedLang }"
                @click="setLang(l.code)"
              >
                {{ l.label }}
              </button>
            </div>
          </transition>
        </div>

        <span class="divider"></span>

        <!-- Login / Cabinet -->
        <a href="https://id.egov.uz" target="_blank" rel="noopener" class="login-btn" title="Yagona identifikatsiya tizimi orqali kirish">
          <svg class="login-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"/>
            <polyline points="10 17 15 12 10 7"/>
            <line x1="15" y1="12" x2="3" y2="12"/>
          </svg>
          <span>Kirish (OneID)</span>
        </a>
      </div>
    </div>
  </div>
</template>

<style scoped>
.top-bar {
  background-color: #f1f5f9;
  border-bottom: 1px solid var(--border-color);
  font-size: 0.8rem;
  color: var(--color-gray-600);
  height: 38px;
  display: flex;
  align-items: center;
  position: relative;
  z-index: 1002;
}

.top-bar-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.top-bar-left,
.top-bar-right {
  display: flex;
  align-items: center;
  gap: 12px;
}

.divider {
  width: 1px;
  height: 14px;
  background-color: var(--color-gray-300);
}

.top-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  color: var(--color-gray-700);
  font-size: 0.8rem;
  padding: 4px 6px;
  border-radius: var(--radius-sm);
  transition: var(--transition-fast);
}

.top-btn:hover {
  color: var(--color-primary);
  background-color: var(--color-gray-200);
}

.top-icon {
  width: 14px;
  height: 14px;
}

.weather-widget {
  display: flex;
  align-items: center;
  gap: 5px;
}

.weather-icon {
  width: 16px;
  height: 16px;
}

.weather-city {
  color: var(--color-gray-500);
}

.weather-temp {
  font-weight: 600;
  color: var(--color-gray-800);
}

.currency-widget {
  display: flex;
  align-items: center;
  gap: 8px;
}

.curr-title {
  color: var(--color-gray-500);
  font-size: 0.75rem;
}

.curr-items {
  display: flex;
  align-items: center;
  gap: 8px;
}

.curr-item {
  color: var(--color-gray-700);
}

.curr-item strong {
  font-weight: 600;
  color: var(--color-primary);
}

.lang-dropdown-wrapper {
  position: relative;
}

.lang-btn {
  font-weight: 500;
}

.chevron-icon {
  width: 12px;
  height: 12px;
  transition: transform var(--transition-fast);
}

.chevron-icon.open {
  transform: rotate(180deg);
}

.lang-menu {
  position: absolute;
  top: 100%;
  right: 0;
  margin-top: 4px;
  background: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-lg);
  min-width: 140px;
  padding: 4px 0;
  z-index: 1010;
}

.lang-option {
  width: 100%;
  text-align: left;
  padding: 6px 12px;
  font-size: 0.8rem;
  color: var(--color-gray-700);
  transition: var(--transition-fast);
}

.lang-option:hover {
  background-color: var(--color-primary-50);
  color: var(--color-primary);
}

.lang-option.active {
  font-weight: 600;
  color: var(--color-primary);
  background-color: var(--color-primary-50);
}

.login-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  background-color: var(--color-primary);
  color: #ffffff;
  font-weight: 600;
  font-size: 0.75rem;
  padding: 4px 10px;
  border-radius: var(--radius-sm);
  transition: var(--transition-fast);
}

.login-btn:hover {
  background-color: var(--color-primary-dark);
}

.login-icon {
  width: 13px;
  height: 13px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.15s ease, transform 0.15s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-4px);
}

@media (max-width: 992px) {
  .currency-widget {
    display: none;
  }
}

@media (max-width: 768px) {
  .weather-widget {
    display: none;
  }
  .btn-text {
    display: none;
  }
}
</style>
