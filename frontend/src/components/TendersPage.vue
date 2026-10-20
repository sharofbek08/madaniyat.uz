<script setup>
import { ref, computed } from 'vue'
import tendersData from '../mock/tenders.json'

const emit = defineEmits(['navigate'])

// Search and Filter State
const searchQuery = ref('')
const selectedCategory = ref('all')
const currentPage = ref(1)
const itemsPerPage = 6

// Active Tender for Detail Modal
const activeTender = ref(null)

const categories = computed(() => {
  const set = new Set(tendersData.data.map(item => item.activity_title || item.category_title))
  return ['all', ...Array.from(set)]
})

const filteredTenders = computed(() => {
  return tendersData.data.filter(item => {
    const q = searchQuery.value.trim().toLowerCase()
    const matchesSearch = !q || 
      item.title.toLowerCase().includes(q) || 
      (item.anons && item.anons.toLowerCase().includes(q))
    
    const cat = item.activity_title || item.category_title
    const matchesCat = selectedCategory.value === 'all' || cat === selectedCategory.value

    return matchesSearch && matchesCat
  })
})

const totalPages = computed(() => {
  return Math.ceil(filteredTenders.value.length / itemsPerPage) || 1
})

const paginatedTenders = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage
  return filteredTenders.value.slice(start, start + itemsPerPage)
})

const selectCategory = (cat) => {
  selectedCategory.value = cat
  currentPage.value = 1
}

// Print Handler
const handlePrint = () => {
  window.print()
}

// Copy URL Handler
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

// Sidebar Navigation
const sidebarItems = [
  { id: 420, title: 'Tanlovlar va tenderlar', url: '#madaniyat/news/e-lon', active: true },
  { id: 13460, title: 'Ekofaol xodim', url: '#madaniyat/news/test', active: false },
  { id: 3825, title: 'Konstitutsiya sahifasi', url: '#madaniyat/sections/konstitutsiya-sahifasi', active: false },
  { id: 3433, title: 'Korrupsiyaga qarshi kurashish', url: '#madaniyat/sections/korrup-bo-lim', active: false },
  { id: 421, title: 'Majlislar', url: '#madaniyat/activity_page/majlislar', active: false },
  { id: 422, title: 'Hisobotlar', url: '#madaniyat/activity_page/hisobotlar', active: false },
  { id: 423, title: 'Tadbirlar', url: '#madaniyat/activity_page/tadbirlar', active: false },
  { id: 424, title: 'Rahbarlar faoliyati', url: '#madaniyat/activity_page/rahbarlar-faoliyati', active: false }
]

const handleSidebarClick = (item) => {
  window.location.hash = item.url
}

const openTenderDetail = (tender) => {
  activeTender.value = tender
}

const closeTenderDetail = () => {
  activeTender.value = null
}
</script>

