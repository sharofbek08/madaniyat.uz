<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  siteInfo: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['open-symbols', 'open-accessibility', 'open-sitemap'])

// Language dropdown state
const selectedLang = ref('oz')
const isLangOpen = ref(false)

const languages = [
  { code: 'oz', label: 'O‘zbekcha' },
  { code: 'uz', label: 'Ўзбекча' },
  { code: 'ru', label: 'Русский' },
  { code: 'en', label: 'English' },
  { code: 'qr', label: 'Qaraqalpaqsha' }
]

const setLang = (code) => {
  selectedLang.value = code
  isLangOpen.value = false
}

// Live UTC Clock formatted 1:1 like gov.uz: HH:mm:ss (UTC) DD.MM.YYYY
const utcTime = ref('')

const updateUtcTime = () => {
  const now = new Date()
  const hours = String(now.getUTCHours()).padStart(2, '0')
  const mins = String(now.getUTCMinutes()).padStart(2, '0')
  const secs = String(now.getUTCSeconds()).padStart(2, '0')
  const day = String(now.getUTCDate()).padStart(2, '0')
  const month = String(now.getUTCMonth() + 1).padStart(2, '0')
  const year = now.getUTCFullYear()
  utcTime.value = `${hours}:${mins}:${secs} (UTC) ${day}.${month}.${year}`
}

let timer = null
onMounted(() => {
  updateUtcTime()
  timer = setInterval(updateUtcTime, 1000)
})

onUnmounted(() => {
  if (timer) clearInterval(timer)
})

// Authorities directory state
const isAuthoritiesOpen = ref(false)
const authorities = [
  { title: "Vazirliklar", count: 21, url: "https://gov.uz/oz/authorities?type=ministry" },
  { title: "Davlat qo'mitalari", count: 3, url: "https://gov.uz/oz/authorities?type=committee" },
  { title: "Agentliklar", count: 14, url: "https://gov.uz/oz/authorities?type=agency" },
  { title: "Inspeksiyalar", count: 6, url: "https://gov.uz/oz/authorities?type=inspection" },
  { title: "Hokimliklar", count: 14, url: "https://gov.uz/oz/authorities?type=region" }
]
</script>

