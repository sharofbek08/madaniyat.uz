<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// Import all mock data
import siteInfo from './mock/siteInfo.json'
import navigation from './mock/navigation.json'
import runner from './mock/runner.json'
import banners from './mock/banners.json'
import news from './mock/news.json'
import activities from './mock/activities.json'
import interactiveServices from './mock/interactiveServices.json'
import statistics from './mock/statistics.json'
import vacanciesDocumentsPoll from './mock/vacanciesDocumentsPoll.json'
import projects from './mock/projects.json'
import regionsMap from './mock/regionsMap.json'
import govLinks from './mock/govLinks.json'
import usefulLinks from './mock/usefulLinks.json'

// Import components
import TopBar from './components/TopBar.vue'
import AppHeader from './components/AppHeader.vue'
import InnerMarquee from './components/InnerMarquee.vue'
import NewsHeroSection from './components/NewsHeroSection.vue'
import BannersCarousel from './components/BannersCarousel.vue'
import ActivitiesSection from './components/ActivitiesSection.vue'
import InteractiveServicesSection from './components/InteractiveServicesSection.vue'
import StatisticsSection from './components/StatisticsSection.vue'
import VacanciesDocPoll from './components/VacanciesDocPoll.vue'
import ProjectsSection from './components/ProjectsSection.vue'
import RegionalMapSection from './components/RegionalMapSection.vue'
import GovPartnersSection from './components/GovPartnersSection.vue'
import SubscribeBanner from './components/SubscribeBanner.vue'
import AppFooter from './components/AppFooter.vue'

// Modals
import SearchModal from './components/SearchModal.vue'
import SitemapDrawer from './components/SitemapDrawer.vue'
import AccessibilityDrawer from './components/AccessibilityDrawer.vue'
import AnthemModal from './components/AnthemModal.vue'
import FeedbackModal from './components/FeedbackModal.vue'

// State for Modals
const isSearchOpen = ref(false)
const isSitemapOpen = ref(false)
const isAccessibilityOpen = ref(false)
const isSymbolsOpen = ref(false)
const isFeedbackOpen = ref(false)
const selectedFeedbackText = ref('')

const openFeedback = (text = '') => {
  selectedFeedbackText.value = text || window.getSelection()?.toString() || ''
  isFeedbackOpen.value = true
}

// Global key listeners (Ctrl+K for search, Ctrl+Enter for feedback)
const handleGlobalKeydown = (e) => {
  if ((e.ctrlKey || e.metaKey) && e.key.toLowerCase() === 'k') {
    e.preventDefault()
    isSearchOpen.value = true
  } else if ((e.ctrlKey || e.metaKey) && e.key === 'Enter') {
    const sel = window.getSelection()?.toString()
    if (sel && sel.trim().length > 0) {
      e.preventDefault()
      openFeedback(sel.trim())
    }
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleGlobalKeydown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleGlobalKeydown)
})
</script>

<template>
  <div class="app-root">
    <!-- 1. Top Bar -->
    <TopBar 
      :siteInfo="siteInfo"
      @open-symbols="isSymbolsOpen = true"
      @open-accessibility="isAccessibilityOpen = true"
    />

    <!-- 2. Main Header & Navigation -->
    <AppHeader 
      :siteInfo="siteInfo"
      :navigation="navigation"
      @open-search="isSearchOpen = true"
      @open-sitemap="isSitemapOpen = true"
    />

    <!-- Main Content Container -->
    <main id="main-content">
      <!-- 3. Inner Marquee Ticker -->
      <InnerMarquee :items="runner" />

      <!-- 4. Hero / Lead News Section -->
      <NewsHeroSection :news="news" />

      <!-- 5. Banners Carousel -->
      <BannersCarousel :banners="banners" />

      <!-- 6. Cultural Activities Section -->
      <ActivitiesSection :activities="activities" />

      <!-- 7. Interactive Services & Events Calendar -->
      <InteractiveServicesSection :services="interactiveServices" />

      <!-- 8. Ministry Statistics Counters -->
      <StatisticsSection :statistics="statistics" />

      <!-- 9. Vacancies, Documents & Citizen Poll -->
      <VacanciesDocPoll :data="vacanciesDocumentsPoll" />

      <!-- 10. National Projects Section -->
      <ProjectsSection :projects="projects" />

      <!-- 11. Regional Map & Directory -->
      <RegionalMapSection :regions="regionsMap" />

      <!-- 12. Government Partners & Useful Links -->
      <GovPartnersSection :govLinks="govLinks" :usefulLinks="usefulLinks" />

      <!-- 13. Push Notification / Subscribe Banner -->
      <SubscribeBanner />
    </main>

    <!-- 14. Comprehensive Footer -->
    <AppFooter 
      :siteInfo="siteInfo" 
      :navigation="navigation" 
    />

    <!-- Modals & Drawers -->
    <SearchModal 
      :isOpen="isSearchOpen"
      :news="news"
      :services="interactiveServices"
      :documents="vacanciesDocumentsPoll.documents"
      @close="isSearchOpen = false"
    />

    <SitemapDrawer 
      :isOpen="isSitemapOpen"
      :navigation="navigation"
      @close="isSitemapOpen = false"
    />

    <AccessibilityDrawer 
      :isOpen="isAccessibilityOpen"
      @close="isAccessibilityOpen = false"
    />

    <AnthemModal 
      :isOpen="isSymbolsOpen"
      @close="isSymbolsOpen = false"
    />

    <FeedbackModal 
      :isOpen="isFeedbackOpen"
      :selectedText="selectedFeedbackText"
      @close="isFeedbackOpen = false"
    />
  </div>
</template>

<style scoped>
.app-root {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

main {
  flex: 1;
}
</style>
