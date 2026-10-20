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
  <section v-if="govLinks && govLinks.length > 0" class="gov-links-section bg-neutral-200">
    <div class="container">
      <div class="gov-links-row">
        <a 
          v-for="item in govLinks" 
          :key="item.id" 
          :href="item.url || '#'" 
          :target="item.target || '_blank'" 
          rel="noopener"
          class="gov-link-item"
        >
          <img 
            v-if="item.icon" 
            :src="item.icon" 
            :alt="item.title" 
            width="64" 
            height="32"
            class="gov-link-icon"
            loading="lazy"
          />
          <h5 class="gov-link-title">{{ item.title }}</h5>
        </a>
      </div>
    </div>
  </section>
</template>

<style scoped>
.gov-links-section {
  background-color: #e5e5e5;
  padding-top: 1.75rem;
  padding-bottom: 1.75rem;
}

.gov-links-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 2rem;
  flex-wrap: wrap;
}

@media (max-width: 992px) {
  .gov-links-row {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 1.25rem;
  }
}

@media (max-width: 540px) {
  .gov-links-row {
    grid-template-columns: 1fr;
  }
}

.gov-link-item {
  display: flex;
  align-items: center;
  gap: 0.875rem;
  text-decoration: none;
  flex: 1;
  min-width: 180px;
  transition: opacity 0.2s ease;
}

.gov-link-item:hover {
  opacity: 0.8;
}

.gov-link-icon {
  width: 64px;
  height: 32px;
  object-fit: contain;
  flex-shrink: 0;
}

.gov-link-title {
  font-size: 0.75rem;
  font-weight: 700;
  color: #333333;
  line-height: 1.3;
  text-transform: uppercase;
  margin: 0;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.gov-link-item:hover .gov-link-title {
  color: var(--color-primary);
}
</style>
