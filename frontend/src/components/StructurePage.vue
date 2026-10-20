<script setup>
import { ref } from 'vue'
import structureData from '../mock/structure.json'

const emit = defineEmits(['navigate'])

// Image Zoom State
const zoomLevel = ref(1)
const isModalOpen = ref(false)

const zoomIn = () => {
  if (zoomLevel.value < 2.5) {
    zoomLevel.value = +(zoomLevel.value + 0.25).toFixed(2)
  }
}

const zoomOut = () => {
  if (zoomLevel.value > 0.75) {
    zoomLevel.value = +(zoomLevel.value - 0.25).toFixed(2)
  }
}

const resetZoom = () => {
  zoomLevel.value = 1
}

const openModal = () => {
  isModalOpen.value = true
}

const closeModal = () => {
  isModalOpen.value = false
}

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
  <div class="structure-page-wrapper">
    <div class="container page-container">
      <div class="page-layout">
        <!-- Main Content Area -->
        <main class="main-content-area">
          <article class="content-paper" id="printable-article">
            <!-- Breadcrumbs & Header Tools -->
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

                <span class="breadcrumb-current" aria-current="page">{{ structureData.title }}</span>
              </nav>

              <!-- Header Tools -->
              <div class="header-tools">
                <span class="age-limit-badge">{{ structureData.age_limit }}</span>
                <span class="tools-divider" aria-hidden="true"></span>
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
            <h1 class="page-title">{{ structureData.title }}</h1>

            <!-- Image Toolbar -->
            <div class="structure-toolbar">
              <div class="zoom-controls">
                <button 
                  type="button" 
                  class="zoom-btn" 
                  @click="zoomIn" 
                  :disabled="zoomLevel >= 2.5"
                  title="Kattalashtirish"
                >
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="11" cy="11" r="8"/>
                    <line x1="21" y1="21" x2="16.65" y2="16.65"/>
                    <line x1="11" y1="8" x2="11" y2="14"/>
                    <line x1="8" y1="11" x2="14" y2="11"/>
                  </svg>
                  <span>Kattalashtirish</span>
                </button>

                <button 
                  type="button" 
                  class="zoom-btn" 
                  @click="zoomOut" 
                  :disabled="zoomLevel <= 0.75"
                  title="Kichiklashtirish"
                >
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="11" cy="11" r="8"/>
                    <line x1="21" y1="21" x2="16.65" y2="16.65"/>
                    <line x1="8" y1="11" x2="14" y2="11"/>
                  </svg>
                  <span>Kichiklashtirish</span>
                </button>

                <button 
                  type="button" 
                  class="zoom-btn reset-btn" 
                  @click="resetZoom" 
                  :disabled="zoomLevel === 1"
                  title="Asl holat"
                >
                  <span>100%</span>
                </button>
              </div>

              <div class="action-controls">
                <button 
                  type="button" 
                  class="fullscreen-btn" 
                  @click="openModal" 
                  title="To‘liq ekranda ko‘rish"
                >
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M8 3H5a2 2 0 0 0-2 2v3m18 0V5a2 2 0 0 0-2-2h-3m0 18h3a2 2 0 0 0 2-2v-3M3 16v3a2 2 0 0 0 2 2h3"/>
                  </svg>
                  <span>To‘liq ekranda</span>
                </button>

                <a 
                  :href="structureData.image" 
                  target="_blank" 
                  download="vazirlik_tuzilmasi.jpg" 
                  class="download-btn"
                  title="Chizmani yuklab olish"
                >
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/>
                    <polyline points="7 10 12 15 17 10"/>
                    <line x1="12" y1="15" x2="12" y2="3"/>
                  </svg>
                  <span>Yuklab olish</span>
                </a>
              </div>
            </div>

            <!-- Structure Diagram Container -->
            <div class="structure-diagram-card">
              <div class="image-viewport">
                <img 
                  :src="structureData.image" 
                  alt="Madaniyat vazirligi tashkiliy tuzilmasi" 
                  class="structure-image"
                  :style="{ transform: `scale(${zoomLevel})` }"
                  @click="openModal"
                  loading="lazy"
                />
              </div>
              <p class="diagram-hint">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <circle cx="12" cy="12" r="10"/>
                  <line x1="12" y1="16" x2="12" y2="12"/>
                  <line x1="12" y1="8" x2="12.01" y2="8"/>
                </svg>
                <span>Chizmani kattalashtirish uchun uning ustiga bosing yoki asboblar panelidan foydalaning.</span>
              </p>
            </div>

            <!-- Article Footer -->
            <footer class="paper-footer">
              <div class="footer-top-row">
                <div class="social-share-group">
                  <span class="share-label">Ulashish:</span>
                  <div class="share-buttons">
                    <a :href="structureData.social_share.telegram" target="_blank" rel="noopener noreferrer" class="share-btn share-tg" title="Telegram" aria-label="Telegram">
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M19.2 4.4 2.9 10.7c-1.1.4-1.1 1.1-.2 1.3l4.1 1.3 1.6 4.8c.2.5.1.7.6.7.4 0 .6-.2.8-.4l2-2 4.2 3.1c.8.4 1.3.2 1.5-.7l2.8-13.1c.3-1.1-.4-1.7-1.1-1.3m-2.1 3-7.8 7.1-.3 3.3L7.4 13l9.2-5.8c.4-.3.8-.1.5.2"/>
                      </svg>
                    </a>
                    <a :href="structureData.social_share.facebook" target="_blank" rel="noopener noreferrer" class="share-btn share-fb" title="Facebook" aria-label="Facebook">
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
                      </svg>
                    </a>
                    <a :href="structureData.social_share.twitter" target="_blank" rel="noopener noreferrer" class="share-btn share-tw" title="X (Twitter)" aria-label="X (Twitter)">
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

                <div class="views-counter" title="Ko‘rishlar soni">
                  <svg viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12 4.5C7 4.5 2.73 7.61 1 12c1.73 4.39 6 7.5 11 7.5s9.27-3.11 11-7.5c-1.73-4.39-6-7.5-11-7.5zM12 17c-2.76 0-5-2.24-5-5s2.24-5 5-5 5 2.24 5 5-2.24 5-5 5zm0-8c-1.66 0-3 1.34-3 3s1.34 3 3 3 3-1.34 3-3-1.34-3-3-3z"/>
                  </svg>
                  <span>{{ Number(structureData.views).toLocaleString('ru-RU') }}</span>
                </div>
              </div>

              <div class="footer-bottom-row">
                <div class="updated-date">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="12" cy="12" r="10"/>
                    <polyline points="12 6 12 12 16 14"/>
                  </svg>
                  <span>Oxirgi yangilanish: {{ structureData.updated_date }}</span>
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
                <li v-for="item in structureData.sidebar" :key="item.id" class="sidebar-menu-item">
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

    <!-- Fullscreen Modal for High-Res Diagram View -->
    <div v-if="isModalOpen" class="fullscreen-modal-backdrop" @click.self="closeModal">
      <div class="modal-dialog">
        <div class="modal-header">
          <h3 class="modal-title">{{ structureData.title }}</h3>
          <button type="button" class="modal-close-btn" @click="closeModal" aria-label="Yopish">
            ✕
          </button>
        </div>
        <div class="modal-body">
          <img :src="structureData.image" alt="Madaniyat vazirligi tuzilmasi" class="modal-image" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.structure-page-wrapper {
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

/* Header Tools */
.header-tools {
  display: flex;
  align-items: center;
  gap: 12px;
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

.page-title {
  margin-top: 24px;
  margin-bottom: 24px;
  font-size: 28px;
  font-weight: 700;
  color: #0f172a;
  text-transform: uppercase;
  letter-spacing: 0.3px;
}

/* Structure Toolbar */
.structure-toolbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  background-color: #f8fafc;
  padding: 12px 18px;
  border-radius: 10px;
  border: 1px solid #e2e8f0;
  margin-bottom: 24px;
  flex-wrap: wrap;
}

.zoom-controls,
.action-controls {
  display: flex;
  align-items: center;
  gap: 10px;
}

.zoom-btn,
.fullscreen-btn,
.download-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 8px 14px;
  font-size: 13.5px;
  font-weight: 600;
  color: #334155;
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  cursor: pointer;
  text-decoration: none;
  transition: all 0.2s ease;
}

