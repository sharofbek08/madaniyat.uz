<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  news: {
    type: Array,
    required: true
  }
})

const activeCategory = ref('all')

const categories = [
  { id: 'all', label: 'Barchasi' },
  { id: 'news', label: 'Yangiliklar' },
  { id: 'events', label: 'Voqealar taqvimi' },
  { id: 'announcements', label: 'E‘lonlar' },
  { id: 'press', label: 'Matbuot xabarlari' }
]

const filteredNews = computed(() => {
  if (activeCategory.value === 'all') return props.news
  if (activeCategory.value === 'news') return props.news.filter(n => n.category_code === 'news' || !n.category_code)
  if (activeCategory.value === 'events') return props.news.filter(n => n.activity_title?.includes('Voqealar') || n.title?.includes('repertuar'))
  return props.news
})

const leadNews = computed(() => filteredNews.value[0] || null)
const sideNews = computed(() => filteredNews.value.slice(1, 4))
const bottomNews = computed(() => filteredNews.value.slice(4, 8))

const formatDate = (dateStr) => {
  if (!dateStr) return ''
  try {
    const d = new Date(dateStr)
    const months = ['Yanvar', 'Fevral', 'Mart', 'Aprel', 'May', 'Iyun', 'Iyul', 'Avgust', 'Sentabr', 'Oktabr', 'Noyabr', 'Dekabr']
    const day = d.getDate()
    const month = months[d.getMonth()]
    const year = d.getFullYear()
    const hours = String(d.getHours()).padStart(2, '0')
    const mins = String(d.getMinutes()).padStart(2, '0')
    return `${day}-${month}, ${year} ${hours}:${mins}`
  } catch (e) {
    return dateStr
  }
}
</script>

