<script setup>
const props = defineProps({
  services: {
    type: Array,
    required: true
  }
})

const events = [
  {
    day: '25',
    month: 'Okt',
    time: '18:30',
    title: 'O‘zbekiston davlat akademik katta teatrida "Sevilya sartaroshi" operasi',
    venue: 'Alisher Navoiy nomidagi GABT'
  },
  {
    day: '28',
    month: 'Okt',
    time: '19:00',
    title: 'Xalqaro maqom san\'ati ansamblining tantanali konsert dasturi',
    venue: 'Turkiston san\'at saroyi'
  },
  {
    day: '02',
    month: 'Noy',
    time: '11:00',
    title: 'Respublika qo‘g‘irchoq teatrida yangi spektakl premyerasi',
    venue: 'O‘zbek milliy qo‘g‘irchoq teatri'
  },
  {
    day: '08',
    month: 'Noy',
    time: '15:00',
    title: '"O‘zbekiston xalq amaliy san\'ati durdonalari" ko‘rgazmasi',
    venue: 'Davlat san\'at muzeyi'
  }
]

const usefulLinks = [
  {
    id: 1,
    title: 'O‘zbekiston Respublikasi ochiq ma’lumotlar portali',
    url: 'https://data.egov.uz'
  },
  {
    id: 2,
    title: 'Yagona interaktiv davlat xizmatlari portali (my.gov.uz)',
    url: 'https://my.gov.uz'
  },
  {
    id: 3,
    title: 'Normativ-huquqiy hujjatlar loyihalari portali',
    url: 'https://regulation.gov.uz'
  },
  {
    id: 4,
    title: 'Yagona identifikatsiya tizimi (OneID)',
    url: 'https://id.egov.uz'
  }
]
</script>

<template>
  <section class="three-stack-section py-5">
    <div class="container">
      <div class="three-stack_three-stack__UIHJu">
        <!-- 1. Interaktiv xizmatlar (Col 1) -->
        <div class="stack-col flex flex-col" data-bordered="1">
          <div class="stack-header">
            <h2 class="font--lg font-bold uppercase" style="color: var(--color-text-primary)">
              Interaktiv xizmatlar
            </h2>
            <a href="https://my.gov.uz" target="_blank" rel="noopener" class="details-link_link__EkQDj">
              Barcha xizmatlar
              <span>
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24">
                  <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 12h10m0 0-4-4m4 4-4 4"/>
                </svg>
              </span>
            </a>
          </div>

          <!-- Services List -->
          <div class="stack-list">
            <a 
              v-for="srv in services.slice(0, 4)" 
              :key="srv.id"
              :href="srv.url || '#'" 
              target="_blank" 
              rel="noopener"
              class="link-box_box__1w0YP"
            >
              <div class="link-box_line__LLu7H"></div>
              <svg class="link-box_box__icon__B0zCB" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
                <polyline points="14 2 14 8 20 8"/>
                <line x1="16" y1="13" x2="8" y2="13"/>
                <line x1="16" y1="17" x2="8" y2="17"/>
              </svg>
              <div class="service-content">
                <p class="service-title">{{ srv.title }}</p>
                <span class="service-tag">{{ srv.category }}</span>
              </div>
              <svg class="external-arrow" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="7" y1="17" x2="17" y2="7"/>
                <polyline points="7 7 17 7 17 17"/>
              </svg>
            </a>
          </div>
        </div>

        <!-- 2. Voqealar taqvimi (Col 2) -->
        <div class="stack-col flex flex-col" data-bordered="2">
          <div class="stack-header">
            <h2 class="font--lg font-bold uppercase" style="color: var(--color-text-primary)">
              Voqealar taqvimi
            </h2>
            <a href="#/madaniyat/news/events" class="details-link_link__EkQDj">
              Barcha voqealar
              <span>
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24">
                  <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 12h10m0 0-4-4m4 4-4 4"/>
                </svg>
              </span>
            </a>
          </div>

          <!-- Events List -->
          <div class="stack-list">
            <div 
              v-for="(evt, idx) in events" 
              :key="idx" 
              class="event-card link-box_box__1w0YP"
            >
              <div class="event-date-block">
                <span class="event-month">{{ evt.month }}</span>
                <strong class="event-day">{{ evt.day }}</strong>
              </div>
              <div class="event-content">
                <p class="event-title">{{ evt.title }}</p>
                <div class="event-footer-meta">
                  <span class="event-time">⏰ {{ evt.time }}</span>
                  <span class="event-venue">📍 {{ evt.venue }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- 3. Foydali havolalar (Col 3) -->
        <div class="stack-col flex flex-col" data-bordered="3">
          <div class="stack-header">
            <h2 class="font--lg font-bold uppercase" style="color: var(--color-text-primary)">
              Foydali havolalar
            </h2>
            <a href="#/madaniyat/usefullinks" class="details-link_link__EkQDj">
              Barcha havolalar
              <span>
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24">
                  <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 12h10m0 0-4-4m4 4-4 4"/>
                </svg>
              </span>
            </a>
          </div>

          <!-- Links List -->
          <div class="stack-list">
            <a 
              v-for="link in usefulLinks" 
              :key="link.id" 
              :href="link.url" 
              target="_blank" 
              rel="noopener"
              class="link-box_box__1w0YP"
            >
              <div class="link-box_line__LLu7H"></div>
              <svg class="link-box_box__icon__B0zCB" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"/>
                <path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"/>
              </svg>
              <div class="service-content">
                <p class="service-title">{{ link.title }}</p>
              </div>
              <svg class="external-arrow" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="7" y1="17" x2="17" y2="7"/>
                <polyline points="7 7 17 7 17 17"/>
              </svg>
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.three-stack-section {
  padding-top: 2rem;
  padding-bottom: 2.5rem;
}

.stack-col {
  display: flex;
  flex-direction: column;
}

.stack-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-bottom: 1.25rem;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.stack-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.service-content {
  flex: 1;
  min-width: 0;
}

.service-title {
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

.service-tag {
  font-size: 0.6875rem;
  color: var(--color-primary);
  font-weight: 500;
  text-transform: uppercase;
  margin-top: 2px;
  display: block;
}

.external-arrow {
  color: #adb5bd;
  flex-shrink: 0;
  transition: all 0.2s ease;
}

.link-box_box__1w0YP:hover .external-arrow {
  color: var(--color-primary);
  transform: translate(2px, -2px);
}

/* Event Card Specifics */
.event-card {
  display: flex;
  align-items: center;
  padding: 0.875rem 1rem;
  gap: 1rem;
}

.event-date-block {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: var(--color-primary);
  color: #ffffff;
  padding: 0.375rem 0.625rem;
  border-radius: 4px;
  min-width: 3.25rem;
  flex-shrink: 0;
}

.event-month {
  font-size: 0.625rem;
  font-weight: 700;
  text-transform: uppercase;
  line-height: 1;
}

.event-day {
  font-size: 1.25rem;
  font-weight: 800;
  line-height: 1.1;
}

.event-content {
  flex: 1;
  min-width: 0;
}

.event-title {
  font-size: 0.8125rem;
  font-weight: 600;
  color: #222222;
  line-height: 1.3;
  margin: 0;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.event-footer-meta {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-top: 4px;
  font-size: 0.6875rem;
  color: #6c757d;
  flex-wrap: wrap;
}
</style>
