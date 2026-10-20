<script setup>
const props = defineProps({
  siteInfo: {
    type: Object,
    required: true
  },
  navigation: {
    type: Array,
    required: true
  }
})

const scrollToTop = () => {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

const footerLinks = [
  { title: 'Vazirlik haqida', url: '#/madaniyat/pages/about' },
  { title: 'Tashkilot tuzilmasi', url: '#/madaniyat/pages/structure' },
  { title: 'Rahbariyat', url: '#/madaniyat/guides' },
  { title: 'Markaziy apparat', url: '#/madaniyat/departments/staff' },
  { title: 'Hududiy boshqarmalar', url: '#/madaniyat/departments/regional' },
  { title: 'Bo‘sh ish o‘rinlari', url: '#/madaniyat/pages/bo-sh-ish-o-rinlari' },
  { title: 'Tanlovlar va tenderlar', url: '#/madaniyat/news/e-lon' },
  { title: 'Ekofaol xodim', url: '#/madaniyat/news/test' },
  { title: 'Konstitutsiya sahifasi', url: '#/madaniyat/sections/konstitutsiya-sahifasi' },
  { title: 'Korrupsiyaga qarshi kurashish', url: '#/madaniyat/sections/korrup-bo-lim' },
  { title: 'Bog‘lanish', url: '#contacts' }
]
</script>

<template>
  <footer id="contacts" class="app-footer">
    <div class="container">
      <!-- 1. Top Real-time Visitors Statistics (Gov.uz official counter) -->
      <div class="footer-counters-row">
        <div class="counter-badge">
          <span class="counter-label">Onlayn:</span>
          <strong class="counter-val">14</strong>
        </div>
        <span class="counter-sep">•</span>
        <div class="counter-badge">
          <span class="counter-label">Amallar:</span>
          <strong class="counter-val">82</strong>
        </div>
        <span class="counter-sep">•</span>
        <div class="counter-badge">
          <span class="counter-label">Tashriflar:</span>
          <strong class="counter-val">2 418</strong>
        </div>
        <span class="counter-sep">•</span>
        <div class="counter-badge">
          <span class="counter-label">Veb-saytdagi o‘rtacha vaqt:</span>
          <strong class="counter-val">04:12</strong>
        </div>
      </div>

      <!-- 2. Footer Menu Links Row (Centered, Uppercase, Underline on hover) -->
      <nav class="footer-menu-nav">
        <a 
          v-for="item in footerLinks" 
          :key="item.title"
          :href="item.url" 
          class="footer-nav-link"
        >
          {{ item.title }}
        </a>
      </nav>

      <!-- 3. Main 3-Column Footer Grid -->
      <div class="footer-main-grid">
        <!-- Col 1: Ministry Info -->
        <div class="footer-col">
          <div class="footer-brand">
            <img src="/img/ministry_logo.png" alt="Madaniyat vazirligi gerbi" class="footer-logo" />
            <div>
              <span class="footer-country">O‘ZBEKISTON RESPUBLIKASI</span>
              <h3 class="footer-title">{{ siteInfo.title }}</h3>
            </div>
          </div>
          <p class="footer-address">
            <strong>Manzil:</strong> {{ siteInfo.address }}
          </p>
          <p class="footer-email-row">
            <strong>Elektron pochta:</strong> 
            <a :href="`mailto:${siteInfo.email}`" class="footer-email-link">{{ siteInfo.email }}</a>
          </p>
          <p class="footer-hours">
            <strong>Ish vaqti:</strong> {{ siteInfo.working_hours }}
          </p>
        </div>

        <!-- Col 2: Social Media -->
        <div class="footer-col footer-col-center">
          <h4 class="col-title">Ijtimoiy tarmoqlar</h4>
          <p class="social-desc">Vazirlikning rasmiy sahifalariga obuna bo‘ling va so‘nggi madaniy yangiliklardan xabardor bo‘ling:</p>
          <div class="footer-social-row">
            <a 
              v-for="s in siteInfo.socials" 
              :key="s.name"
              :href="s.link" 
              target="_blank" 
              rel="noopener"
              class="footer-social-btn"
              :title="s.name"
            >
              <svg v-if="s.name === 'Telegram'" viewBox="0 0 24 24" width="18" height="18" fill="currentColor">
                <path d="M12 0C5.373 0 0 5.373 0 12s5.373 12 12 12 12-5.373 12-12S18.627 0 12 0zm5.894 8.221l-1.97 9.28c-.145.658-.537.818-1.084.508l-3-2.21-1.446 1.394c-.16.16-.295.295-.605.295l.213-3.053 5.56-5.023c.242-.213-.054-.333-.373-.121l-6.871 4.326-2.962-.924c-.643-.204-.657-.643.136-.953l11.57-4.458c.538-.196 1.006.128.832.941z"/>
              </svg>
              <svg v-else-if="s.name === 'Facebook'" viewBox="0 0 24 24" width="18" height="18" fill="currentColor">
                <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
              </svg>
              <svg v-else-if="s.name === 'Instagram'" viewBox="0 0 24 24" width="18" height="18" fill="currentColor">
                <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/>
              </svg>
              <svg v-else-if="s.name === 'YouTube'" viewBox="0 0 24 24" width="18" height="18" fill="currentColor">
                <path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/>
              </svg>
              <svg v-else viewBox="0 0 24 24" width="18" height="18" fill="currentColor">
                <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
              </svg>
            </a>
          </div>
        </div>

        <!-- Col 3: Helpline & STIR -->
        <div class="footer-col footer-col-right">
          <h4 class="col-title">{{ siteInfo.helpline_title }}</h4>
          <a :href="`tel:${siteInfo.helpline_value?.replace(/[^0-9+]/g, '')}`" class="footer-phone-big">
            {{ siteInfo.helpline_value }}
          </a>
          <a href="#/madaniyat/pages/contacts" class="footer-all-contacts-btn">
            {{ siteInfo.contact_title }}
          </a>
          <div class="footer-tin">
            <span>STIR:</span> <strong>{{ siteInfo.tin }}</strong>
          </div>
        </div>
      </div>

      <!-- 4. Ctrl+Enter Mistake Notification Box -->
      <div class="footer-mistake-notice">
        <svg class="notice-svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="12" cy="12" r="10"/>
          <line x1="12" y1="8" x2="12" y2="12"/>
          <line x1="12" y1="16" x2="12.01" y2="16"/>
        </svg>
        <p class="notice-p">
          Diqqat! Agar siz matnda xatoliklarni aniqlasangiz, ularni belgilab, ma'muriyatni xabardor qilish uchun 
          <kbd class="kbd-btn">Ctrl</kbd> + <kbd class="kbd-btn">Enter</kbd> tugmalarini bosing.
        </p>
      </div>

      <!-- 5. Copyright & Bottom Row -->
      <div class="footer-bottom-bar">
        <p class="copyright-p">
          Barcha huquqlar himoyalangan. Ushbu veb-saytdagi ma’lumotlardan foydalanganda havola ko‘rsatilishi shart.
        </p>
        <div class="footer-bottom-actions">
          <span class="updated-time">Oxirgi yangilanish: {{ siteInfo.last_updated }}</span>
          <button class="scroll-top-btn" @click="scrollToTop" title="Yuqoriga qaytish">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
              <polyline points="18 15 12 9 6 15"/>
            </svg>
            <span>Yuqoriga</span>
          </button>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.app-footer {
  background-color: #0b1a30;
  color: #d1d5db;
  padding-top: 2rem;
  padding-bottom: 2rem;
  border-top: 3px solid var(--color-primary);
  font-size: 0.875rem;
}

/* Counters Row */
.footer-counters-row {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1.25rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  flex-wrap: wrap;
}

.counter-badge {
  display: flex;
  align-items: center;
  gap: 0.375rem;
  font-size: 0.8125rem;
  color: #94a3b8;
}

.counter-val {
  color: #ffffff;
  font-weight: 700;
}

.counter-sep {
  color: rgba(255, 255, 255, 0.2);
}

/* Navigation Links */
.footer-menu-nav {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  column-gap: 1.5rem;
  row-gap: 0.75rem;
  padding-top: 1.5rem;
  padding-bottom: 2rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.footer-nav-link {
  font-size: 0.8125rem;
  font-weight: 600;
  color: #e2e8f0;
  text-transform: uppercase;
  transition: color 0.15s ease;
  letter-spacing: 0.3px;
}

.footer-nav-link:hover {
  color: #38bdf8;
  text-decoration: underline;
}

/* Main Grid */
.footer-main-grid {
  display: grid;
  grid-template-columns: 1.3fr 1fr 1fr;
  gap: 2.5rem;
  padding-top: 2rem;
  padding-bottom: 2rem;
}

@media (max-width: 992px) {
  .footer-main-grid {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
}

.footer-brand {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1rem;
}

.footer-logo {
  width: 48px;
  height: 48px;
  object-fit: contain;
  flex-shrink: 0;
}

.footer-country {
  font-size: 0.6875rem;
  color: #94a3b8;
  letter-spacing: 0.5px;
  display: block;
}

.footer-title {
  font-size: 1rem;
  font-weight: 700;
  color: #ffffff;
  line-height: 1.3;
}

.footer-address,
.footer-email-row,
.footer-hours {
  font-size: 0.8125rem;
  color: #cbd5e1;
  margin-bottom: 0.5rem;
  line-height: 1.45;
}

.footer-email-link {
  color: #38bdf8;
  text-decoration: none;
}

.footer-email-link:hover {
  text-decoration: underline;
}

.col-title {
  font-size: 1rem;
  font-weight: 700;
  color: #ffffff;
  margin-bottom: 0.75rem;
}

.social-desc {
  font-size: 0.8125rem;
  color: #94a3b8;
  margin-bottom: 1rem;
  line-height: 1.4;
}

.footer-social-row {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.footer-social-btn {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.1);
  color: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.footer-social-btn:hover {
  background-color: var(--color-primary);
  transform: translateY(-2px);
}

.footer-phone-big {
  font-size: 1.35rem;
  font-weight: 800;
  color: #38bdf8;
  display: block;
  margin-bottom: 0.75rem;
  text-decoration: none;
}

.footer-phone-big:hover {
  text-decoration: underline;
}

.footer-all-contacts-btn {
  display: inline-block;
  font-size: 0.75rem;
  font-weight: 700;
  color: #ffffff;
  background-color: rgba(255, 255, 255, 0.1);
  padding: 4px 12px;
  border-radius: 4px;
  text-transform: uppercase;
  margin-bottom: 1rem;
  transition: background 0.2s;
}

.footer-all-contacts-btn:hover {
  background-color: var(--color-primary);
}

.footer-tin {
  font-size: 0.8125rem;
  color: #94a3b8;
}

.footer-tin strong {
  color: #ffffff;
}

/* Mistake Notice */
.footer-mistake-notice {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  background-color: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 0.875rem 1.25rem;
  border-radius: 4px;
  margin-bottom: 1.5rem;
}

.notice-svg {
  color: #facc15;
  flex-shrink: 0;
}

.notice-p {
  font-size: 0.8125rem;
  color: #cbd5e1;
  line-height: 1.4;
  margin: 0;
}

.kbd-btn {
  background: rgba(255, 255, 255, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.3);
  padding: 2px 6px;
  border-radius: 3px;
  font-size: 0.75rem;
  color: #ffffff;
}

/* Bottom Bar */
.footer-bottom-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 1.5rem;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  flex-wrap: wrap;
  gap: 1rem;
}

.copyright-p {
  font-size: 0.75rem;
  color: #94a3b8;
  max-width: 40rem;
  line-height: 1.4;
  margin: 0;
}

.footer-bottom-actions {
  display: flex;
  align-items: center;
  gap: 1.25rem;
}

.updated-time {
  font-size: 0.75rem;
  color: #64748b;
}

.scroll-top-btn {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 0.75rem;
  font-weight: 600;
  color: #ffffff;
  background-color: rgba(255, 255, 255, 0.1);
  padding: 6px 12px;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.scroll-top-btn:hover {
  background-color: var(--color-primary);
  transform: translateY(-2px);
}
</style>
