<script setup>
import { ref, computed } from 'vue'
import vacanciesData from '../mock/vacancies.json'

const emit = defineEmits(['navigate'])

// Filter State
const searchQuery = ref('')
const selectedDepartment = ref('all')

const departments = computed(() => {
  const depts = new Set(vacanciesData.vacancies.map(v => v.department))
  return ['all', ...Array.from(depts)]
})

const filteredVacancies = computed(() => {
  return vacanciesData.vacancies.filter(v => {
    const matchesDept = selectedDepartment.value === 'all' || v.department === selectedDepartment.value
    const q = searchQuery.value.trim().toLowerCase()
    const matchesSearch = !q || 
      v.title.toLowerCase().includes(q) || 
      v.department.toLowerCase().includes(q) ||
      v.requirements.some(r => r.toLowerCase().includes(q))
    return matchesDept && matchesSearch
  })
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
  <div class="vacancies-page-wrapper">
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

                <span class="breadcrumb-current" aria-current="page">{{ vacanciesData.title }}</span>
              </nav>

              <!-- Header Tools -->
              <div class="header-tools">
                <span class="age-limit-badge">{{ vacanciesData.age_limit }}</span>
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
            <h1 class="page-title">{{ vacanciesData.title }}</h1>

            <!-- Official Gov.uz Announcement Banner -->
            <div class="official-announcement-card">
              <div class="announcement-icon-badge">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/>
                </svg>
              </div>
              <div class="announcement-text-area">
                <h3 class="announcement-heading">Hurmatli fuqaro!</h3>
                <p class="announcement-desc">
                  O‘zbekiston Respublikasi Prezidentining 2019-yil 3-oktabrdagi “O‘zbekiston Respublikasida kadrlar siyosati va davlat fuqarolik xizmati tizimini tubdan takomillashtirish chora-tadbirlari to‘g‘risida”gi <strong>PF-5843-sonli</strong> Farmoniga muvofiq, O‘zbekiston Respublikasi Madaniyat vazirligi tomonidan tasdiqlangan Tartibga asosan vazirlik tizimidagi vakant lavozimlarga kadrlar <strong>Davlat fuqarolik xizmatchilari vakant lavozimlarining yagona ochiq portali (<a href="https://vacancy.argos.uz" target="_blank" rel="noopener">vacancy.argos.uz</a>)</strong> orqali ochiq tanlov asosida ishga qabul qilinadi.
                </p>
                <div class="announcement-actions">
                  <a 
                    :href="vacanciesData.argos_url" 
                    target="_blank" 
                    rel="noopener noreferrer" 
                    class="argos-btn"
                  >
                    <span>Vazirlik tizimidagi vakant lavozimlarga o‘tish (vacancy.argos.uz)</span>
                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                      <line x1="7" y1="17" x2="17" y2="7"/>
                      <polyline points="7 7 17 7 17 17"/>
                    </svg>
                  </a>
                </div>
              </div>
            </div>

            <!-- Search & Filter Controls -->
            <div class="filters-row">
              <div class="search-input-box">
                <svg class="search-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <circle cx="11" cy="11" r="8"/>
                  <line x1="21" y1="21" x2="16.65" y2="16.65"/>
                </svg>
                <input 
                  type="text" 
                  v-model="searchQuery" 
                  placeholder="Lavozim yoki yo‘nalish bo‘yicha qidirish..." 
                  class="search-input"
                />
              </div>

              <!-- Department Filter Select -->
              <div class="dept-select-box">
                <select v-model="selectedDepartment" class="dept-select">
                  <option value="all">Barcha boshqarmalar</option>
                  <option 
                    v-for="d in departments.filter(x => x !== 'all')" 
                    :key="d" 
                    :value="d"
                  >
                    {{ d }}
                  </option>
                </select>
              </div>
            </div>

            <!-- Active Vacancies Cards List -->
            <div class="vacancies-list">
              <div 
                v-for="item in filteredVacancies" 
                :key="item.id" 
                class="vacancy-card"
              >
                <!-- Card Header -->
                <div class="vacancy-card-header">
                  <div>
                    <span class="dept-tag">{{ item.department }}</span>
                    <h2 class="vacancy-title">{{ item.title }}</h2>
                  </div>
                  <div class="salary-box">
                    <span class="salary-label">Oylik maosh</span>
                    <span class="salary-val">{{ item.salary }}</span>
                  </div>
                </div>

                <!-- Info Grid -->
                <div class="vacancy-info-grid">
                  <div class="info-item">
                    <span class="info-label">Bandlik turi:</span>
                    <span class="info-val">{{ item.type }}</span>
                  </div>
                  <div class="info-item">
                    <span class="info-label">Talab etiladigan tajriba:</span>
                    <span class="info-val">{{ item.experience }}</span>
                  </div>
                  <div class="info-item">
                    <span class="info-label">Ma'lumoti:</span>
                    <span class="info-val">{{ item.education }}</span>
                  </div>
                  <div class="info-item">
                    <span class="info-label">Ariza muddati:</span>
                    <span class="info-val deadline-text">{{ item.deadline }} gacha</span>
                  </div>
                </div>

                <!-- Requirements List -->
                <div class="requirements-box">
                  <h3 class="req-title">Asosiy talablar:</h3>
                  <ul class="req-list">
                    <li v-for="(req, i) in item.requirements" :key="i">{{ req }}</li>
                  </ul>
                </div>

                <!-- Card Actions -->
                <div class="vacancy-card-footer">
                  <a 
                    :href="item.apply_url" 
                    target="_blank" 
                    rel="noopener noreferrer" 
                    class="apply-btn"
                  >
                    <span>Ariza topshirish</span>
                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                      <line x1="5" y1="12" x2="19" y2="12"/>
                      <polyline points="12 5 19 12 12 19"/>
                    </svg>
                  </a>
                </div>
              </div>

              <!-- Empty Results -->
              <div v-if="filteredVacancies.length === 0" class="no-results">
                <p>Tanlangan mezonlar bo‘yicha hozircha ochiq vakansiya topilmadi.</p>
              </div>
            </div>

            <!-- Footer -->
            <footer class="paper-footer">
              <div class="footer-top-row">
                <div class="social-share-group">
                  <span class="share-label">Ulashish:</span>
                  <div class="share-buttons">
                    <a :href="vacanciesData.social_share.telegram" target="_blank" rel="noopener noreferrer" class="share-btn share-tg" title="Telegram" aria-label="Telegram">
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M19.2 4.4 2.9 10.7c-1.1.4-1.1 1.1-.2 1.3l4.1 1.3 1.6 4.8c.2.5.1.7.6.7.4 0 .6-.2.8-.4l2-2 4.2 3.1c.8.4 1.3.2 1.5-.7l2.8-13.1c.3-1.1-.4-1.7-1.1-1.3m-2.1 3-7.8 7.1-.3 3.3L7.4 13l9.2-5.8c.4-.3.8-.1.5.2"/>
                      </svg>
                    </a>
                    <a :href="vacanciesData.social_share.facebook" target="_blank" rel="noopener noreferrer" class="share-btn share-fb" title="Facebook" aria-label="Facebook">
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
                      </svg>
                    </a>
                    <a :href="vacanciesData.social_share.twitter" target="_blank" rel="noopener noreferrer" class="share-btn share-tw" title="X (Twitter)" aria-label="X (Twitter)">
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
                  <span>{{ Number(vacanciesData.views).toLocaleString('ru-RU') }}</span>
                </div>
              </div>

              <div class="footer-bottom-row">
                <div class="updated-date">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="12" cy="12" r="10"/>
                    <polyline points="12 6 12 12 16 14"/>
                  </svg>
                  <span>Oxirgi yangilanish: {{ vacanciesData.updated_date }}</span>
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
                <li v-for="item in vacanciesData.sidebar" :key="item.id" class="sidebar-menu-item">
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
.vacancies-page-wrapper {
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

.breadcrumb-link:hover { color: #005a9c; text-decoration: underline; }
.breadcrumb-link.active-link { color: #1e293b; font-weight: 600; }
.home-icon { width: 16px; height: 16px; }
.breadcrumb-separator { display: inline-flex; align-items: center; color: #94a3b8; }
.breadcrumb-separator svg { width: 14px; height: 14px; }
.breadcrumb-current { color: #005a9c; font-weight: 600; }

.header-tools { display: flex; align-items: center; gap: 12px; }
.age-limit-badge {
  font-size: 12px;
  font-weight: 700;
  color: #475569;
  background-color: #f8fafc;
  border: 1px solid #cbd5e1;
  border-radius: 4px;
  padding: 3px 8px;
}
.tools-divider { width: 1px; height: 18px; background-color: #e2e8f0; }
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

/* Official Announcement Card */
.official-announcement-card {
  display: flex;
  gap: 20px;
  background: linear-gradient(135deg, #f0f7ff 0%, #e0f2fe 100%);
  border: 1px solid #bae6fd;
  border-radius: 12px;
  padding: 24px 28px;
  margin-bottom: 32px;
}

.announcement-icon-badge {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background-color: #005a9c;
  color: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  box-shadow: 0 4px 12px rgba(0, 90, 156, 0.25);
}

.announcement-icon-badge svg { width: 24px; height: 24px; }

.announcement-text-area { flex: 1; }
.announcement-heading { margin: 0 0 10px 0; font-size: 19px; font-weight: 700; color: #0f172a; }
.announcement-desc { margin: 0 0 18px 0; font-size: 15px; line-height: 1.65; color: #334155; }
.announcement-desc strong { color: #0f172a; }
.announcement-desc a { color: #005a9c; font-weight: 600; text-decoration: underline; }

.argos-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 12px 22px;
  font-size: 14.5px;
  font-weight: 700;
  color: #ffffff;
  background-color: #005a9c;
  border-radius: 8px;
  text-decoration: none;
  box-shadow: 0 4px 12px rgba(0, 90, 156, 0.25);
  transition: all 0.2s ease;
}

.argos-btn svg { width: 18px; height: 18px; }
.argos-btn:hover { background-color: #004578; transform: translateY(-2px); box-shadow: 0 6px 16px rgba(0, 90, 156, 0.35); }

/* Filters Row */
.filters-row {
  display: flex;
  gap: 16px;
  margin-bottom: 28px;
  flex-wrap: wrap;
}

.search-input-box {
  position: relative;
  flex: 1;
  min-width: 280px;
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

.search-input {
  width: 100%;
  padding: 11px 16px 11px 44px;
  font-size: 14.5px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  background-color: #f8fafc;
  color: #0f172a;
  transition: all 0.2s ease;
}

.search-input:focus {
  outline: none;
  background-color: #ffffff;
  border-color: #005a9c;
  box-shadow: 0 0 0 3px rgba(0, 90, 156, 0.12);
}

.dept-select-box { min-width: 240px; }
.dept-select {
  width: 100%;
  padding: 11px 16px;
  font-size: 14px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  background-color: #f8fafc;
  color: #0f172a;
  cursor: pointer;
}

.dept-select:focus { outline: none; border-color: #005a9c; }

/* Vacancies Cards */
.vacancies-list { display: flex; flex-direction: column; gap: 20px; }

.vacancy-card {
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  padding: 24px 28px;
  background-color: #ffffff;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.03);
  transition: all 0.2s ease;
}

.vacancy-card:hover {
  border-color: #005a9c;
  box-shadow: 0 6px 20px rgba(0, 90, 156, 0.08);
}

.vacancy-card-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 20px;
  margin-bottom: 20px;
}

.dept-tag {
  display: inline-block;
  background-color: #f1f5f9;
  color: #475569;
  font-size: 12px;
  font-weight: 700;
  padding: 4px 10px;
  border-radius: 4px;
  margin-bottom: 8px;
}

.vacancy-title { margin: 0; font-size: 20px; font-weight: 700; color: #0f172a; line-height: 1.35; }

.salary-box {
  text-align: right;
  flex-shrink: 0;
  background-color: #ecfdf5;
  border: 1px solid #a7f3d0;
  padding: 8px 16px;
  border-radius: 8px;
}

.salary-label { display: block; font-size: 11px; font-weight: 700; color: #065f46; text-transform: uppercase; }
.salary-val { display: block; font-size: 16px; font-weight: 700; color: #047857; }

.vacancy-info-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 14px;
  background-color: #f8fafc;
  border: 1px solid #edf2f7;
  padding: 14px 18px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.info-item { display: flex; flex-direction: column; gap: 2px; }
.info-label { font-size: 11.5px; font-weight: 600; color: #64748b; }
.info-val { font-size: 13.5px; font-weight: 600; color: #1e293b; }
.deadline-text { color: #b91c1c; }

.requirements-box { margin-bottom: 20px; }
.req-title { margin: 0 0 8px 0; font-size: 14px; font-weight: 700; color: #334155; }
.req-list { margin: 0; padding-left: 20px; color: #475569; font-size: 14px; line-height: 1.6; }

.vacancy-card-footer { display: flex; justify-content: flex-end; }
.apply-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 9px 20px;
  font-size: 14px;
  font-weight: 700;
  color: #005a9c;
  background-color: #f0f7ff;
  border: 1px solid #bae6fd;
  border-radius: 6px;
  text-decoration: none;
  transition: all 0.2s ease;
}

.apply-btn svg { width: 16px; height: 16px; }
.apply-btn:hover { background-color: #005a9c; color: #ffffff; border-color: #005a9c; }

.no-results {
  padding: 32px;
  text-align: center;
  color: #64748b;
  background-color: #f8fafc;
  border: 1px dashed #cbd5e1;
  border-radius: 8px;
}

/* Footer */
.paper-footer { margin-top: 48px; padding-top: 24px; border-top: 1px solid #f1f3f5; }
.footer-top-row { display: flex; align-items: center; justify-content: space-between; gap: 16px; flex-wrap: wrap; }
.social-share-group { display: flex; align-items: center; gap: 12px; }
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

.copy-toast { font-size: 12px; font-weight: 600; color: #16a34a; background-color: #dcfce7; padding: 3px 8px; border-radius: 4px; }
.views-counter { display: flex; align-items: center; gap: 8px; color: #64748b; font-size: 14.5px; font-weight: 500; }
.views-counter svg { width: 20px; height: 20px; color: #94a3b8; }
.footer-bottom-row { display: flex; align-items: center; margin-top: 12px; }
.updated-date { display: flex; align-items: center; gap: 8px; font-size: 13.5px; color: #64748b; }
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

.sidebar-header { padding: 16px 20px; background-color: #f8fafc; border-bottom: 1px solid #e2e8f0; }
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

.sidebar-menu-link:hover { background-color: #f1f5f9; color: #005a9c; padding-left: 24px; }
.sidebar-menu-link.active { background-color: #005a9c; color: #ffffff; font-weight: 600; }
.sidebar-menu-link .chevron-icon { width: 16px; height: 16px; color: #94a3b8; transition: transform 0.2s ease; }
.sidebar-menu-link:hover .chevron-icon { color: #005a9c; transform: translateX(4px); }
.sidebar-menu-link.active .chevron-icon { color: #ffffff; transform: translateX(2px); }

@media (max-width: 1024px) {
  .page-layout { grid-template-columns: 1fr; }
  .content-paper { padding: 28px 24px; }
  .right-menu-sidebar { order: 2; }
  .sidebar-sticky-wrapper { position: static; }
  .vacancy-info-grid { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 640px) {
  .vacancies-page-wrapper { padding: 16px 0 40px 0; }
  .content-paper { padding: 20px 16px; border-radius: 8px; }
  .paper-header { flex-direction: column; align-items: flex-start; }
  .page-title { font-size: 22px; }
  .official-announcement-card { flex-direction: column; padding: 20px 16px; }
  .vacancy-card-header { flex-direction: column; }
  .salary-box { text-align: left; width: 100%; }
  .vacancy-info-grid { grid-template-columns: 1fr; }
  .filters-row { flex-direction: column; }
}

@media print {
  .vacancies-page-wrapper { background: none; padding: 0; }
  .page-layout { display: block; }
  .right-menu-sidebar, .header-tools, .filters-row, .paper-footer, .breadcrumbs, .apply-btn { display: none !important; }
  .content-paper { border: none; box-shadow: none; padding: 0; }
}
</style>