<template>
  <section class="home-section news-section">
    <div class="container">
      <!-- Section Header -->
      <div class="section-header-row">
        <div>
          <h2 class="section-title">So‘nggi yangiliklar</h2>
          <p class="section-subtitle">Madaniyat vazirligi va tizimdagi tashkilotlarning rasmiy xabarlari</p>
        </div>

        <!-- Filter tabs -->
        <div class="category-tabs">
          <button 
            v-for="cat in categories" 
            :key="cat.id" 
            class="tab-btn"
            :class="{ active: activeCategory === cat.id }"
            @click="activeCategory = cat.id"
          >
            {{ cat.label }}
          </button>
        </div>
      </div>

      <!-- Main News Grid -->
      <div class="news-main-grid">
        <!-- Lead Featured Article (Big Card) -->
        <article v-if="leadNews" class="lead-news-card">
          <a :href="`#news-${leadNews.id}`" class="lead-image-link">
            <img 
              :src="leadNews.anons_image" 
              :alt="leadNews.title"
              class="lead-image"
              loading="lazy" 
            />
            <div class="image-overlay-gradient"></div>
            <span class="lead-badge badge badge-primary">
              {{ leadNews.activity_title || leadNews.category_title || 'Yangiliklar' }}
            </span>
          </a>

          <div class="lead-content">
            <div class="news-meta">
              <span class="meta-item date-meta">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <rect x="3" y="4" width="18" height="18" rx="2" ry="2"/>
                  <line x1="16" y1="2" x2="16" y2="6"/>
                  <line x1="8" y1="2" x2="8" y2="6"/>
                  <line x1="3" y1="10" x2="21" y2="10"/>
                </svg>
                {{ formatDate(leadNews.date) }}
              </span>

              <span class="meta-item views-meta">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/>
                  <circle cx="12" cy="12" r="3"/>
                </svg>
                {{ leadNews.views || 0 }} ko‘rildi
              </span>
            </div>

            <h3 class="lead-title">
              <a :href="`#news-${leadNews.id}`">{{ leadNews.title }}</a>
            </h3>

            <p class="lead-anons">
              {{ leadNews.anons || 'O‘zbekiston Respublikasi Madaniyat vazirligi tomonidan tasdiqlangan dastur doirasida yurtimiz teatrlari va madaniyat maskanlarida yangi ijodiy mavsum spektakllari va ko‘rgazmalari boshlandi.' }}
            </p>

            <div class="lead-footer">
              <a :href="`#news-${leadNews.id}`" class="read-more-btn">
                <span>Batafsil o‘qish</span>
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <line x1="5" y1="12" x2="19" y2="12"/>
                  <polyline points="12 5 19 12 12 19"/>
                </svg>
              </a>
            </div>
          </div>
        </article>

        <!-- Side News Column (3 items) -->
        <div class="side-news-column">
          <article v-for="item in sideNews" :key="item.id" class="side-news-card">
            <a :href="`#news-${item.id}`" class="side-image-link">
              <img 
                :src="item.anons_image" 
                :alt="item.title"
                class="side-image"
                loading="lazy" 
              />
            </a>

            <div class="side-content">
              <div class="news-meta">
                <span class="meta-tag">{{ item.activity_title || item.category_title || 'Yangilik' }}</span>
                <span class="meta-bullet">•</span>
                <span class="meta-date">{{ formatDate(item.date) }}</span>
              </div>

              <h4 class="side-title">
                <a :href="`#news-${item.id}`">{{ item.title }}</a>
              </h4>

              <div class="side-footer">
                <span class="views-mini">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/>
                    <circle cx="12" cy="12" r="3"/>
                  </svg>
                  {{ item.views || 0 }}
                </span>
              </div>
            </div>
          </article>
        </div>
      </div>

      <!-- Additional News Row -->
      <div v-if="bottomNews.length > 0" class="bottom-news-row">
        <article v-for="item in bottomNews" :key="item.id" class="grid-news-card">
          <a :href="`#news-${item.id}`" class="grid-image-link">
            <img 
              :src="item.anons_image" 
              :alt="item.title"
              class="grid-image"
              loading="lazy" 
            />
            <span class="grid-badge badge badge-primary">
              {{ item.category_title || 'Yangilik' }}
            </span>
          </a>

          <div class="grid-content">
            <div class="news-meta">
              <span class="meta-date">{{ formatDate(item.date) }}</span>
              <span class="meta-bullet">•</span>
              <span class="views-mini">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/>
                  <circle cx="12" cy="12" r="3"/>
                </svg>
                {{ item.views || 0 }}
              </span>
            </div>

            <h4 class="grid-title">
              <a :href="`#news-${item.id}`">{{ item.title }}</a>
            </h4>
          </div>
        </article>
      </div>

      <!-- Bottom Call to action -->
      <div class="news-bottom-actions">
        <a href="#all-news" class="see-all-btn">
          <span>Barcha yangiliklar bilan tanishish</span>
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="5" y1="12" x2="19" y2="12"/>
            <polyline points="12 5 19 12 12 19"/>
          </svg>
        </a>
      </div>
    </div>
  </section>
</template>

<style scoped>
.news-section {
  padding-top: 24px;
}

.category-tabs {
  display: flex;
  align-items: center;
  gap: 8px;
  background-color: var(--color-gray-100);
  padding: 4px;
  border-radius: var(--radius-full);
  overflow-x: auto;
}

.tab-btn {
  padding: 6px 16px;
  border-radius: var(--radius-full);
  font-size: 0.84rem;
  font-weight: 600;
  color: var(--color-gray-600);
  transition: var(--transition-fast);
  white-space: nowrap;
}

.tab-btn:hover {
  color: var(--color-primary);
}

.tab-btn.active {
  background-color: var(--color-primary);
  color: #ffffff;
  box-shadow: 0 2px 4px rgba(1, 61, 140, 0.2);
}

/* Main Grid */
.news-main-grid {
  display: grid;
  grid-template-columns: 1.4fr 1fr;
  gap: 24px;
  margin-bottom: 24px;
}

/* Lead Card */
.lead-news-card {
  background-color: #ffffff;
  border-radius: var(--radius-lg);
  border: 1px solid var(--border-color);
  box-shadow: var(--shadow-sm);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: transform var(--transition-normal), box-shadow var(--transition-normal);
}

.lead-news-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}

.lead-image-link {
  position: relative;
  display: block;
  height: 320px;
  overflow: hidden;
  background-color: #e2e8f0;
}

.lead-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.lead-news-card:hover .lead-image {
  transform: scale(1.05);
}

.image-overlay-gradient {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.5) 0%, transparent 60%);
}

