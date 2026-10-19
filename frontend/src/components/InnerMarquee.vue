<script setup>
import { ref } from 'vue'

const props = defineProps({
  items: {
    type: Array,
    required: true
  }
})

const isPaused = ref(false)

const togglePause = () => {
  isPaused.value = !isPaused.value
}
</script>

<template>
  <div class="inner-marquee-section">
    <div class="container">
      <div 
        class="marquee-card"
        @mouseenter="isPaused = true"
        @mouseleave="isPaused = false"
      >
        <!-- Badge Tag -->
        <div class="marquee-badge">
          <svg class="megaphone-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"/>
            <path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"/>
          </svg>
          <span class="badge-title">Tezkor xabar</span>
        </div>

        <!-- Running Track -->
        <div class="marquee-content-wrapper">
          <div class="marquee-track" :class="{ paused: isPaused }">
            <span v-for="(item, idx) in [...items, ...items]" :key="idx" class="marquee-item">
              <a :href="item.url || '#'" class="marquee-link">
                <span class="star-separator">★</span>
                <span class="item-text">{{ item.title }}</span>
              </a>
            </span>
          </div>
        </div>

        <!-- Pause / Play control -->
        <button class="marquee-ctrl-btn" @click="togglePause" :title="isPaused ? 'Harakatlantirish' : 'To‘xtatish'">
          <svg v-if="!isPaused" viewBox="0 0 24 24" fill="currentColor">
            <rect x="6" y="4" width="4" height="16"/>
            <rect x="14" y="4" width="4" height="16"/>
          </svg>
          <svg v-else viewBox="0 0 24 24" fill="currentColor">
            <polygon points="5 3 19 12 5 21 5 3"/>
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.inner-marquee-section {
  padding-top: 14px;
  padding-bottom: 6px;
}

.marquee-card {
  display: flex;
  align-items: center;
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-full);
  padding: 4px 10px 4px 6px;
  box-shadow: var(--shadow-sm);
  overflow: hidden;
  position: relative;
}

.marquee-badge {
  display: flex;
  align-items: center;
  gap: 6px;
  background: linear-gradient(135deg, #dc2626, #b91c1c);
  color: #ffffff;
  padding: 6px 14px;
  border-radius: var(--radius-full);
  font-size: 0.75rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  white-space: nowrap;
  flex-shrink: 0;
  z-index: 2;
  box-shadow: 0 2px 4px rgba(220, 38, 38, 0.25);
}

.megaphone-icon {
  width: 14px;
  height: 14px;
  animation: pulseGlow 1.5s infinite;
}

.marquee-content-wrapper {
  flex: 1;
  overflow: hidden;
  position: relative;
  mask-image: linear-gradient(to right, transparent, black 20px, black calc(100% - 20px), transparent);
  -webkit-mask-image: linear-gradient(to right, transparent, black 20px, black calc(100% - 20px), transparent);
}

.marquee-track {
  display: flex;
  align-items: center;
  white-space: nowrap;
  width: max-content;
  animation: marquee 35s linear infinite;
}

.marquee-track.paused {
  animation-play-state: paused;
}

.marquee-item {
  display: inline-flex;
  align-items: center;
  padding-right: 28px;
}

.marquee-link {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  color: var(--color-gray-800);
  font-size: 0.88rem;
  font-weight: 500;
  transition: color var(--transition-fast);
}

.marquee-link:hover {
  color: var(--color-primary);
  text-decoration: underline;
}

.star-separator {
  color: var(--color-accent);
  font-size: 0.75rem;
}

.marquee-ctrl-btn {
  width: 28px;
  height: 28px;
  border-radius: var(--radius-full);
  background-color: var(--color-gray-100);
  color: var(--color-gray-600);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  margin-left: 8px;
  transition: var(--transition-fast);
  z-index: 2;
}

.marquee-ctrl-btn:hover {
  background-color: var(--color-primary-50);
  color: var(--color-primary);
}

.marquee-ctrl-btn svg {
  width: 12px;
  height: 12px;
}

@keyframes marquee {
  0% { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}

@media (max-width: 640px) {
  .badge-title {
    display: none;
  }
  .marquee-badge {
    padding: 6px 8px;
  }
}
</style>
