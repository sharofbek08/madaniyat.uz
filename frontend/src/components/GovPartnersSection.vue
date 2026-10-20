<script setup>
const props = defineProps({
  govLinks: {
    type: Array,
    required: true
  },
  usefulLinks: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <section class="gov-partners-section py-5">
    <div class="container">
      <div class="partners-header">
        <h2 class="font--lg font-bold uppercase" style="color: var(--color-text-primary)">
          Hukumat portallari
        </h2>
      </div>

      <!-- Partner Logos Carousel / Grid -->
      <div class="partners-row">
        <a 
          v-for="item in govLinks" 
          :key="item.id" 
          :href="item.url" 
          target="_blank" 
          rel="noopener"
          class="partner-box"
          :title="item.title"
        >
          <div class="partner-emblem">
            <img 
              v-if="item.icon" 
              :src="item.icon" 
              :alt="item.title" 
              class="partner-logo-img"
              loading="lazy"
            />
            <img 
              v-else 
              src="/img/gerb_in.png" 
              alt="Uzbekistan Gerb" 
              class="partner-logo-img" 
            />
          </div>
          <p class="partner-name">{{ item.title }}</p>
        </a>
      </div>
    </div>
  </section>
</template>

<style scoped>
.gov-partners-section {
  padding-top: 2rem;
  padding-bottom: 2.5rem;
  background-color: #fafbfc;
  border-top: 1px solid #f1f3f5;
  border-bottom: 1px solid #f1f3f5;
}

.partners-header {
  margin-bottom: 1.5rem;
}

.partners-row {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 1.25rem;
}

@media (max-width: 1200px) {
  .partners-row {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 768px) {
  .partners-row {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 480px) {
  .partners-row {
    grid-template-columns: 1fr;
  }
}

.partner-box {
  background-color: #ffffff;
  border: 1px solid var(--color-border);
  padding: 1.25rem 1rem;
  border-radius: 4px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: 0.75rem;
  transition: var(--transition);
  text-decoration: none;
}

.partner-box:hover {
  border-color: var(--color-primary);
  box-shadow: 0 4px 14px rgba(1, 61, 140, 0.08);
  transform: translateY(-2px);
}

.partner-emblem {
  width: 52px;
  height: 52px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.partner-logo-img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}

.partner-name {
  font-size: 0.75rem;
  font-weight: 700;
  color: #333333;
  line-height: 1.35;
  text-transform: uppercase;
  letter-spacing: 0.2px;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.partner-box:hover .partner-name {
  color: var(--color-text-primary);
}
</style>
