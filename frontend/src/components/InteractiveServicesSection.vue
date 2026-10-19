<script setup>
import { ref } from 'vue'

const props = defineProps({
  services: {
    type: Array,
    required: true
  }
})

const selectedDate = ref(19)

const events = [
  {
    day: 19,
    month: 'Oktabr',
    time: '18:30',
    title: 'O‘zbekiston davlat akademik katta teatrida "Sevilya sartaroshi" operasi',
    venue: 'Alisher Navoiy nomidagi GABT',
    type: 'Teatr'
  },
  {
    day: 22,
    month: 'Oktabr',
    time: '19:00',
    title: "Xalqaro maqom san'ati ansamblining milliy konsert dasturi",
    venue: "Turkiston san'at saroyi",
    type: 'Konsert'
  },
  {
    day: 25,
    month: 'Oktabr',
    time: '11:00',
    title: 'Respublika qo‘g‘irchoq teatrida "Sehrli chiroq" ertak-spektakli',
    venue: 'O‘zbek milliy qo‘g‘irchoq teatri',
    type: 'Bolalar uchun'
  },
  {
    day: 28,
    month: 'Oktabr',
    time: '15:00',
    title: '"O‘zbekiston xalq amaliy san\'ati durdonalari" ko‘rgazmasi ochilishi',
    venue: 'O‘zbekiston Davlat san\'at muzeyi',
    type: 'Ko‘rgazma'
  }
]
</script>

<template>
  <section class="home-section interactive-section home-section-alt">
    <div class="container">
      <div class="interactive-grid">
        <!-- Left: Interactive Services -->
        <div class="services-column">
          <div class="section-header-compact">
            <div>
              <h2 class="section-title">Interaktiv xizmatlar</h2>
              <p class="section-subtitle">Aholi va yuridik shaxslar uchun onlayn davlat xizmatlari</p>
            </div>
            <a href="https://my.gov.uz" target="_blank" rel="noopener" class="see-all-btn">
              <span>Barcha xizmatlar</span>
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="5" y1="12" x2="19" y2="12"/>
                <polyline points="12 5 19 12 12 19"/>
              </svg>
            </a>
          </div>

          <div class="services-list">
            <a 
              v-for="srv in services" 
              :key="srv.id" 
              :href="srv.url" 
              target="_blank" 
              rel="noopener"
              class="service-item-card"
            >
              <div class="service-icon-box">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
                  <polyline points="14 2 14 8 20 8"/>
                  <line x1="16" y1="13" x2="8" y2="13"/>
                  <line x1="16" y1="17" x2="8" y2="17"/>
                  <polyline points="10 9 9 9 8 9"/>
                </svg>
              </div>

              <div class="service-details">
                <div class="service-header-meta">
                  <span class="service-cat-badge">{{ srv.category }}</span>
                  <span v-if="srv.online" class="online-indicator">
                    <span class="online-dot"></span> Onlayn
                  </span>
                </div>
                <h3 class="service-item-title">{{ srv.title }}</h3>
              </div>

              <div class="service-link-arrow">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <line x1="7" y1="17" x2="17" y2="7"/>
                  <polyline points="7 7 17 7 17 17"/>
                </svg>
              </div>
            </a>
          </div>
        </div>

        <!-- Right: Events Calendar -->
        <div class="events-column">
          <div class="section-header-compact">
            <div>
              <h2 class="section-title">Voqealar taqvimi</h2>
              <p class="section-subtitle">Yurtimiz madaniy hayotidagi muhim tadbirlar</p>
            </div>
            <a href="#all-events" class="see-all-btn">
              <span>Barcha voqealar</span>
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="5" y1="12" x2="19" y2="12"/>
                <polyline points="12 5 19 12 12 19"/>
              </svg>
            </a>
          </div>

          <!-- Calendar Mini Header / Month View -->
          <div class="events-container-card">
            <div class="calendar-month-header">
              <span class="month-title">Oktabr, 2026</span>
              <div class="days-strip">
                <button 
                  v-for="d in [18, 19, 20, 21, 22, 23, 24, 25]" 
                  :key="d"
                  class="day-chip"
                  :class="{ active: selectedDate === d, today: d === 19 }"
                  @click="selectedDate = d"
                >
                  <span class="day-num">{{ d }}</span>
                </button>
              </div>
            </div>

            <!-- Events List -->
            <div class="upcoming-events-list">
              <div v-for="(ev, idx) in events" :key="idx" class="event-card">
                <div class="event-date-badge">
                  <span class="event-day">{{ ev.day }}</span>
                  <span class="event-month">{{ ev.month }}</span>
                </div>

                <div class="event-info">
                  <div class="event-meta">
                    <span class="event-time">
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <circle cx="12" cy="12" r="10"/>
                        <polyline points="12 6 12 12 16 14"/>
                      </svg>
                      {{ ev.time }}
                    </span>
                    <span class="meta-bullet">•</span>
                    <span class="event-type-badge">{{ ev.type }}</span>
                  </div>

                  <h3 class="event-title">{{ ev.title }}</h3>
                  
                  <div class="event-venue">
                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                      <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                      <circle cx="12" cy="10" r="3"/>
                    </svg>
                    <span>{{ ev.venue }}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.interactive-section {
  padding-top: 36px;
  padding-bottom: 40px;
}