.lead-badge {
  position: absolute;
  top: 16px;
  left: 16px;
  z-index: 2;
  box-shadow: var(--shadow-sm);
}

.lead-content {
  padding: 24px;
  display: flex;
  flex-direction: column;
  flex: 1;
}

.news-meta {
  display: flex;
  align-items: center;
  gap: 14px;
  font-size: 0.8rem;
  color: var(--color-gray-500);
  margin-bottom: 12px;
}

.meta-item {
  display: inline-flex;
  align-items: center;
  gap: 5px;
}

.meta-item svg,
.views-mini svg {
  width: 14px;
  height: 14px;
}

.lead-title {
  font-size: 1.35rem;
  font-weight: 800;
  line-height: 1.35;
  color: var(--color-gray-900);
  margin-bottom: 12px;
}

.lead-title a:hover {
  color: var(--color-primary);
}

.lead-anons {
  font-size: 0.92rem;
  color: var(--color-gray-600);
  line-height: 1.55;
  margin-bottom: 20px;
  flex: 1;
}

.lead-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-top: 1px solid var(--border-color);
  padding-top: 16px;
}

.read-more-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-size: 0.88rem;
  font-weight: 700;
  color: var(--color-primary);
  transition: gap var(--transition-fast);
}

.read-more-btn svg {
  width: 16px;
  height: 16px;
  transition: transform var(--transition-fast);
}

.read-more-btn:hover {
  color: var(--color-primary-dark);
}

.read-more-btn:hover svg {
  transform: translateX(4px);
}

/* Side Column */
.side-news-column {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.side-news-card {
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: 12px;
  display: flex;
  gap: 14px;
  transition: all var(--transition-fast);
}

.side-news-card:hover {
  border-color: var(--color-primary-light);
  transform: translateX(4px);
  box-shadow: var(--shadow-md);
}

.side-image-link {
  width: 120px;
  height: 90px;
  border-radius: var(--radius-sm);
  overflow: hidden;
  flex-shrink: 0;
  background-color: #e2e8f0;
}

.side-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform var(--transition-normal);
}

.side-news-card:hover .side-image {
  transform: scale(1.08);
}

.side-content {
  display: flex;
  flex-direction: column;
  flex: 1;
}

.meta-tag {
  color: var(--color-primary);
  font-weight: 600;
  font-size: 0.72rem;
  text-transform: uppercase;
}

.meta-bullet {
  color: var(--color-gray-400);
}

.meta-date {
  font-size: 0.75rem;
  color: var(--color-gray-400);
}

.side-title {
  font-size: 0.95rem;
  font-weight: 700;
  line-height: 1.35;
  color: var(--color-gray-800);
  margin-top: 4px;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  flex: 1;
}

.side-title a:hover {
  color: var(--color-primary);
}

.side-footer {
  margin-top: 8px;
}

.views-mini {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  font-size: 0.75rem;
  color: var(--color-gray-400);
}

/* Bottom News Row */
.bottom-news-row {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  margin-top: 10px;
  margin-bottom: 28px;
}

.grid-news-card {
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: all var(--transition-fast);
}

.grid-news-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-md);
}

.grid-image-link {
  position: relative;
  display: block;
  height: 160px;
  overflow: hidden;
}

.grid-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform var(--transition-normal);
}

.grid-news-card:hover .grid-image {
  transform: scale(1.06);
}

.grid-badge {
  position: absolute;
  top: 10px;
  left: 10px;
}

.grid-content {
  padding: 16px;
  display: flex;
  flex-direction: column;
  flex: 1;
}

.grid-title {
  font-size: 0.92rem;
  font-weight: 700;
  line-height: 1.4;
  color: var(--color-gray-800);
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
  flex: 1;
}

.grid-title a:hover {
  color: var(--color-primary);
}

.news-bottom-actions {
  display: flex;
  justify-content: center;
}

@media (max-width: 1024px) {
  .news-main-grid {
    grid-template-columns: 1fr;
  }
  .bottom-news-row {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .lead-image-link {
    height: 220px;
  }
  .bottom-news-row {
    grid-template-columns: 1fr;
  }
  .side-news-card {
    flex-direction: column;
  }
  .side-image-link {
    width: 100%;
    height: 150px;
  }
}
</style>
