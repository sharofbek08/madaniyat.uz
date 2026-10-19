<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  banners: {
    type: Array,
    required: true
  }
})

const currentIndex = ref(0)
const isAutoPlay = ref(true)
let timer = null

const next = () => {
  if (props.banners.length === 0) return
  currentIndex.value = (currentIndex.value + 1) % props.banners.length
}

const prev = () => {
  if (props.banners.length === 0) return
  currentIndex.value = (currentIndex.value - 1 + props.banners.length) % props.banners.length
}

const togglePlay = () => {
  isAutoPlay.value = !isAutoPlay.value
  if (isAutoPlay.value) {
    startTimer()
  } else {
    stopTimer()
  }
}

const startTimer = () => {
  stopTimer()
  timer = setInterval(() => {
    next()
  }, 5000)
}

const stopTimer = () => {
  if (timer) clearInterval(timer)
}

onMounted(() => {
  startTimer()
})

onUnmounted(() => {
  stopTimer()
})
</script>

<template>
  <section v-if="banners && banners.length > 0" class="home-section banners-section">
    <div class="container">
      <div 
        class="banners-carousel-wrapper"
        @mouseenter="stopTimer"
        @mouseleave="isAutoPlay && startTimer()"
      >
        <!-- Slide Items -->
        <div class="slides-container">
          <div 
            v-for="(banner, idx) in banners" 
            :key="banner.id"
            class="slide-item"
            :class="{ active: idx === currentIndex }"
          >
            <div class="banner-card">
              <!-- Banner Image with overlay -->
              <div class="banner-image-box">
                <img :src="banner.image" :alt="banner.title" class="banner-img" />
                <div class="banner-overlay"></div>
              </div>

              <!-- Banner Content -->
              <div class="banner-content">
                <span class="banner-badge badge badge-accent">{{ banner.badge || 'Muhim sana' }}</span>
                <h3 class="banner-title">{{ banner.title }}</h3>
                <p class="banner-subtitle">{{ banner.subtitle }}</p>
                <div class="banner-actions">
                  <a :href="banner.url || '#'" class="banner-btn">
                    <span>Batafsil ma'lumot</span>
                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                      <line x1="5" y1="12" x2="19" y2="12"/>
                      <polyline points="12 5 19 12 12 19"/>
                    </svg>
                  </a>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Carousel Controls -->
        <div class="carousel-navigation-bar">
          <!-- Indicators -->
          <div class="indicators">
            <button 
              v-for="(_, idx) in banners" 
              :key="idx" 
              class="dot"
              :class="{ active: idx === currentIndex }"
              @click="currentIndex = idx"
              :aria-label="`Slayd ${idx + 1}`"
            />
          </div>

          <!-- Buttons: Prev / Play / Next -->
          <div class="nav-buttons">
            <button class="nav-btn" @click="prev" aria-label="Oldingi slayd">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="15 18 9 12 15 6"/>
              </svg>
            </button>

            <button class="nav-btn play-btn" @click="togglePlay" :title="isAutoPlay ? 'To‘xtatish' : 'Ijro etish'">
              <svg v-if="isAutoPlay" viewBox="0 0 24 24" fill="currentColor">
                <rect x="6" y="4" width="4" height="16"/>
                <rect x="14" y="4" width="4" height="16"/>
              </svg>
              <svg v-else viewBox="0 0 24 24" fill="currentColor">
                <polygon points="5 3 19 12 5 21 5 3"/>
              </svg>
            </button>

            <button class="nav-btn" @click="next" aria-label="Keyingi slayd">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="9 18 15 12 9 6"/>
              </svg>
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.banners-section {
  padding-top: 10px;
  padding-bottom: 24px;
}

.banners-carousel-wrapper {
  position: relative;
  border-radius: var(--radius-lg);
  overflow: hidden;
  box-shadow: var(--shadow-md);
  background-color: var(--color-primary-dark);
}

.slides-container {
  position: relative;
  min-height: 280px;
}

.slide-item {
  position: absolute;
  inset: 0;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.6s ease, visibility 0.6s ease;
}

.slide-item.active {
  position: relative;
  opacity: 1;
  visibility: visible;
}

.banner-card {
  position: relative;
  display: flex;
  align-items: center;
  min-height: 320px;
  padding: 40px;
}

.banner-image-box {
  position: absolute;
  inset: 0;
  z-index: 1;
}

.banner-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.banner-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, rgba(1, 40, 96, 0.95) 0%, rgba(1, 61, 140, 0.8) 50%, rgba(1, 40, 96, 0.4) 100%);
}

.banner-content {
  position: relative;
  z-index: 2;
  max-width: 650px;
  color: #ffffff;
}

.banner-badge {
  margin-bottom: 14px;
}

.banner-title {
  font-size: 2rem;
  font-weight: 800;
  line-height: 1.25;
  color: #ffffff;
  margin-bottom: 12px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.banner-subtitle {
  font-size: 1.05rem;
  color: #e2e8f0;
  line-height: 1.5;
  margin-bottom: 24px;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
}

.banner-btn {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  background: linear-gradient(135deg, var(--color-accent), var(--color-accent-dark));
  color: #ffffff;
  font-weight: 700;
  font-size: 0.95rem;
  padding: 12px 24px;
  border-radius: var(--radius-full);
  box-shadow: 0 4px 10px rgba(197, 160, 89, 0.4);
  transition: all var(--transition-fast);
}

.banner-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 15px rgba(197, 160, 89, 0.6);
}

.banner-btn svg {
  width: 18px;
  height: 18px;
  transition: transform var(--transition-fast);
}

.banner-btn:hover svg {
  transform: translateX(4px);
}

/* Navigation Bar */
.carousel-navigation-bar {
  position: absolute;
  bottom: 20px;
  right: 28px;
  z-index: 3;
  display: flex;
  align-items: center;
  gap: 16px;
  background: rgba(0, 0, 0, 0.45);
  backdrop-filter: blur(8px);
  padding: 6px 14px;
  border-radius: var(--radius-full);
  border: 1px solid rgba(255, 255, 255, 0.15);
}

.indicators {
  display: flex;
  align-items: center;
  gap: 6px;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: var(--radius-full);
  background-color: rgba(255, 255, 255, 0.4);
  transition: all var(--transition-fast);
}

.dot.active {
  width: 24px;
  background-color: var(--color-accent);
}

.nav-buttons {
  display: flex;
  align-items: center;
  gap: 6px;
}

.nav-btn {
  width: 28px;
  height: 28px;
  border-radius: var(--radius-full);
  background: rgba(255, 255, 255, 0.15);
  color: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background var(--transition-fast);
}

.nav-btn:hover {
  background: rgba(255, 255, 255, 0.3);
}

.nav-btn svg {
  width: 14px;
  height: 14px;
}

@media (max-width: 768px) {
  .banner-card {
    padding: 24px;
    min-height: 260px;
  }
  .banner-title {
    font-size: 1.4rem;
  }
  .banner-subtitle {
    font-size: 0.9rem;
    margin-bottom: 16px;
  }
  .carousel-navigation-bar {
    position: static;
    border-radius: 0;
    justify-content: space-between;
  }
}
</style>
