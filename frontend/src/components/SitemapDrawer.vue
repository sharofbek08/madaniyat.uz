<script setup>
import { onMounted, onUnmounted } from 'vue'

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  },
  navigation: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['close'])

const handleKeydown = (e) => {
  if (e.key === 'Escape' && props.isOpen) {
    emit('close')
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
})
</script>

<template>
  <div v-if="isOpen" class="sitemap-overlay" @click.self="emit('close')">
    <div class="sitemap-drawer">
      <!-- Drawer Header -->
      <div class="drawer-header">
        <div class="header-title-box">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="3" width="7" height="7"/>
            <rect x="14" y="3" width="7" height="7"/>
            <rect x="14" y="14" width="7" height="7"/>
            <rect x="3" y="14" width="7" height="7"/>
          </svg>
          <h2 class="drawer-title">Sayt xaritasi</h2>
        </div>

        <button class="close-btn" @click="emit('close')" aria-label="Yopish">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="18" y1="6" x2="6" y2="18"/>
            <line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>
      </div>

      <!-- Sitemap Columns Grid -->
      <div class="drawer-content">
        <div class="sitemap-columns">
          <div v-for="menu in navigation" :key="menu.id" class="sitemap-section-col">
            <h3 class="section-heading">{{ menu.title }}</h3>
            <ul v-if="menu.children?.length > 0" class="sublinks-list">
              <li v-for="sub in menu.children" :key="sub.id">
                <a :href="`#${sub.url}`" class="sublink" @click="emit('close')">
                  <span class="bullet">•</span>
                  <span>{{ sub.title }}</span>
                </a>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.sitemap-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(1, 40, 96, 0.7);
  backdrop-filter: blur(6px);
  z-index: 2000;
  display: flex;
  justify-content: flex-end;
  animation: fadeIn 0.2s ease;
}

.sitemap-drawer {
  width: 100%;
  max-width: 900px;
  background-color: #ffffff;
  height: 100vh;
  display: flex;
  flex-direction: column;
  box-shadow: -10px 0 25px rgba(0, 0, 0, 0.2);
  animation: slideLeft 0.25s cubic-bezier(0.16, 1, 0.3, 1);
}

.drawer-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 24px 32px;
  background-color: var(--color-primary-dark);
  color: #ffffff;
}

.header-title-box {
  display: flex;
  align-items: center;
  gap: 12px;
}

.header-title-box svg {
  width: 24px;
  height: 24px;
  color: var(--color-accent);
}

.drawer-title {
  font-size: 1.4rem;
  font-weight: 800;
  color: #ffffff;
}

.close-btn {
  width: 36px;
  height: 36px;
  border-radius: var(--radius-full);
  background: rgba(255, 255, 255, 0.15);
  color: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition-fast);
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: rotate(90deg);
}

.close-btn svg {
  width: 18px;
  height: 18px;
}

.drawer-content {
  flex: 1;
  overflow-y: auto;
  padding: 32px;
}

.sitemap-columns {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 28px;
}

.sitemap-section-col {
  display: flex;
  flex-direction: column;
}

.section-heading {
  font-size: 1.05rem;
  font-weight: 800;
  color: var(--color-primary-dark);
  padding-bottom: 8px;
  margin-bottom: 12px;
  border-bottom: 2px solid var(--color-accent);
}

.sublinks-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.sublink {
  display: flex;
  align-items: flex-start;
  gap: 8px;
  font-size: 0.88rem;
  color: var(--color-gray-700);
  line-height: 1.4;
  transition: all var(--transition-fast);
}

.sublink .bullet {
  color: var(--color-accent);
}

.sublink:hover {
  color: var(--color-primary);
  transform: translateX(4px);
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideLeft {
  from { transform: translateX(100%); }
  to { transform: translateX(0); }
}

@media (max-width: 768px) {
  .sitemap-columns {
    grid-template-columns: 1fr;
  }
}
</style>
