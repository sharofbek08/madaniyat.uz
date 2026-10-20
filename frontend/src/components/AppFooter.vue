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
</script>

<template>
  <footer id="contacts" class="app-footer">
    <div class="footer-top-accent"></div>
    <div class="container footer-container">
      <!-- Main Footer Columns -->
      <div class="footer-columns-grid">
        <!-- 1. Ministry Brand & Contact -->
        <div class="footer-col brand-col">
          <div class="footer-brand">
            <img :src="siteInfo.logo" alt="Madaniyat vazirligi gerbi" class="footer-logo" />
            <div>
              <span class="footer-country">O‘zbekiston Respublikasi</span>
              <h3 class="footer-title">{{ siteInfo.title }}</h3>
            </div>
          </div>

          <p class="footer-about-text">
            O‘zbekiston Respublikasida madaniyat, teatr, musiqa, xalq ijodiyoti va madaniy merosni rivojlantirish sohasidagi davlat siyosatini amalga oshiruvchi vakolatli davlat organi.
          </p>

          <!-- Social Links -->
          <div class="social-links-row">
            <a 
              v-for="s in siteInfo.socials" 
              :key="s.name"
              :href="s.link" 
              target="_blank" 
              rel="noopener"
              class="social-btn"
              :title="s.name"
            >
              <!-- Telegram -->
              <svg v-if="s.name === 'Telegram'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="22" y1="2" x2="11" y2="13"/>
                <polygon points="22 2 15 22 11 13 2 9 22 2"/>
              </svg>
              <!-- Facebook -->
              <svg v-else-if="s.name === 'Facebook'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"/>
              </svg>
              <!-- Instagram -->
              <svg v-else-if="s.name === 'Instagram'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="2" y="2" width="20" height="20" rx="5" ry="5"/>
                <path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"/>
                <line x1="17.5" y1="6.5" x2="17.51" y2="6.5"/>
              </svg>
              <!-- YouTube -->
              <svg v-else-if="s.name === 'YouTube'" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M22.54 6.42a2.78 2.78 0 0 0-1.94-2C18.88 4 12 4 12 4s-6.88 0-8.6.46a2.78 2.78 0 0 0-1.94 2A29 29 0 0 0 1 11.75a29 29 0 0 0 .46 5.33A2.78 2.78 0 0 0 3.4 19c1.72.46 8.6.46 8.6.46s6.88 0 8.6-.46a2.78 2.78 0 0 0 1.94-2 29 29 0 0 0 .46-5.25 29 29 0 0 0-.46-5.33z"/>
                <polygon points="9.75 15.02 15.5 11.75 9.75 8.48 9.75 15.02"/>
              </svg>
              <!-- X -->
              <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M4 4l16 16M4 20L20 4"/>
              </svg>
            </a>
          </div>
        </div>

        <!-- 2. Contacts Info -->
        <div class="footer-col contacts-col">
          <h4 class="col-heading">Bog‘lanish</h4>
          <ul class="contacts-list">
            <li class="contact-item">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                <circle cx="12" cy="10" r="3"/>
              </svg>
              <div>
                <span class="label">Manzil:</span>
                <span class="val">{{ siteInfo.address }}</span>
              </div>
            </li>

            <li class="contact-item">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
              </svg>
              <div>
                <span class="label">Ishonch telefoni:</span>
                <a :href="`tel:${siteInfo.helpline_value?.replace(/[^0-9+]/g, '')}`" class="val highlight">
                  {{ siteInfo.helpline_value }}
                </a>
              </div>
            </li>

            <li class="contact-item">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                <polyline points="22,6 12,13 2,6"/>
              </svg>
              <div>
                <span class="label">Elektron pochta:</span>
                <a :href="`mailto:${siteInfo.email}`" class="val highlight">{{ siteInfo.email }}</a>
              </div>
            </li>

            <li class="contact-item">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <circle cx="12" cy="12" r="10"/>
                <polyline points="12 6 12 12 16 14"/>
              </svg>
              <div>
                <span class="label">Ish vaqti:</span>
                <span class="val">{{ siteInfo.working_hours }}</span>
              </div>
            </li>

            <li class="contact-item">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <rect x="2" y="7" width="20" height="14" rx="2" ry="2"/>
                <path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"/>
              </svg>
              <div>
                <span class="label">STIR (TIN):</span>
                <span class="val">{{ siteInfo.tin }}</span>
              </div>
            </li>
          </ul>
        </div>

        <!-- 3. Navigation Links (Column 1) -->
        <div class="footer-col links-col">
          <h4 class="col-heading">Vazirlik haqida</h4>
          <ul class="footer-links-list">
            <li><a href="#madaniyat/pages/about">Vazirlik haqida</a></li>
            <li><a href="#madaniyat/pages/structure">Tashkilot tuzilmasi</a></li>
            <li><a href="#madaniyat/guides">Rahbariyat</a></li>
            <li><a href="#madaniyat/departments/staff?active=1321">Markaziy apparat</a></li>
            <li><a href="#subordinate">Tizimdagi tashkilotlar</a></li>
            <li><a href="#regional">Hududiy boshqarmalar</a></li>
            <li><a href="#vacancies">Bo‘sh ish o‘rinlari</a></li>
          </ul>
        </div>

        <!-- 4. Navigation Links (Column 2) -->
        <div class="footer-col links-col">
          <h4 class="col-heading">Axborot xizmati</h4>
          <ul class="footer-links-list">
            <li><a href="#news">So‘nggi yangiliklar</a></li>
            <li><a href="#events">Voqealar taqvimi</a></li>
            <li><a href="#announcements">Rasmiy e'lonlar</a></li>
            <li><a href="#press-release">Matbuot relizlari</a></li>
            <li><a href="#photos">Fotogalereya</a></li>
            <li><a href="#videos">Videogalereya</a></li>
            <li><a href="#open-data">Ochiq ma'lumotlar</a></li>
          </ul>
        </div>
      </div>

      <!-- Mistake / Error text notice banner (Ctrl+Enter) -->
      <div class="error-notice-card">
        <div class="notice-icon">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="12" cy="12" r="10"/>
            <line x1="12" y1="8" x2="12" y2="12"/>
            <line x1="12" y1="16" x2="12.01" y2="16"/>
          </svg>
        </div>
        <p class="notice-text">
          <strong>Diqqat!</strong> Agar siz matnda imlo yoki mazmuniy xatoliklarni aniqlasangiz, ma'muriyatni xabardor qilish uchun matnni belgilab, 
          <kbd class="kbd-key">Ctrl</kbd> + <kbd class="kbd-key">Enter</kbd> tugmalarini bosing.
        </p>
      </div>

      <!-- Footer Bottom Copyright & Meta -->
      <div class="footer-bottom-bar">
        <div class="copyright-text">
          <p>© 2026 O‘zbekiston Respublikasi Madaniyat vazirligi. Barcha huquqlar himoyalangan.</p>
          <p class="copy-sub">Ushbu vebsaytdagi ma'lumotlardan foydalanilganda rasmiy havola ko‘rsatilishi shart.</p>
        </div>

        <div class="footer-bottom-actions">
          <span class="last-updated">Oxirgi yangilanish: {{ siteInfo.last_updated }}</span>

          <button class="scroll-top-btn" @click="scrollToTop" title="Yuqoriga qaytish">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <line x1="12" y1="19" x2="12" y2="5"/>
              <polyline points="5 12 12 5 19 12"/>
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
  background-color: #0b192c;
  color: #cbd5e1;
  position: relative;
  font-size: 0.9rem;
  padding-top: 50px;
  padding-bottom: 30px;
}

