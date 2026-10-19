<script setup>
import { ref } from 'vue'

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

const emit = defineEmits(['open-search', 'open-sitemap'])

const activeMenuId = ref(null)
const isMobileNavOpen = ref(false)

const handleMouseEnter = (id) => {
  activeMenuId.value = id
}

const handleMouseLeave = () => {
  activeMenuId.value = null
}

const toggleMobileNav = () => {
  isMobileNavOpen.value = !isMobileNavOpen.value
}
</script>

<template>
  <header class="app-header">
    <!-- Main Branding & Contacts Bar -->
    <div class="main-branding">
      <div class="container branding-container">
        <!-- Logo and Ministry Name -->
        <a href="/" class="brand-link">
          <img :src="siteInfo.logo" alt="O‘zbekiston Respublikasi Madaniyat vazirligi gerbi" class="brand-logo" />
          <div class="brand-text">
            <span class="country-name">O‘zbekiston Respublikasi</span>
            <h1 class="ministry-name">Madaniyat vazirligi</h1>
            <span class="portal-badge">Rasmiy vebsayt</span>
          </div>
        </a>

        <!-- Right Side: Search Box + Hotlines -->
        <div class="header-actions">
          <!-- Search trigger bar -->
          <button class="search-trigger-btn" @click="emit('open-search')" title="Sayt bo‘yicha qidiruv (Ctrl+K)">
            <svg class="search-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="11" cy="11" r="8"/>
              <line x1="21" y1="21" x2="16.65" y2="16.65"/>
            </svg>
            <span class="search-placeholder">Portal bo‘yicha qidiruv...</span>
            <kbd class="search-kbd">Ctrl K</kbd>
          </button>

          <!-- Helpline Contact Box -->
          <div class="helpline-box">
            <div class="phone-icon-bubble">
              <span class="pulse-ring"></span>
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
              </svg>
            </div>
            <div class="helpline-details">
              <span class="helpline-label">{{ siteInfo.helpline_title }}</span>
              <a :href="`tel:${siteInfo.helpline_value?.replace(/[^0-9+]/g, '')}`" class="helpline-number">
                {{ siteInfo.helpline_value }}
              </a>
              <a href="#contacts" class="all-contacts-link">{{ siteInfo.contact_title }}</a>
            </div>
          </div>

          <!-- Mobile Hamburger Toggle -->
          <button class="mobile-toggle-btn" @click="toggleMobileNav" aria-label="Mobil menyu">
            <svg v-if="!isMobileNavOpen" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <line x1="3" y1="12" x2="21" y2="12"/>
              <line x1="3" y1="6" x2="21" y2="6"/>
              <line x1="3" y1="18" x2="21" y2="18"/>
            </svg>
            <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <line x1="18" y1="6" x2="6" y2="18"/>
              <line x1="6" y1="6" x2="18" y2="18"/>
            </svg>
          </button>
        </div>
      </div>
    </div>

    <!-- Sticky Main Navigation Bar -->
    <nav class="main-navigation" :class="{ 'mobile-open': isMobileNavOpen }">
      <div class="container nav-container">
        <!-- Desktop Menu Items -->
        <ul class="nav-menu">
          <li 
            v-for="item in navigation" 
            :key="item.id" 
            class="nav-item"
            :class="{ 'has-dropdown': item.children?.length > 0, 'active': activeMenuId === item.id }"
            @mouseenter="handleMouseEnter(item.id)"
            @mouseleave="handleMouseLeave"
          >
            <a :href="item.url || '#'" class="nav-link" @click.prevent>
              <span>{{ item.title }}</span>
              <svg v-if="item.children?.length > 0" class="dropdown-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polyline points="6 9 12 15 18 9"/>
              </svg>
            </a>

            <!-- Submenu Dropdown -->
            <div v-if="item.children?.length > 0 && activeMenuId === item.id" class="dropdown-menu">
              <ul class="dropdown-list">
                <li v-for="sub in item.children" :key="sub.id" class="dropdown-item">
                  <a :href="`#${sub.url}`" class="dropdown-link">
                    <span class="bullet">•</span>
                    <span>{{ sub.title }}</span>
                  </a>
                </li>
              </ul>
            </div>
          </li>
        </ul>

        <!-- Sitemap Button at the end of Navbar -->
        <button class="sitemap-trigger-btn" @click="emit('open-sitemap')" title="Barcha bo‘limlar (Sayt xaritasi)">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="3" width="7" height="7"/>
            <rect x="14" y="3" width="7" height="7"/>
            <rect x="14" y="14" width="7" height="7"/>
            <rect x="3" y="14" width="7" height="7"/>
          </svg>
          <span class="sitemap-label">Sayt xaritasi</span>
        </button>
      </div>
    </nav>
  </header>
