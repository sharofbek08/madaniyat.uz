<script setup>
import { ref } from 'vue'
import guidesData from '../mock/guides.json'

const emit = defineEmits(['navigate'])

// State for active expanded biography per guide
const expandedBio = ref({
  931: true // Nazarbekov open by default
})

const toggleBio = (id) => {
  expandedBio.value[id] = !expandedBio.value[id]
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
  <div class="guides-page-wrapper">
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

                <span class="breadcrumb-current" aria-current="page">{{ guidesData.title }}</span>
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
            <h1 class="page-title">{{ guidesData.title }}</h1>

            <!-- Leadership Profiles List -->
            <div class="guides-list">
              <div 
                v-for="guide in guidesData.guides" 
                :key="guide.id" 
                class="employee-card"
                :id="`guide-${guide.id}`"
              >
                <!-- Top Section: Photo + Core Info + Contact Grid -->
                <div class="card-main">
                  <!-- Person Photo -->
                  <div class="photo-wrapper">
                    <img 
                      :src="guide.image" 
                      :alt="guide.title" 
                      class="person-photo"
                      loading="lazy"
                    />
                    <div class="photo-badge">{{ guide.department }}</div>
                  </div>

                  <!-- Details Area -->
                  <div class="details-area">
                    <h2 class="person-name">{{ guide.title }}</h2>
                    <p class="person-position">{{ guide.position_title }}</p>

                    <!-- Contact Details Grid -->
                    <div class="contacts-grid">
                      <!-- Phone -->
                      <div class="contact-box">
                        <div class="contact-icon-bubble">
                          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                            <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
                          </svg>
                        </div>
                        <div class="contact-text">
                          <span class="contact-label">Telefon:</span>
                          <a :href="`tel:${guide.phone?.replace(/[^0-9+]/g, '')}`" class="contact-val phone-link">
                            {{ guide.phone }}
                          </a>
                        </div>
                      </div>

                      <!-- Email -->
                      <div class="contact-box">
                        <div class="contact-icon-bubble">
                          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                            <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                            <polyline points="22,6 12,13 2,6"/>
                          </svg>
                        </div>
                        <div class="contact-text">
                          <span class="contact-label">Elektron pochta:</span>
                          <a :href="`mailto:${guide.email}`" class="contact-val email-link">
                            {{ guide.email }}
                          </a>
                        </div>
                      </div>

                      <!-- Reception Days -->
                      <div class="contact-box">
                        <div class="contact-icon-bubble">
                          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                            <circle cx="12" cy="12" r="10"/>
                            <polyline points="12 6 12 12 16 14"/>
                          </svg>
                        </div>
                        <div class="contact-text">
                          <span class="contact-label">Qabul kunlari:</span>
                          <span class="contact-val highlight-time">{{ guide.reception_day }}</span>
                        </div>
                      </div>

                      <!-- Address -->
                      <div class="contact-box">
                        <div class="contact-icon-bubble">
                          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                            <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                            <circle cx="12" cy="10" r="3"/>
                          </svg>
                        </div>
                        <div class="contact-text">
                          <span class="contact-label">Manzil:</span>
                          <span class="contact-val">{{ guide.address }}</span>
                        </div>
                      </div>
                    </div>

                    <!-- Toggle Biography Button -->
                    <div class="bio-toggle-row">
                      <button 
                        type="button" 
                        class="bio-toggle-btn" 
                        @click="toggleBio(guide.id)"
                        :aria-expanded="expandedBio[guide.id]"
                      >
                        <span>Mehnat faoliyati va tarjimai holi</span>
                        <svg 
                          class="toggle-chevron" 
                          :class="{ 'rotate-open': expandedBio[guide.id] }"
                          viewBox="0 0 24 24" 
                          fill="none" 
                          stroke="currentColor" 
                          stroke-width="2"
                        >
                          <polyline points="6 9 12 15 18 9"/>
                        </svg>
                      </button>
                    </div>
                  </div>
                </div>

                <!-- Collapsible Biography Content -->
                <transition name="accordion-slide">
                  <div v-if="expandedBio[guide.id]" class="bio-accordion-content">
                    <div class="bio-header-badge">
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
                        <polyline points="14 2 14 8 20 8"/>
                        <line x1="16" y1="13" x2="8" y2="13"/>
                        <line x1="16" y1="17" x2="8" y2="17"/>
                        <polyline points="10 9 9 9 8 9"/>
                      </svg>
                      <span>Biografik ma’lumotlar va mehnat faoliyati</span>
                    </div>

                    <div 
                      class="bio-body-html" 
                      v-html="guide.biography_html"
                    ></div>
                  </div>
                </transition>
              </div>
            </div>

            <!-- Footer -->
            <footer class="paper-footer">
              <div class="footer-top-row">
                <div class="social-share-group">
                  <span class="share-label">Ulashish:</span>
                  <div class="share-buttons">
                    <a :href="guidesData.social_share.telegram" target="_blank" rel="noopener noreferrer" class="share-btn share-tg" title="Telegram" aria-label="Telegram">
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M19.2 4.4 2.9 10.7c-1.1.4-1.1 1.1-.2 1.3l4.1 1.3 1.6 4.8c.2.5.1.7.6.7.4 0 .6-.2.8-.4l2-2 4.2 3.1c.8.4 1.3.2 1.5-.7l2.8-13.1c.3-1.1-.4-1.7-1.1-1.3m-2.1 3-7.8 7.1-.3 3.3L7.4 13l9.2-5.8c.4-.3.8-.1.5.2"/>
                      </svg>
                    </a>
                    <a :href="guidesData.social_share.facebook" target="_blank" rel="noopener noreferrer" class="share-btn share-fb" title="Facebook" aria-label="Facebook">
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
                      </svg>
                    </a>
                    <a :href="guidesData.social_share.twitter" target="_blank" rel="noopener noreferrer" class="share-btn share-tw" title="X (Twitter)" aria-label="X (Twitter)">
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

                <div class="updated-date">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="12" cy="12" r="10"/>
                    <polyline points="12 6 12 12 16 14"/>
                  </svg>
                  <span>Oxirgi yangilanish: {{ guidesData.updated_date }}</span>
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
                <li v-for="item in guidesData.sidebar" :key="item.id" class="sidebar-menu-item">
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
.guides-page-wrapper {
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

.header-tools {
  display: flex;
  align-items: center;
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
  margin-bottom: 28px;
  font-size: 28px;
  font-weight: 700;
  color: #0f172a;
  text-transform: uppercase;
  letter-spacing: 0.3px;
}

/* Guides List */
.guides-list {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.employee-card {
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.03);
  transition: box-shadow 0.2s ease, border-color 0.2s ease;
}

.employee-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.06);
}

