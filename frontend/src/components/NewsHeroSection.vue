<script setup>
import { computed } from 'vue'

const props = defineProps({
  news: {
    type: Array,
    required: true
  }
})

// Social links matching gov.uz SocialMediaLinks component in the news header
const socials = [
  {
    name: 'X',
    url: 'https://twitter.com/madaniyatuz',
    icon: 'X'
  },
  {
    name: 'Youtube',
    url: 'https://youtube.com/@madaniyatvazirligirasmiy?si=74SawE1KuLPowou_',
    icon: 'Youtube'
  },
  {
    name: 'Facebook',
    url: 'https://www.facebook.com/madaniyatvazirligiuz',
    icon: 'Facebook'
  },
  {
    name: 'Telegram',
    url: 'https://t.me/s/madaniyatvazirligi',
    icon: 'Telegram'
  },
  {
    name: 'Instagram',
    url: 'https://www.instagram.com/madaniyatvazirligi/',
    icon: 'Instagram'
  }
]

// The 2 featured lead cards on the left
const leadCards = computed(() => props.news.slice(0, 2))

// The 5 compact news items on the right column
const rightListItems = computed(() => props.news.slice(2, 7))

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
  <section class="section-news pt-3 pb-5 3xl:pb-3" id="main-content">
    <div class="container">
      <!-- 1. Header: Left Title "Yangiliklar", Right SocialMediaLinks circles -->
      <div class="news-header-row">
        <h2 class="font--lg font-bold uppercase" style="color: var(--color-text-primary)">
          Yangiliklar
        </h2>

        <!-- Circular Social Media Links Icons (Gov.uz signature SocialMediaLinks) -->
        <div class="social-circles-row">
          <a 
            v-for="s in socials" 
            :key="s.name"
            :href="s.url"
            target="_blank" 
            rel="noopener"
            class="social-circle-btn"
            :title="s.name"
          >
            <!-- X -->
            <svg v-if="s.name === 'X'" viewBox="0 0 24 24" width="16" height="16" fill="currentColor">
              <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
            </svg>
            <!-- Youtube -->
            <svg v-else-if="s.name === 'Youtube'" viewBox="0 0 24 24" width="17" height="17" fill="currentColor">
              <path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/>
            </svg>
            <!-- Facebook -->
            <svg v-else-if="s.name === 'Facebook'" viewBox="0 0 24 24" width="17" height="17" fill="currentColor">
              <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
            </svg>
            <!-- Telegram -->
            <svg v-else-if="s.name === 'Telegram'" viewBox="0 0 24 24" width="16" height="16" fill="currentColor">
              <path d="M12 0C5.373 0 0 5.373 0 12s5.373 12 12 12 12-5.373 12-12S18.627 0 12 0zm5.894 8.221l-1.97 9.28c-.145.658-.537.818-1.084.508l-3-2.21-1.446 1.394c-.16.16-.295.295-.605.295l.213-3.053 5.56-5.023c.242-.213-.054-.333-.373-.121l-6.871 4.326-2.962-.924c-.643-.204-.657-.643.136-.953l11.57-4.458c.538-.196 1.006.128.832.941z"/>
            </svg>
            <!-- Instagram -->
            <svg v-else viewBox="0 0 24 24" width="17" height="17" fill="currentColor">
              <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/>
            </svg>
          </a>
        </div>
      </div>

      <!-- 2. Gov.uz Signature News Grid: 2 Big Cards + 1 Right Column with 5 compact items -->
      <div class="news-layout-grid">
        <!-- Lead Card 1 -->
        <article v-if="leadCards[0]" class="news-card_news__qjYn_">
          <a :href="`#/madaniyat/news/${leadCards[0].id}`" class="news-card_news__img__odWx_">
            <img 
              :src="leadCards[0].anons_image || '/img/ministry_logo.png'" 
              :alt="leadCards[0].title"
              class="news-card-img"
              loading="lazy"
            />
          </a>

          <div class="news-card_news__content__sqBXx">
            <div class="news-main-body">
              <div class="news-card_news__date__hlyaP">
                <span class="month-short">{{ getMonthShort(leadCards[0].date) }}</span>
                <strong class="day-number">{{ getDay(leadCards[0].date) }}</strong>
              </div>
              <div class="news-text-box">
                <p class="news-card_news__title__2kejC">
                  <a :href="`#/madaniyat/news/${leadCards[0].id}`">{{ leadCards[0].title }}</a>
                </p>
                <p class="news-card_news__anons__O53sh">
                  {{ leadCards[0].anons || leadCards[0].activity_title || 'O‘zbekiston Respublikasi Madaniyat vazirligi rasmiy axboroti' }}
                </p>
              </div>
            </div>

            <div class="news-card_news__footer__NnhqK">
              <span class="footer-date">{{ formatDate(leadCards[0].date) }}</span>
              <a :href="`#/madaniyat/news/${leadCards[0].id}`" class="news-card_news__type__1D3lj">
                {{ leadCards[0].category_title || 'Yangiliklar' }}
              </a>
            </div>
          </div>
        </article>

        <!-- Lead Card 2 -->
        <article v-if="leadCards[1]" class="news-card_news__qjYn_">
          <a :href="`#/madaniyat/news/${leadCards[1].id}`" class="news-card_news__img__odWx_">
            <img 
              :src="leadCards[1].anons_image || '/img/ministry_logo.png'" 
              :alt="leadCards[1].title"
              class="news-card-img"
              loading="lazy"
            />
          </a>

          <div class="news-card_news__content__sqBXx">
            <div class="news-main-body">
              <div class="news-card_news__date__hlyaP">
                <span class="month-short">{{ getMonthShort(leadCards[1].date) }}</span>
                <strong class="day-number">{{ getDay(leadCards[1].date) }}</strong>
              </div>
              <div class="news-text-box">
                <p class="news-card_news__title__2kejC">
                  <a :href="`#/madaniyat/news/${leadCards[1].id}`">{{ leadCards[1].title }}</a>
                </p>
                <p class="news-card_news__anons__O53sh">
                  {{ leadCards[1].anons || leadCards[1].activity_title || 'O‘zbekiston Respublikasi Madaniyat vazirligi rasmiy axboroti' }}
                </p>
              </div>
            </div>

            <div class="news-card_news__footer__NnhqK">
              <span class="footer-date">{{ formatDate(leadCards[1].date) }}</span>
              <a :href="`#/madaniyat/news/${leadCards[1].id}`" class="news-card_news__type__1D3lj">
                {{ leadCards[1].category_title || 'Yangiliklar' }}
              </a>
            </div>
          </div>
        </article>

        <!-- Right Column: 5 Compact News List Items (Gov.uz signature) -->
        <div class="compact-news-column">
          <div 
            v-for="item in rightListItems" 
            :key="item.id" 
            class="compact-news-item"
          >
            <div class="compact-news-meta">
              <span class="compact-date">{{ formatDate(item.date) }}</span>
              <span class="compact-dot">•</span>
              <span class="compact-category">{{ item.category_title || 'Yangiliklar' }}</span>
            </div>
            <h4 class="compact-title">
              <a :href="`#/madaniyat/news/${item.id}`">{{ item.title }}</a>
            </h4>
          </div>
        </div>
      </div>

      <!-- 3. Bottom Centered 'Barcha yangiliklar' Link (gov.uz standard) -->
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
  padding-bottom: 2rem;
}

