<script setup>
import { ref } from 'vue'

const props = defineProps({
  regions: {
    type: Array,
    required: true
  }
})

const selectedRegionId = ref('tashkent_city')

const selectedRegion = ref(props.regions.find(r => r.id === selectedRegionId.value) || props.regions[0])

const selectRegion = (id) => {
  selectedRegionId.value = id
  selectedRegion.value = props.regions.find(r => r.id === id) || props.regions[0]
}
</script>

<template>
  <section class="home-section regional-map-section">
    <div class="container">
      <div class="section-header-row">
        <div>
          <h2 class="section-title">Hududiy boshqarmalar</h2>
          <p class="section-subtitle">O‘zbekiston Respublikasi viloyat, shahar va tuman madaniyat bo‘limlari</p>
        </div>

        <span class="badge badge-accent">14 ta hudud</span>
      </div>

      <div class="regional-interactive-layout">
        <!-- Left: Region Selection Chips -->
        <div class="regions-list-column">
          <h4 class="list-title">Hududni tanlang:</h4>
          <div class="regions-chips-grid">
            <button 
              v-for="reg in regions" 
              :key="reg.id"
              class="region-chip"
              :class="{ active: selectedRegionId === reg.id }"
              @click="selectRegion(reg.id)"
            >
              <span class="pin-icon">📍</span>
              <span class="region-name">{{ reg.name }}</span>
            </button>
          </div>
        </div>

        <!-- Right: Active Region Detailed Card -->
        <div v-if="selectedRegion" class="region-details-column">
          <div class="region-detail-card">
            <!-- Region Header -->
            <div class="detail-header">
              <div class="detail-title-group">
                <span class="detail-pretitle">Madaniyat boshqarmasi</span>
                <h3 class="detail-region-title">{{ selectedRegion.name }}</h3>
              </div>
              <div class="emblem-mini">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>
                </svg>
              </div>
            </div>

            <!-- Leader Section -->
            <div class="leader-row">
              <div class="leader-avatar">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/>
                  <circle cx="12" cy="7" r="4"/>
                </svg>
              </div>
              <div class="leader-text">
                <span class="leader-role">Boshqarma boshlig‘i:</span>
                <h4 class="leader-name">{{ selectedRegion.leader }}</h4>
              </div>
            </div>

            <!-- Stats in Region -->
            <div class="region-stats-row">
              <div class="reg-stat-item">
                <span class="reg-stat-num">{{ selectedRegion.centers }}</span>
                <span class="reg-stat-label">Madaniyat markazlari</span>
              </div>
              <div class="reg-stat-item">
                <span class="reg-stat-num">{{ selectedRegion.theaters }}</span>
                <span class="reg-stat-label">Davlat teatrlari</span>
              </div>
              <div class="reg-stat-item">
                <span class="reg-stat-num">{{ selectedRegion.museums }}</span>
                <span class="reg-stat-label">Muzeylar</span>
              </div>
            </div>

            <!-- Contact rows -->
            <div class="contact-details-list">
              <div class="contact-row">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                  <circle cx="12" cy="10" r="3"/>
                </svg>
                <div class="contact-text">
                  <span class="contact-label">Manzil:</span>
                  <span class="contact-val">{{ selectedRegion.address }}</span>
                </div>
              </div>

              <div class="contact-row">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
                </svg>
                <div class="contact-text">
                  <span class="contact-label">Telefon:</span>
                  <a :href="`tel:${selectedRegion.phone}`" class="contact-val link">{{ selectedRegion.phone }}</a>
                </div>
              </div>

              <div class="contact-row">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                  <polyline points="22,6 12,13 2,6"/>
                </svg>
                <div class="contact-text">
                  <span class="contact-label">Elektron pochta:</span>
                  <a :href="`mailto:${selectedRegion.email}`" class="contact-val link">{{ selectedRegion.email }}</a>
                </div>
              </div>
            </div>

            <!-- Actions -->
            <div class="region-actions">
              <a :href="`tel:${selectedRegion.phone}`" class="action-btn-primary">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
                </svg>
                <span>Bog‘lanish</span>
              </a>

              <a :href="`mailto:${selectedRegion.email}`" class="action-btn-outline">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                  <polyline points="22,6 12,13 2,6"/>
                </svg>
                <span>Xat yozish</span>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.regional-map-section {
  padding-top: 36px;
  padding-bottom: 40px;
}

.regional-interactive-layout {
  display: grid;
  grid-template-columns: 1.1fr 1fr;
  gap: 28px;
}

