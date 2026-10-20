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
  { id: 'tenders', label: 'Tanlovlar va tenderlar' },
  { id: 'eco', label: 'Ekofaol xodim' },
  { id: 'speeches', label: 'Nutqlar' },
  { id: 'council', label: 'Jamoatchilik kengashi' }
]

const filteredNews = computed(() => {
  if (activeCategory.value === 'all') return props.news
  if (activeCategory.value === 'news') {
    return props.news.filter(n => n.category_code === 'news' || n.category_title === 'Yangiliklar')
  }
  if (activeCategory.value === 'tenders') {
    return props.news.filter(n => n.title?.toLowerCase().includes('tanlov') || n.title?.toLowerCase().includes('tender') || n.category_code === 'tenders')
  }
  if (activeCategory.value === 'eco') {
    return props.news.filter(n => n.title?.toLowerCase().includes('eko') || n.category_code === 'eco')
  }
  return props.news
})

const getDay = (dateStr) => {
  if (!dateStr) return '20'
  try {
    const d = new Date(dateStr)
    return String(d.getDate()).padStart(2, '0')
  } catch (e) {
    return '20'
  }
}

const getMonthShort = (dateStr) => {
  if (!dateStr) return 'OKT'
  try {
    const d = new Date(dateStr)
    const months = ['YAN', 'FEV', 'MAR', 'APR', 'MAY', 'IYUN', 'IYUL', 'AVG', 'SEN', 'OKT', 'NOY', 'DEK']
    return months[d.getMonth()] || 'OKT'
  } catch (e) {
    return 'OKT'
  }
}

const formatDate = (dateStr) => {
  if (!dateStr) return '20.10.2026'
  try {
    const d = new Date(dateStr)
    const day = String(d.getDate()).padStart(2, '0')
    const month = String(d.getMonth() + 1).padStart(2, '0')
    const year = d.getFullYear()
    return `${day}.${month}.${year}`
  } catch (e) {
    return dateStr
  }
}
</script>

<template>
  <section class="section-news">
    <div class="container">
      <!-- Section Header with Category Filter Buttons -->
      <div class="news-header-row">
        <h2 class="font--lg font-bold uppercase" style="color: var(--color-text-primary)">
          Yangiliklar
        </h2>

        <!-- Category Pills -->
        <div class="category-pills">
          <button 
            v-for="cat in categories" 
            :key="cat.id"
            class="cat-pill-btn"
            :class="{ active: activeCategory === cat.id }"
            @click="activeCategory = cat.id"
          >
            {{ cat.label }}
          </button>
        </div>
      </div>

      <!-- 3-Column News Grid (Gov.uz Mantine 3-stack) -->
      <div class="news-grid">
        <article 
          v-for="item in filteredNews.slice(0, 6)" 
          :key="item.id" 
          class="news-card_news__qjYn_"
        >
          <!-- Image Container -->
          <a :href="`#/madaniyat/news/${item.id}`" class="news-card_news__img__odWx_">
            <img 
              :src="item.anons_image || '/img/ministry_logo.png'" 
              :alt="item.title"
              class="news-card-img"
              loading="lazy"
            />
          </a>

          <!-- Signature Overlapping White Content Box -->
          <div class="news-card_news__content__sqBXx">
            <div class="news-main-body">
              <!-- Left Date Box -->
              <div class="news-card_news__date__hlyaP">
                <span class="month-short">{{ getMonthShort(item.date) }}</span>
                <strong class="day-number">{{ getDay(item.date) }}</strong>
              </div>

              <!-- Right Text Box -->
              <div class="news-text-box">
                <p class="news-card_news__title__2kejC">
                  <a :href="`#/madaniyat/news/${item.id}`">{{ item.title }}</a>
                </p>
                <p v-if="item.anons" class="news-card_news__anons__O53sh">
                  {{ item.anons }}
                </p>
                <p v-else class="news-card_news__anons__O53sh">
                  {{ item.activity_title || 'O‘zbekiston Respublikasi Madaniyat vazirligi rasmiy axboroti' }}
                </p>
              </div>
            </div>

            <!-- Card Footer -->
            <div class="news-card_news__footer__NnhqK">
              <span class="footer-date">{{ formatDate(item.date) }}</span>
              <a :href="`#/madaniyat/news/${item.id}`" class="news-card_news__type__1D3lj">
                {{ item.category_title || 'Yangiliklar' }}
              </a>
            </div>
          </div>
        </article>
      </div>

      <!-- Bottom 'Barcha yangiliklar' link -->
      <div class="news-bottom-link">
        <a href="#/madaniyat/news/news" class="details-link_link__EkQDj">
          Barcha yangiliklar
          <span>
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 12h10m0 0-4-4m4 4-4 4"/>
            </svg>
          </span>
        </a>
      </div>
    </div>
  </section>