<template>
  <div class="tenders-page-wrapper">
    <div class="container page-container">
      <div class="page-layout">
        <!-- Main Content Area -->
        <main class="main-content-area">
          <article class="content-paper" id="printable-article">
            <!-- Header & Breadcrumbs -->
            <div class="paper-header">
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

                <span class="breadcrumb-link active-link">Faoliyat</span>

                <span class="breadcrumb-separator" aria-hidden="true">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="9 18 15 12 9 6"/>
                  </svg>
                </span>

                <span class="breadcrumb-current" aria-current="page">Tanlovlar va tenderlar</span>
              </nav>

              <!-- Header Tools -->
              <div class="header-tools">
                <button 
                  class="tool-btn" 
                  title="Havolani nusxalash" 
                  aria-label="Havolani nusxalash"
                  @click="copyPageUrl"
                >
                  <svg v-if="!copySuccess" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"/>
                    <path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"/>
                  </svg>
                  <svg v-else class="success-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="20 6 9 17 4 12"/>
                  </svg>
                </button>

                <button 
                  class="tool-btn" 
                  title="Chop etish" 
                  aria-label="Chop etish"
                  @click="handlePrint"
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
            <h1 class="page-main-title">TANLOVLAR VA TENDERLAR</h1>

            <!-- Search and Filter Bar -->
            <div class="filter-controls">
              <div class="search-input-box">
                <svg class="search-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <circle cx="11" cy="11" r="8"/>
                  <line x1="21" y1="21" x2="16.65" y2="16.65"/>
                </svg>
                <input 
                  type="text" 
                  v-model="searchQuery" 
                  placeholder="Tenderlar va e'lonlar bo'yicha qidiruv..." 
                  class="search-input"
                  aria-label="Tenderlar bo'yicha qidiruv"
                />
                <button 
                  v-if="searchQuery" 
                  @click="searchQuery = ''" 
                  class="clear-search-btn"
                  title="Tozalash"
                >
                  &times;
                </button>
              </div>

              <div class="filter-tabs">
                <button 
                  class="filter-tab"
                  :class="{ active: selectedCategory === 'all' }"
                  @click="selectCategory('all')"
                >
                  Barchasi ({{ tendersData.data.length }})
                </button>
                <button 
                  v-for="cat in categories.filter(c => c !== 'all')" 
                  :key="cat"
                  class="filter-tab"
                  :class="{ active: selectedCategory === cat }"
                  @click="selectCategory(cat)"
                >
                  {{ cat }}
                </button>
              </div>
            </div>

            <!-- Tenders Grid -->
            <div v-if="paginatedTenders.length > 0" class="tenders-grid">
              <div 
                v-for="tender in paginatedTenders" 
                :key="tender.id"
                class="tender-card"
                @click="openTenderDetail(tender)"
              >
                <div class="tender-card-header">
                  <div class="tender-meta">
                    <span class="meta-date">
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <rect x="3" y="4" width="18" height="18" rx="2" ry="2"/>
                        <line x1="16" y1="2" x2="16" y2="6"/>
                        <line x1="8" y1="2" x2="8" y2="6"/>
                        <line x1="3" y1="10" x2="21" y2="10"/>
                      </svg>
                      {{ tender.date }}
                    </span>
                    <span class="meta-views">
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/>
                        <circle cx="12" cy="12" r="3"/>
                      </svg>
                      {{ tender.views }}
                    </span>
                  </div>
                  <span class="badge-elon">{{ tender.activity_title || 'E’lon' }}</span>
                </div>

                <div class="tender-card-body">
                  <h3 class="tender-title">{{ tender.title }}</h3>
                  <p class="tender-anons">{{ tender.anons }}</p>
                </div>

                <div class="tender-card-footer">
                  <span class="read-more-link">
                    Batafsil ma'lumot
                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                      <line x1="5" y1="12" x2="19" y2="12"/>
                      <polyline points="12 5 19 12 12 19"/>
                    </svg>
                  </span>
                </div>
              </div>
            </div>

            <!-- Empty State -->
            <div v-else class="empty-state">
              <div class="empty-icon">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                  <circle cx="11" cy="11" r="8"/>
                  <line x1="21" y1="21" x2="16.65" y2="16.65"/>
                </svg>
              </div>
              <h3>Ma'lumot topilmadi</h3>
              <p>Kiritilgan qidiruv so‘rovi bo‘yicha e’lonlar mavjud emas.</p>
              <button class="reset-filter-btn" @click="searchQuery = ''; selectedCategory = 'all'">
                Qidiruvni tozalash
              </button>
            </div>

            <!-- Pagination -->
            <div v-if="totalPages > 1" class="pagination-bar">
              <button 
                class="page-nav-btn" 
                :disabled="currentPage === 1"
                @click="currentPage--"
              >
                &lsaquo;
              </button>
              <button 
                v-for="p in totalPages" 
                :key="p"
                class="page-num-btn"
                :class="{ active: currentPage === p }"
                @click="currentPage = p"
              >
                {{ p }}
              </button>
              <button 
                class="page-nav-btn" 
                :disabled="currentPage === totalPages"
                @click="currentPage++"
              >
                &rsaquo;
              </button>
            </div>

            <!-- Social Share Bar -->
            <div class="social-share-container">
              <span class="share-label">Ulashish:</span>
              <div class="share-buttons">
                <a 
                  href="https://facebook.com" 
                  target="_blank" 
                  rel="noopener" 
                  class="share-btn fb" 
                  title="Facebook-da ulashish"
                >
                  <svg viewBox="0 0 24 24" fill="currentColor">
                    <path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"/>
                  </svg>
                </a>
                <a 
                  href="https://twitter.com" 
                  target="_blank" 
                  rel="noopener" 
                  class="share-btn tw" 
                  title="X (Twitter)-da ulashish"
                >
                  <svg viewBox="0 0 24 24" fill="currentColor">
                    <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
                  </svg>
                </a>
                <a 
                  href="https://t.me/share/url" 
                  target="_blank" 
                  rel="noopener" 
                  class="share-btn tg" 
                  title="Telegram-da ulashish"
                >
                  <svg viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm4.64 6.8c-.15 1.58-.8 5.42-1.13 7.19-.14.75-.42 1-.68 1.03-.58.05-1.02-.38-1.58-.75-.88-.58-1.38-.94-2.23-1.5-.99-.65-.35-1.01.22-1.59.15-.15 2.71-2.48 2.76-2.69a.2.2 0 0 0-.05-.18c-.06-.05-.14-.03-.21-.02-.09.02-1.49.95-4.22 2.79-.4.27-.76.41-1.08.4-.36-.01-1.04-.2-1.55-.37-.63-.2-1.12-.31-1.08-.66.02-.18.27-.36.74-.55 2.92-1.27 4.86-2.11 5.83-2.51 2.78-1.16 3.35-1.36 3.73-1.36.08 0 .27.02.39.12.1.08.13.19.14.27-.01.06.01.24 0 .38z"/>
                  </svg>
                </a>
              </div>
            </div>
          </article>
        </main>

        <!-- Sidebar Navigation Area -->
        <aside class="sidebar-navigation-area">
          <div class="sidebar-card">
            <h2 class="sidebar-heading">Faoliyat</h2>
            <nav class="sidebar-nav-list" aria-label="Faoliyat bo'limlari">
              <a 
                v-for="item in sidebarItems" 
                :key="item.id"
                :href="item.url"
                class="sidebar-nav-item"
                :class="{ 'is-active': item.active }"
                @click.prevent="handleSidebarClick(item)"
              >
                <span class="nav-title">{{ item.title }}</span>
                <svg class="chevron-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <polyline points="9 18 15 12 9 6"/>
                </svg>
              </a>
            </nav>
          </div>
        </aside>
      </div>
    </div>

    <!-- Tender Detail Modal -->
    <div v-if="activeTender" class="modal-backdrop" @click="closeTenderDetail">
      <div class="modal-dialog" @click.stop>
        <div class="modal-header">
          <div class="modal-meta">
            <span class="badge-elon">{{ activeTender.activity_title || 'E’lon' }}</span>
            <span class="modal-date">{{ activeTender.date }}</span>
            <span class="modal-views">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="16" height="16">
                <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/>
                <circle cx="12" cy="12" r="3"/>
              </svg>
              {{ activeTender.views }} marta ko'rildi
            </span>
          </div>
          <button class="modal-close-btn" @click="closeTenderDetail" aria-label="Yopish">&times;</button>
        </div>

        <div class="modal-body">
          <h2 class="modal-tender-title">{{ activeTender.title }}</h2>
          <div class="tender-body-content" v-html="activeTender.body"></div>
        </div>

        <div class="modal-footer">
          <button class="download-doc-btn" @click="alert('Hujjat yuklab olinmoqda...')">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="18" height="18">
              <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/>
              <polyline points="7 10 12 15 17 10"/>
              <line x1="12" y1="15" x2="12" y2="3"/>
            </svg>
            Tender hujjatini yuklab olish (.PDF)
          </button>
          <button class="close-action-btn" @click="closeTenderDetail">
            Yopish
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.tenders-page-wrapper {
  background-color: #f4f6fa;
  min-height: 80vh;
  padding: 30px 0 60px;
}