.footer-top-accent {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, var(--color-primary), var(--color-accent), var(--color-primary));
}

.footer-columns-grid {
  display: grid;
  grid-template-columns: 1.5fr 1.2fr 1fr 1fr;
  gap: 36px;
  margin-bottom: 40px;
}

/* Brand Col */
.footer-brand {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-bottom: 16px;
}

.footer-logo {
  width: 58px;
  height: 58px;
  object-fit: contain;
  filter: brightness(1.1);
}

.footer-country {
  display: block;
  font-size: 0.72rem;
  color: var(--color-accent);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.footer-title {
  font-size: 1.15rem;
  font-weight: 800;
  color: #ffffff;
  line-height: 1.25;
}

.footer-about-text {
  font-size: 0.85rem;
  color: #94a3b8;
  line-height: 1.6;
  margin-bottom: 22px;
}

.social-links-row {
  display: flex;
  align-items: center;
  gap: 10px;
}

.social-btn {
  width: 38px;
  height: 38px;
  border-radius: var(--radius-full);
  background-color: rgba(255, 255, 255, 0.08);
  color: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition-fast);
}

.social-btn:hover {
  background-color: var(--color-accent);
  color: #000000;
  transform: translateY(-2px);
}

.social-btn svg {
  width: 18px;
  height: 18px;
}