<template>
  <nav class="navbar_navbar__5hEv_ navbar_authority__o_3eB" aria-label="Hukumat portali navbari">
    <!-- 1. Davlat tashkilotlari Button -->
    <div class="authorities_authorities__3zHuY">
      <button 
        type="button" 
        class="authorities_authorities__btn__N0B2A" 
        aria-label="Davlat tashkilotlari"
        @click="isAuthoritiesOpen = !isAuthoritiesOpen"
      >
        <span class="font--sm text-left">Davlat tashkilotlari</span>
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 32 32" class="shrink-0">
          <g fill="currentColor">
            <path d="M16 0C7.163 0 0 7.163 0 16s7.163 16 16 16 16-7.164 16-16S24.836 0 16 0m0 30C8.28 30 2 23.72 2 16S8.28 1.969 16 1.969 30 8.28 30 16s-6.28 14-14 14"/>
            <path d="M13.808 8.271a.994.994 0 0 0-1.414 0 1.017 1.017 0 0 0 0 1.429l6.195 6.285-6.196 6.285a1.017 1.017 0 0 0 0 1.429.994.994 0 0 0 1.414 0l6.9-6.999a1.037 1.037 0 0 0 0-1.429z"/>
          </g>
        </svg>
      </button>

      <!-- Authorities Dropdown Menu -->
      <div v-if="isAuthoritiesOpen" class="authorities-dropdown-menu">
        <div class="authorities-menu-header">
          <span>O‘zbekiston Respublikasi davlat organlari</span>
          <button class="authorities-close-btn" @click="isAuthoritiesOpen = false">&times;</button>
        </div>
        <div class="authorities-grid">
          <a 
            v-for="auth in authorities" 
            :key="auth.title"
            :href="auth.url"
            target="_blank" 
            rel="noopener"
            class="authority-item-card"
          >
            <span class="auth-title">{{ auth.title }}</span>
            <span class="auth-count">{{ auth.count }} ta</span>
          </a>
        </div>
      </div>
    </div>

    <!-- 2. Navbar Center & Right Elements -->
    <div class="navbar_navbar__wrapper__k9aPL">
      <div class="navbar_navbar__inner__UbSvj">
        <!-- Gov.uz Logo link -->
        <a href="https://gov.uz/oz" target="_blank" rel="noopener" class="navbar_navbar__logo__rZdBG" title="O‘zbekiston Respublikasi Hukumat portali">
          <img 
            alt="Uzbekistan Gerb" 
            width="42" 
            height="43" 
            class="navbar_img__gerb__xe0CC" 
            src="/img/gerb_in.png" 
          />
          <img 
            alt="Uzbekistan Flag" 
            width="4" 
            height="32" 
            class="navbar_img__flagline__Rke2b" 
            src="/img/FlagLine.png" 
          />
          <span class="navbar_navbar__title__u5noB">
            O‘zbekiston Respublikasi <br/>
            <strong>Hukumat portali</strong>
          </span>
        </a>

        <!-- Middle widgets: Building image, UTC date, Language switcher, Accessibility -->
        <div class="navbar_center_widgets">
          <!-- Building Graphic -->
          <a href="https://gov.uz/oz" target="_blank" rel="noopener" class="navbar_img__building__hPwEg" title="gov.uz">
            <img 
              alt="gov.uz" 
              width="250" 
              height="50" 
              src="/img/building.png" 
            />
          </a>

          <div class="navbar_navbar__widgets__tBFch">
            <!-- Dynamic UTC Date/Time -->
            <span class="navbar_navbar__date__E7cQk" translate="no">{{ utcTime }}</span>

            <!-- Language and Accessibility -->
            <div class="navbar_actions_row">
              <!-- Language Switcher Button -->
              <div class="lang-dropdown-container">
                <button 
                  class="language-switcher_root__CeCHp" 
                  type="button" 
                  aria-label="Tilni tanlash" 
                  @click="isLangOpen = !isLangOpen"
                >
                  <span class="language-switcher_label__yV3u2">
                    {{ languages.find(l => l.code === selectedLang)?.label }}
                  </span>
                  <svg class="lang-caret" :class="{ 'open': isLangOpen }" viewBox="0 0 24 24" width="12" height="12" fill="none" stroke="currentColor" stroke-width="2">
                    <polyline points="6 9 12 15 18 9"/>
                  </svg>
                </button>

                <!-- Language Menu Dropdown -->
                <div v-if="isLangOpen" class="lang-options-dropdown">
                  <button 
                    v-for="l in languages" 
                    :key="l.code"
                    class="lang-select-option"
                    :class="{ 'is-selected': l.code === selectedLang }"
                    @click="setLang(l.code)"
                  >
                    {{ l.label }}
                  </button>
                </div>
              </div>

              <!-- Accessibility Button (Eye Icon) -->
              <div class="accessibility_accessibility__1oncY">
                <button 
                  class="accessibility_trigger__er4Ys" 
                  aria-label="Maxsus imkoniyatlar (zaif ko‘ruvchilar uchun)"
                  title="Maxsus imkoniyatlar"
                  @click="emit('open-accessibility')"
                >
                  <svg xmlns="http://www.w3.org/2000/svg" width="26" height="26" fill="currentColor" viewBox="0 0 512 512">
                    <path d="M503.698 231.895c-28.735-36.843-65.956-67.318-107.637-88.128-42.548-21.243-88.321-32.265-136.104-32.843-1.316-.036-6.6-.036-7.916 0-47.782.579-93.556 11.6-136.104 32.843-41.681 20.81-78.9 51.284-107.636 88.128a39.145 39.145 0 0 0 0 48.211c28.735 36.843 65.955 67.318 107.636 88.128 42.548 21.243 88.321 32.265 136.104 32.843 1.316.036 6.6.036 7.916 0 47.782-.579 93.556-11.6 136.104-32.843 41.681-20.81 78.901-51.284 107.637-88.128a39.145 39.145 0 0 0 0-48.211M125.242 349.599c-38.92-19.432-73.678-47.892-100.517-82.303a18.344 18.344 0 0 1 0-22.591c26.838-34.411 61.596-62.871 100.517-82.303a294 294 0 0 1 33.839-14.33c-29.578 26.588-48.213 65.12-48.213 107.928 0 42.81 18.636 81.345 48.217 107.932a294 294 0 0 1-33.843-14.333M256 380.303c-68.542 0-124.304-55.762-124.304-124.304S187.458 131.696 256 131.696 380.304 187.458 380.304 256 324.542 380.303 256 380.303m231.275-113.008c-26.838 34.411-61.596 62.871-100.517 82.303a294 294 0 0 1-33.805 14.299c29.558-26.587 48.179-65.107 48.179-107.898 0-42.814-18.64-81.351-48.223-107.939a294 294 0 0 1 33.85 14.34c38.92 19.432 73.678 47.892 100.517 82.303a18.346 18.346 0 0 1-.001 22.592"/>
                    <path d="M256 202.804c-29.332 0-53.195 23.863-53.195 53.195s23.863 53.195 53.195 53.195 53.195-23.863 53.195-53.195c.001-29.332-23.862-53.195-53.195-53.195m0 85.563c-17.847 0-32.368-14.519-32.368-32.368 0-17.848 14.519-32.367 32.368-32.367 17.847 0 32.367 14.519 32.367 32.367.001 17.849-14.52 32.368-32.367 32.368"/>
                  </svg>
                </button>
              </div>
            </div>
          </div>
        </div>

        <!-- 3. OneID Authorization Button -->
        <div class="navbar_avtorizatsiya__zCfbN">
          <a 
            target="_blank" 
            rel="noopener" 
            class="oneid-link-btn" 
            href="https://id.egov.uz"
            title="Yagona identifikatsiya tizimi (OneID)"
          >
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" class="fill-[#8a8a8b]" viewBox="0 0 21.412 21.411">
              <path d="M0 10.704A10.706 10.706 0 0 1 10.706.004a10.706 10.706 0 0 1 10.706 10.7 10.706 10.706 0 0 1-10.706 10.707A10.706 10.706 0 0 1 0 10.704m18.024 5.849a9.37 9.37 0 0 0 1.125-9.909 9.37 9.37 0 0 0-8.442-5.306 9.37 9.37 0 0 0-8.441 5.306 9.37 9.37 0 0 0 1.123 9.909c.951-1.53 3.042-3.171 7.318-3.171s6.365 1.639 7.317 3.171M6.691 8.029a4.016 4.016 0 0 1 4.015-4.016 4.016 4.016 0 0 1 4.015 4.016 4.014 4.014 0 0 1-4.015 4.013 4.014 4.014 0 0 1-4.015-4.013"/>
            </svg>
          </a>
        </div>
      </div>

      <!-- 4. Far Right Sitemap Hamburger -->
      <div class="navbar_navbar__sitemap__rEANM" @click="emit('open-sitemap')" title="Sayt xaritasi">
        <div class="sitemap_burger_icon">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </div>
    </div>
  </nav>
</template>

<style scoped>
/* Exact gov.uz navbar stylesheet */
.navbar_navbar__5hEv_ {
  position: sticky;
  top: 0;
  left: 0;
  right: 0;
  height: 64px;
  display: flex;
  background-color: #ffffff;
  z-index: 9999;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
  border-bottom: 1px solid #e2e8f0;
}

/* Davlat tashkilotlari left button */
.authorities_authorities__3zHuY {
  position: relative;
  height: 100%;
  display: flex;
  align-items: center;
  border-right: 1px solid #e2e8f0;
  flex-shrink: 0;
}

.authorities_authorities__btn__N0B2A {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 0 24px;
  height: 100%;
  background: transparent;
  border: none;
  cursor: pointer;
  color: #1e293b;
  font-size: 14px;
  font-weight: 600;
  transition: all 0.2s;
}

.authorities_authorities__btn__N0B2A:hover {
  background: #f8fafc;
  color: #1e40af;
}

.authorities_authorities__btn__N0B2A svg {
  color: #64748b;
  transition: transform 0.2s;
}

.authorities_authorities__btn__N0B2A:hover svg {
  color: #1e40af;
  transform: translateX(3px);
}

/* Authorities Dropdown */
.authorities-dropdown-menu {
  position: absolute;
  top: 64px;
  left: 0;
  width: 360px;
  background: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 0 0 12px 12px;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.12);
  padding: 16px;
  z-index: 10000;
}

