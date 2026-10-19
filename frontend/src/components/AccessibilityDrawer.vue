<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['close'])

const fontScale = ref(1) // 0.9, 1, 1.15, 1.3
const viewMode = ref('normal') // 'normal', 'grayscale', 'contrast'
const isSpeaking = ref(false)

const setFontScale = (scale) => {
  fontScale.value = scale
  document.documentElement.style.setProperty('--font-scale', scale)
}

const setViewMode = (mode) => {
  viewMode.value = mode
  document.body.classList.remove('accessibility-grayscale', 'accessibility-contrast')
  if (mode === 'grayscale') {
    document.body.classList.add('accessibility-grayscale')
  } else if (mode === 'contrast') {
    document.body.classList.add('accessibility-contrast')
  }
}

const resetAll = () => {
  setFontScale(1)
  setViewMode('normal')
}

// Web Speech API demo
const speakDemo = () => {
  if (!('speechSynthesis' in window)) {
    alert('Brauzeringiz ovozli o‘qishni qo‘llab-quvvatlamaydi.')
    return
  }

  if (isSpeaking.value) {
    window.speechSynthesis.cancel()
    isSpeaking.value = false
    return
  }

  const text = "O‘zbekiston Respublikasi Madaniyat vazirligi rasmiy portaliga xush kelibsiz. Ushbu tizim zaif ko‘ruvchi foydalanuvchilar uchun matnni ovozli o‘qib berish imkoniyatiga ega."
  const utterance = new SpeechSynthesisUtterance(text)
  utterance.lang = 'uz-UZ'
  utterance.rate = 0.95

  utterance.onend = () => {
    isSpeaking.value = false
  }

  isSpeaking.value = true
  window.speechSynthesis.speak(utterance)
}
</script>

<template>
  <div v-if="isOpen" class="modal-backdrop" @click.self="emit('close')">
    <div class="accessibility-modal">
      <div class="modal-header">
        <div class="title-row">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="12" cy="12" r="3"/>
            <path d="M2 12s3-7 10-7 10 7 10 7-3 7-10 7-10-7-10-7z"/>
          </svg>
          <h3 class="modal-title">Maxsus imkoniyatlar</h3>
        </div>

        <button class="modal-close-btn" @click="emit('close')" aria-label="Yopish">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="18" y1="6" x2="6" y2="18"/>
            <line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>
      </div>

      <div class="modal-body">
        <!-- 1. Font Size Scaling -->
        <div class="control-group">
          <h4 class="group-label">Shrift o‘lchami:</h4>
          <div class="font-scale-buttons">
            <button 
              class="scale-btn" 
              :class="{ active: fontScale === 0.9 }"
              @click="setFontScale(0.9)"
            >
              A- (Kichik)
            </button>
            <button 
              class="scale-btn" 
              :class="{ active: fontScale === 1 }"
              @click="setFontScale(1)"
            >
              A (Standart)
            </button>
            <button 
              class="scale-btn" 
              :class="{ active: fontScale === 1.15 }"
              @click="setFontScale(1.15)"
            >
              A+ (Katta)
            </button>
            <button 
              class="scale-btn" 
              :class="{ active: fontScale === 1.3 }"
              @click="setFontScale(1.3)"
            >
              A++ (Maksimal)
            </button>
          </div>
        </div>

        <!-- 2. Display View Mode -->
        <div class="control-group">
          <h4 class="group-label">Ko‘rinish rejimi:</h4>
          <div class="view-mode-buttons">
            <button 
              class="mode-btn mode-normal" 
              :class="{ active: viewMode === 'normal' }"
              @click="setViewMode('normal')"
            >
              <span class="preview-box normal-preview"></span>
              <span>Oddiy ko‘rinish</span>
            </button>

            <button 
              class="mode-btn mode-grayscale" 
              :class="{ active: viewMode === 'grayscale' }"
              @click="setViewMode('grayscale')"
            >
              <span class="preview-box gray-preview"></span>
              <span>Rangsiz ko‘rinish</span>
            </button>

            <button 
              class="mode-btn mode-contrast" 
              :class="{ active: viewMode === 'contrast' }"
              @click="setViewMode('contrast')"
            >
              <span class="preview-box contrast-preview"></span>
              <span>Yuqori kontrast</span>
            </button>
          </div>
        </div>

        <!-- 3. Screen Reader (Ekran suhandoni) -->
        <div class="control-group">
          <h4 class="group-label">Ekran suhandoni (Ovozli o‘qish):</h4>
          <p class="reader-desc">
            “Ekran suhandoni” zaif ko‘radigan foydalanuvchilar uchun sahifadagi matnlarni ovozli o‘qib beradi.
          </p>

          <button class="speaker-btn" :class="{ playing: isSpeaking }" @click="speakDemo">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"/>
              <path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"/>
            </svg>
            <span>{{ isSpeaking ? 'O‘qishni to‘xtatish' : 'Namuna matnni eshitish' }}</span>
          </button>
        </div>
      </div>

      <!-- Footer Reset -->
      <div class="modal-footer">
        <button class="reset-btn" @click="resetAll">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8"/>
            <path d="M3 3v5h5"/>
          </svg>
          <span>Boshlang‘ich holatga qaytarish</span>
        </button>

        <button class="done-btn" @click="emit('close')">
          <span>Saqlash va yopish</span>
        </button>
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

