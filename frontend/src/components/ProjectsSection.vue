<script setup>
const props = defineProps({
  projects: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <section class="projects-section py-5">
    <div class="container">
      <div class="projects-header">
        <h2 class="font--lg font-bold uppercase" style="color: var(--color-text-primary)">
          Amalga oshirilayotgan loyihalar
        </h2>
        <a href="#/madaniyat/pages/about" class="details-link_link__EkQDj">
          Barcha loyihalar
          <span>
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24">
              <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 12h10m0 0-4-4m4 4-4 4"/>
            </svg>
          </span>
        </a>
      </div>

      <div class="projects-grid">
        <div v-for="proj in projects" :key="proj.id" class="project-card">
          <div class="project-card-top">
            <span class="project-badge">{{ proj.badge }}</span>
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
              </svg>
              <span>{{ proj.participants }}</span>
            </div>

            <span class="project-arrow-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="5" y1="12" x2="19" y2="12"/>
                <polyline points="12 5 19 12 12 19"/>
              </svg>
            </span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.projects-section {
  padding-top: 2rem;
  padding-bottom: 2.5rem;
}

.projects-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-bottom: 1.5rem;
  gap: 1rem;
  flex-wrap: wrap;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1.5rem;
}

.project-card {
  background-color: #ffffff;
  border: 1px solid var(--color-border);
  border-radius: 4px;
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  transition: var(--transition);
  position: relative;
}

.project-card:hover {
  border-color: var(--color-primary);
  box-shadow: 0 4px 16px rgba(1, 61, 140, 0.08);
  transform: translateY(-3px);
}

.project-card-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.project-badge {
  font-size: 0.6875rem;
  font-weight: 700;
  text-transform: uppercase;
  color: var(--color-primary);
  background: rgba(1, 61, 140, 0.08);
  padding: 3px 8px;
  border-radius: 4px;
}

.project-status {
  font-size: 0.75rem;
  font-weight: 600;
  color: #2b8a3e;
}

.project-title {
  font-size: 1.05rem;
  font-weight: 700;
  color: #222222;
  line-height: 1.35;
  margin-bottom: 0.5rem;
}

.project-desc {
  font-size: 0.8125rem;
  color: #707070;
  line-height: 1.45;
  margin-bottom: 1.5rem;
  flex: 1;
}

.project-card-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-top: 1px solid #f1f3f5;
  padding-top: 1rem;
}

.participants-info {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.75rem;
  font-weight: 500;
  color: #868e96;
}

.participants-info svg {
  width: 14px;
  height: 14px;
  color: var(--color-primary);
}

.project-arrow-icon {
  color: #adb5bd;
  transition: all 0.2s ease;
}

.project-card:hover .project-arrow-icon {
  color: var(--color-primary);
  transform: translateX(4px);
}

@media (max-width: 1200px) {
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
