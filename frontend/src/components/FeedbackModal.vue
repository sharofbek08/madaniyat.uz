<script setup>
import { ref } from 'vue'

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  },
  selectedText: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['close'])

const comment = ref('')
const isSent = ref(false)

const sendFeedback = () => {
  isSent.value = true
  setTimeout(() => {
    isSent.value = false
    comment.value = ''
    emit('close')
  }, 2000)
}
</script>

<template>
  <div v-if="isOpen" class="modal-backdrop" @click.self="emit('close')">
    <div class="feedback-modal-container">
      <div class="modal-header">
        <h3 class="modal-title">Matnda xatolik haqida xabar berish</h3>
        <button class="modal-close-btn" @click="emit('close')" aria-label="Yopish">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="18" y1="6" x2="6" y2="18"/>
            <line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>
      </div>

      <div class="modal-body">
        <div v-if="!isSent">
          <div class="selected-text-box">
            <span class="box-label">Xatolik aniqlangan matn:</span>
            <p class="quoted-text">"{{ selectedText || 'Madaniyat vazirligi rasmiy portali' }}"</p>
          </div>

          <div class="comment-field">
            <label class="comment-label">Izohingiz (ixtiyoriy):</label>
            <textarea 
              v-model="comment" 
              placeholder="To‘g‘ri variantni yoki xatolik haqida izohingizni yozing..."
              class="comment-textarea"
              rows="3"
            ></textarea>
          </div>

          <div class="actions-row">
            <button class="cancel-btn" @click="emit('close')">Bekor qilish</button>
            <button class="submit-btn" @click="sendFeedback">
              <span>Xabarni yuborish</span>
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="22" y1="2" x2="11" y2="13"/>
                <polygon points="22 2 15 22 11 13 2 9 22 2"/>
              </svg>
            </button>
          </div>
        </div>

        <div v-else class="sent-success-box">
          <div class="success-icon">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/>
              <polyline points="22 4 12 14.01 9 11.01"/>
            </svg>
          </div>
          <h4 class="success-title">Rahmat!</h4>
          <p class="success-desc">Xabaringiz ma'muriyatga muvaffaqiyatli yetkazildi.</p>
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

.feedback-modal-container {
  width: 100%;
  max-width: 520px;
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
  font-size: 1.1rem;
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

.modal-body {
  padding: 24px;
}

.selected-text-box {
  background-color: var(--color-gray-100);
  border-left: 4px solid var(--color-primary);
  padding: 12px 16px;
  border-radius: 0 var(--radius-md) var(--radius-md) 0;
  margin-bottom: 18px;
}

.box-label {
  display: block;
  font-size: 0.75rem;
  color: var(--color-gray-500);
  font-weight: 700;
  text-transform: uppercase;
  margin-bottom: 4px;
}

.quoted-text {
  font-size: 0.95rem;
  color: var(--color-gray-800);
  font-style: italic;
  line-height: 1.4;
}

.comment-field {
  margin-bottom: 20px;
}

.comment-label {
  display: block;
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--color-gray-700);
  margin-bottom: 6px;
}

.comment-textarea {
  width: 100%;
  padding: 10px 14px;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  font-size: 0.9rem;
  font-family: inherit;
  color: var(--color-gray-800);
  resize: vertical;
}

.comment-textarea:focus {
  outline: none;
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px var(--color-primary-50);
}

.actions-row {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 12px;
}

.cancel-btn {
  padding: 10px 18px;
  font-size: 0.88rem;
  font-weight: 600;
  color: var(--color-gray-600);
  border-radius: var(--radius-md);
}

.submit-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background-color: var(--color-primary);
  color: #ffffff;
  padding: 10px 22px;
  border-radius: var(--radius-md);
  font-size: 0.88rem;
  font-weight: 600;
  transition: all var(--transition-fast);
}

.submit-btn:hover {
  background-color: var(--color-primary-dark);
}

.submit-btn svg {
  width: 16px;
  height: 16px;
}

.sent-success-box {
  text-align: center;
  padding: 30px 10px;
}

.success-icon {
  width: 56px;
  height: 56px;
  border-radius: var(--radius-full);
  background-color: var(--color-success-bg);
  color: var(--color-success);
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 16px;
}

.success-icon svg {
  width: 28px;
  height: 28px;
}

.success-title {
  font-size: 1.3rem;
  font-weight: 800;
  color: var(--color-gray-900);
  margin-bottom: 6px;
}

.success-desc {
  font-size: 0.92rem;
  color: var(--color-gray-600);
}
</style>