.accessibility-modal {
  width: 100%;
  max-width: 540px;
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
  padding: 20px 24px;
  background-color: var(--color-primary-dark);
  color: #ffffff;
}

.title-row {
  display: flex;
  align-items: center;
  gap: 10px;
}

.title-row svg {
  width: 22px;
  height: 22px;
  color: var(--color-accent);
}

.modal-title {
  font-size: 1.2rem;
  font-weight: 700;
  color: #ffffff;
}

.modal-close-btn {
  color: #ffffff;
  opacity: 0.8;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: opacity var(--transition-fast);
}

.modal-close-btn:hover {
  opacity: 1;
}

.modal-close-btn svg {
  width: 20px;
  height: 20px;
}

.modal-body {
  padding: 24px;
  display: flex;
  flex-direction: column;
  gap: 22px;
}

.control-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.group-label {
  font-size: 0.92rem;
  font-weight: 700;
  color: var(--color-gray-800);
}

.font-scale-buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 8px;
}

.scale-btn {
  background-color: var(--color-gray-100);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: 10px 8px;
  font-weight: 600;
  font-size: 0.84rem;
  color: var(--color-gray-700);
  transition: all var(--transition-fast);
}

.scale-btn:hover {
  border-color: var(--color-primary);
  color: var(--color-primary);
}

.scale-btn.active {
  background-color: var(--color-primary);
  color: #ffffff;
  border-color: var(--color-primary);
}

.view-mode-buttons {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}

.mode-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  padding: 12px;
  border-radius: var(--radius-md);
  border: 1px solid var(--border-color);
  background-color: var(--color-gray-50);
  font-size: 0.82rem;
  font-weight: 600;
  color: var(--color-gray-700);
  transition: all var(--transition-fast);
}

.mode-btn:hover {
  border-color: var(--color-primary);
}

.mode-btn.active {
  border-color: var(--color-primary);
  background-color: var(--color-primary-50);
  color: var(--color-primary);
}

.preview-box {
  width: 36px;
  height: 24px;
  border-radius: 4px;
  border: 1px solid var(--border-color);
}

.normal-preview {
  background: linear-gradient(135deg, #013d8c, #c5a059);
}

.gray-preview {
  background: linear-gradient(135deg, #475569, #cbd5e1);
}

.contrast-preview {
  background: #000000;
  border-color: #ffff00;
}

.reader-desc {
  font-size: 0.84rem;
  color: var(--color-gray-500);
  line-height: 1.45;
}

.speaker-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background-color: var(--color-primary-50);
  color: var(--color-primary);
  border: 1px solid var(--color-primary-100);
  padding: 10px 18px;
  border-radius: var(--radius-full);
  font-size: 0.88rem;
  font-weight: 700;
  align-self: flex-start;
  transition: all var(--transition-fast);
}

.speaker-btn:hover {
  background-color: var(--color-primary);
  color: #ffffff;
}

.speaker-btn.playing {
  background-color: #dc2626;
  color: #ffffff;
  border-color: #dc2626;
}

.speaker-btn svg {
  width: 18px;
  height: 18px;
}

.modal-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 24px;
  background-color: var(--color-gray-50);
  border-top: 1px solid var(--border-color);
}

.reset-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-size: 0.82rem;
  font-weight: 600;
  color: var(--color-gray-600);
}

.reset-btn svg {
  width: 14px;
  height: 14px;
}

.reset-btn:hover {
  color: var(--color-danger);
}

.done-btn {
  background-color: var(--color-primary);
  color: #ffffff;
  padding: 8px 18px;
  border-radius: var(--radius-full);
  font-size: 0.88rem;
  font-weight: 600;
  transition: background-color var(--transition-fast);
}

.done-btn:hover {
  background-color: var(--color-primary-dark);
}
</style>