.card-main {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 28px;
  padding: 28px;
}

.photo-wrapper {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.person-photo {
  width: 100%;
  aspect-ratio: 4 / 5;
  object-fit: cover;
  border-radius: 10px;
  border: 1px solid #e2e8f0;
  background-color: #f1f5f9;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

.photo-badge {
  position: absolute;
  bottom: -10px;
  background: #005a9c;
  color: #ffffff;
  font-size: 11.5px;
  font-weight: 700;
  padding: 3px 12px;
  border-radius: 20px;
  letter-spacing: 0.5px;
  text-transform: uppercase;
  box-shadow: 0 2px 6px rgba(0, 90, 156, 0.3);
}

.details-area {
  display: flex;
  flex-direction: column;
}

.person-name {
  margin: 0 0 6px 0;
  font-size: 22px;
  font-weight: 700;
  color: #0f172a;
  line-height: 1.3;
}

.person-position {
  margin: 0 0 20px 0;
  font-size: 15px;
  font-weight: 500;
  color: #005a9c;
  line-height: 1.4;
}

/* Contacts Grid */
.contacts-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
  background-color: #f8fafc;
  padding: 18px;
  border-radius: 10px;
  border: 1px solid #edf2f7;
  margin-bottom: 20px;
}

.contact-box {
  display: flex;
  align-items: flex-start;
  gap: 12px;
}

.contact-icon-bubble {
  width: 34px;
  height: 34px;
  border-radius: 8px;
  background-color: #e0f2fe;
  color: #005a9c;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.contact-icon-bubble svg {
  width: 18px;
  height: 18px;
}

.contact-text {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.contact-label {
  font-size: 12px;
  font-weight: 600;
  color: #64748b;
  text-transform: uppercase;
  letter-spacing: 0.4px;
}

.contact-val {
  font-size: 14.5px;
  font-weight: 600;
  color: #1e293b;
}

.phone-link,
.email-link {
  color: #005a9c;
  text-decoration: none;
  transition: color 0.2s ease;
}

.phone-link:hover,
.email-link:hover {
  text-decoration: underline;
  color: #0369a1;
}

.highlight-time {
  color: #047857;
}

/* Bio Toggle Row */
.bio-toggle-row {
  margin-top: auto;
  display: flex;
  justify-content: flex-end;
}

.bio-toggle-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 10px 18px;
  font-size: 14px;
  font-weight: 600;
  color: #005a9c;
  background-color: #f0f7ff;
  border: 1px solid #bae6fd;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.bio-toggle-btn:hover {
  background-color: #005a9c;
  color: #ffffff;
  border-color: #005a9c;
}

.toggle-chevron {
  width: 16px;
  height: 16px;
  transition: transform 0.25s ease;
}

.toggle-chevron.rotate-open {
  transform: rotate(180deg);
}

/* Accordion Bio Content */
.bio-accordion-content {
  border-top: 1px solid #e2e8f0;
  background-color: #fafbfc;
  padding: 24px 28px;
}

.bio-header-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background-color: #e2e8f0;
  color: #334155;
  font-size: 13px;
  font-weight: 700;
  padding: 6px 14px;
  border-radius: 6px;
  margin-bottom: 18px;
}

