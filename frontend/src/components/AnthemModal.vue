<script setup>
import { ref } from 'vue'

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['close'])

const activeSymbolTab = ref('flag') // 'flag', 'coat', 'anthem'
</script>

<template>
  <div v-if="isOpen" class="modal-backdrop" @click.self="emit('close')">
    <div class="symbols-modal-container">
      <div class="modal-header">
        <h3 class="modal-title">O‘zbekiston Respublikasi Davlat Ramzlari</h3>
        <button class="modal-close-btn" @click="emit('close')" aria-label="Yopish">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="18" y1="6" x2="6" y2="18"/>
            <line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>
      </div>

      <!-- Symbols Tab Navigation -->
      <div class="symbols-tabs">
        <button 
          class="tab-btn" 
          :class="{ active: activeSymbolTab === 'flag' }"
          @click="activeSymbolTab = 'flag'"
        >
          Davlat Bayrog‘i
        </button>
        <button 
          class="tab-btn" 
          :class="{ active: activeSymbolTab === 'coat' }"
          @click="activeSymbolTab = 'coat'"
        >
          Davlat Gerbi
        </button>
        <button 
          class="tab-btn" 
          :class="{ active: activeSymbolTab === 'anthem' }"
          @click="activeSymbolTab = 'anthem'"
        >
          Davlat Madhiyasi
        </button>
      </div>

      <div class="modal-body">
        <!-- Flag Tab -->
        <div v-if="activeSymbolTab === 'flag'" class="symbol-content">
          <div class="flag-visual">
            <div class="flag-stripe stripe-blue"></div>
            <div class="flag-margin margin-red"></div>
            <div class="flag-stripe stripe-white"></div>
            <div class="flag-margin margin-red"></div>
            <div class="flag-stripe stripe-green"></div>
          </div>
          <h4 class="symbol-subtitle">O‘zbekiston Respublikasining Davlat Bayrog‘i</h4>
          <p class="symbol-desc">
            O‘zbekiston Respublikasining Davlat bayrog‘i 1991-yil 18-noyabrda qabul qilingan. Bayroqdagi moviy rang – musaffo osmon va tiriklik ramzi, oq rang – tinchlik va poklik, yashil rang – tabiatning yangilanishi va unumdorlik ramzidir. Qizil hoshiyalar esa tomirlarimizdagi jo‘shqin hayotiy qudratni anglatadi.
          </p>
        </div>

        <!-- Coat of Arms Tab -->
        <div v-else-if="activeSymbolTab === 'coat'" class="symbol-content">
          <div class="coat-visual">
            <img 
              src="https://api-portal.gov.uz/uploads/19/2025/06/20/068812a9-b916-337f-c7f3-86fc511c9bcb_authority_19.png" 
              alt="O‘zbekiston Gerbi" 
              class="coat-img"
            />
          </div>
          <h4 class="symbol-subtitle">O‘zbekiston Respublikasining Davlat Gerbi</h4>
          <p class="symbol-desc">
            O‘zbekiston Respublikasining Davlat gerbi 1992-yil 2-iyulda qabul qilingan. Gerb markazida baxt va erksevarlik ramzi bo‘lgan Humo qushi qanotlarini yozib turibdi. Gerbning yuqori qismida respublika jipsligining ramzi sifatida sakkiz qirrali yulduz, uning ichida yarim oy va yulduz tasvirlangan.
          </p>
        </div>

        <!-- Anthem Tab -->
        <div v-else class="symbol-content">
          <h4 class="symbol-subtitle">O‘zbekiston Respublikasining Davlat Madhiyasi</h4>
          <p class="anthem-author">Mutal Burhonov musiqasi, Abdulla Oripov so‘zi</p>
          <div class="anthem-lyrics">
            <p>
              Serquyosh, hur o‘lkam, elga baxt, najot,<br>
              Sen o‘zing do‘stlarga yo‘ldosh, mehribon!<br>
              Yashnagay to abad ilmu fan, ijod,<br>
              Shuhrating porlasin toki bor jahon!<br>
              <br>
              <strong>Naqorat:</strong><br>
              Oltin bu vodiylar — jon O‘zbekiston,<br>
              Ajdodlar mardona ruhi senga yor!<br>
              Ulug‘ xalq qudrati jo‘sh urgan zamon,<br>
              Olamni mahliyo aylagan diyor!<br>
              <br>
              Bag‘ri keng o‘zbekning o‘chmas iymoni,<br>
              Erkin, yosh avlodlar senga zo‘r qanot!<br>
              Istiqlol mash'ali, tinchlik posboni,<br>
              Haqsevar, ona yurt, mangu bo‘l obod!
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-backdrop {
  position: fixed;
  inset: 0;
  background-color: rgba(15, 23, 42, 0.65);
  backdrop-filter: blur(4px);
  z-index: 2000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.symbols-modal-container {
  width: 100%;
  max-width: 600px;
  background-color: #ffffff;
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-xl);
  overflow: hidden;
  border: 1px solid var(--border-color);
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 18px 24px;
  background-color: var(--color-primary-dark);
  color: #ffffff;
}

.modal-title {
  font-size: 1.15rem;
  font-weight: 700;
}

.modal-close-btn {
  color: #ffffff;
  opacity: 0.8;
  display: flex;
  align-items: center;
}

.modal-close-btn svg {
  width: 20px;
  height: 20px;
}

.symbols-tabs {
  display: flex;
  border-bottom: 1px solid var(--border-color);
  background-color: var(--color-gray-100);
}

.tab-btn {
  flex: 1;
  padding: 12px;
  font-size: 0.88rem;
  font-weight: 700;
  color: var(--color-gray-600);
  border-bottom: 3px solid transparent;
  transition: all var(--transition-fast);
}

.tab-btn.active {
  background-color: #ffffff;
  color: var(--color-primary);
  border-bottom-color: var(--color-primary);
}

.modal-body {
  padding: 28px;
  max-height: 480px;
  overflow-y: auto;
}

.symbol-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.flag-visual {
  width: 240px;
  height: 120px;
  border: 1px solid #cbd5e1;
  border-radius: var(--radius-sm);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  margin-bottom: 18px;
  box-shadow: var(--shadow-md);
}

.flag-stripe {
  flex: 1;
}

.stripe-blue { background-color: #0099b8; }
.stripe-white { background-color: #ffffff; }
.stripe-green { background-color: #1eb53a; }
.margin-red { height: 4px; background-color: #d62626; }

.coat-visual {
  margin-bottom: 18px;
}

.coat-img {
  width: 100px;
  height: 100px;
  object-fit: contain;
}

.symbol-subtitle {
  font-size: 1.15rem;
  font-weight: 800;
  color: var(--color-primary-dark);
  margin-bottom: 8px;
}

.anthem-author {
  font-size: 0.84rem;
  color: var(--color-accent-dark);
  font-weight: 600;
  margin-bottom: 16px;
}

.symbol-desc {
  font-size: 0.9rem;
  color: var(--color-gray-600);
  line-height: 1.6;
  text-align: justify;
}

.anthem-lyrics {
  font-size: 0.92rem;
  color: var(--color-gray-800);
  line-height: 1.7;
  text-align: center;
  background-color: var(--color-gray-50);
  padding: 16px 24px;
  border-radius: var(--radius-md);
  width: 100%;
}
</style>