.zoom-btn svg,
.fullscreen-btn svg,
.download-btn svg {
  width: 16px;
  height: 16px;
  color: #005a9c;
}

.zoom-btn:hover:not(:disabled),
.fullscreen-btn:hover,
.download-btn:hover {
  background-color: #005a9c;
  color: #ffffff;
  border-color: #005a9c;
}

.zoom-btn:hover:not(:disabled) svg,
.fullscreen-btn:hover svg,
.download-btn:hover svg {
  color: #ffffff;
}

.zoom-btn:disabled {
  opacity: 0.45;
  cursor: not-allowed;
}

.reset-btn {
  font-weight: 700;
  min-width: 60px;
  justify-content: center;
}

/* Diagram Card */
.structure-diagram-card {
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  padding: 24px;
  background-color: #ffffff;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.03);
}

.image-viewport {
  overflow: auto;
  max-height: 75vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  border-radius: 8px;
  background-color: #fafbfc;
  padding: 16px;
}

.structure-image {
  max-width: 100%;
  height: auto;
  transition: transform 0.2s cubic-bezier(0.2, 0, 0, 1);
  transform-origin: top center;
  cursor: zoom-in;
  border-radius: 6px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
}

.diagram-hint {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13.5px;
  color: #64748b;
  margin-top: 16px;
}