.bio-header-badge svg {
  width: 16px;
  height: 16px;
  color: #005a9c;
}

.bio-body-html {
  font-size: 15px;
  line-height: 1.7;
  color: #334155;
}

.bio-body-html :deep(p) {
  margin-bottom: 12px;
  text-align: justify;
}

.bio-body-html :deep(p strong) {
  color: #0f172a;
  font-weight: 700;
}

/* Accordion Animation */
.accordion-slide-enter-active,
.accordion-slide-leave-active {
  transition: all 0.3s ease-in-out;
  max-height: 2000px;
  opacity: 1;
  overflow: hidden;
}

.accordion-slide-enter-from,
.accordion-slide-leave-to {
  max-height: 0;
  opacity: 0;
  padding-top: 0;
  padding-bottom: 0;
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

.updated-date {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13.5px;
  color: #64748b;
}

.updated-date svg { width: 16px; height: 16px; color: #94a3b8; }

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
  .card-main { grid-template-columns: 1fr; }
  .photo-wrapper { max-width: 260px; margin: 0 auto; }
}

@media (max-width: 640px) {
  .guides-page-wrapper { padding: 16px 0 40px 0; }
  .content-paper { padding: 20px 16px; border-radius: 8px; }
  .paper-header { flex-direction: column; align-items: flex-start; }
  .page-title { font-size: 22px; }
  .contacts-grid { grid-template-columns: 1fr; }
  .card-main { padding: 16px; }
  .bio-accordion-content { padding: 16px; }
}

@media print {
  .guides-page-wrapper { background: none; padding: 0; }
  .page-layout { display: block; }
  .right-menu-sidebar, .header-tools, .paper-footer, .breadcrumbs, .bio-toggle-row { display: none !important; }
  .content-paper { border: none; box-shadow: none; padding: 0; }
  .bio-accordion-content { display: block !important; }
}
</style>
