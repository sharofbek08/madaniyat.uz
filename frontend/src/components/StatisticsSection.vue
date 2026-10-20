<script setup>
const props = defineProps({
  statistics: {
    type: Array,
    required: true
  }
})

// Statistics items matching official authority statistics
const defaultStats = [
  {
    id: 1,
    title: '40+',
    text: 'Davlat teatrlari va sirk tashkilotlari',
    icon: 'theatre'
  },
  {
    id: 2,
    title: '110+',
    text: 'Tarixiy va san’at muzeylari',
    icon: 'museum'
  },
  {
    id: 3,
    title: '825',
    text: 'Aholi madaniyat markazlari',
    icon: 'center'
  },
  {
    id: 4,
    title: '8 200+',
    text: 'Moddiy madaniy meros ko‘chmas mulk ob’yektlari',
    icon: 'heritage'
  }
]
</script>

<template>
  <section class="authority-stats-section py-5">
    <div class="container">
      <div class="stats-grid">
        <div 
          v-for="st in (statistics && statistics.length > 0 ? statistics : defaultStats)" 
          :key="st.id" 
          class="stat-card-outer"
        >
          <div class="stat-card-gradient-border">
            <div class="stat-card-inner">
              <div class="stat-card-content">
                <div class="stat-icon-box">
                  <!-- Theater Icon -->
                  <svg v-if="st.icon === 'theatre' || st.id === 1" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="#004094" stroke-width="2">
                    <path d="M2 12h20M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
                    <circle cx="9" cy="9" r="1" fill="#004094"/>
                    <circle cx="15" cy="9" r="1" fill="#004094"/>
                    <path d="M8 15s1.5 2 4 2 4-2 4-2"/>
                  </svg>
                  <!-- Museum Icon -->
                  <svg v-else-if="st.icon === 'museum' || st.id === 2" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="#004094" stroke-width="2">
                    <rect x="4" y="2" width="16" height="20" rx="2" ry="2"/>
                    <line x1="9" y1="22" x2="9" y2="2"/>
                    <line x1="15" y1="22" x2="15" y2="2"/>
                    <line x1="4" y1="12" x2="20" y2="12"/>
                  </svg>
                  <!-- Center Icon -->
                  <svg v-else-if="st.icon === 'center' || st.id === 3" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="#004094" stroke-width="2">
                    <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/>
                  </svg>
                  <!-- Heritage Icon -->
                  <svg v-else width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="#004094" stroke-width="2">
                    <path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>
                  </svg>
                </div>

                <div class="stat-text-box">
                  <p class="stat-number">{{ st.title || st.value }}</p>
                  <p class="stat-label">{{ st.text || st.label }}</p>
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
.authority-stats-section {
  padding-top: 1.5rem;
  padding-bottom: 2rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1.25rem;
}

@media (max-width: 1200px) {
  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .stats-grid {
    grid-template-columns: 1fr;
  }
}

/* Gov.uz exact card styles from page-8e1d61a2e61cb617.js */
.stat-card-outer {
  width: 100%;
  height: 100%;
  box-shadow: 1px 2px 7px rgba(0, 0, 0, 0.09);
  border-radius: 1rem;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.stat-card-outer:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(0, 64, 148, 0.12);
}

.stat-card-gradient-border {
  display: flex;
  flex-direction: column;
  height: 100%;
  padding: 1px;
  border-radius: 1rem;
  background: linear-gradient(to right, #ffffff, #458be6);
}

.stat-card-inner {
  background-color: #ffffff;
  border-radius: 1rem;
  height: 100%;
}

.stat-card-content {
  border-radius: 1rem;
  background: linear-gradient(to right, rgba(255, 255, 255, 1), rgba(0, 64, 148, 0.05), rgba(255, 255, 255, 1));
  padding: 1.5rem;
  height: 100%;
  display: flex;
  align-items: center;
  gap: 1.25rem;
}

.stat-icon-box {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
  background-color: rgba(0, 64, 148, 0.05);
  border-radius: 4px;
  flex-shrink: 0;
}

.stat-text-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  min-width: 0;
}

.stat-number {
  color: #004094;
  font-size: 1.875rem;
  font-weight: 700;
  line-height: 1.1;
  margin: 0;
}

.stat-label {
  color: #3d4646;
  font-size: 0.875rem;
  line-height: 1.35;
  margin-top: 4px;
}
</style>
