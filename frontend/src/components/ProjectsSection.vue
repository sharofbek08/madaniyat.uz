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
      </div>

      <div class="projects-grid">
        <a 
          v-for="proj in projects" 
          :key="proj.id" 
          :href="proj.url || '#/madaniyat'" 
          class="project-box-item"
        >
          <div class="project-emblem-wrap">
            <svg viewBox="0 0 24 24" width="44" height="44" fill="none" stroke="#004094" stroke-width="1.8">
              <path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>
            </svg>
          </div>

          <div class="project-content">
            <span class="project-badge">{{ proj.badge || 'Respublika loyihasi' }}</span>
            <h3 class="project-title">{{ proj.title }}</h3>
            <p class="project-desc">{{ proj.desc }}</p>
          </div>

          <svg class="project-chevron" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="9 18 15 12 9 6"/>
          </svg>
        </a>
      </div>
    </div>
  </section>
</template>

<style scoped>
.projects-section {
  padding-top: 1.5rem;
  padding-bottom: 2.5rem;
}

.projects-header {
  margin-bottom: 1.5rem;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
}

@media (max-width: 768px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }
}

.project-box-item {
  display: flex;
  align-items: center;
  gap: 1.25rem;
  background-color: #ffffff;
  border: 1px solid var(--color-border);
  padding: 1.25rem 1.5rem;
  border-radius: 4px;
  text-decoration: none;
  transition: var(--transition);
  position: relative;
}

.project-box-item:hover {
  border-color: var(--color-primary);
  box-shadow: 0 4px 16px rgba(1, 61, 140, 0.08);
  transform: translateY(-2px);
}

.project-emblem-wrap {
  width: 70px;
  height: 70px;
  background-color: rgba(0, 64, 148, 0.05);
  border-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.project-content {
  flex: 1;
  min-width: 0;
}

.project-badge {
  display: inline-block;
  font-size: 0.6875rem;
  font-weight: 700;
  color: var(--color-primary);
  text-transform: uppercase;
  letter-spacing: 0.4px;
  margin-bottom: 0.25rem;
}

.project-title {
  font-size: 1rem;
  font-weight: 700;
  color: #222222;
  line-height: 1.35;
  margin: 0;
  transition: color 0.15s ease;
}

.project-box-item:hover .project-title {
  color: var(--color-text-primary);
}

.project-desc {
  font-size: 0.8125rem;
  color: #707070;
  line-height: 1.4;
  margin-top: 0.375rem;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.project-chevron {
  color: #adb5bd;
  flex-shrink: 0;
  transition: all 0.2s;
}

.project-box-item:hover .project-chevron {
  color: var(--color-primary);
  transform: translateX(3px);
}
</style>
