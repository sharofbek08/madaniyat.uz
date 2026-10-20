<script setup>
import { ref, computed } from 'vue'
import aboutData from '../mock/about.json'

const emit = defineEmits(['navigate'])

// Font sizing control
const fontSizeLevel = ref(1) // 0: small (15px), 1: normal (16px), 2: large (18px), 3: xl (20px)
const fontSizes = ['15px', '16.5px', '18px', '20px']
const currentFontSize = computed(() => fontSizes[fontSizeLevel.value])

const increaseFontSize = () => {
  if (fontSizeLevel.value < fontSizes.length - 1) {
    fontSizeLevel.value++
  }
}

const decreaseFontSize = () => {
  if (fontSizeLevel.value > 0) {
    fontSizeLevel.value--
  }
}

// Print page handler
const handlePrint = () => {
  window.print()
}

// Copy link feedback
const copySuccess = ref(false)
const copyPageUrl = async () => {
  try {
    await navigator.clipboard.writeText(window.location.href)
    copySuccess.value = true
    setTimeout(() => {
      copySuccess.value = false
    }, 2500)
  } catch (err) {
    console.error('Failed to copy URL:', err)
  }
}

// Search filter inside about page
const searchQuery = ref('')

const filteredTasks = computed(() => {
  const tasks = aboutData.sections[0]?.items || []
  if (!searchQuery.value.trim()) return tasks
  const q = searchQuery.value.toLowerCase()
  return tasks.filter(t => t.toLowerCase().includes(q))
})

const filteredFunctions = computed(() => {
  const funcs = aboutData.sections[1]?.items || []
  if (!searchQuery.value.trim()) return funcs
  const q = searchQuery.value.toLowerCase()
  return funcs.filter(f => f.toLowerCase().includes(q))
})

const hasSearch = computed(() => searchQuery.value.trim().length > 0)

const highlightText = (text) => {
  if (!searchQuery.value.trim()) return text
  const regex = new RegExp(`(${searchQuery.value.trim()})`, 'gi')
  return text.replace(regex, '<mark class="highlight">$1</mark>')
}

const handleSidebarClick = (item) => {
  if (item.url.startsWith('#')) {
    window.location.hash = item.url
  }
}
</script>

