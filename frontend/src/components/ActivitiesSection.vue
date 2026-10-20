<script setup>
const props = defineProps({
  activities: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <section class="activities-section py-5">
    <div class="container">
      <!-- Section Header -->
      <div class="activities-header">
        <h2 class="font--lg font-bold uppercase" style="color: var(--color-text-primary)">
          Faoliyatlar yo‘nalishi
        </h2>
        <a href="#/madaniyat/pages/about" class="details-link_link__EkQDj">
          Batafsil
          <span>
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 12h10m0 0-4-4m4 4-4 4"/>
            </svg>
          </span>
        </a>
      </div>

      <!-- Activities Grid (Gov.uz activities_activities__E96Ff) -->
      <div class="activities_activities__E96Ff">
        <a 
          v-for="act in activities" 
          :key="act.id" 
          :href="act.link || '#'" 
          class="link-box_box__1w0YP"
        >
          <!-- Signature Blue Vertical Bar -->
          <div class="link-box_line__LLu7H"></div>

          <!-- Icon -->
          <div class="icon-wrap">
            <svg v-if="act.id === 1" class="link-box_box__icon__B0zCB" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
              <path d="M2 12h20M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
              <circle cx="9" cy="9" r="1" fill="currentColor"/>
              <circle cx="15" cy="9" r="1" fill="currentColor"/>
              <path d="M8 15s1.5 2 4 2 4-2 4-2"/>
            </svg>
            <svg v-else-if="act.id === 2" class="link-box_box__icon__B0zCB" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
              <rect x="4" y="2" width="16" height="20" rx="2" ry="2"/>
              <line x1="9" y1="22" x2="9" y2="2"/>
              <line x1="15" y1="22" x2="15" y2="2"/>
              <line x1="4" y1="12" x2="20" y2="12"/>
            </svg>
            <svg v-else-if="act.id === 3" class="link-box_box__icon__B0zCB" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
              <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/>
            </svg>
            <svg v-else-if="act.id === 4" class="link-box_box__icon__B0zCB" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
              <path d="M22 10v6M2 10l10-5 10 5-10 5z"/>
              <path d="M6 12v5c3 3 9 3 12 0v-5"/>
            </svg>
            <svg v-else-if="act.id === 5" class="link-box_box__icon__B0zCB" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
              <line x1="3" y1="21" x2="21" y2="21"/>
              <line x1="3" y1="10" x2="21" y2="10"/>
              <polyline points="5 10 5 21"/>
              <polyline points="19 10 19 21"/>
              <polygon points="12 2 20 7 4 7 12 2"/>
            </svg>
            <svg v-else-if="act.id === 6" class="link-box_box__icon__B0zCB" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
              <path d="M9 18V5l12-2v13"/>
              <circle cx="6" cy="18" r="3"/>
              <circle cx="18" cy="16" r="3"/>
            </svg>
            <svg v-else-if="act.id === 7" class="link-box_box__icon__B0zCB" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
              <circle cx="12" cy="12" r="10"/>
              <line x1="2" y1="12" x2="22" y2="12"/>
              <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
            </svg>
            <svg v-else class="link-box_box__icon__B0zCB" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
              <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/>
              <circle cx="9" cy="7" r="4"/>
              <path d="M23 21v-2a4 4 0 0 0-3-3.87"/>
            </svg>
          </div>

          <!-- Activity Text Info -->
          <div class="activity-text">
            <span class="activity-name">{{ act.title }}</span>
            <span v-if="act.count" class="activity-meta">{{ act.count }}</span>
          </div>

          <!-- Subtle Arrow on hover -->
          <svg class="chevron-arrow" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="9 18 15 12 9 6"/>
          </svg>
        </a>
      </div>
    </div>
  </section>
</template>

<style scoped>
.activities-section {
  padding-top: 2rem;
  padding-bottom: 2.5rem;
}

.activities-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-bottom: 1.5rem;
  flex-wrap: wrap;
  gap: 1rem;
}

.activities_activities__E96Ff {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(22rem, 1fr));
  row-gap: 1.25rem;
  column-gap: 1.5rem;
}

@media (max-width: 600px) {
  .activities_activities__E96Ff {
    grid-template-columns: 100%;
  }
}

.icon-wrap {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.activity-text {
  display: flex;
  flex-direction: column;
  flex: 1;
  min-width: 0;
}

.activity-name {
  font-size: 0.9375rem;
  font-weight: 600;
  color: #222222;
  line-height: 1.35;
  transition: color 0.15s linear;
}

.link-box_box__1w0YP:hover .activity-name {
  color: var(--color-text-primary);
}

.activity-meta {
  font-size: 0.75rem;
  color: #868e96;
  margin-top: 3px;
}

.chevron-arrow {
  color: #ced4da;
  flex-shrink: 0;
  transition: all 0.2s ease;
  transform: translateX(-4px);
  opacity: 0.5;
}

.link-box_box__1w0YP:hover .chevron-arrow {
  color: var(--color-primary);
  transform: translateX(0);
  opacity: 1;
}
</style>
