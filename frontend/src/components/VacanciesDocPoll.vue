<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  data: {
    type: Object,
    required: true
  }
})

// Poll State
const pollData = ref({ ...props.data.poll })
const selectedAnswerId = ref(null)
const hasVoted = ref(false)

const totalVotes = computed(() => {
  return pollData.value.answers.reduce((sum, a) => sum + Number(a.count), 0)
})

const handleVote = () => {
  if (!selectedAnswerId.value || hasVoted.value) return
  const ans = pollData.value.answers.find(a => a.id === selectedAnswerId.value)
  if (ans) {
    ans.count = Number(ans.count) + 1
    hasVoted.value = true
  }
}

const getPercentage = (count) => {
  if (totalVotes.value === 0) return 0
  return Math.round((Number(count) / totalVotes.value) * 100)
}
</script>

<template>
  <section class="vac-doc-poll-section py-5">
    <div class="container">
      <div class="three-stack_three-stack__np7ES">
        <!-- 1. Hujjatlar (Col 1) -->
        <div class="stack-col flex flex-col" data-bordered="1">
          <div class="stack-header">
            <h2 class="font--lg font-bold uppercase" style="color: var(--color-text-primary)">
              Hujjatlar
            </h2>
            <a 
              href="https://lex.uz/uz/search/nat?body_id=819" 
              target="_blank" 
              rel="noopener"
              class="details-link_link__EkQDj"
            >
              Barcha hujjatlar
              <span>
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" class="to-details_svg__feather to-details_svg__feather-arrow-up-right" viewBox="0 0 24 24">
                  <path d="M7 17 17 7M7 7h10v10"></path>
                </svg>
              </span>
            </a>
          </div>

          <!-- Documents List -->
          <div class="documents-list">
            <a 
              v-for="doc in data.documents" 
              :key="doc.id" 
              :href="doc.url || 'https://lex.uz'" 
              target="_blank" 
              rel="noopener"
              class="documents_box__bGa_5"
            >
              <div class="doc-icon-wrap">
                <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                  <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
                  <polyline points="14 2 14 8 20 8"/>
                  <line x1="16" y1="13" x2="8" y2="13"/>
                  <line x1="16" y1="17" x2="8" y2="17"/>
                  <polyline points="10 9 9 9 8 9"/>
                </svg>
              </div>

              <div class="doc-info">
                <h3>{{ doc.title }}</h3>
                <p>
                  <span>{{ doc.type }} • </span>
                  <span class="doc-num">{{ doc.number }} • </span>
                  <span class="doc-date">{{ doc.date }}</span>
                </p>
              </div>

              <div class="doc-arrow">
                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M7 17 17 7M7 7h10v10"></path>
                </svg>
              </div>
            </a>
          </div>
        </div>

        <!-- 2. So‘rovnomalar (Col 2) -->
        <div class="stack-col flex flex-col" data-bordered="2">
          <div class="stack-header">
            <h2 class="font--lg font-bold uppercase" style="color: var(--color-text-primary)">
              So‘rovnomalar
            </h2>
            <a href="#/madaniyat/survey" class="details-link_link__EkQDj">
              Barcha so‘rovnomalar
              <span>
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24">
                  <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 12h10m0 0-4-4m4 4-4 4"/>
                </svg>
              </span>
            </a>
          </div>

          <!-- Poll Card -->
          <div class="poll-box">
            <h3 class="poll-question">{{ pollData.title }}</h3>

            <div class="poll-options">
              <label 
                v-for="ans in pollData.answers" 
                :key="ans.id" 
                class="poll_poll__radio__dnUgP"
                :class="{ 'selected': selectedAnswerId === ans.id }"
              >
                <div class="radio-label-group">
                  <input 
                    type="radio" 
                    name="home_poll" 
                    :value="ans.id" 
                    v-model="selectedAnswerId" 
                    :disabled="hasVoted"
                    class="poll-radio-input"
                  />
                  <span class="poll_poll__label__9l8QC">{{ ans.title }}</span>
                </div>

                <!-- Percentage indicator if voted -->
                <div v-if="hasVoted" class="poll-stat">
                  <span class="percent-text">{{ getPercentage(ans.count) }}%</span>
                </div>
              </label>
            </div>

            <!-- Progress bars if voted -->
            <div v-if="hasVoted" class="voted-progress-list">
              <div v-for="ans in pollData.answers" :key="'prog-' + ans.id" class="prog-item">
                <div class="prog-head">
                  <span class="prog-title">{{ ans.title }}</span>
                  <span class="prog-val">{{ getPercentage(ans.count) }}% ({{ ans.count }} ovoz)</span>
                </div>
                <div class="prog-bar-bg">
                  <div class="prog-bar-fill" :style="{ width: `${getPercentage(ans.count)}%` }"></div>
                </div>
              </div>
            </div>

            <!-- Vote Action Button -->
            <div class="poll-actions">
              <button 
                type="button" 
                class="poll_poll__btn__yb12B" 
                :disabled="!selectedAnswerId || hasVoted"
                @click="handleVote"
              >
                {{ hasVoted ? 'Ovozingiz qabul qilindi' : 'Ovoz berish' }}
              </button>

              <div class="total-votes-count">
                Jami ovozlar soni: <strong>{{ totalVotes }}</strong>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.vac-doc-poll-section {
  padding-top: 2rem;
  padding-bottom: 2.5rem;
  background-color: #fafbfc;
  border-top: 1px solid #f1f3f5;
  border-bottom: 1px solid #f1f3f5;
}