<template>
  <div class="about-page-wrapper">
    <div class="container page-container">
      <div class="page-layout">
        <!-- Main Content Column -->
        <main class="main-content-area">
          <article class="content-paper" id="printable-article">
            <!-- Top Controls & Breadcrumbs -->
            <div class="paper-header">
              <!-- Breadcrumbs Navigation -->
              <nav class="breadcrumbs" aria-label="Yo‘nalish">
                <a 
                  href="#/" 
                  class="breadcrumb-link" 
                  @click.prevent="emit('navigate', '/')"
                >
                  <svg class="home-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/>
                    <polyline points="9 22 9 12 15 12 15 22"/>
                  </svg>
                  <span>Bosh sahifa</span>
                </a>
                
                <span class="breadcrumb-separator" aria-hidden="true">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="9 18 15 12 9 6"/>
                  </svg>
                </span>

                <a 
                  href="#madaniyat/pages/about" 
                  class="breadcrumb-link active-link"
                >
                  Vazirlik haqida
                </a>

                <span class="breadcrumb-separator" aria-hidden="true">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="9 18 15 12 9 6"/>
                  </svg>
                </span>

                <span class="breadcrumb-current" aria-current="page">Vazirlik haqida</span>
              </nav>

              <!-- Header Right Tools: Age Limit, Font Resizer, Print -->
              <div class="header-tools">
                <!-- Font Resizer -->
                <div class="font-resizer" title="Shrift o‘lchamini o‘zgartirish">
                  <button 
                    type="button" 
                    class="font-btn" 
                    :disabled="fontSizeLevel === 0" 
                    @click="decreaseFontSize"
                    aria-label="Kichikroq shrift"
                  >
                    A-
                  </button>
                  <button 
                    type="button" 
                    class="font-btn" 
                    :disabled="fontSizeLevel === fontSizes.length - 1" 
                    @click="increaseFontSize"
                    aria-label="Kattaroq shrift"
                  >
                    A+
                  </button>
                </div>

                <!-- Age Limit Badge -->
                <span class="age-limit-badge">{{ aboutData.age_limit }}</span>

                <span class="tools-divider" aria-hidden="true"></span>

                <!-- Print Page Button -->
                <button 
                  type="button" 
                  class="tool-btn print-btn" 
                  @click="handlePrint" 
                  title="Sahifani chop etish"
                  aria-label="Sahifani chop etish"
                >
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="6 9 6 2 18 2 18 9"/>
                    <path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"/>
                    <rect x="6" y="14" width="12" height="8"/>
                  </svg>
                </button>
              </div>
            </div>

            <!-- Page Title -->
            <h1 class="page-title">{{ aboutData.title }}</h1>

            <!-- Quick In-Page Filter / Search -->
            <div class="page-search-bar">
              <div class="search-input-wrapper">
                <svg class="search-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <circle cx="11" cy="11" r="8"/>
                  <line x1="21" y1="21" x2="16.65" y2="16.65"/>
                </svg>
                <input 
                  type="text" 
                  v-model="searchQuery" 
                  placeholder="Sahifa ichidan qidirish (vazifalar, funksiyalar)..." 
                  class="search-input"
                />
                <button 
                  v-if="searchQuery" 
                  type="button" 
                  class="clear-btn" 
                  @click="searchQuery = ''"
                  title="Tozalash"
                >
                  ✕
                </button>
              </div>
              <span v-if="hasSearch" class="search-count">
                Topildi: {{ filteredTasks.length + filteredFunctions.length }} ta band
              </span>
            </div>

            <!-- Main Body Content -->
            <div 
              class="article-body" 
              :style="{ fontSize: currentFontSize }"
            >
              <!-- When searching, show highlighted structured sections -->
              <div v-if="hasSearch" class="search-results-section">
                <!-- Section 1 -->
                <div v-if="filteredTasks.length > 0" class="content-block">
                  <h2 class="section-heading">
                    <u>{{ aboutData.sections[0].title }}</u>
                  </h2>
                  <div class="items-list">
                    <p 
                      v-for="(task, idx) in filteredTasks" 
                      :key="'task-' + idx"
                      class="body-paragraph"
                      v-html="highlightText(task)"
                    ></p>
                  </div>
                </div>

                <!-- Section 2 -->
                <div v-if="filteredFunctions.length > 0" class="content-block">
                  <h2 class="section-heading">
                    <u>{{ aboutData.sections[1].title }}</u>
                  </h2>
                  <div class="items-list">
                    <p 
                      v-for="(func, idx) in filteredFunctions" 
                      :key="'func-' + idx"
                      class="body-paragraph"
                      v-html="highlightText(func)"
                    ></p>
                  </div>
                </div>

                <div v-if="filteredTasks.length === 0 && filteredFunctions.length === 0" class="no-results">
                  <p>So‘rovingiz bo‘yicha hech qanday ma’lumot topilmadi.</p>
                </div>
              </div>

              <!-- Standard 1:1 HTML View -->
              <div 
                v-else 
                class="raw-body-html" 
                v-html="aboutData.body_html"
              ></div>
            </div>

            <!-- Article Footer: Social Media Sharing, Views, Updated Date -->
            <footer class="paper-footer">
              <div class="footer-top-row">
                <!-- Social Sharing Links -->
                <div class="social-share-group">
                  <span class="share-label">Ulashish:</span>
                  <div class="share-buttons">
                    <!-- Telegram -->
                    <a 
                      :href="aboutData.social_share.telegram" 
                      target="_blank" 
                      rel="noopener noreferrer" 
                      class="share-btn share-tg" 
                      title="Telegram orqali ulashish"
                      aria-label="Telegram"
                    >
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M19.2 4.4 2.9 10.7c-1.1.4-1.1 1.1-.2 1.3l4.1 1.3 1.6 4.8c.2.5.1.7.6.7.4 0 .6-.2.8-.4l2-2 4.2 3.1c.8.4 1.3.2 1.5-.7l2.8-13.1c.3-1.1-.4-1.7-1.1-1.3m-2.1 3-7.8 7.1-.3 3.3L7.4 13l9.2-5.8c.4-.3.8-.1.5.2"/>
                      </svg>
                    </a>

                    <!-- Facebook -->
                    <a 
                      :href="aboutData.social_share.facebook" 
                      target="_blank" 
                      rel="noopener noreferrer" 
                      class="share-btn share-fb" 
                      title="Facebook orqali ulashish"
                      aria-label="Facebook"
                    >
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
                      </svg>
                    </a>

                    <!-- Twitter (X) -->
                    <a 
                      :href="aboutData.social_share.twitter" 
                      target="_blank" 
                      rel="noopener noreferrer" 
                      class="share-btn share-tw" 
                      title="X (Twitter) orqali ulashish"
                      aria-label="X (Twitter)"
                    >
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
                      </svg>
                    </a>

                    <!-- Copy link -->
                    <button 
                      type="button" 
                      class="share-btn share-copy" 
                      @click="copyPageUrl"
                      :title="copySuccess ? 'Nusxalandi!' : 'Havolani nusxalash'"
                      aria-label="Havolani nusxalash"
                    >
                      <svg v-if="!copySuccess" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"/>
                        <path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"/>
                      </svg>
                      <svg v-else viewBox="0 0 24 24" fill="none" stroke="#16a34a" stroke-width="2">
                        <polyline points="20 6 9 17 4 12"/>
                      </svg>
                    </button>
                  </div>
                  <span v-if="copySuccess" class="copy-toast">Nusxalandi!</span>
                </div>

                <!-- Page Views Counter -->
                <div class="views-counter" title="Ko‘rishlar soni">
                  <svg viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12 4.5C7 4.5 2.73 7.61 1 12c1.73 4.39 6 7.5 11 7.5s9.27-3.11 11-7.5c-1.73-4.39-6-7.5-11-7.5zM12 17c-2.76 0-5-2.24-5-5s2.24-5 5-5 5 2.24 5 5-2.24 5-5 5zm0-8c-1.66 0-3 1.34-3 3s1.34 3 3 3 3-1.34 3-3-1.34-3-3-3z"/>
                  </svg>
                  <span>{{ Number(aboutData.views).toLocaleString('ru-RU') }}</span>
                </div>
              </div>

              <!-- Last Updated Date -->
              <div class="footer-bottom-row">
                <div class="updated-date">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="12" cy="12" r="10"/>
                    <polyline points="12 6 12 12 16 14"/>
                  </svg>
                  <span>Oxirgi yangilanish: {{ aboutData.updated_date }}</span>
                </div>
              </div>
            </footer>
          </article>
        </main>

        <!-- Right Sibling Menu Sidebar -->
        <aside class="right-menu-sidebar">
          <div class="sidebar-sticky-wrapper">
            <div class="sidebar-header">
              <h3 class="sidebar-heading">Vazirlik haqida</h3>
            </div>
            <nav class="sidebar-nav">
              <ul class="sidebar-menu-list">
                <li 
                  v-for="item in aboutData.sidebar" 
                  :key="item.id" 
                  class="sidebar-menu-item"
                >
                  <a 
                    :href="item.url" 
                    class="sidebar-menu-link" 
                    :class="{ 'active': item.active }"
                    @click="handleSidebarClick(item)"
                  >
                    <span class="item-title">{{ item.title }}</span>
                    <svg class="chevron-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                      <polyline points="9 18 15 12 9 6"/>
                    </svg>
                  </a>
                </li>
              </ul>
            </nav>
          </div>
        </aside>
      </div>
    </div>
  </div>