.interactive-grid {
  display: grid;
  grid-template-columns: 1.15fr 1fr;
  gap: 32px;
}

.section-header-compact {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  margin-bottom: 20px;
  gap: 12px;
  flex-wrap: wrap;
}

/* Services Column */
.services-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.service-item-card {
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: 14px 18px;
  display: flex;
  align-items: center;
  gap: 16px;
  transition: all var(--transition-fast);
}

.service-item-card:hover {
  transform: translateX(4px);
  border-color: var(--color-primary);
  box-shadow: var(--shadow-md);
}

.service-icon-box {
  width: 44px;
  height: 44px;
  border-radius: var(--radius-sm);
  background-color: var(--color-primary-50);
  color: var(--color-primary);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  transition: all var(--transition-fast);
}

.service-item-card:hover .service-icon-box {
  background-color: var(--color-primary);
  color: #ffffff;
}

.service-icon-box svg {
  width: 22px;
  height: 22px;
}

.service-details {
  flex: 1;
}

.service-header-meta {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 4px;
}

.service-cat-badge {
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--color-accent-dark);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.online-indicator {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  font-size: 0.72rem;
  font-weight: 600;
  color: var(--color-success);
}

.online-dot {
  width: 6px;
  height: 6px;
  border-radius: var(--radius-full);
  background-color: var(--color-success);
  animation: pulseGlow 1.8s infinite;
}

.service-item-title {
  font-size: 0.96rem;
  font-weight: 600;
  color: var(--color-gray-800);
  line-height: 1.35;
}

.service-link-arrow {
  color: var(--color-gray-400);
  transition: all var(--transition-fast);
}

.service-link-arrow svg {
  width: 18px;
  height: 18px;
}

.service-item-card:hover .service-link-arrow {
  color: var(--color-primary);
  transform: translate(2px, -2px);
}

/* Events Column */
.events-container-card {
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow-sm);
}

.calendar-month-header {
  background-color: var(--color-primary-50);
  padding: 16px 20px;
  border-bottom: 1px solid var(--border-color);
}

.month-title {
  display: block;
  font-size: 0.9rem;
  font-weight: 700;
  color: var(--color-primary-dark);
  margin-bottom: 10px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.days-strip {
  display: flex;
  align-items: center;
  gap: 8px;
  overflow-x: auto;
  padding-bottom: 4px;
}

.day-chip {
  width: 36px;
  height: 36px;
  border-radius: var(--radius-sm);
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.88rem;
  font-weight: 700;
  color: var(--color-gray-700);
  transition: all var(--transition-fast);
  flex-shrink: 0;
}

.day-chip:hover {
  border-color: var(--color-primary);
  color: var(--color-primary);
}

.day-chip.today {
  border-color: var(--color-accent);
}

.day-chip.active {
  background-color: var(--color-primary);
  color: #ffffff;
  border-color: var(--color-primary);
}

/* Upcoming Events List */
.upcoming-events-list {
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.event-card {
  display: flex;
  gap: 14px;
  padding: 12px;
  border-radius: var(--radius-md);
  background-color: var(--color-gray-50);
  border: 1px solid transparent;
  transition: all var(--transition-fast);
}

.event-card:hover {
  background-color: #ffffff;
  border-color: var(--color-primary-100);
  box-shadow: var(--shadow-sm);
}

.event-date-badge {
  width: 52px;
  height: 52px;
  border-radius: var(--radius-sm);
  background-color: var(--color-primary);
  color: #ffffff;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.event-day {
  font-size: 1.15rem;
  font-weight: 800;
  line-height: 1;
}

.event-month {
  font-size: 0.65rem;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  opacity: 0.9;
}

.event-info {
  flex: 1;
}

.event-meta {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.75rem;
  color: var(--color-gray-500);
  margin-bottom: 4px;
}

.event-time {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  font-weight: 600;
  color: var(--color-gray-700);
}

.event-time svg,
.event-venue svg {
  width: 12px;
  height: 12px;
}

.event-type-badge {
  font-size: 0.7rem;
  font-weight: 600;
  color: var(--color-primary);
  background: var(--color-primary-50);
  padding: 2px 6px;
  border-radius: 4px;
}

.event-title {
  font-size: 0.92rem;
  font-weight: 700;
  color: var(--color-gray-900);
  line-height: 1.35;
  margin-bottom: 6px;
}

.event-venue {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 0.78rem;
  color: var(--color-gray-500);
}

@media (max-width: 992px) {
  .interactive-grid {
    grid-template-columns: 1fr;
  }
}
</style>