.page-container {
  max-width: 1320px;
  margin: 0 auto;
  padding: 0 20px;
}

.page-layout {
  display: grid;
  grid-template-columns: 1fr 340px;
  gap: 30px;
  align-items: start;
}

@media (max-width: 1024px) {
  .page-layout {
    grid-template-columns: 1fr;
  }
}

/* Paper Content */
.content-paper {
  background: #ffffff;
  border-radius: 12px;
  padding: 36px 40px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.04);
}

@media (max-width: 640px) {
  .content-paper {
    padding: 24px 18px;
  }
}

/* Breadcrumbs & Header Tools */
.paper-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
  flex-wrap: wrap;
  gap: 15px;
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
  transition: color 0.2s;
}

.breadcrumb-link:hover {
  color: #1e40af;
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

.active-link {
  color: #1e40af;
  font-weight: 500;
}

.breadcrumb-current {
  color: #0f172a;
  font-weight: 600;
}

.header-tools {
  display: flex;
  align-items: center;
  gap: 10px;
}

.tool-btn {
  background: #f1f5f9;
  border: 1px solid #e2e8f0;
  width: 38px;
  height: 38px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #475569;
  cursor: pointer;
  transition: all 0.2s;
}

.tool-btn svg {
  width: 18px;
  height: 18px;
}

.tool-btn:hover {
  background: #e2e8f0;
  color: #0f172a;
}

.tool-btn .success-icon {
  color: #10b981;
}

/* Page Title */
.page-main-title {
  font-size: 26px;
  font-weight: 800;
  color: #111827;
  letter-spacing: -0.01em;
  margin-bottom: 24px;
  border-bottom: 2px solid #f1f5f9;
  padding-bottom: 16px;
}

/* Filter Controls */
.filter-controls {
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-bottom: 28px;
}

.search-input-box {
  position: relative;
  display: flex;
  align-items: center;
  width: 100%;
}

.search-icon {
  position: absolute;
  left: 14px;
  width: 18px;
  height: 18px;
  color: #94a3b8;
}

.search-input {
  width: 100%;
  padding: 12px 40px 12px 42px;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  font-size: 15px;
  color: #1e293b;
  outline: none;
  transition: all 0.2s;
}

.search-input:focus {
  background: #ffffff;
  border-color: #2563eb;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}

.clear-search-btn {
  position: absolute;
  right: 12px;
  background: transparent;
  border: none;
  font-size: 20px;
  color: #94a3b8;
  cursor: pointer;
}

.filter-tabs {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.filter-tab {
  background: #f1f5f9;
  border: 1px solid #e2e8f0;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 13px;
  font-weight: 500;
  color: #475569;
  cursor: pointer;
  transition: all 0.2s;
}

.filter-tab:hover {
  background: #e2e8f0;
  color: #0f172a;
}

.filter-tab.active {
  background: #1e40af;
  border-color: #1e40af;
  color: #ffffff;
}

/* Tenders Grid */
.tenders-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(360px, 1fr));
  gap: 20px;
  margin-bottom: 30px;
}

@media (max-width: 640px) {
  .tenders-grid {
    grid-template-columns: 1fr;
  }
}

.tender-card {
  background: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 10px;
  padding: 20px 22px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  cursor: pointer;
  transition: all 0.25s ease;
  position: relative;
  overflow: hidden;
}

.tender-card::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 4px;
  background: #2563eb;
  opacity: 0;
  transition: opacity 0.2s;
}