</template>

<style scoped>
.about-page-wrapper {
  background-color: #f7f9fa;
  min-height: calc(100vh - 200px);
  padding: 32px 0 64px 0;
}

.page-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 20px;
}

.page-layout {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 320px;
  gap: 32px;
  align-items: start;
}

/* Main Content Area */
.main-content-area {
  min-width: 0;
}

.content-paper {
  background-color: #ffffff;
  border-radius: 12px;
  border: 1px solid #e5e9f0;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.04);
  padding: 36px 48px;
  transition: box-shadow 0.2s ease;
}

/* Header & Breadcrumbs */
.paper-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  padding-bottom: 20px;
  border-bottom: 1px solid #f1f3f5;
  flex-wrap: wrap;
}

.breadcrumbs {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 6px;
  font-size: 14px;
}

.breadcrumb-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  color: #64748b;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s ease;
}

.breadcrumb-link:hover {
  color: #005a9c;
  text-decoration: underline;
}

.breadcrumb-link.active-link {
  color: #1e293b;
  font-weight: 600;
}

.home-icon {
  width: 16px;
  height: 16px;
}

.breadcrumb-separator {
  display: inline-flex;
  align-items: center;
  color: #94a3b8;
}

.breadcrumb-separator svg {
  width: 14px;
  height: 14px;
}

.breadcrumb-current {
  color: #005a9c;
  font-weight: 600;
}

/* Header Right Tools */
.header-tools {
  display: flex;
  align-items: center;
  gap: 12px;
}

.font-resizer {
  display: flex;
  align-items: center;
  background-color: #f1f5f9;
  border-radius: 6px;
  padding: 2px;
}

