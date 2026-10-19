<script setup>
import { ref, computed, onMounted, onUnmounted, nextTick } from 'vue'

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  },
  news: {
    type: Array,
    default: () => []
  },
  services: {
    type: Array,
    default: () => []
  },
  documents: {
    type: Array,
    default: () => []
  }
})

const emit = defineEmits(['close'])

const searchQuery = ref('')
const searchInput = ref(null)

const recentSearches = ['Teatr spektakllari', 'Musiqa maktablari', 'Litsenziya', 'Bo‘sh ish o‘rinlari', 'Meros']

const searchResults = computed(() => {
  const q = searchQuery.value.trim().toLowerCase()
  if (!q) return { news: [], services: [], documents: [] }

  const matchedNews = props.news.filter(n => 
    n.title?.toLowerCase().includes(q) || n.activity_title?.toLowerCase().includes(q)
  ).slice(0, 4)

  const matchedServices = props.services.filter(s => 
    s.title?.toLowerCase().includes(q) || s.category?.toLowerCase().includes(q)
  ).slice(0, 3)

  const matchedDocs = props.documents.filter(d => 
    d.title?.toLowerCase().includes(q) || d.number?.toLowerCase().includes(q)
  ).slice(0, 3)

  return { news: matchedNews, services: matchedServices, documents: matchedDocs }
})

const totalResultsCount = computed(() => {
  return searchResults.value.news.length + searchResults.value.services.length + searchResults.value.documents.length
})

const handleKeydown = (e) => {
  if (e.key === 'Escape' && props.isOpen) {
    emit('close')
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
})
</script>

<template>
  <div v-if="isOpen" class="modal-backdrop" @click.self="emit('close')">
    <div class="search-modal-container">
      <!-- Search Input Header -->
      <div class="search-input-box">
        <svg class="search-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="11" cy="11" r="8"/>
          <line x1="21" y1="21" x2="16.65" y2="16.65"/>
        </svg>

        <input 
          ref="searchInput"
          v-model="searchQuery" 
          type="text" 
          placeholder="Madaniyat portali bo‘yicha izlash..."
          class="modal-search-input"
          autofocus
        />

        <button v-if="searchQuery" class="clear-btn" @click="searchQuery = ''" title="Tozalash">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="18" y1="6" x2="6" y2="18"/>
            <line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>

        <button class="close-modal-btn" @click="emit('close')" title="Yopish (Esc)">
          <kbd>Esc</kbd>
        </button>
      </div>

      <!-- Quick suggestions when query is empty -->
      <div v-if="!searchQuery" class="search-empty-state">
        <span class="suggestions-label">Ommabop so‘rovlar:</span>
        <div class="tags-row">
          <button 
            v-for="tag in recentSearches" 
            :key="tag"
            class="suggestion-tag"
            @click="searchQuery = tag"
          >
            {{ tag }}
          </button>
        </div>
      </div>

      <!-- Results view when querying -->
      <div v-else class="search-results-box">
        <div v-if="totalResultsCount === 0" class="no-results-msg">
          <p>"{{ searchQuery }}" so‘rovi bo‘yicha ma'lumot topilmadi.</p>
          <span class="hint">Iltimos, boshqa kalit so‘zlardan foydalanib ko‘ring.</span>
        </div>

        <div v-else class="results-sections">
          <!-- News Results -->
          <div v-if="searchResults.news.length > 0" class="result-group">
            <h4 class="group-title">Yangiliklar</h4>
            <div class="results-list">
              <a 
                v-for="n in searchResults.news" 
                :key="n.id" 
                :href="`#news-${n.id}`" 
                class="result-item"
                @click="emit('close')"
              >
                <div class="result-text">
                  <span class="result-tag">{{ n.activity_title || 'Yangilik' }}</span>
                  <h5 class="result-title">{{ n.title }}</h5>
                  <span class="result-date">{{ n.date }}</span>
                </div>
              </a>
            </div>
          </div>

          <!-- Services Results -->
          <div v-if="searchResults.services.length > 0" class="result-group">
            <h4 class="group-title">Xizmatlar</h4>
            <div class="results-list">
              <a 
                v-for="s in searchResults.services" 
                :key="s.id" 
                :href="s.url" 
                target="_blank" 
                rel="noopener"
                class="result-item"
                @click="emit('close')"
              >
                <div class="result-text">
                  <span class="result-tag">{{ s.category }}</span>
                  <h5 class="result-title">{{ s.title }}</h5>
                </div>
              </a>
            </div>
          </div>

          <!-- Documents Results -->
          <div v-if="searchResults.documents.length > 0" class="result-group">
            <h4 class="group-title">Normativ hujjatlar</h4>
            <div class="results-list">
              <a 
                v-for="d in searchResults.documents" 
                :key="d.id" 
                :href="d.url" 
                target="_blank" 
                rel="noopener"
                class="result-item"
                @click="emit('close')"
              >
                <div class="result-text">
                  <span class="result-tag">{{ d.number }}</span>
                  <h5 class="result-title">{{ d.title }}</h5>
                </div>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-backdrop {
  position: fixed;
  inset: 0;
  background-color: rgba(15, 23, 42, 0.65);
  backdrop-filter: blur(4px);
  z-index: 2000;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  padding-top: 80px;
  animation: fadeIn 0.15s ease;
}