</template>

<style scoped>
.app-header {
  background-color: #ffffff;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.05);
  position: relative;
  z-index: 1000;
}

.main-branding {
  padding-top: 18px;
  padding-bottom: 18px;
  border-bottom: 1px solid var(--border-color);
}

.branding-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
}

.brand-link {
  display: flex;
  align-items: center;
  gap: 16px;
  text-decoration: none;
}

.brand-logo {
  width: 78px;
  height: 78px;
  object-fit: contain;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.08));
  transition: transform var(--transition-normal);
}

.brand-link:hover .brand-logo {
  transform: scale(1.04);
}

.brand-text {
  display: flex;
  flex-direction: column;
}

.country-name {
  font-size: 0.85rem;
  font-weight: 500;
  color: var(--color-gray-500);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.ministry-name {
  font-size: 1.5rem;
  font-weight: 800;
  color: var(--color-primary-dark);
  line-height: 1.15;
  margin: 2px 0 4px;
}

.portal-badge {
  display: inline-block;
  align-self: flex-start;
  font-size: 0.7rem;
  font-weight: 600;
  background-color: var(--color-primary-50);
  color: var(--color-primary);
  padding: 2px 8px;
  border-radius: var(--radius-sm);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.header-actions {
  display: flex;
  align-items: center;
  gap: 24px;
}

/* Search Box Trigger */
.search-trigger-btn {
  display: flex;
  align-items: center;
  gap: 10px;
  background-color: var(--color-gray-100);
  border: 1px solid var(--border-color);
  padding: 8px 16px;
  border-radius: var(--radius-full);
  color: var(--color-gray-500);
  font-size: 0.875rem;
  min-width: 240px;
  transition: var(--transition-fast);
}

.search-trigger-btn:hover {
  background-color: #ffffff;
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px var(--color-primary-50);
  color: var(--color-gray-800);
}

.search-icon {
  width: 16px;
  height: 16px;
  color: var(--color-gray-400);
}

.search-placeholder {
  flex: 1;
  text-align: left;
}

.search-kbd {
  font-size: 0.7rem;
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: 4px;
  padding: 2px 6px;
  color: var(--color-gray-400);
}

/* Helpline Contact */
.helpline-box {
  display: flex;
  align-items: center;
  gap: 12px;
}

.phone-icon-bubble {
  width: 44px;
  height: 44px;
  border-radius: var(--radius-full);
  background-color: var(--color-primary-50);
  color: var(--color-primary);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.phone-icon-bubble svg {
  width: 20px;
  height: 20px;
}

.pulse-ring {
  position: absolute;
  inset: -3px;
  border: 2px solid var(--color-primary);
  border-radius: var(--radius-full);
  opacity: 0.4;
  animation: pulseGlow 2.5s infinite;
}

.helpline-details {
  display: flex;
  flex-direction: column;
}

.helpline-label {
  font-size: 0.75rem;
  color: var(--color-gray-500);
  font-weight: 500;
}

.helpline-number {
  font-size: 1.05rem;
  font-weight: 800;
  color: var(--color-primary-dark);
  line-height: 1.2;
}

.helpline-number:hover {
  color: var(--color-primary);
}

.all-contacts-link {
  font-size: 0.7rem;
  font-weight: 600;
  color: var(--color-accent-dark);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.all-contacts-link:hover {
  text-decoration: underline;
}

.mobile-toggle-btn {
  display: none;
  width: 40px;
  height: 40px;
  align-items: center;
  justify-content: center;
  background-color: var(--color-gray-100);
  border-radius: var(--radius-md);
  color: var(--color-gray-800);
}

.mobile-toggle-btn svg {
  width: 24px;
  height: 24px;
}

/* Main Navigation */
.main-navigation {
  background-color: #f1f5f9;
  border-top: 1px solid rgba(0, 0, 0, 0.05);
  position: sticky;
  top: 0;
  z-index: 999;
}

.nav-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-left: 0;
  padding-right: 0;
}

.nav-menu {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
  flex: 1;
}

.nav-item {
  position: relative;
}

.nav-link {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 14px 18px;
  font-size: 0.92rem;
  font-weight: 600;
  color: var(--color-gray-700);
  transition: all var(--transition-fast);
  white-space: nowrap;
}

.dropdown-chevron {
  width: 14px;
  height: 14px;
  transition: transform var(--transition-fast);
  opacity: 0.7;
}

.nav-item:hover .nav-link,
.nav-item.active .nav-link {
  color: #ffffff;
  background-color: var(--color-primary);
}

.nav-item:hover .dropdown-chevron {
  transform: rotate(180deg);
  opacity: 1;
}

/* Dropdown Menu */
.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  min-width: 270px;
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-top: 3px solid var(--color-primary);
  border-radius: 0 0 var(--radius-md) var(--radius-md);
  box-shadow: var(--shadow-xl);
  padding: 8px 0;
  z-index: 1010;
  animation: fadeIn 0.15s ease;
}

.dropdown-list {
  list-style: none;
  margin: 0;
  padding: 0;
}

.dropdown-item {
  padding: 0;
}

.dropdown-link {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 9px 18px;
  font-size: 0.88rem;
  font-weight: 500;
  color: var(--color-gray-700);
  transition: var(--transition-fast);
}

.dropdown-link .bullet {
  color: var(--color-accent);
  font-size: 1.1rem;
  line-height: 1;
}

.dropdown-link:hover {
  background-color: var(--color-primary-50);
  color: var(--color-primary);
  padding-left: 22px;
}

/* Sitemap Button */
.sitemap-trigger-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 14px 20px;
  background-color: var(--color-primary);
  color: #ffffff;
  font-size: 0.88rem;
  font-weight: 600;
  transition: var(--transition-fast);
  border-left: 1px solid rgba(255, 255, 255, 0.15);
}

.sitemap-trigger-btn:hover {
  background-color: var(--color-primary-dark);
}

.sitemap-trigger-btn svg {
  width: 18px;
  height: 18px;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(4px); }
  to { opacity: 1; transform: translateY(0); }
}