.stack-col {
  display: flex;
  flex-direction: column;
}

.stack-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-bottom: 1.25rem;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.documents-list {
  display: flex;
  flex-direction: column;
  gap: 0.875rem;
}

.documents_box__bGa_5 {
  border: 1px solid var(--color-border);
  transition: var(--transition);
  padding: 1rem 1.25rem;
  background-color: #ffffff;
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  text-decoration: none;
  border-radius: 2px;
}

.documents_box__bGa_5:hover {
  border-color: var(--color-text-primary);
  box-shadow: 0 4px 12px rgba(1, 61, 140, 0.08);
}

.doc-icon-wrap {
  color: #adb5bd;
  flex-shrink: 0;
  margin-top: 2px;
  transition: color 0.15s ease;
}

.documents_box__bGa_5:hover .doc-icon-wrap {
  color: var(--color-primary);
}

.doc-info {
  flex: 1;
  min-width: 0;
}

.doc-info h3 {
  font-size: 0.875rem;
  font-weight: 600;
  color: #222222;
  line-height: 1.35;
  margin: 0;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  transition: color 0.15s linear;
}

.documents_box__bGa_5:hover .doc-info h3 {
  color: var(--color-text-primary);
}

.doc-info p {
  font-size: 0.75rem;
  color: #868e96;
  margin-top: 4px;
}

.doc-arrow {
  color: #ced4da;
  flex-shrink: 0;
  transition: all 0.2s;
}

.documents_box__bGa_5:hover .doc-arrow {
  color: var(--color-primary);
  transform: translate(2px, -2px);
}

/* Poll Box */
.poll-box {
  background-color: #ffffff;
  border: 1px solid var(--color-border);
  padding: 1.5rem;
  border-radius: 2px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.03);
}

.poll-question {
  font-size: 1rem;
  font-weight: 700;
  color: #222222;
  line-height: 1.35;
  margin-bottom: 1.25rem;
}

.poll-options {
  display: flex;
  flex-direction: column;
}

.poll_poll__radio__dnUgP {
  border-bottom: 1px solid var(--color-border);
  padding-top: 0.75rem;
  padding-bottom: 0.75rem;
  transition: var(--transition);
  display: flex;
  align-items: center;
  justify-content: space-between;
  cursor: pointer;
}

.poll_poll__radio__dnUgP:hover {
  border-bottom-color: var(--color-primary);
}

.radio-label-group {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.poll-radio-input {
  width: 1.125rem;
  height: 1.125rem;
  accent-color: var(--color-primary);
  cursor: pointer;
}

.poll_poll__label__9l8QC {
  font-size: 0.875rem;
  color: #495057;
  line-height: 1.3;
}

.poll_poll__radio__dnUgP.selected .poll_poll__label__9l8QC {
  color: var(--color-primary);
  font-weight: 600;
}

.percent-text {
  font-size: 0.875rem;
  font-weight: 700;
  color: var(--color-primary);
}

.voted-progress-list {
  margin-top: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.prog-item {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.prog-head {
  display: flex;
  justify-content: space-between;
  font-size: 0.75rem;
  color: #495057;
}

.prog-val {
  font-weight: 600;
  color: var(--color-primary);
}

.prog-bar-bg {
  height: 6px;
  background-color: #e9ecef;
  border-radius: 9999px;
  overflow: hidden;
}

.prog-bar-fill {
  height: 100%;
  background-color: var(--color-primary);
  border-radius: 9999px;
  transition: width 0.5s ease-out;
}

.poll-actions {
  margin-top: 1.5rem;
}

.poll_poll__btn__yb12B {
  padding: 0.75rem 1.5rem;
  background-color: var(--color-primary) !important;
  font-size: 0.9375rem;
  font-weight: 600;
  color: #ffffff;
  border: 1px solid var(--color-primary);
  border-radius: 4px;
  transition: var(--transition);
  width: 100%;
  text-align: center;
  cursor: pointer;
}

.poll_poll__btn__yb12B:hover:not(:disabled) {
  background-color: #002860 !important;
  border-color: #002860;
}

.poll_poll__btn__yb12B:disabled {
  opacity: 0.65;
  cursor: not-allowed;
}

.total-votes-count {
  margin-top: 0.75rem;
  font-size: 0.75rem;
  color: #868e96;
  text-align: center;
}
</style>
