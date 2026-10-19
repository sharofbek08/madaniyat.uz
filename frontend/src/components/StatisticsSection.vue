<script setup>
const props = defineProps({
  statistics: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <section class="home-section stats-section">
    <div class="stats-bg-overlay"></div>
    <div class="container stats-container">
      <div class="stats-header">
        <span class="stats-pretitle">Rasmiy ko‘rsatkichlar</span>
        <h2 class="stats-main-title">O‘zbekiston Respublikasi madaniyat sohasi raqamlarda</h2>
        <p class="stats-desc">Yurtimiz bo‘ylab faoliyat ko‘rsatayotgan madaniy-ma'rifiy muassasalar va yillik faoliyat natijalari</p>
      </div>

      <div class="stats-grid">
        <div v-for="st in statistics" :key="st.id" class="stat-card">
          <div class="stat-num-box">
            <span class="stat-value">{{ st.value?.toLocaleString() }}</span>
            <span class="stat-suffix">{{ st.suffix }}</span>
          </div>
          <h3 class="stat-label">{{ st.label }}</h3>
          <p class="stat-sub">{{ st.sub }}</p>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.stats-section {
  position: relative;
  background: radial-gradient(circle at 50% 30%, #012b63 0%, #00193d 100%);
  color: #ffffff;
  padding-top: 60px;
  padding-bottom: 60px;
  overflow: hidden;
}

.stats-bg-overlay {
  position: absolute;
  inset: 0;
  background-image: 
    radial-gradient(rgba(197, 160, 89, 0.15) 1px, transparent 1px),
    linear-gradient(to bottom, rgba(1, 43, 99, 0.5), rgba(0, 25, 61, 0.95));
  background-size: 24px 24px, 100% 100%;
  pointer-events: none;
}

.stats-container {
  position: relative;
  z-index: 2;
}

.stats-header {
  text-align: center;
  max-width: 700px;
  margin: 0 auto 48px;
}

.stats-pretitle {
  display: inline-block;
  font-size: 0.78rem;
  font-weight: 700;
  color: var(--color-accent);
  text-transform: uppercase;
  letter-spacing: 1.5px;
  margin-bottom: 8px;
}

.stats-main-title {
  font-size: 2rem;
  font-weight: 800;
  line-height: 1.25;
  color: #ffffff;
  margin-bottom: 12px;
}

.stats-desc {
  font-size: 0.95rem;
  color: #94a3b8;
  line-height: 1.5;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 20px;
}

.stat-card {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(8px);
  border-radius: var(--radius-lg);
  padding: 28px 20px;
  text-align: center;
  transition: all var(--transition-normal);
}

.stat-card:hover {
  transform: translateY(-6px);
  background: rgba(255, 255, 255, 0.09);
  border-color: var(--color-accent);
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.3), 0 0 15px rgba(197, 160, 89, 0.2);
}

.stat-num-box {
  display: flex;
  align-items: baseline;
  justify-content: center;
  margin-bottom: 10px;
  color: var(--color-accent);
}

.stat-value {
  font-size: 2.6rem;
  font-weight: 800;
  line-height: 1;
  font-family: var(--font-serif);
  letter-spacing: -1px;
}

.stat-suffix {
  font-size: 1.8rem;
  font-weight: 700;
  margin-left: 2px;
}

.stat-label {
  font-size: 1.05rem;
  font-weight: 700;
  color: #ffffff;
  margin-bottom: 6px;
  line-height: 1.3;
}

.stat-sub {
  font-size: 0.78rem;
  color: #94a3b8;
  line-height: 1.4;
}

@media (max-width: 1024px) {
  .stats-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 768px) {
  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  .stats-main-title {
    font-size: 1.5rem;
  }
  .stat-value {
    font-size: 2rem;
  }
}

@media (max-width: 480px) {
  .stats-grid {
    grid-template-columns: 1fr;
  }
}
</style>