/* Responsive */
@media (max-width: 1200px) {
  .nav-link {
    padding: 12px 12px;
    font-size: 0.85rem;
  }
}

@media (max-width: 992px) {
  .search-trigger-btn {
    min-width: 180px;
  }
  .search-kbd {
    display: none;
  }
  .brand-logo {
    width: 60px;
    height: 60px;
  }
  .ministry-name {
    font-size: 1.25rem;
  }
}

@media (max-width: 860px) {
  .mobile-toggle-btn {
    display: flex;
  }
  .search-trigger-btn {
    display: none;
  }
  .helpline-box {
    display: none;
  }
  .main-navigation {
    display: none;
  }
  .main-navigation.mobile-open {
    display: block;
    position: static;
  }
  .nav-container {
    flex-direction: column;
    padding: 10px 16px;
  }
  .nav-menu {
    flex-direction: column;
    width: 100%;
  }
  .nav-link {
    padding: 12px 0;
    border-bottom: 1px solid var(--border-color);
  }
  .dropdown-menu {
    position: static;
    box-shadow: none;
    border: none;
    background: transparent;
    padding-left: 16px;
  }
  .sitemap-trigger-btn {
    width: 100%;
    justify-content: center;
    border-radius: var(--radius-md);
    margin-top: 10px;
  }
}
</style>