/* Heading */
.col-heading {
  font-size: 1.05rem;
  font-weight: 700;
  color: #ffffff;
  margin-bottom: 20px;
  position: relative;
  padding-bottom: 8px;
}

.col-heading::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 32px;
  height: 2px;
  background-color: var(--color-accent);
}

/* Contacts */
.contacts-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.contact-item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  font-size: 0.85rem;
}

.contact-item svg {
  width: 18px;
  height: 18px;
  color: var(--color-accent);
  flex-shrink: 0;
  margin-top: 2px;
}

.contact-item .label {
  display: block;
  font-size: 0.72rem;
  color: #64748b;
  font-weight: 600;
  text-transform: uppercase;
}

.contact-item .val {
  color: #e2e8f0;
  line-height: 1.4;
}

.contact-item .val.highlight {
  color: var(--color-accent);
  font-weight: 700;
}

.contact-item .val.highlight:hover {
  text-decoration: underline;
}

/* Links */
.footer-links-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.footer-links-list a {
  color: #94a3b8;
  font-size: 0.86rem;
  transition: all var(--transition-fast);
  display: inline-block;
}

.footer-links-list a:hover {
  color: var(--color-accent);
  transform: translateX(4px);
}

/* Error Notice Card */
.error-notice-card {
  background-color: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: var(--radius-md);
  padding: 16px 20px;
  display: flex;
  align-items: center;
  gap: 14px;
  margin-bottom: 30px;
}

.notice-icon {
  color: var(--color-accent);
  flex-shrink: 0;
}

.notice-icon svg {
  width: 22px;
  height: 22px;
}

.notice-text {
  font-size: 0.85rem;
  color: #94a3b8;
  line-height: 1.5;
}

.kbd-key {
  background-color: rgba(255, 255, 255, 0.12);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 4px;
  padding: 2px 6px;
  font-size: 0.75rem;
  color: #ffffff;
  font-family: inherit;
}

/* Bottom Bar */
.footer-bottom-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-top: 1px solid rgba(255, 255, 255, 0.08);
  padding-top: 24px;
  flex-wrap: wrap;
  gap: 16px;
}

.copyright-text p {
  font-size: 0.82rem;
  color: #94a3b8;
}

.copy-sub {
  font-size: 0.75rem !important;
  color: #64748b !important;
  margin-top: 2px;
}

.footer-bottom-actions {
  display: flex;
  align-items: center;
  gap: 20px;
}

.last-updated {
  font-size: 0.75rem;
  color: #64748b;
}

.scroll-top-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  background-color: rgba(255, 255, 255, 0.08);
  color: #ffffff;
  padding: 8px 14px;
  border-radius: var(--radius-full);
  font-size: 0.8rem;
  font-weight: 600;
  transition: all var(--transition-fast);
}

.scroll-top-btn:hover {
  background-color: var(--color-primary);
  transform: translateY(-2px);
}

.scroll-top-btn svg {
  width: 14px;
  height: 14px;
}

@media (max-width: 1024px) {
  .footer-columns-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .footer-columns-grid {
    grid-template-columns: 1fr;
  }
  .footer-bottom-bar {
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>