.news-header-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1.5rem;
  padding-bottom: 1.5rem;
  flex-wrap: wrap;
}

/* Social Media Circles */
.social-circles-row {
  display: flex;
  align-items: center;
  gap: 0.625rem;
}

.social-circle-btn {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background-color: #f1f3f5;
  color: #495057;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  text-decoration: none;
}

.social-circle-btn:hover {
  background-color: var(--color-primary);
  color: #ffffff;
  transform: translateY(-2px);
}

/* Layout Grid: 3 columns (2 lead cards + 1 column of 5 compact items) */
.news-layout-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 2rem;
}

@media (max-width: 1200px) {
  .news-layout-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
  .compact-news-column {
    grid-column: span 2;
  }
}

@media (max-width: 768px) {
  .news-layout-grid {
    grid-template-columns: 1fr;
  }
  .compact-news-column {
    grid-column: span 1;
  }
}

/* Lead Cards */
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

/* Compact News Column (Right side, 5 items) */
.compact-news-column {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
  background-color: #ffffff;
  border: 1px solid var(--color-border);
  padding: 1.5rem 1.25rem;
  border-radius: 2px;
}

.compact-news-item {
  display: flex;
  flex-direction: column;
  padding-bottom: 1.125rem;
  border-bottom: 1px solid #f1f3f5;
}

.compact-news-item:last-child {
  padding-bottom: 0;
  border-bottom: none;
}

.compact-news-meta {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.75rem;
  color: #868e96;
  margin-bottom: 0.375rem;
}

.compact-dot {
  color: #ced4da;
}

.compact-category {
  color: var(--color-text-primary);
  font-weight: 500;
}

.compact-title {
  font-size: 0.875rem;
  font-weight: 600;
  color: #222222;
  line-height: 1.35;
  margin: 0;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.compact-title a:hover {
  color: var(--color-text-primary);
  text-decoration: underline;
}

/* Centered Bottom Link */
.news-bottom-link {
  margin-top: 2rem;
  display: flex;
  justify-content: center;
}
</style>
