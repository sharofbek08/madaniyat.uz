<script setup>
import { ref, computed } from 'vue'
import regionalData from '../mock/regional.json'

const emit = defineEmits(['navigate'])

// Active Accordion State (first region active by default)
const activeRegionId = ref(573)

const toggleRegion = (id) => {
  activeRegionId.value = activeRegionId.value === id ? null : id
}

// Search Filter
const searchQuery = ref('')

const filteredDepartments = computed(() => {
  const query = searchQuery.value.trim().toLowerCase()
  if (!query) return regionalData.departments
  return regionalData.departments.filter(d => 
    d.title.toLowerCase().includes(query) ||
    d.leader.toLowerCase().includes(query) ||
    d.address.toLowerCase().includes(query)
  )
})

// Print Handler
const handlePrint = () => {
  window.print()
}

// Copy Link Handler
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

const handleSidebarClick = (item) => {
  if (item.url.startsWith('#')) {
    window.location.hash = item.url
  }
}
</script>

<template>
  <div class="regional-page-wrapper">
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

                <span class="breadcrumb-current" aria-current="page">{{ regionalData.title }}</span>
              </nav>

              <!-- Header Tools -->
              <div class="header-tools">
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
            <h1 class="page-title">{{ regionalData.title }}</h1>

            <!-- Quick Search Bar -->
            <div class="search-filter-wrapper">
              <div class="search-input-box">
                <svg class="search-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <circle cx="11" cy="11" r="8"/>
                  <line x1="21" y1="21" x2="16.65" y2="16.65"/>
                </svg>
                <input 
                  type="text" 
                  v-model="searchQuery" 
                  placeholder="Hudud nomi, rahbar F.I.Sh. yoki manzil bo‘yicha izlash..." 
                  class="search-field"
                />
                <button 
                  v-if="searchQuery" 
                  type="button" 
                  class="clear-btn" 
                  @click="searchQuery = ''"
                >
                  ✕
                </button>
              </div>
              <span class="badge-count">{{ filteredDepartments.length }} ta hudud</span>
            </div>

            <!-- Regional Accordion List -->
            <div class="regional-accordion">
              <div 
                v-for="dept in filteredDepartments" 
                :key="dept.id" 
                class="accordion-card"
                :class="{ 'card-open': activeRegionId === dept.id }"
              >
                <!-- Accordion Header -->
                <button 
                  type="button" 
                  class="accordion-header-btn" 
                  @click="toggleRegion(dept.id)"
                  :aria-expanded="activeRegionId === dept.id"
                >
                  <div class="region-title-group">
                    <span class="region-pin-dot"></span>
                    <span class="region-title-text">{{ dept.title }}</span>
                  </div>

                  <span class="region-chevron" :class="{ 'rotate-open': activeRegionId === dept.id }">
                    <svg viewBox="0 0 32 32" fill="currentColor">
                      <path d="M24.285 11.284 16 19.571l-8.285-8.288a1.01 1.01 0 1 0-1.429 1.43l8.999 9.002a1.01 1.01 0 0 0 1.428 0l8.999-9.002a1.01 1.01 0 1 0-1.427-1.429"/>
                    </svg>
                  </span>
                </button>

                <!-- Accordion Body -->
                <transition name="panel-expand">
                  <div v-if="activeRegionId === dept.id" class="accordion-panel-body">
                    <div class="panel-inner">
                      <!-- Left: Leader Photo + Basic -->
                      <div class="leader-section">
                        <div class="leader-photo-wrap">
                          <img 
                            :src="dept.image" 
                            :alt="dept.leader" 
                            class="leader-photo"
                            loading="lazy"
                          />
                        </div>
                        <div class="leader-meta">
                          <h3 class="leader-name">{{ dept.leader }}</h3>
                          <p class="leader-pos">{{ dept.position }}</p>
                          <div class="stat-pills">
                            <span class="stat-badge" title="Madaniyat markazlari">
                              <strong>{{ dept.stats.centers }}</strong> markaz
                            </span>
                            <span class="stat-badge" title="Teatrlar">
                              <strong>{{ dept.stats.theaters }}</strong> teatr
                            </span>
                            <span class="stat-badge" title="Muzeylar">
                              <strong>{{ dept.stats.museums }}</strong> muzey
                            </span>
                          </div>
                        </div>
                      </div>

                      <!-- Right: Contact Information Grid -->
                      <div class="contacts-grid">
                        <!-- Address -->
                        <div class="contact-card">
                          <div class="icon-circle">
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                              <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                              <circle cx="12" cy="10" r="3"/>
                            </svg>
                          </div>
                          <div class="contact-info">
                            <span class="label">Manzil:</span>
                            <span class="val">{{ dept.address }}</span>
                          </div>
                        </div>

                        <!-- Phone -->
                        <div class="contact-card">
                          <div class="icon-circle">
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                              <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
                            </svg>
                          </div>
                          <div class="contact-info">
                            <span class="label">Telefon:</span>
                            <a :href="`tel:${dept.phone.replace(/[^0-9+]/g, '')}`" class="val link">
                              {{ dept.phone }}
                            </a>
                          </div>
                        </div>

                        <!-- Email -->
                        <div class="contact-card">
                          <div class="icon-circle">
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                              <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                              <polyline points="22,6 12,13 2,6"/>
                            </svg>
                          </div>
                          <div class="contact-info">
                            <span class="label">Elektron pochta:</span>
                            <a :href="`mailto:${dept.email}`" class="val link">
                              {{ dept.email }}
                            </a>
                          </div>
                        </div>

                        <!-- Work Hours & Reception -->
                        <div class="contact-card">
                          <div class="icon-circle">
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                              <circle cx="12" cy="12" r="10"/>
                              <polyline points="12 6 12 12 16 14"/>
                            </svg>
                          </div>
                          <div class="contact-info">
                            <span class="label">Ish vaqti va Qabul:</span>
                            <span class="val">{{ dept.work_time }}</span>
                            <span class="val highlight-rec">Qabul: {{ dept.reception_day }}</span>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </transition>
              </div>

              <!-- No Results State -->
              <div v-if="filteredDepartments.length === 0" class="no-results-box">
                <p>«{{ searchQuery }}» so‘rovi bo‘yicha hududiy boshqarma topilmadi.</p>
              </div>
            </div>

            <!-- Footer -->
            <footer class="paper-footer">
              <div class="footer-top-row">
                <div class="social-share-group">
                  <span class="share-label">Ulashish:</span>
                  <div class="share-buttons">
                    <a :href="regionalData.social_share.telegram" target="_blank" rel="noopener noreferrer" class="share-btn share-tg" title="Telegram" aria-label="Telegram">
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M19.2 4.4 2.9 10.7c-1.1.4-1.1 1.1-.2 1.3l4.1 1.3 1.6 4.8c.2.5.1.7.6.7.4 0 .6-.2.8-.4l2-2 4.2 3.1c.8.4 1.3.2 1.5-.7l2.8-13.1c.3-1.1-.4-1.7-1.1-1.3m-2.1 3-7.8 7.1-.3 3.3L7.4 13l9.2-5.8c.4-.3.8-.1.5.2"/>
                      </svg>
                    </a>
                    <a :href="regionalData.social_share.facebook" target="_blank" rel="noopener noreferrer" class="share-btn share-fb" title="Facebook" aria-label="Facebook">
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
                      </svg>
                    </a>
                    <a :href="regionalData.social_share.twitter" target="_blank" rel="noopener noreferrer" class="share-btn share-tw" title="X (Twitter)" aria-label="X (Twitter)">
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
                      </svg>
                    </a>
                    <button type="button" class="share-btn share-copy" @click="copyPageUrl" :title="copySuccess ? 'Nusxalandi!' : 'Havolani nusxalash'">
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
                <li v-for="item in regionalData.sidebar" :key="item.id" class="sidebar-menu-item">
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
.regional-page-wrapper {
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

.main-content-area {
  min-width: 0;
}

.content-paper {
  background-color: #ffffff;
  border-radius: 12px;
  border: 1px solid #e5e9f0;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.04);
  padding: 36px 48px;
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

.home-icon { width: 16px; height: 16px; }
.breadcrumb-separator { display: inline-flex; align-items: center; color: #94a3b8; }
.breadcrumb-separator svg { width: 14px; height: 14px; }
.breadcrumb-current { color: #005a9c; font-weight: 600; }

.header-tools { display: flex; align-items: center; }
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
.tool-btn svg { width: 17px; height: 17px; }
.tool-btn:hover { background-color: #005a9c; color: #fff; border-color: #005a9c; }

.page-title {
  margin-top: 24px;
  margin-bottom: 24px;
  font-size: 28px;
  font-weight: 700;
  color: #0f172a;
  text-transform: uppercase;
  letter-spacing: 0.3px;
}

/* Search Bar */
.search-filter-wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  margin-bottom: 24px;
  flex-wrap: wrap;
}

.search-input-box {
  position: relative;
  flex: 1;
  max-width: 520px;
}

.search-icon {
  position: absolute;
  left: 14px;
  top: 50%;
  transform: translateY(-50%);
  width: 18px;
  height: 18px;
  color: #94a3b8;
}

.search-field {
  width: 100%;
  padding: 11px 40px 11px 44px;
  font-size: 14.5px;
  background-color: #f8fafc;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  color: #0f172a;
  transition: all 0.2s ease;
}

.search-field:focus {
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
}

.badge-count {
  font-size: 13.5px;
  font-weight: 700;
  color: #005a9c;
  background-color: #e0f2fe;
  padding: 6px 14px;
  border-radius: 20px;
}

/* Regional Accordion */
.regional-accordion {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.accordion-card {
  border: 1px solid #e2e8f0;
  border-radius: 10px;
  overflow: hidden;
  background-color: #ffffff;
  transition: all 0.2s ease;
}

.accordion-card.card-open {
  border-color: #005a9c;
  box-shadow: 0 4px 16px rgba(0, 90, 156, 0.08);
}

.accordion-header-btn {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 22px;
  background-color: #f8fafc;
  border: none;
  cursor: pointer;
  text-align: left;
  transition: background-color 0.2s ease;
}

.accordion-header-btn:hover {
  background-color: #f1f5f9;
}

.card-open .accordion-header-btn {
  background-color: #f0f7ff;
}

.region-title-group {
  display: flex;
  align-items: center;
  gap: 12px;
}

.region-pin-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background-color: #005a9c;
  flex-shrink: 0;
}

.region-title-text {
  font-size: 16.5px;
  font-weight: 700;
  color: #0f172a;
}

.card-open .region-title-text {
  color: #005a9c;
}

.region-chevron {
  width: 24px;
  height: 24px;
  color: #64748b;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.25s ease;
}

.region-chevron svg { width: 20px; height: 20px; }
.region-chevron.rotate-open { transform: rotate(180deg); color: #005a9c; }

/* Panel Body */
.accordion-panel-body {
  border-top: 1px solid #e2e8f0;
  background-color: #ffffff;
}

.panel-inner {
  padding: 24px;
  display: grid;
  grid-template-columns: 280px 1fr;
  gap: 28px;
}

.leader-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 10px;
  padding: 20px;
}

.leader-photo-wrap {
  width: 140px;
  height: 175px;
  border-radius: 8px;
  overflow: hidden;
  margin-bottom: 14px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

.leader-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.leader-name {
  margin: 0 0 6px 0;
  font-size: 17px;
  font-weight: 700;
  color: #0f172a;
}

.leader-pos {
  margin: 0 0 14px 0;
  font-size: 13.5px;
  color: #005a9c;
  font-weight: 600;
  line-height: 1.35;
}

.stat-pills {
  display: flex;
  flex-direction: column;
  gap: 6px;
  width: 100%;
}

.stat-badge {
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  padding: 5px 10px;
  border-radius: 6px;
  font-size: 12.5px;
  color: #475569;
}

.stat-badge strong {
  color: #005a9c;
}

/* Contacts Grid */
.contacts-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 14px;
}

.contact-card {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  background-color: #f8fafc;
  border: 1px solid #edf2f7;
  border-radius: 8px;
  padding: 14px;
}

.icon-circle {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background-color: #e0f2fe;
  color: #005a9c;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.icon-circle svg { width: 17px; height: 17px; }

.contact-info {
  display: flex;
  flex-direction: column;
  gap: 3px;
}

.contact-info .label {
  font-size: 11.5px;
  font-weight: 700;
  color: #64748b;
  text-transform: uppercase;
}

.contact-info .val {
  font-size: 14px;
  font-weight: 600;
  color: #1e293b;
  line-height: 1.4;
}

.contact-info .link {
  color: #005a9c;
  text-decoration: none;
}

.contact-info .link:hover {
  text-decoration: underline;
}

.highlight-rec {
  color: #059669;
  font-weight: 700;
  margin-top: 4px;
}

.no-results-box {
  padding: 32px;
  text-align: center;
  color: #64748b;
  background-color: #f8fafc;
  border-radius: 8px;
  border: 1px dashed #cbd5e1;
}

/* Panel Transition */
.panel-expand-enter-active,
.panel-expand-leave-active {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  max-height: 800px;
  opacity: 1;
  overflow: hidden;
}

.panel-expand-enter-from,
.panel-expand-leave-to {
  max-height: 0;
  opacity: 0;
}

/* Footer */
.paper-footer {
  margin-top: 48px;
  padding-top: 24px;
  border-top: 1px solid #f1f3f5;
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
}

.share-label { font-size: 14px; font-weight: 600; color: #64748b; }
.share-buttons { display: flex; align-items: center; gap: 8px; }

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

.share-btn svg { width: 18px; height: 18px; }
.share-tg:hover { background-color: #229ED9; border-color: #229ED9; color: #fff; transform: translateY(-2px); }
.share-fb:hover { background-color: #1877F2; border-color: #1877F2; color: #fff; transform: translateY(-2px); }
.share-tw:hover { background-color: #000000; border-color: #000000; color: #fff; transform: translateY(-2px); }
.share-copy:hover { background-color: #005a9c; border-color: #005a9c; color: #fff; transform: translateY(-2px); }

.copy-toast {
  font-size: 12px;
  font-weight: 600;
  color: #16a34a;
  background-color: #dcfce7;
  padding: 3px 8px;
  border-radius: 4px;
}

/* Sidebar */
.right-menu-sidebar { min-width: 0; }
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

.sidebar-heading { margin: 0; font-size: 16px; font-weight: 700; color: #0f172a; }
.sidebar-menu-list { list-style: none; padding: 0; margin: 0; }
.sidebar-menu-item { border-bottom: 1px solid #f1f5f9; }
.sidebar-menu-item:last-child { border-bottom: none; }

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

.sidebar-menu-link .chevron-icon { width: 16px; height: 16px; color: #94a3b8; transition: transform 0.2s ease; }
.sidebar-menu-link:hover .chevron-icon { color: #005a9c; transform: translateX(4px); }
.sidebar-menu-link.active .chevron-icon { color: #ffffff; transform: translateX(2px); }

@media (max-width: 1024px) {
  .page-layout { grid-template-columns: 1fr; }
  .content-paper { padding: 28px 24px; }
  .right-menu-sidebar { order: 2; }
  .sidebar-sticky-wrapper { position: static; }
  .panel-inner { grid-template-columns: 1fr; }
  .leader-section { max-width: 320px; margin: 0 auto; }
}

@media (max-width: 640px) {
  .regional-page-wrapper { padding: 16px 0 40px 0; }
  .content-paper { padding: 20px 16px; border-radius: 8px; }
  .paper-header { flex-direction: column; align-items: flex-start; }
  .page-title { font-size: 22px; }
  .contacts-grid { grid-template-columns: 1fr; }
}

@media print {
  .regional-page-wrapper { background: none; padding: 0; }
  .page-layout { display: block; }
  .right-menu-sidebar, .header-tools, .search-filter-wrapper, .paper-footer, .breadcrumbs { display: none !important; }
  .content-paper { border: none; box-shadow: none; padding: 0; }
  .accordion-panel-body { display: block !important; }
}
</style>