.diagram-hint svg {
  width: 16px;
  height: 16px;
  color: #005a9c;
  flex-shrink: 0;
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

/* Sidebar */
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
  transition: transform 0.2s ease;
}

.sidebar-menu-link:hover .chevron-icon {
  color: #005a9c;
  transform: translateX(4px);
}

.sidebar-menu-link.active .chevron-icon {
  color: #ffffff;
  transform: translateX(2px);
}

/* Modal */
.fullscreen-modal-backdrop {
  position: fixed;
  inset: 0;
  background-color: rgba(15, 23, 42, 0.85);
  backdrop-filter: blur(8px);
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 24px;
}

.modal-dialog {
  background-color: #ffffff;
  border-radius: 12px;
  max-width: 95vw;
  max-height: 95vh;
  width: 1400px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 24px;
  border-bottom: 1px solid #e2e8f0;
}

.modal-title {
  margin: 0;
  font-size: 18px;
  font-weight: 700;
  color: #0f172a;
}

.modal-close-btn {
  background: none;
  border: none;
  font-size: 18px;
  color: #64748b;
  cursor: pointer;
  padding: 6px;
  border-radius: 6px;
}

.modal-close-btn:hover {
  background-color: #f1f5f9;
  color: #0f172a;
}

.modal-body {
  overflow: auto;
  padding: 24px;
  display: flex;
  justify-content: center;
  background-color: #f8fafc;
}

.modal-image {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
}

@media (max-width: 1024px) {
  .page-layout { grid-template-columns: 1fr; }
  .content-paper { padding: 28px 24px; }
  .right-menu-sidebar { order: 2; }
  .sidebar-sticky-wrapper { position: static; }
}

@media (max-width: 640px) {
  .structure-page-wrapper { padding: 16px 0 40px 0; }
  .content-paper { padding: 20px 16px; border-radius: 8px; }
  .paper-header { flex-direction: column; align-items: flex-start; }
  .page-title { font-size: 22px; }
  .structure-toolbar { flex-direction: column; align-items: stretch; }
  .zoom-controls, .action-controls { justify-content: space-between; width: 100%; }
}

@media print {
  .structure-page-wrapper { background: none; padding: 0; }
  .page-layout { display: block; }
  .right-menu-sidebar, .header-tools, .structure-toolbar, .diagram-hint, .paper-footer, .breadcrumbs { display: none !important; }
  .content-paper { border: none; box-shadow: none; padding: 0; }
}
</style>