.tender-card:hover {
  border-color: #bfdbfe;
  box-shadow: 0 10px 25px rgba(37, 99, 235, 0.08);
  transform: translateY(-2px);
}

.tender-card:hover::before {
  opacity: 1;
}

.tender-card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 14px;
}

.tender-meta {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 12px;
  color: #64748b;
}

.meta-date, .meta-views {
  display: inline-flex;
  align-items: center;
  gap: 4px;
}

.meta-date svg, .meta-views svg {
  width: 14px;
  height: 14px;
  color: #94a3b8;
}

.badge-elon {
  background: #eff6ff;
  color: #1d4ed8;
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  padding: 3px 8px;
  border-radius: 4px;
  letter-spacing: 0.04em;
}

.tender-card-body {
  flex: 1;
  margin-bottom: 16px;
}

.tender-title {
  font-size: 16px;
  font-weight: 700;
  color: #1e293b;
  line-height: 1.45;
  margin-bottom: 8px;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  transition: color 0.2s;
}

.tender-card:hover .tender-title {
  color: #1e40af;
}

.tender-anons {
  font-size: 13px;
  color: #64748b;
  line-height: 1.5;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.tender-card-footer {
  border-top: 1px dashed #f1f5f9;
  padding-top: 12px;
  display: flex;
  justify-content: flex-end;
}

.read-more-link {
  font-size: 13px;
  font-weight: 600;
  color: #2563eb;
  display: inline-flex;
  align-items: center;
  gap: 6px;
  transition: transform 0.2s;
}

.read-more-link svg {
  width: 15px;
  height: 15px;
  transition: transform 0.2s;
}

.tender-card:hover .read-more-link svg {
  transform: translateX(4px);
}

/* Empty State */
.empty-state {
  text-align: center;
  padding: 60px 20px;
  background: #f8fafc;
  border-radius: 12px;
  margin-bottom: 30px;
}

.empty-icon svg {
  width: 48px;
  height: 48px;
  color: #94a3b8;
  margin-bottom: 12px;
}

.empty-state h3 {
  font-size: 18px;
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 6px;
}

.empty-state p {
  color: #64748b;
  font-size: 14px;
  margin-bottom: 18px;
}

.reset-filter-btn {
  background: #1e40af;
  color: #ffffff;
  border: none;
  padding: 9px 20px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}

/* Pagination Bar */
.pagination-bar {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  margin: 30px 0 20px;
}

.page-num-btn, .page-nav-btn {
  min-width: 38px;
  height: 38px;
  padding: 0 10px;
  border-radius: 8px;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  color: #475569;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
}

.page-num-btn:hover, .page-nav-btn:hover:not(:disabled) {
  background: #e2e8f0;
  color: #0f172a;
}

.page-num-btn.active {
  background: #1e40af;
  border-color: #1e40af;
  color: #ffffff;
}

.page-nav-btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}