.authorities-menu-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 13px;
  font-weight: 700;
  color: #0f172a;
  padding-bottom: 12px;
  border-bottom: 1px solid #f1f5f9;
  margin-bottom: 12px;
}

.authorities-close-btn {
  background: transparent;
  border: none;
  font-size: 20px;
  color: #94a3b8;
  cursor: pointer;
}

.authorities-grid {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.authority-item-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 14px;
  border-radius: 8px;
  background: #f8fafc;
  text-decoration: none;
  color: #1e293b;
  font-size: 13px;
  font-weight: 500;
  transition: all 0.2s;
}

.authority-item-card:hover {
  background: #eff6ff;
  color: #1d4ed8;
}

.auth-count {
  font-size: 12px;
  color: #64748b;
  background: #e2e8f0;
  padding: 2px 8px;
  border-radius: 12px;
}

/* Navbar Wrapper */
.navbar_navbar__wrapper__k9aPL {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.navbar_navbar__inner__UbSvj {
  height: 100%;
  width: 100%;
  padding-left: 24px;
  padding-right: 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
}

/* Logo link */
.navbar_navbar__logo__rZdBG {
  display: flex;
  align-items: center;
  gap: 14px;
  text-decoration: none;
  color: #0f172a;
}

.navbar_img__gerb__xe0CC {
  width: 36px;
  height: 37px;
  object-fit: contain;
}

.navbar_img__flagline__Rke2b {
  width: 4px;
  height: 28px;
  object-fit: cover;
}

.navbar_navbar__title__u5noB {
  font-size: 13px;
  line-height: 1.25;
  color: #1e293b;
  font-weight: 500;
}

.navbar_navbar__title__u5noB strong {
  font-weight: 700;
  color: #0f172a;
}

/* Center widgets */
.navbar_center_widgets {
  display: flex;
  align-items: center;
  gap: 28px;
}

.navbar_img__building__hPwEg {
  display: flex;
  align-items: center;
  height: 38px;
}

.navbar_img__building__hPwEg img {
  height: 36px;
  width: auto;
  object-fit: contain;
}

.navbar_navbar__widgets__tBFch {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
}

.navbar_navbar__date__E7cQk {
  font-size: 11px;
  color: #64748b;
  font-family: inherit;
  font-weight: 500;
}

.navbar_actions_row {
  display: flex;
  align-items: center;
  gap: 12px;
}

/* Language Switcher */
.lang-dropdown-container {
  position: relative;
}

.language-switcher_root__CeCHp {
  display: flex;
  align-items: center;
  gap: 6px;
  background: transparent;
  border: 1px solid #cbd5e1;
  padding: 4px 12px;
  border-radius: 6px;
  font-size: 13px;
  font-weight: 600;
  color: #334155;
  cursor: pointer;
  transition: all 0.2s;
}

.language-switcher_root__CeCHp:hover {
  border-color: #2563eb;
  color: #2563eb;
}

.lang-caret {
  transition: transform 0.2s;
}

.lang-caret.open {
  transform: rotate(180deg);
}

.lang-options-dropdown {
  position: absolute;
  top: 100%;
  right: 0;
  margin-top: 6px;
  background: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  padding: 6px;
  min-width: 140px;
  z-index: 10000;
}

.lang-select-option {
  width: 100%;
  text-align: left;
  padding: 8px 12px;
  border: none;
  background: transparent;
  font-size: 13px;
  color: #334155;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.15s;
}

.lang-select-option:hover {
  background: #f1f5f9;
  color: #1e40af;
}

.lang-select-option.is-selected {
  background: #eff6ff;
  color: #1d4ed8;
  font-weight: 700;
}

/* Accessibility button */
.accessibility_trigger__er4Ys {
  background: transparent;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #475569;
  cursor: pointer;
  padding: 4px;
  border-radius: 6px;
  transition: all 0.2s;
}

.accessibility_trigger__er4Ys:hover {
  color: #2563eb;
  background: #f1f5f9;
}

/* OneID Button */
.navbar_avtorizatsiya__zCfbN {
  display: flex;
  align-items: center;
}

.oneid-link-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  border-radius: 8px;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  transition: all 0.2s;
}

