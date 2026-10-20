<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

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

// Import layout components
import TopBar from './components/TopBar.vue'
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'

// Import page views
import HomePage from './components/HomePage.vue'
import AboutPage from './components/AboutPage.vue'
import StructurePage from './components/StructurePage.vue'
import GuidesPage from './components/GuidesPage.vue'
import StaffPage from './components/StaffPage.vue'
import RegionalPage from './components/RegionalPage.vue'
import VacanciesPage from './components/VacanciesPage.vue'
import TendersPage from './components/TendersPage.vue'
import EcoActivePage from './components/EcoActivePage.vue'
import ConstitutionPage from './components/ConstitutionPage.vue'
import CorruptionPage from './components/CorruptionPage.vue'
import MeetingsPage from './components/MeetingsPage.vue'
import ReportsPage from './components/ReportsPage.vue'

// Modals & Drawers
import SearchModal from './components/SearchModal.vue'
import SitemapDrawer from './components/SitemapDrawer.vue'
import AccessibilityDrawer from './components/AccessibilityDrawer.vue'
import AnthemModal from './components/AnthemModal.vue'
import FeedbackModal from './components/FeedbackModal.vue'

// Routing State based on URL Hash
const currentHash = ref(window.location.hash || '#/')

const isStructurePage = computed(() => {
  const h = currentHash.value.toLowerCase()
  return h.includes('structure') || h.includes('tuzilmasi') || h.includes('1958')
})

const isGuidesPage = computed(() => {
  const h = currentHash.value.toLowerCase()
  return h.includes('guides') || h.includes('rahbariyat') || h.includes('1962')
})

const isStaffPage = computed(() => {
  const h = currentHash.value.toLowerCase()
  return h.includes('staff') || h.includes('markaziy-apparat') || h.includes('1959')
})

const isRegionalPage = computed(() => {
  const h = currentHash.value.toLowerCase()
  return h.includes('regional') || h.includes('hududiy') || h.includes('1960')
})

const isVacanciesPage = computed(() => {
  const h = currentHash.value.toLowerCase()
  return (
    h.includes('bo-sh-ish-o-rinlari') ||
    h.includes('vacancies') ||
    h.includes('vakansiyalar') ||
    h.includes('2921')
  )
})

const isTendersPage = computed(() => {
  const h = currentHash.value.toLowerCase()
  return (
    h.includes('news/e-lon') ||
    h.includes('e-lon') ||
    h.includes('tanlovlar') ||
    h.includes('tender') ||
    h.includes('17063')
  )
})

const isEcoActivePage = computed(() => {
  const h = currentHash.value.toLowerCase()
  return (
    h.includes('news/test') ||
    h.includes('ekofaol') ||
    h.includes('13460')
  )
})

const isConstitutionPage = computed(() => {
  const h = currentHash.value.toLowerCase()
  return (
    h.includes('sections/konstitutsiya-sahifasi') ||
    h.includes('konstitutsiya-sahifasi') ||
    h.includes('konstitutsiya') ||
    h.includes('3825')
  )
})

const isCorruptionPage = computed(() => {
  const h = currentHash.value.toLowerCase()
  return (
    h.includes('sections/korrup-bo-lim') ||
    h.includes('korrup-bo-lim') ||
    h.includes('korrup') ||
    h.includes('3433')
  )
})

const isMeetingsPage = computed(() => {
  const h = currentHash.value.toLowerCase()
  return (
    h.includes('activity_page/majlislar') ||
    h.includes('majlislar') ||
    h.includes('2971')
  )
})

const isReportsPage = computed(() => {
  const h = currentHash.value.toLowerCase()
  return (
    h.includes('activity_page/hisobotlar') ||
    h.includes('hisobotlar') ||
    h.includes('2966')
  )
})

const isAboutPage = computed(() => {
  const h = currentHash.value.toLowerCase()
  // Check about only if not matching any of the specific sub-pages
  if (
    isStructurePage.value ||
    isGuidesPage.value ||
    isStaffPage.value ||
    isRegionalPage.value ||
    isVacanciesPage.value ||
    isTendersPage.value ||
    isEcoActivePage.value ||
    isConstitutionPage.value ||
    isCorruptionPage.value ||
    isMeetingsPage.value ||
    isReportsPage.value
  ) {
    return false
  }
  return (
    h.includes('about') ||
    h.includes('pages/about') ||
    h.includes('vazirlik-haqida') ||
    h.includes('1957')
  )
})

const handleHashChange = () => {
  currentHash.value = window.location.hash || '#/'
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

const navigateTo = (path) => {
  if (path === '/' || path === '#/' || path === '#') {
    window.location.hash = '#/'
  } else if (path.startsWith('#')) {
    window.location.hash = path
  } else {
    window.location.hash = `#${path}`
  }
}

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
  window.addEventListener('hashchange', handleHashChange)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleGlobalKeydown)
  window.removeEventListener('hashchange', handleHashChange)
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

    <!-- Main Dynamic Content Container -->
    <main id="main-content">
      <!-- 1:1 Structure Page View -->
      <StructurePage 
        v-if="isStructurePage"
        @navigate="navigateTo"
      />

      <!-- 1:1 Leadership / Guides Page View -->
      <GuidesPage 
        v-else-if="isGuidesPage"
        @navigate="navigateTo"
      />

      <!-- 1:1 Central Apparatus / Staff Page View -->
      <StaffPage 
        v-else-if="isStaffPage"
        @navigate="navigateTo"
      />

      <!-- 1:1 Regional Departments Page View -->
      <RegionalPage 
        v-else-if="isRegionalPage"
        @navigate="navigateTo"
      />

      <!-- 1:1 Vacancies / Bo'sh ish o'rinlari Page View -->
      <VacanciesPage 
        v-else-if="isVacanciesPage"
        @navigate="navigateTo"
      />

      <!-- 1:1 About Ministry Page View -->
      <AboutPage 
        v-else-if="isAboutPage"
        @navigate="navigateTo"
      />

      <!-- 1:1 Tenders / E-lon Page View -->
      <TendersPage 
        v-else-if="isTendersPage"
        @navigate="navigateTo"
      />

      <!-- 1:1 Eco-Active / Test Page View -->
      <EcoActivePage 
        v-else-if="isEcoActivePage"
        @navigate="navigateTo"
      />

      <!-- 1:1 Constitution Page View -->
      <ConstitutionPage 
        v-else-if="isConstitutionPage"
        @navigate="navigateTo"
      />

      <!-- 1:1 Anti-Corruption / Korrupsiyaga qarshi kurashish Page View -->
      <CorruptionPage 
        v-else-if="isCorruptionPage"
        @navigate="navigateTo"
      />

      <!-- 1:1 Meetings / Majlislar Page View -->
      <MeetingsPage 
        v-else-if="isMeetingsPage"
        @navigate="navigateTo"
      />

      <!-- 1:1 Reports / Hisobotlar Page View -->
      <ReportsPage 
        v-else-if="isReportsPage"
        @navigate="navigateTo"
      />

      <!-- Home Page View -->
      <HomePage 
        v-else
        :runner="runner"
        :news="news"
        :banners="banners"
        :activities="activities"
        :interactiveServices="interactiveServices"
        :statistics="statistics"
        :vacanciesDocumentsPoll="vacanciesDocumentsPoll"
        :projects="projects"
        :regionsMap="regionsMap"
        :govLinks="govLinks"
        :usefulLinks="usefulLinks"
      />
    </main>

    <!-- Comprehensive Footer -->
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
