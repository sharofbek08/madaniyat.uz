<script setup>
const props = defineProps({
  projects: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <section class="home-section projects-section home-section-alt">
    <div class="container">
      <div class="section-header-row">
        <div>
          <h2 class="section-title">Amalga oshirilayotgan loyihalar</h2>
          <p class="section-subtitle">Davlatimiz rahbari tashabbusi bilan madaniyat sohasida olib borilayotgan keng ko‘lamli loyihalar</p>
        </div>

        <a href="#all-projects" class="see-all-btn">
          <span>Barcha loyihalar</span>
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="5" y1="12" x2="19" y2="12"/>
            <polyline points="12 5 19 12 12 19"/>
          </svg>
        </a>
      </div>

      <div class="projects-grid">
        <div v-for="proj in projects" :key="proj.id" class="project-card">
          <div class="project-card-top">
            <span class="project-badge badge badge-primary">{{ proj.badge }}</span>
            <span class="project-status">{{ proj.status }}</span>
          </div>

          <h3 class="project-title">{{ proj.title }}</h3>
          <p class="project-desc">{{ proj.desc }}</p>

          <div class="project-card-footer">
            <div class="participants-info">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/>
                <circle cx="9" cy="7" r="4"/>
                <path d="M23 21v-2a4 4 0 0 0-3-3.87"/>
                <path d="M16 3.13a4 4 0 0 1 0 7.75"/>
              </svg>
              <span>{{ proj.participants }}</span>
            </div>

            <a href="#project-details" class="project-arrow-btn">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="5" y1="12" x2="19" y2="12"/>
                <polyline points="12 5 19 12 12 19"/>
              </svg>
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.projects-section {
  padding-top: 36px;
  padding-bottom: 36px;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

.project-card {
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: 24px;
  display: flex;
  flex-direction: column;
  box-shadow: var(--shadow-sm);
  transition: all var(--transition-normal);
  position: relative;
}

.project-card:hover {
  transform: translateY(-6px);
  border-color: var(--color-primary);
  box-shadow: var(--shadow-xl);
}

.project-card-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 14px;
}

.project-status {
  font-size: 0.75rem;
  font-weight: 700;
  color: var(--color-success);
}

.project-title {
  font-size: 1.15rem;
  font-weight: 800;
  color: var(--color-gray-900);
  line-height: 1.35;
  margin-bottom: 10px;
}

.project-desc {
  font-size: 0.88rem;
  color: var(--color-gray-600);
  line-height: 1.5;
  margin-bottom: 20px;
  flex: 1;
}

.project-card-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-top: 1px solid var(--border-color);
  padding-top: 14px;
}

.participants-info {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.8rem;
  font-weight: 600;
  color: var(--color-gray-500);
}

.participants-info svg {
  width: 15px;
  height: 15px;
  color: var(--color-primary);
}

.project-arrow-btn {
  width: 32px;
  height: 32px;
  border-radius: var(--radius-full);
  background-color: var(--color-primary-50);
  color: var(--color-primary);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition-fast);
}

.project-arrow-btn svg {
  width: 16px;
  height: 16px;
}

.project-card:hover .project-arrow-btn {
  background-color: var(--color-primary);
  color: #ffffff;
  transform: translateX(4px);
}

@media (max-width: 1100px) {
  .projects-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }
}
</style>