.oneid-link-btn:hover {
  background: #e2e8f0;
  border-color: #cbd5e1;
}

/* Sitemap Hamburger Far Right */
.navbar_navbar__sitemap__rEANM {
  height: 64px;
  width: 64px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-left: 1px solid #e2e8f0;
  cursor: pointer;
  background: #ffffff;
  transition: background 0.2s;
  flex-shrink: 0;
}

.navbar_navbar__sitemap__rEANM:hover {
  background: #f8fafc;
}

.sitemap_burger_icon {
  display: flex;
  flex-direction: column;
  gap: 5px;
  width: 20px;
}

.sitemap_burger_icon span {
  display: block;
  width: 100%;
  height: 2px;
  background-color: #334155;
  border-radius: 2px;
  transition: all 0.2s;
}

.navbar_navbar__sitemap__rEANM:hover .sitemap_burger_icon span {
  background-color: #1e40af;
}

@media (max-width: 1024px) {
  .navbar_img__building__hPwEg {
    display: none;
  }
}

@media (max-width: 768px) {
  .authorities_authorities__3zHuY {
    display: none;
  }
  .navbar_navbar__inner__UbSvj {
    padding-left: 12px;
    padding-right: 12px;
  }
  .navbar_navbar__date__E7cQk {
    display: none;
  }
}
</style>