.search-modal-container {
  width: 100%;
  max-width: 680px;
  background-color: #ffffff;
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-xl);
  overflow: hidden;
  border: 1px solid var(--border-color);
  animation: slideDown 0.2s ease;
}

.search-input-box {
  display: flex;
  align-items: center;
  padding: 16px 20px;
  border-bottom: 1px solid var(--border-color);
  gap: 12px;
}

.search-icon {
  width: 22px;
  height: 22px;
  color: var(--color-primary);
  flex-shrink: 0;
}

.modal-search-input {
  flex: 1;
  border: none;
  outline: none;
  font-size: 1.1rem;
  color: var(--color-gray-900);
  background: transparent;
  font-family: inherit;
}

.clear-btn {
  color: var(--color-gray-400);
  display: flex;
  align-items: center;
  justify-content: center;
}

.clear-btn svg {
  width: 18px;
  height: 18px;
}

.close-modal-btn kbd {
  background-color: var(--color-gray-100);
  border: 1px solid var(--border-color);
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 700;
  color: var(--color-gray-500);
}

.search-empty-state {
  padding: 24px;
}

.suggestions-label {
  display: block;
  font-size: 0.8rem;
  font-weight: 600;
  color: var(--color-gray-500);
  margin-bottom: 12px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.tags-row {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.suggestion-tag {
  background-color: var(--color-gray-100);
  color: var(--color-gray-700);
  padding: 6px 14px;
  border-radius: var(--radius-full);
  font-size: 0.85rem;
  font-weight: 500;
  transition: all var(--transition-fast);
}

.suggestion-tag:hover {
  background-color: var(--color-primary-50);
  color: var(--color-primary);
}

/* Results */
.search-results-box {
  max-height: 440px;
  overflow-y: auto;
  padding: 16px 20px;
}

.no-results-msg {
  text-align: center;
  padding: 40px 20px;
  color: var(--color-gray-600);
}

.no-results-msg .hint {
  display: block;
  font-size: 0.82rem;
  color: var(--color-gray-400);
  margin-top: 6px;
}

.result-group {
  margin-bottom: 20px;
}

.group-title {
  font-size: 0.78rem;
  font-weight: 700;
  color: var(--color-primary);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 10px;
  border-bottom: 1px solid var(--border-color);
  padding-bottom: 4px;
}

.results-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.result-item {
  display: block;
  padding: 10px 14px;
  border-radius: var(--radius-md);
  background-color: var(--color-gray-50);
  transition: all var(--transition-fast);
}

.result-item:hover {
  background-color: var(--color-primary-50);
  transform: translateX(4px);
}

.result-tag {
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--color-accent-dark);
  text-transform: uppercase;
  display: block;
  margin-bottom: 2px;
}

.result-title {
  font-size: 0.92rem;
  font-weight: 600;
  color: var(--color-gray-900);
  line-height: 1.35;
}

.result-date {
  font-size: 0.72rem;
  color: var(--color-gray-400);
  margin-top: 4px;
  display: block;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideDown {
  from { transform: translateY(-16px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}
</style>
