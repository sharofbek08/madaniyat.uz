<script setup>
const props = defineProps({
  activities: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <section class="home-section activities-section pattern-bg">
    <div class="container">
      <div class="section-header-row">
        <div>
          <h2 class="section-title">Faoliyat yo‘nalishlari</h2>
          <p class="section-subtitle">O‘zbekiston Respublikasi Madaniyat vazirligi qoshidagi asosiy sohalar</p>
        </div>

        <a href="#all-activities" class="see-all-btn">
          <span>Barcha yo‘nalishlar</span>
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="5" y1="12" x2="19" y2="12"/>
            <polyline points="12 5 19 12 12 19"/>
          </svg>
        </a>
      </div>

      <!-- Activities Grid -->
      <div class="activities-grid">
        <a 
          v-for="act in activities" 
          :key="act.id" 
          :href="act.link || '#'" 
          class="activity-card"
        >
          <div class="activity-icon-box">
            <!-- Dynamic SVG Icons based on activity -->
            <svg v-if="act.id === 1" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M2 12h20M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
              <circle cx="9" cy="9" r="1"/>
              <circle cx="15" cy="9" r="1"/>
              <path d="M8 15s1.5 2 4 2 4-2 4-2"/>
            </svg>
            <svg v-else-if="act.id === 2" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <rect x="4" y="2" width="16" height="20" rx="2" ry="2"/>
              <line x1="9" y1="22" x2="9" y2="2"/>
              <line x1="15" y1="22" x2="15" y2="2"/>
              <line x1="4" y1="12" x2="20" y2="12"/>
            </svg>
            <svg v-else-if="act.id === 3" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/>
            </svg>
            <svg v-else-if="act.id === 4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M22 10v6M2 10l10-5 10 5-10 5z"/>
              <path d="M6 12v5c3 3 9 3 12 0v-5"/>
            </svg>
            <svg v-else-if="act.id === 5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <line x1="3" y1="21" x2="21" y2="21"/>
              <line x1="3" y1="10" x2="21" y2="10"/>
              <polyline points="5 10 5 21"/>
              <polyline points="19 10 19 21"/>
              <polyline points="10 10 10 21"/>
              <polyline points="14 10 14 21"/>
              <polygon points="12 2 20 7 4 7 12 2"/>
            </svg>
            <svg v-else-if="act.id === 6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M9 18V5l12-2v13"/>
              <circle cx="6" cy="18" r="3"/>
              <circle cx="18" cy="16" r="3"/>
            </svg>
            <svg v-else-if="act.id === 7" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="12" cy="12" r="10"/>
              <line x1="2" y1="12" x2="22" y2="12"/>
              <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
            </svg>
            <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/>
              <circle cx="9" cy="7" r="4"/>
              <path d="M23 21v-2a4 4 0 0 0-3-3.87"/>
              <path d="M16 3.13a4 4 0 0 1 0 7.75"/>
            </svg>
          </div>

          <div class="activity-info">
            <span v-if="act.count" class="activity-count">{{ act.count }}</span>
            <h3 class="activity-title">{{ act.title }}</h3>
            <p class="activity-desc">{{ act.description }}</p>
          </div>

          <div class="activity-arrow">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <line x1="5" y1="12" x2="19" y2="12"/>
              <polyline points="12 5 19 12 12 19"/>
            </svg>
          </div>
        </a>
      </div>
    </div>
  </section>
</template>

<style scoped>
.activities-section {
  padding-top: 20px;
  padding-bottom: 36px;
}

.activities-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

.activity-card {
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: 24px;
  display: flex;
  flex-direction: column;
  position: relative;
  transition: all var(--transition-normal);
  box-shadow: var(--shadow-sm);
}

.activity-card:hover {
  transform: translateY(-6px);
  border-color: var(--color-primary-light);
  box-shadow: var(--shadow-xl);
}

.activity-icon-box {
  width: 52px;
  height: 52px;
  border-radius: var(--radius-md);
  background: linear-gradient(135deg, var(--color-primary-50), var(--color-primary-100));
  color: var(--color-primary);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 18px;
  transition: all var(--transition-fast);
}

.activity-card:hover .activity-icon-box {
  background: linear-gradient(135deg, var(--color-primary), var(--color-primary-dark));
  color: #ffffff;
  transform: scale(1.08);
}

.activity-icon-box svg {
  width: 26px;
  height: 26px;
}

.activity-count {
  display: inline-block;
  font-size: 0.72rem;
  font-weight: 700;
  color: var(--color-accent-dark);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 6px;
}

.activity-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--color-gray-900);
  line-height: 1.35;
  margin-bottom: 8px;
}

.activity-desc {
  font-size: 0.86rem;
  color: var(--color-gray-500);
  line-height: 1.5;
  margin-bottom: 18px;
  flex: 1;
}

.activity-arrow {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  color: var(--color-primary);
  opacity: 0.4;
  transition: all var(--transition-fast);
}

.activity-arrow svg {
  width: 20px;
  height: 20px;
}

.activity-card:hover .activity-arrow {
  opacity: 1;
  transform: translateX(4px);
}

@media (max-width: 1200px) {
  .activities-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .activities-grid {
    grid-template-columns: 1fr;
  }
}
</style>
