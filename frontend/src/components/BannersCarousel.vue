<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  banners: {
    type: Array,
    required: true
  }
})

const currentIndex = ref(0)
const isPlaying = ref(true)
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
  isPlaying.value = !isPlaying.value
  if (isPlaying.value) {
    startTimer()
  } else {
    stopTimer()
  }
}

const startTimer = () => {
  stopTimer()
  timer = setInterval(next, 5000)
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
  <section v-if="banners && banners.length > 0" class="banners-section">
    <div class="container">
      <div 
        class="banners_carousel__F2HyJ"
        @mouseenter="stopTimer"
        @mouseleave="isPlaying && startTimer()"
      >
        <!-- Prev Button -->
        <button 
          type="button" 
          class="banners_carousel__prevBtn__8ygtM" 
          aria-label="Oldingi banner"
          @click="prev"
        >
          <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M15 18l-6-6 6-6" stroke="var(--color-primary)" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>

        <!-- Carousel Viewport -->
        <div class="carousel-viewport">
          <div 
            class="carousel-track" 
            :style="{ transform: `translateX(-${currentIndex * 100}%)` }"
          >
            <div 
              v-for="b in banners" 
              :key="b.id" 
              class="banners_carousel__item__txead"
            >
              <a :href="b.url || '#'" target="_blank" rel="noopener" class="banner-slide-link">
                <img :src="b.image" :alt="b.title" class="banner-img" />
                <div class="banner-slide-info">
                  <span v-if="b.badge" class="banner-pill">{{ b.badge }}</span>
                  <h4 class="banner-slide-title">{{ b.title }}</h4>
                </div>
              </a>
            </div>
          </div>
        </div>

        <!-- Next Button -->
        <button 
          type="button" 
          class="banners_carousel__nextBtn__jhNYR" 
          aria-label="Keyingi banner"
          @click="next"
        >
          <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M9 18l6-6-6-6" stroke="var(--color-primary)" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>

      <!-- Play / Pause control underneath -->
      <div class="banners_play_and_stop__fVoTc" @click="togglePlay" :title="isPlaying ? 'To‘xtatish' : 'Davom ettirish'">
        <svg v-if="isPlaying" width="16" height="16" viewBox="0 0 24 24" fill="currentColor" color="var(--color-secondary)">
          <rect x="6" y="4" width="4" height="16"/>
          <rect x="14" y="4" width="4" height="16"/>
        </svg>
        <svg v-else width="16" height="16" viewBox="0 0 24 24" fill="currentColor" color="var(--color-secondary)">
          <polygon points="5 3 19 12 5 21 5 3"/>
        </svg>
      </div>
    </div>
  </section>
</template>

<style scoped>
.banners-section {
  padding-top: 1.25rem;
  padding-bottom: 1.5rem;
}

.banners_carousel__F2HyJ {
  position: relative;
  padding-left: 60px;
  padding-right: 60px;
}

.carousel-viewport {
  overflow: hidden;
  width: 100%;
  border-radius: 4px;
}

.carousel-track {
  display: flex;
  transition: transform 0.4s ease-in-out;
  width: 100%;
}

.banners_carousel__item__txead {
  position: relative;
  min-width: 100%;
  height: 150px;
  outline: none;
}

.banner-slide-link {
  position: relative;
  display: block;
  width: 100%;
  height: 100%;
  overflow: hidden;
  border-radius: 4px;
}

.banner-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 4px;
}

.banner-slide-info {
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, rgba(1, 61, 140, 0.85) 0%, rgba(1, 61, 140, 0.4) 60%, transparent 100%);
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 1.5rem 2.5rem;
  color: #ffffff;
}

.banner-pill {
  display: inline-block;
  align-self: flex-start;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(4px);
  padding: 2px 10px;
  border-radius: 9999px;
  font-size: 0.75rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  text-transform: uppercase;
}

.banner-slide-title {
  font-size: 1.25rem;
  font-weight: 700;
  color: #ffffff;
  max-width: 32rem;
  line-height: 1.3;
}

.banners_carousel__prevBtn__8ygtM,
.banners_carousel__nextBtn__jhNYR {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  cursor: pointer;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.9);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  z-index: 5;
}

.banners_carousel__prevBtn__8ygtM {
  left: 0;
}

.banners_carousel__nextBtn__jhNYR {
  right: 0;
}

.banners_carousel__prevBtn__8ygtM:hover,
.banners_carousel__nextBtn__jhNYR:hover {
  background-color: #ffffff;
  box-shadow: 0 4px 12px rgba(1, 61, 140, 0.2);
  transform: translateY(-50%) scale(1.05);
}

.banners_carousel__prevBtn__8ygtM svg,
.banners_carousel__nextBtn__jhNYR svg {
  width: 22px;
  height: 22px;
}

.banners_play_and_stop__fVoTc {
  display: flex;
  justify-content: flex-end;
  margin-top: 8px;
  cursor: pointer;
  opacity: 0.7;
  transition: opacity 0.2s;
}

.banners_play_and_stop__fVoTc:hover {
  opacity: 1;
}

@media (max-width: 768px) {
  .banners_carousel__F2HyJ {
    padding-left: 0;
    padding-right: 0;
  }
  .banners_carousel__prevBtn__8ygtM,
  .banners_carousel__nextBtn__jhNYR {
    display: none;
  }
  .banners_carousel__item__txead {
    height: 120px;
  }
  .banner-slide-info {
    padding: 1rem;
  }
  .banner-slide-title {
    font-size: 1rem;
  }
}
</style>
