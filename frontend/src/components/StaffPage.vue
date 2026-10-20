<script setup>
import { ref, onMounted, watch } from 'vue'
import staffData from '../mock/staff.json'

const emit = defineEmits(['navigate'])

// Active department accordion state
const activeDepartmentId = ref(1321)

// Toggle for sub-departments (Boshqarmalar / Bo'limlar child list)
const openSubdivisions = ref({
  7057: true, // Boshqarmalar open by default
  7060: true  // Bo'limlar open by default
})

const toggleSubdivision = (deptId) => {
  openSubdivisions.value[deptId] = !openSubdivisions.value[deptId]
}

const toggleDepartment = (id) => {
  if (activeDepartmentId.value === id) {
    activeDepartmentId.value = null
  } else {
    activeDepartmentId.value = id
    // update url hash parameter without reload
    window.location.hash = `#madaniyat/departments/staff?active=${id}`
  }
}

// Read active id from current URL on load or hash change
const syncFromHash = () => {
  const hash = window.location.hash
  if (hash.includes('active=')) {
    const match = hash.match(/active=(\d+)/)
    if (match && match[1]) {
      activeDepartmentId.value = parseInt(match[1], 10)
    }
  }
}

onMounted(() => {
  syncFromHash()
  window.addEventListener('hashchange', syncFromHash)
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
  <div class="staff-page-wrapper">
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

                <span class="breadcrumb-current" aria-current="page">{{ staffData.title }}</span>
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
            <h1 class="page-title">{{ staffData.title }}</h1>

            <!-- Accordion of Departments -->
            <div class="departments-accordion">
              <div 
                v-for="dept in staffData.departments" 
                :key="dept.id" 
                class="accordion-item"
                :class="{ 'item-expanded': activeDepartmentId === dept.id }"
              >
                <!-- Accordion Header Control -->
                <button 
                  type="button" 
                  class="accordion-control" 
                  @click="toggleDepartment(dept.id)"
                  :aria-expanded="activeDepartmentId === dept.id"
                >
                  <span class="accordion-label">{{ dept.title }}</span>
                  <span class="accordion-chevron" :class="{ 'rotate-chevron': activeDepartmentId === dept.id }">
                    <svg viewBox="0 0 32 32" fill="currentColor">
                      <path d="M24.285 11.284 16 19.571l-8.285-8.288a1.01 1.01 0 1 0-1.429 1.43l8.999 9.002a1.01 1.01 0 0 0 1.428 0l8.999-9.002a1.01 1.01 0 1 0-1.427-1.429"/>
                    </svg>
                  </span>
                </button>

                <!-- Accordion Panel Body -->
                <transition name="accordion-collapse">
                  <div v-if="activeDepartmentId === dept.id" class="accordion-panel">
                    <div class="panel-content">
                      <!-- Primary Employees List -->
                      <div class="employees-list">
                        <div 
                          v-for="guide in dept.guides" 
                          :key="guide.id" 
                          class="employee-row"
                        >
                          <!-- Employee Photo -->
                          <div class="employee-photo-box">
                            <img 
                              :src="guide.image" 
                              :alt="guide.title" 
                              class="employee-img"
                              loading="lazy"
                            />
                          </div>

                          <!-- Employee Info -->
                          <div class="employee-meta-box">
                            <h3 class="employee-name">{{ guide.title }}</h3>
                            <p class="employee-role">{{ guide.position_title }}</p>

                            <!-- Contacts Row -->
                            <div class="employee-contacts-row">
                              <!-- Phone -->
                              <div class="contact-pill">
                                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                  <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
                                </svg>
                                <div>
                                  <span class="pill-label">Telefon:</span>
                                  <a :href="`tel:${guide.phone?.replace(/[^0-9+]/g, '')}`" class="pill-val">
                                    {{ guide.phone }}
                                  </a>
                                </div>
                              </div>

                              <!-- Email -->
                              <div class="contact-pill">
                                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                  <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                                  <polyline points="22,6 12,13 2,6"/>
                                </svg>
                                <div>
                                  <span class="pill-label">E-pochta:</span>
                                  <a :href="`mailto:${guide.email}`" class="pill-val">
                                    {{ guide.email }}
                                  </a>
                                </div>
                              </div>

                              <!-- Reception -->
                              <div class="contact-pill">
                                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                  <circle cx="12" cy="12" r="10"/>
                                  <polyline points="12 6 12 12 16 14"/>
                                </svg>
                                <div>
                                  <span class="pill-label">Qabul:</span>
                                  <span class="pill-val text-green">{{ guide.reception_day }}</span>
                                </div>
                              </div>

                              <!-- Address -->
                              <div class="contact-pill">
                                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                  <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                                  <circle cx="12" cy="10" r="3"/>
                                </svg>
                                <div>
                                  <span class="pill-label">Manzil:</span>
                                  <span class="pill-val">{{ guide.address }}</span>
                                </div>
                              </div>
                            </div>
                          </div>
                        </div>
                      </div>

                      <!-- Nested Subdivisions (Bo‘linmalar / Bo‘limlar) -->
                      <div v-if="dept.child && dept.child.length > 0" class="subdivisions-section">
                        <div class="subdivisions-toggle-bar">
                          <button 
                            type="button" 
                            class="subdivisions-toggle-btn" 
                            @click="toggleSubdivision(dept.id)"
                          >
                            <span>Tarkibiy bo‘linmalar ({{ dept.child.length }} ta)</span>
                            <svg 
                              class="sub-chevron" 
                              :class="{ 'rotate-sub': openSubdivisions[dept.id] }"
                              viewBox="0 0 24 24" 
                              fill="none" 
                              stroke="currentColor" 
                              stroke-width="2"
                            >
                              <polyline points="6 9 12 15 18 9"/>
                            </svg>
                          </button>
                        </div>

                        <!-- Subdivisions Grid -->
                        <transition name="accordion-collapse">
                          <div v-if="openSubdivisions[dept.id]" class="subdivisions-grid">
                            <div 
                              v-for="sub in dept.child" 
                              :key="sub.id" 
                              class="subdivision-card"
                            >
                              <div class="subdivision-header">
                                <span class="sub-dot"></span>
                                <h4 class="subdivision-title">{{ sub.title }}</h4>
                              </div>
                              <div class="subdivision-meta">
                                <p v-if="sub.head" class="sub-head">
                                  <strong>Boshliq:</strong> {{ sub.head }}
                                </p>
                                <div class="sub-links">
                                  <a v-if="sub.phone" :href="`tel:${sub.phone}`" class="sub-phone">
                                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                      <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
                                    </svg>
                                    <span>{{ sub.phone }}</span>
                                  </a>
                                  <a v-if="sub.email" :href="`mailto:${sub.email}`" class="sub-email">
                                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                      <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                                      <polyline points="22,6 12,13 2,6"/>
                                    </svg>
                                    <span>{{ sub.email }}</span>
                                  </a>
                                </div>
                              </div>
                            </div>
                          </div>
                        </transition>
                      </div>
                    </div>
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
                    <a :href="staffData.social_share.telegram" target="_blank" rel="noopener noreferrer" class="share-btn share-tg" title="Telegram" aria-label="Telegram">
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M19.2 4.4 2.9 10.7c-1.1.4-1.1 1.1-.2 1.3l4.1 1.3 1.6 4.8c.2.5.1.7.6.7.4 0 .6-.2.8-.4l2-2 4.2 3.1c.8.4 1.3.2 1.5-.7l2.8-13.1c.3-1.1-.4-1.7-1.1-1.3m-2.1 3-7.8 7.1-.3 3.3L7.4 13l9.2-5.8c.4-.3.8-.1.5.2"/>
                      </svg>
                    </a>
                    <a :href="staffData.social_share.facebook" target="_blank" rel="noopener noreferrer" class="share-btn share-fb" title="Facebook" aria-label="Facebook">
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
                      </svg>
                    </a>
                    <a :href="staffData.social_share.twitter" target="_blank" rel="noopener noreferrer" class="share-btn share-tw" title="X (Twitter)" aria-label="X (Twitter)">
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
                <li v-for="item in staffData.sidebar" :key="item.id" class="sidebar-menu-item">
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
.staff-page-wrapper {
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
  margin-bottom: 28px;
  font-size: 28px;
  font-weight: 700;
  color: #0f172a;
  text-transform: uppercase;
  letter-spacing: 0.3px;
}

/* Departments Accordion */
.departments-accordion {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.accordion-item {
  border: 1px solid #e2e8f0;
  border-radius: 10px;
  overflow: hidden;
  background-color: #ffffff;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.accordion-item.item-expanded {
  border-color: #005a9c;
  box-shadow: 0 4px 16px rgba(0, 90, 156, 0.08);
}

.accordion-control {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 24px;
  background-color: #f8fafc;
  border: none;
  cursor: pointer;
  text-align: left;
  transition: background-color 0.2s ease;
}

.accordion-control:hover {
  background-color: #f1f5f9;
}

.item-expanded .accordion-control {
  background-color: #f0f7ff;
}

.accordion-label {
  font-size: 17px;
  font-weight: 700;
  color: #0f172a;
}

.item-expanded .accordion-label {
  color: #005a9c;
}

.accordion-chevron {
  width: 24px;
  height: 24px;
  color: #64748b;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.25s ease;
}

.accordion-chevron svg {
  width: 20px;
  height: 20px;
}

.accordion-chevron.rotate-chevron {
  transform: rotate(180deg);
  color: #005a9c;
}

/* Accordion Panel */
.accordion-panel {
  border-top: 1px solid #e2e8f0;
  background-color: #ffffff;
}

.panel-content {
  padding: 24px;
}

.employees-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.employee-row {
  display: grid;
  grid-template-columns: 180px 1fr;
  gap: 24px;
  background-color: #f8fafc;
  border: 1px solid #edf2f7;
  border-radius: 10px;
  padding: 20px;
}

.employee-photo-box {
  display: flex;
  justify-content: center;
}

.employee-img {
  width: 100%;
  aspect-ratio: 4 / 5;
  object-fit: cover;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
  background-color: #f1f5f9;
}

.employee-meta-box {
  display: flex;
  flex-direction: column;
}

.employee-name {
  margin: 0 0 6px 0;
  font-size: 20px;
  font-weight: 700;
  color: #0f172a;
}

.employee-role {
  margin: 0 0 16px 0;
  font-size: 14.5px;
  font-weight: 600;
  color: #005a9c;
  line-height: 1.4;
}

.employee-contacts-row {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 12px;
}

.contact-pill {
  display: flex;
  align-items: flex-start;
  gap: 10px;
  background-color: #ffffff;
  padding: 10px 14px;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.contact-pill svg {
  width: 16px;
  height: 16px;
  color: #005a9c;
  margin-top: 2px;
  flex-shrink: 0;
}

.pill-label {
  display: block;
  font-size: 11.5px;
  font-weight: 600;
  color: #64748b;
  text-transform: uppercase;
}

.pill-val {
  display: block;
  font-size: 13.5px;
  font-weight: 600;
  color: #1e293b;
  text-decoration: none;
}

.pill-val:hover {
  color: #005a9c;
}

.text-green {
  color: #059669 !important;
}

/* Subdivisions */
.subdivisions-section {
  margin-top: 24px;
  padding-top: 20px;
  border-top: 1px dashed #cbd5e1;
}

.subdivisions-toggle-bar {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 16px;
}

.subdivisions-toggle-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  font-size: 13.5px;
  font-weight: 600;
  color: #005a9c;
  background-color: #f0f7ff;
  border: 1px solid #bae6fd;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.subdivisions-toggle-btn:hover {
  background-color: #005a9c;
  color: #ffffff;
}

.sub-chevron {
  width: 15px;
  height: 15px;
  transition: transform 0.25s ease;
}

.sub-chevron.rotate-sub {
  transform: rotate(180deg);
}

.subdivisions-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 16px;
}

.subdivision-card {
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: transform 0.2s ease, border-color 0.2s ease;
}

.subdivision-card:hover {
  transform: translateY(-2px);
  border-color: #005a9c;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

.subdivision-header {
  display: flex;
  align-items: flex-start;
  gap: 10px;
  margin-bottom: 10px;
}

.sub-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: #005a9c;
  margin-top: 6px;
  flex-shrink: 0;
}

.subdivision-title {
  margin: 0;
  font-size: 14.5px;
  font-weight: 700;
  color: #0f172a;
  line-height: 1.35;
}

.sub-head {
  margin: 0 0 10px 0;
  font-size: 13px;
  color: #475569;
}

.sub-links {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  font-size: 12.5px;
}

.sub-phone,
.sub-email {
  display: inline-flex;
  align-items: center;
  gap: 5px;
  color: #005a9c;
  text-decoration: none;
  font-weight: 600;
}

.sub-phone:hover,
.sub-email:hover {
  text-decoration: underline;
}

.sub-phone svg,
.sub-email svg {
  width: 14px;
  height: 14px;
}

/* Accordion Transition */
.accordion-collapse-enter-active,
.accordion-collapse-leave-active {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  max-height: 3000px;
  opacity: 1;
  overflow: hidden;
}

.accordion-collapse-enter-from,
.accordion-collapse-leave-to {
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
  .employee-row { grid-template-columns: 1fr; }
  .employee-photo-box { max-width: 200px; }
  .employee-contacts-row { grid-template-columns: 1fr; }
}

@media (max-width: 640px) {
  .staff-page-wrapper { padding: 16px 0 40px 0; }
  .content-paper { padding: 20px 16px; border-radius: 8px; }
  .paper-header { flex-direction: column; align-items: flex-start; }
  .page-title { font-size: 22px; }
  .accordion-control { padding: 14px 16px; }
  .panel-content { padding: 16px; }
}

@media print {
  .staff-page-wrapper { background: none; padding: 0; }
  .page-layout { display: block; }
  .right-menu-sidebar, .header-tools, .paper-footer, .breadcrumbs { display: none !important; }
  .content-paper { border: none; box-shadow: none; padding: 0; }
  .accordion-panel { display: block !important; }
}
</style>