.font-btn {
  background: none;
  border: none;
  padding: 4px 8px;
  font-size: 12px;
  font-weight: 700;
  color: #475569;
  cursor: pointer;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.font-btn:hover:not(:disabled) {
  background-color: #ffffff;
  color: #005a9c;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.font-btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}

.age-limit-badge {
  font-size: 12px;
  font-weight: 700;
  color: #475569;
  background-color: #f8fafc;
  border: 1px solid #cbd5e1;
  border-radius: 4px;
  padding: 3px 8px;
}

.tools-divider {
  width: 1px;
  height: 18px;
  background-color: #e2e8f0;
}

.tool-btn {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #475569;
  cursor: pointer;
  transition: all 0.2s ease;
}

.tool-btn svg {
  width: 17px;
  height: 17px;
}

.tool-btn:hover {
  background-color: #005a9c;
  color: #ffffff;
  border-color: #005a9c;
}

/* Title */
.page-title {
  margin-top: 24px;
  margin-bottom: 20px;
  font-size: 28px;
  font-weight: 700;
  color: #0f172a;
  text-transform: uppercase;
  letter-spacing: 0.3px;
  line-height: 1.3;
}

/* Page Search / Filter */
.page-search-bar {
  margin-bottom: 28px;
  display: flex;
  align-items: center;
  gap: 16px;
  flex-wrap: wrap;
}

.search-input-wrapper {
  position: relative;
  flex: 1;
  max-width: 460px;
}

.search-input-wrapper .search-icon {
  position: absolute;
  left: 14px;
  top: 50%;
  transform: translateY(-50%);
  width: 18px;
  height: 18px;
  color: #94a3b8;
}

.search-input {
  width: 100%;
  padding: 10px 38px 10px 42px;
  font-size: 14.5px;
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  color: #1e293b;
  transition: all 0.2s ease;
}

.search-input:focus {
  outline: none;
  background-color: #ffffff;
  border-color: #005a9c;
  box-shadow: 0 0 0 3px rgba(0, 90, 156, 0.12);
}

.clear-btn {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  font-size: 14px;
  color: #94a3b8;
  cursor: pointer;
  padding: 4px;
}

.clear-btn:hover {
  color: #ef4444;
}

.search-count {
  font-size: 13.5px;
  color: #005a9c;
  font-weight: 600;
  background: #e0f2fe;
  padding: 6px 12px;
  border-radius: 6px;
}

/* Article Body */
.article-body {
  color: #1e293b;
  line-height: 1.75;
  text-align: justify;
}

.section-heading {
  font-size: 19px;
  font-weight: 700;
  color: #0f172a;
  margin-top: 28px;
  margin-bottom: 16px;
  line-height: 1.4;
}

.body-paragraph {
  margin-bottom: 16px;
  color: #334155;
  text-indent: 1.5rem;
}

:deep(mark.highlight) {
  background-color: #fef08a;
  color: #854d0e;
  padding: 2px 4px;
  border-radius: 3px;
}

/* Raw HTML styles for 1:1 view */
.raw-body-html :deep(p) {
  margin-bottom: 16px;
  color: #27272a;
  line-height: 1.75;
  text-align: justify;
  text-indent: 1.5rem;
}

.raw-body-html :deep(p strong) {
  color: #0f172a;
  font-weight: 700;
}

.raw-body-html :deep(p u) {
  text-decoration: underline;
  text-underline-offset: 4px;
  text-decoration-color: #005a9c;
}

.raw-body-html :deep(p:has(strong u)) {
  text-indent: 0;
  margin-top: 28px;
  margin-bottom: 16px;
  font-size: 1.08em;
}

.raw-body-html :deep(ul) {
  list-style: disc;
  padding-left: 2.2rem;
  margin-bottom: 16px;
}

.raw-body-html :deep(ol) {
  list-style: decimal;
  padding-left: 2.2rem;
  margin-bottom: 16px;
}

.raw-body-html :deep(li) {
  margin-bottom: 8px;
}

.raw-body-html :deep(a) {
  color: #005a9c;
  font-weight: 600;
  text-decoration: underline;
}

.raw-body-html :deep(a:hover) {
  text-decoration: none;
}

/* Footer / Metadata */
.paper-footer {
  margin-top: 48px;
  padding-top: 24px;
  border-top: 1px solid #f1f3f5;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.footer-top-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  flex-wrap: wrap;
}

.social-share-group {
  display: flex;
  align-items: center;
  gap: 12px;
  position: relative;
}

.share-label {
  font-size: 14px;
  font-weight: 600;
  color: #64748b;
}

.share-buttons {
  display: flex;
  align-items: center;
  gap: 8px;
}

.share-btn {
  width: 36px;
  height: 36px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #e2e8f0;
  background-color: #f8fafc;
  color: #475569;
  cursor: pointer;
  text-decoration: none;
  transition: all 0.2s ease;
}

.share-btn svg {
  width: 18px;
  height: 18px;
}

.share-tg:hover {
  background-color: #229ED9;
  border-color: #229ED9;
  color: #ffffff;
  transform: translateY(-2px);
}

.share-fb:hover {
  background-color: #1877F2;
  border-color: #1877F2;
  color: #ffffff;
  transform: translateY(-2px);
}

.share-tw:hover {
  background-color: #000000;
  border-color: #000000;
  color: #ffffff;
  transform: translateY(-2px);
}

.share-copy:hover {
  background-color: #005a9c;
  border-color: #005a9c;
  color: #ffffff;
  transform: translateY(-2px);
}

.copy-toast {
  font-size: 12px;
  font-weight: 600;
  color: #16a34a;
  background-color: #dcfce7;
  padding: 3px 8px;
  border-radius: 4px;
  animation: fadeIn 0.2s ease-in;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(4px); }
  to { opacity: 1; transform: translateY(0); }
}

.views-counter {
  display: flex;
  align-items: center;
  gap: 8px;
  color: #64748b;
  font-size: 14.5px;
  font-weight: 500;
}

.views-counter svg {
  width: 20px;
  height: 20px;
  color: #94a3b8;
}

.footer-bottom-row {
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

.updated-date {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13.5px;
  color: #64748b;
}

.updated-date svg {
  width: 16px;
  height: 16px;
  color: #94a3b8;
}

/* Right Menu Sidebar */
.right-menu-sidebar {
  min-width: 0;
}

.sidebar-sticky-wrapper {
  position: sticky;
  top: 100px;
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.03);
}

.sidebar-header {
  padding: 16px 20px;
  background-color: #f8fafc;
  border-bottom: 1px solid #e2e8f0;
}

.sidebar-heading {
  margin: 0;
  font-size: 16px;
  font-weight: 700;
  color: #0f172a;
}

.sidebar-menu-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.sidebar-menu-item {
  border-bottom: 1px solid #f1f5f9;
}

.sidebar-menu-item:last-child {
  border-bottom: none;
}

.sidebar-menu-link {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 14px 20px;
  font-size: 14.5px;
  color: #475569;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.sidebar-menu-link:hover {
  background-color: #f1f5f9;
  color: #005a9c;
  padding-left: 24px;
}

.sidebar-menu-link.active {
  background-color: #005a9c;
  color: #ffffff;
  font-weight: 600;
}

.sidebar-menu-link .chevron-icon {
  width: 16px;
  height: 16px;
  color: #94a3b8;
  transition: transform 0.2s ease, color 0.2s ease;
  flex-shrink: 0;
}

.sidebar-menu-link:hover .chevron-icon {
  color: #005a9c;
  transform: translateX(4px);
}

.sidebar-menu-link.active .chevron-icon {
  color: #ffffff;
  transform: translateX(2px);
}

/* Responsive Styles */
@media (max-width: 1024px) {
  .page-layout {
    grid-template-columns: 1fr;
  }

  .content-paper {
    padding: 28px 24px;
  }

  .right-menu-sidebar {
    order: 2;
  }

  .sidebar-sticky-wrapper {
    position: static;
  }
}

@media (max-width: 640px) {
  .about-page-wrapper {
    padding: 16px 0 40px 0;
  }

  .content-paper {
    padding: 20px 16px;
    border-radius: 8px;
  }

  .paper-header {
    flex-direction: column;
    align-items: flex-start;
  }

  .page-title {
    font-size: 22px;
  }

  .footer-top-row {
    flex-direction: column;
    align-items: flex-start;
  }
}

/* Print Styles */
@media print {
  .about-page-wrapper {
    background: none;
    padding: 0;
  }

  .page-layout {
    display: block;
  }

  .right-menu-sidebar,
  .header-tools,
  .page-search-bar,
  .social-share-group,
  .breadcrumb-separator,
  .breadcrumbs {
    display: none !important;
  }

  .content-paper {
    border: none;
    box-shadow: none;
    padding: 0;
  }

  .page-title {
    font-size: 24pt;
    margin-bottom: 20pt;
  }

  .article-body {
    font-size: 12pt;
    line-height: 1.6;
    color: #000;
  }
}
</style>