</template>

<style scoped>
.section-news {
  padding-top: 1.5rem;
  padding-bottom: 2.5rem;
}

.news-header-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1.5rem;
  padding-bottom: 1.75rem;
  flex-wrap: wrap;
}

.category-pills {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.cat-pill-btn {
  padding: 0.375rem 1rem;
  border-radius: 9999px;
  font-size: 0.875rem;
  font-weight: 500;
  border: 1px solid #ced4da;
  background-color: transparent;
  color: #495057;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.cat-pill-btn:hover {
  border-color: var(--color-primary);
  color: var(--color-primary);
}

.cat-pill-btn.active {
  background-color: var(--color-primary);
  border-color: var(--color-primary);
  color: #ffffff;
}

/* News Grid */
.news-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  column-gap: 2rem;
  row-gap: 3.5rem;
}

@media (max-width: 1200px) {
  .news-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 768px) {
  .news-grid {
    grid-template-columns: 1fr;
  }
}

/* Gov.uz 1:1 News Card */
.news-card_news__qjYn_ {
  display: flex;
  flex-direction: column;
  position: relative;
  width: 100%;
}

.news-card_news__img__odWx_ {
  position: relative;
  display: block;
  width: 100%;
  height: 17.5rem;
  overflow: hidden;
  background-color: #f1f3f5;
  border-radius: 2px;
}

.news-card-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.news-card_news__qjYn_:hover .news-card-img {
  transform: scale(1.03);
}

/* Overlapping White Box */
.news-card_news__content__sqBXx {
  position: relative;
  width: calc(100% - 2.5rem);
  margin-top: -3.5rem;
  margin-left: auto;
  margin-right: auto;
  background-color: #ffffff;
  border: 1px solid var(--color-border);
  padding: 1.25rem 1.25rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  min-height: 11rem;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.05);
  border-radius: 2px;
}

.news-main-body {
  display: flex;
  gap: 1rem;
  align-items: flex-start;
}

.news-card_news__date__hlyaP {
  flex-basis: 3.5rem;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-right: 1px solid #e9ecef;
  padding-right: 0.75rem;
  text-align: center;
}

.month-short {
  font-size: 0.6875rem;
  font-weight: 700;
  color: #000000;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  line-height: 1;
}

.day-number {
  font-size: 1.5rem;
  font-weight: 800;
  color: #495057;
  line-height: 1.2;
}

.news-text-box {
  flex: 1;
  min-width: 0;
}

.news-card_news__title__2kejC {
  color: #222222;
  font-size: 0.9375rem;
  font-weight: 600;
  line-height: 1.35;
  margin: 0;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  transition: color 0.15s linear;
}

.news-card_news__title__2kejC a:hover {
  color: var(--color-text-primary);
  text-decoration: underline;
}

.news-card_news__anons__O53sh {
  font-size: 0.8125rem;
  color: #707070;
  line-height: 1.35;
  margin-top: 0.5rem;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.news-card_news__footer__NnhqK {
  margin-top: 1rem;
  padding-top: 0.75rem;
  border-top: 1px solid #f1f3f5;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.footer-date {
  font-size: 0.75rem;
  color: #868e96;
}

.news-card_news__type__1D3lj {
  font-size: 0.8125rem;
  font-weight: 500;
  color: var(--color-text-primary);
  text-decoration: none;
}

.news-card_news__type__1D3lj:hover {
  text-decoration: underline;
}

.news-bottom-link {
  margin-top: 2rem;
  display: flex;
}
</style>