/* Social Share */
.social-share-container {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-top: 30px;
  padding-top: 20px;
  border-top: 1px solid #f1f5f9;
}

.share-label {
  font-size: 13px;
  font-weight: 600;
  color: #64748b;
}

.share-buttons {
  display: flex;
  gap: 8px;
}

.share-btn {
  width: 34px;
  height: 34px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #ffffff;
  transition: transform 0.2s, opacity 0.2s;
  text-decoration: none;
}

.share-btn:hover {
  transform: translateY(-2px);
  opacity: 0.9;
}

.share-btn svg {
  width: 18px;
  height: 18px;
}

.share-btn.fb { background: #1877f2; }
.share-btn.tw { background: #000000; }
.share-btn.tg { background: #229ed9; }

/* Sidebar */
.sidebar-card {
  background: #ffffff;
  border-radius: 12px;
  padding: 24px 20px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.04);
  position: sticky;
  top: 100px;
}

.sidebar-heading {
  font-size: 18px;
  font-weight: 800;
  color: #0f172a;
  padding-bottom: 14px;
  margin-bottom: 12px;
  border-bottom: 2px solid #f1f5f9;
}

.sidebar-nav-list {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.sidebar-nav-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 14px;
  border-radius: 8px;
  color: #334155;
  font-size: 14px;
  font-weight: 500;
  text-decoration: none;
  transition: all 0.2s;
}

.sidebar-nav-item:hover {
  background: #f8fafc;
  color: #1e40af;
}

.sidebar-nav-item.is-active {
  background: #eff6ff;
  color: #1d4ed8;
  font-weight: 700;
}

.chevron-icon {
  width: 14px;
  height: 14px;
  color: #94a3b8;
  transition: transform 0.2s;
}

.sidebar-nav-item:hover .chevron-icon,
.sidebar-nav-item.is-active .chevron-icon {
  color: #1d4ed8;
  transform: translateX(3px);
}

/* Modal */
.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(15, 23, 42, 0.6);
  backdrop-filter: blur(4px);
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.modal-dialog {
  background: #ffffff;
  border-radius: 16px;
  max-width: 720px;
  width: 100%;
  max-height: 90vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
  overflow: hidden;
}

.modal-header {
  padding: 20px 24px;
  border-bottom: 1px solid #e2e8f0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-meta {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 13px;
  color: #64748b;
  flex-wrap: wrap;
}

.modal-views {
  display: inline-flex;
  align-items: center;
  gap: 4px;
}

.modal-close-btn {
  background: transparent;
  border: none;
  font-size: 28px;
  line-height: 1;
  color: #94a3b8;
  cursor: pointer;
  transition: color 0.2s;
}

.modal-close-btn:hover {
  color: #0f172a;
}

.modal-body {
  padding: 24px;
  overflow-y: auto;
  flex: 1;
}

.modal-tender-title {
  font-size: 20px;
  font-weight: 800;
  color: #0f172a;
  line-height: 1.4;
  margin-bottom: 20px;
}

.tender-body-content {
  font-size: 15px;
  line-height: 1.7;
  color: #334155;
}

.tender-body-content :deep(h4) {
  margin: 18px 0 10px;
  color: #0f172a;
}

.tender-body-content :deep(ul) {
  padding-left: 20px;
  margin-bottom: 16px;
}

.tender-body-content :deep(li) {
  margin-bottom: 6px;
}

.modal-footer {
  padding: 16px 24px;
  border-top: 1px solid #e2e8f0;
  background: #f8fafc;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
}

.download-doc-btn {
  background: #1e40af;
  color: #ffffff;
  border: none;
  padding: 10px 18px;
  border-radius: 8px;
  font-weight: 600;
  font-size: 14px;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  transition: background 0.2s;
}

.download-doc-btn:hover {
  background: #1d4ed8;
}

.close-action-btn {
  background: #e2e8f0;
  color: #475569;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.2s;
}

.close-action-btn:hover {
  background: #cbd5e1;
  color: #0f172a;
}
</style>
