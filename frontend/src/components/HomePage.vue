<script setup>
import InnerMarquee from './InnerMarquee.vue'
import NewsHeroSection from './NewsHeroSection.vue'
import BannersCarousel from './BannersCarousel.vue'
import ActivitiesSection from './ActivitiesSection.vue'
import InteractiveServicesSection from './InteractiveServicesSection.vue'
import StatisticsSection from './StatisticsSection.vue'
import VacanciesDocPoll from './VacanciesDocPoll.vue'
import ProjectsSection from './ProjectsSection.vue'
import RegionalMapSection from './RegionalMapSection.vue'
import GovPartnersSection from './GovPartnersSection.vue'
import SubscribeBanner from './SubscribeBanner.vue'

defineProps({
  runner: { type: Array, required: true },
  news: { type: [Array, Object], required: true },
  banners: { type: Array, required: true },
  activities: { type: Array, required: true },
  interactiveServices: { type: Array, required: true },
  statistics: { type: [Array, Object], required: true },
  vacanciesDocumentsPoll: { type: Object, required: true },
  projects: { type: Array, required: true },
  regionsMap: { type: Array, required: true },
  govLinks: { type: Array, required: true },
  usefulLinks: { type: Array, required: true }
})
</script>

<template>
  <div class="home-page">
    <!-- 1. Inner Marquee Ticker -->
    <InnerMarquee :items="runner" />

    <!-- 2. Hero / Lead News Section -->
    <NewsHeroSection :news="news" />

    <!-- 3. Banners Carousel -->
    <BannersCarousel :banners="banners" />

    <!-- 4. Cultural Activities Section -->
    <ActivitiesSection :activities="activities" />

    <!-- 5. Interactive Services & Events Calendar -->
    <InteractiveServicesSection :services="interactiveServices" />

    <!-- 6. Ministry Statistics Counters -->
    <StatisticsSection :statistics="statistics" />

    <!-- 7. Vacancies, Documents & Citizen Poll -->
    <VacanciesDocPoll :data="vacanciesDocumentsPoll" />

    <!-- 8. National Projects Section -->
    <ProjectsSection :projects="projects" />

    <!-- 9. Regional Map & Directory -->
    <RegionalMapSection :regions="regionsMap" />

    <!-- 10. Government Partners & Useful Links -->
    <GovPartnersSection :govLinks="govLinks" :usefulLinks="usefulLinks" />

    <!-- 11. Push Notification / Subscribe Banner -->
    <SubscribeBanner />
  </div>
</template>

<style scoped>
.home-page {
  width: 100%;
}
</style>
