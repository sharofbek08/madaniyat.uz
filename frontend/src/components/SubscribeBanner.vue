<script setup>
import { ref } from 'vue'

const email = ref('')
const isSubmitted = ref(false)

const handleSubscribe = () => {
  if (!email.value || !email.value.includes('@')) return
  isSubmitted.value = true
  setTimeout(() => {
    email.value = ''
    setTimeout(() => {
      isSubmitted.value = false
    }, 4000)
  }, 1000)
}
</script>

<template>
  <section class="subscribe-banner-section">
    <div class="container">
      <div class="subscribe-banner-card">
        <div class="subscribe-text-group">
          <div class="mail-icon-bubble">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
              <polyline points="22,6 12,13 2,6"/>
            </svg>
          </div>
          <div>
            <h3 class="subscribe-title">Yangiliklardan xabardor bo‘ling!</h3>
            <p class="subscribe-desc">Madaniyat sohasidagi eng muhim xabarlar, teatr premyeralari va e'lonlarni elektron pochtangizda qabul qiling.</p>
          </div>
        </div>

        <!-- Form / Success message -->
        <div class="subscribe-action-area">
          <form v-if="!isSubmitted" @submit.prevent="handleSubscribe" class="subscribe-form">
            <input 
              v-model="email"
              type="email" 
              placeholder="Elektron pochtangizni kiriting..."
              required
              class="subscribe-input"
            />
            <button type="submit" class="subscribe-btn">
              <span>Obuna bo‘lish</span>
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <line x1="22" y1="2" x2="11" y2="13"/>
                <polygon points="22 2 15 22 11 13 2 9 22 2"/>
              </svg>
            </button>
          </form>

          <div v-else class="subscribed-success">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/>
              <polyline points="22 4 12 14.01 9 11.01"/>
            </svg>
            <span>Obunangiz muvaffaqiyatli qabul qilindi!</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.subscribe-banner-section {
  padding-top: 10px;
  padding-bottom: 30px;
}

.subscribe-banner-card {
  background: linear-gradient(135deg, var(--color-primary-dark), var(--color-primary));
  border-radius: var(--radius-lg);
  padding: 36px 40px;
  color: #ffffff;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 32px;
  box-shadow: var(--shadow-lg);
  position: relative;
  overflow: hidden;
}

.subscribe-banner-card::before {
  content: '';
  position: absolute;
  right: -50px;
  top: -50px;
  width: 250px;
  height: 250px;
  border-radius: var(--radius-full);
  background: radial-gradient(circle, rgba(197, 160, 89, 0.25) 0%, transparent 70%);
  pointer-events: none;
}

.subscribe-text-group {
  display: flex;
  align-items: center;
  gap: 20px;
  max-width: 600px;
}

.mail-icon-bubble {
  width: 56px;
  height: 56px;
  border-radius: var(--radius-full);
  background: rgba(255, 255, 255, 0.15);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.mail-icon-bubble svg {
  width: 28px;
  height: 28px;
  color: var(--color-accent);
}

.subscribe-title {
  font-size: 1.45rem;
  font-weight: 800;
  color: #ffffff;
  line-height: 1.25;
  margin-bottom: 6px;
}

.subscribe-desc {
  font-size: 0.92rem;
  color: #d8e8f8;
  line-height: 1.5;
}

.subscribe-action-area {
  flex-shrink: 0;
  min-width: 380px;
}

.subscribe-form {
  display: flex;
  align-items: center;
  background-color: #ffffff;
  border-radius: var(--radius-full);
  padding: 6px 6px 6px 20px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
}

.subscribe-input {
  border: none;
  outline: none;
  font-size: 0.92rem;
  color: var(--color-gray-800);
  background: transparent;
  flex: 1;
}

.subscribe-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background-color: var(--color-primary);
  color: #ffffff;
  font-weight: 700;
  font-size: 0.88rem;
  padding: 10px 22px;
  border-radius: var(--radius-full);
  transition: all var(--transition-fast);
}

.subscribe-btn:hover {
  background-color: var(--color-primary-dark);
}

.subscribe-btn svg {
  width: 15px;
  height: 15px;
}

.subscribed-success {
  display: flex;
  align-items: center;
  gap: 10px;
  background: rgba(255, 255, 255, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.3);
  padding: 12px 24px;
  border-radius: var(--radius-full);
  font-weight: 700;
  color: #ffffff;
}

.subscribed-success svg {
  width: 20px;
  height: 20px;
  color: #4ade80;
}

@media (max-width: 992px) {
  .subscribe-banner-card {
    flex-direction: column;
    align-items: flex-start;
    padding: 28px;
  }
  .subscribe-action-area {
    width: 100%;
    min-width: auto;
  }
}

@media (max-width: 640px) {
  .subscribe-form {
    flex-direction: column;
    padding: 10px;
    border-radius: var(--radius-lg);
  }
  .subscribe-input {
    width: 100%;
    padding: 8px 10px;
    margin-bottom: 8px;
  }
  .subscribe-btn {
    width: 100%;
    justify-content: center;
  }
}
</style>
