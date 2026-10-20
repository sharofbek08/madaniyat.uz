<script setup>
const props = defineProps({
  items: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <div v-if="items && items.length > 0" class="inner-marquee_marquee__vX671">
    <div class="inner-marquee_track">
      <template v-for="(item, idx) in [...items, ...items]" :key="idx">
        <a 
          v-if="item.url" 
          :href="item.url" 
          target="_blank" 
          rel="noopener"
          class="marquee-text-link"
        >
          {{ item.title }}
        </a>
        <p v-else class="marquee-text-p">{{ item.title }}</p>
      </template>
    </div>
  </div>
</template>

<style scoped>
.inner-marquee_marquee__vX671 {
  display: block;
  width: 100%;
  background-color: rgba(1, 61, 140, 0.9);
  padding: 7px 0;
  white-space: nowrap;
  overflow: hidden;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.inner-marquee_track {
  width: max-content;
  display: flex;
  align-items: center;
  gap: 50px;
  padding-left: 100%;
  animation: inner-marquee_scroll__w8RSD 35s linear infinite;
}

.inner-marquee_marquee__vX671:hover .inner-marquee_track {
  animation-play-state: paused;
}

.marquee-text-link,
.marquee-text-p {
  display: inline-block;
  margin: 0;
  color: #ffffff;
  font-size: 0.9375rem;
  font-weight: 500;
  text-decoration: none;
}

.marquee-text-link:hover {
  text-decoration: underline;
}

@keyframes inner-marquee_scroll__w8RSD {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-100%);
  }
}
</style>