.list-title {
  font-size: 0.95rem;
  font-weight: 700;
  color: var(--color-gray-700);
  margin-bottom: 14px;
}

.regions-chips-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
}

.region-chip {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px 16px;
  border-radius: var(--radius-md);
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  font-size: 0.88rem;
  font-weight: 600;
  color: var(--color-gray-700);
  text-align: left;
  transition: all var(--transition-fast);
  box-shadow: var(--shadow-sm);
}

.region-chip:hover {
  border-color: var(--color-primary);
  color: var(--color-primary);
  transform: translateX(4px);
}

.region-chip.active {
  background-color: var(--color-primary);
  color: #ffffff;
  border-color: var(--color-primary);
  box-shadow: 0 4px 10px rgba(1, 61, 140, 0.25);
}

.pin-icon {
  font-size: 1rem;
}

/* Detail Card */
.region-detail-card {
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: 28px;
  box-shadow: var(--shadow-md);
}

.detail-header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  margin-bottom: 20px;
  border-bottom: 1px solid var(--border-color);
  padding-bottom: 16px;
}

.detail-pretitle {
  font-size: 0.78rem;
  font-weight: 700;
  color: var(--color-accent-dark);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.detail-region-title {
  font-size: 1.45rem;
  font-weight: 800;
  color: var(--color-primary-dark);
  line-height: 1.25;
  margin-top: 2px;
}

.emblem-mini {
  width: 44px;
  height: 44px;
  border-radius: var(--radius-full);
  background-color: var(--color-primary-50);
  color: var(--color-primary);
  display: flex;
  align-items: center;
  justify-content: center;
}

.emblem-mini svg {
  width: 22px;
  height: 22px;
}

/* Leader */
.leader-row {
  display: flex;
  align-items: center;
  gap: 14px;
  background-color: var(--color-primary-50);
  padding: 14px 18px;
  border-radius: var(--radius-md);
  margin-bottom: 20px;
}

.leader-avatar {
  width: 44px;
  height: 44px;
  border-radius: var(--radius-full);
  background-color: #ffffff;
  color: var(--color-primary);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: var(--shadow-sm);
  flex-shrink: 0;
}

.leader-avatar svg {
  width: 22px;
  height: 22px;
}

.leader-role {
  display: block;
  font-size: 0.75rem;
  color: var(--color-gray-500);
  font-weight: 600;
}

.leader-name {
  font-size: 1rem;
  font-weight: 700;
  color: var(--color-primary-dark);
}

/* Stats */
.region-stats-row {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 12px;
  margin-bottom: 22px;
}

.reg-stat-item {
  background-color: var(--color-gray-50);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: 12px;
  text-align: center;
}

.reg-stat-num {
  display: block;
  font-size: 1.4rem;
  font-weight: 800;
  color: var(--color-primary);
}

.reg-stat-label {
  font-size: 0.72rem;
  color: var(--color-gray-500);
  font-weight: 600;
}

/* Contacts */
.contact-details-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-bottom: 24px;
}

.contact-row {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  font-size: 0.88rem;
}

.contact-row svg {
  width: 18px;
  height: 18px;
  color: var(--color-primary);
  flex-shrink: 0;
  margin-top: 2px;
}

.contact-label {
  display: block;
  font-size: 0.75rem;
  color: var(--color-gray-500);
  font-weight: 600;
}

.contact-val {
  color: var(--color-gray-800);
  font-weight: 500;
}

.contact-val.link {
  color: var(--color-primary);
  font-weight: 600;
}

.contact-val.link:hover {
  text-decoration: underline;
}

/* Actions */
.region-actions {
  display: flex;
  align-items: center;
  gap: 12px;
}

.action-btn-primary {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background-color: var(--color-primary);
  color: #ffffff;
  font-weight: 600;
  font-size: 0.88rem;
  padding: 10px 20px;
  border-radius: var(--radius-md);
  transition: all var(--transition-fast);
}

.action-btn-primary:hover {
  background-color: var(--color-primary-dark);
}

.action-btn-outline {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background-color: var(--color-gray-100);
  color: var(--color-gray-700);
  font-weight: 600;
  font-size: 0.88rem;
  padding: 10px 20px;
  border-radius: var(--radius-md);
  transition: all var(--transition-fast);
}

.action-btn-outline:hover {
  background-color: var(--color-gray-200);
  color: var(--color-primary);
}

.action-btn-primary svg,
.action-btn-outline svg {
  width: 16px;
  height: 16px;
}

@media (max-width: 992px) {
  .regional-interactive-layout {
    grid-template-columns: 1fr;
  }
}
</style>
