<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  data: {
    type: Object,
    required: true
  }
})

const activeTab = ref('poll') // 'vacancies', 'documents', 'poll'

// Poll state
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
  <section class="home-section vac-doc-poll-section">
    <div class="container">
      <!-- Section Tabs Navigation -->
      <div class="tab-header-wrapper">
        <div class="tabs-container">
          <button 
            class="main-tab-btn"
            :class="{ active: activeTab === 'vacancies' }"
            @click="activeTab = 'vacancies'"
          >
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <rect x="2" y="7" width="20" height="14" rx="2" ry="2"/>
              <path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"/>
            </svg>
            <span>Bo‘sh ish o‘rinlari</span>
            <span class="tab-badge">{{ data.vacancies?.length || 0 }}</span>
          </button>

          <button 
            class="main-tab-btn"
            :class="{ active: activeTab === 'documents' }"
            @click="activeTab = 'documents'"
          >
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
              <polyline points="14 2 14 8 20 8"/>
              <line x1="16" y1="13" x2="8" y2="13"/>
              <line x1="16" y1="17" x2="8" y2="17"/>
            </svg>
            <span>Hujjatlar</span>
            <span class="tab-badge">{{ data.documents?.length || 0 }}</span>
          </button>

          <button 
            class="main-tab-btn"
            :class="{ active: activeTab === 'poll' }"
            @click="activeTab = 'poll'"
          >
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M18 20V10"/>
              <path d="M12 20V4"/>
              <path d="M6 20v-6"/>
            </svg>
            <span>So‘rovnoma</span>
            <span class="tab-badge pulse-badge">Faol</span>
          </button>
        </div>
      </div>

      <!-- Tab Content Cards -->
      <div class="tab-content-area">
        <!-- 1. Vacancies Tab -->
        <div v-if="activeTab === 'vacancies'" class="tab-pane">
          <div class="vacancies-grid">
            <div v-for="vac in data.vacancies" :key="vac.id" class="vacancy-card">
              <div class="vac-header">
                <span class="vac-dept">{{ vac.department }}</span>
                <span class="vac-location">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                    <circle cx="12" cy="10" r="3"/>
                  </svg>
                  {{ vac.location }}
                </span>
              </div>

              <h3 class="vac-title">{{ vac.title }}</h3>

              <div class="vac-meta">
                <div class="meta-row">
                  <span class="meta-label">Ish haqi:</span>
                  <span class="meta-val salary-text">{{ vac.salary }}</span>
                </div>
                <div class="meta-row">
                  <span class="meta-label">Muddati:</span>
                  <span class="meta-val deadline-text">{{ vac.deadline }} gacha</span>
                </div>
              </div>

              <a :href="vac.url" target="_blank" rel="noopener" class="apply-btn">
                <span>Ariza topshirish (Argos)</span>
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <line x1="5" y1="12" x2="19" y2="12"/>
                  <polyline points="12 5 19 12 12 19"/>
                </svg>
              </a>
            </div>
          </div>
        </div>

        <!-- 2. Documents Tab -->
        <div v-else-if="activeTab === 'documents'" class="tab-pane">
          <div class="documents-list">
            <div v-for="doc in data.documents" :key="doc.id" class="document-item-card">
              <div class="doc-icon-box">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
                  <polyline points="14 2 14 8 20 8"/>
                </svg>
              </div>

              <div class="doc-details">
                <div class="doc-top-row">
                  <span class="doc-number-badge">{{ doc.number }}</span>
                  <span class="doc-type-badge">{{ doc.type }}</span>
                  <span class="doc-date">{{ doc.date }}</span>
                </div>
                <h3 class="doc-title">{{ doc.title }}</h3>
              </div>

              <a :href="doc.url" target="_blank" rel="noopener" class="doc-download-btn" title="Hujjatni o‘qish / yuklab olish (Lex.uz)">
                <span>Lex.uz</span>
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/>
                  <polyline points="15 3 21 3 21 9"/>
                  <line x1="10" y1="14" x2="21" y2="3"/>
                </svg>
              </a>
            </div>
          </div>
        </div>

        <!-- 3. Poll Tab -->
        <div v-else-if="activeTab === 'poll'" class="tab-pane">
          <div class="poll-box-card">
            <div class="poll-header">
              <span class="poll-badge badge badge-primary">Fuqarolar fikri</span>
              <h3 class="poll-question">{{ pollData.title }}</h3>
              <p class="poll-info">Jami berilgan ovozlar soni: <strong>{{ totalVotes }} ta</strong></p>
            </div>

            <div class="poll-options-list">
              <div 
                v-for="ans in pollData.answers" 
                :key="ans.id"
                class="poll-option-item"
                :class="{ 
                  selected: selectedAnswerId === ans.id && !hasVoted,
                  voted: hasVoted
                }"
                @click="!hasVoted && (selectedAnswerId = ans.id)"
              >
                <!-- Radio Selector (Before Voting) -->
                <div v-if="!hasVoted" class="radio-circle" :class="{ checked: selectedAnswerId === ans.id }">
                  <div v-if="selectedAnswerId === ans.id" class="radio-inner"></div>
                </div>

                <div class="poll-option-content">
                  <span class="option-title">{{ ans.title }}</span>

                  <!-- Results Progress Bar (After Voting or always visible as stats) -->
                  <div v-if="hasVoted" class="poll-progress-container">
                    <div 
                      class="poll-progress-bar" 
                      :style="{ width: `${getPercentage(ans.count)}%` }"
                    ></div>
                    <span class="poll-percent-text">{{ getPercentage(ans.count) }}% ({{ ans.count }} ovoz)</span>
                  </div>
                </div>
              </div>
            </div>

            <!-- Poll Actions -->
            <div class="poll-footer-actions">
              <button 
                v-if="!hasVoted"
                class="vote-submit-btn" 
                :disabled="!selectedAnswerId"
                @click="handleVote"
              >
                <span>Ovoz berish</span>
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <polyline points="20 6 9 17 4 12"/>
                </svg>
              </button>

              <div v-else class="vote-success-message">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/>
                  <polyline points="22 4 12 14.01 9 11.01"/>
                </svg>
                <span>Ovozingiz muvaffaqiyatli qabul qilindi. Rahmat!</span>
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
  padding-top: 36px;
  padding-bottom: 36px;
}

.tab-header-wrapper {
  margin-bottom: 24px;
}

.tabs-container {
  display: flex;
  align-items: center;
  gap: 12px;
  background-color: var(--color-gray-100);
  padding: 6px;
  border-radius: var(--radius-lg);
  max-width: fit-content;
}

.main-tab-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 10px 20px;
  border-radius: var(--radius-md);
  font-size: 0.95rem;
  font-weight: 700;
  color: var(--color-gray-600);
  transition: all var(--transition-fast);
}

.main-tab-btn svg {
  width: 18px;
  height: 18px;
}

.main-tab-btn:hover {
  color: var(--color-primary);
}

.main-tab-btn.active {
  background-color: #ffffff;
  color: var(--color-primary-dark);
  box-shadow: var(--shadow-sm);
}

.tab-badge {
  font-size: 0.75rem;
  font-weight: 700;
  padding: 2px 8px;
  border-radius: var(--radius-full);
  background-color: var(--color-gray-200);
  color: var(--color-gray-700);
}

.main-tab-btn.active .tab-badge {
  background-color: var(--color-primary-50);
  color: var(--color-primary);
}

.pulse-badge {
  background-color: var(--color-success-bg) !important;
  color: var(--color-success) !important;
}

/* Vacancies */
.vacancies-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}

.vacancy-card {
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: 24px;
  display: flex;
  flex-direction: column;
  transition: all var(--transition-normal);
  box-shadow: var(--shadow-sm);
}

.vacancy-card:hover {
  transform: translateY(-4px);
  border-color: var(--color-primary);
  box-shadow: var(--shadow-lg);
}

.vac-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 12px;
  font-size: 0.78rem;
}

.vac-dept {
  color: var(--color-accent-dark);
  font-weight: 700;
  text-transform: uppercase;
}

.vac-location {
  display: flex;
  align-items: center;
  gap: 4px;
  color: var(--color-gray-500);
}

.vac-location svg {
  width: 14px;
  height: 14px;
}

.vac-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--color-gray-900);
  line-height: 1.35;
  margin-bottom: 16px;
  flex: 1;
}

.vac-meta {
  background-color: var(--color-gray-50);
  border-radius: var(--radius-md);
  padding: 12px;
  margin-bottom: 18px;
}

.meta-row {
  display: flex;
  justify-content: space-between;
  font-size: 0.84rem;
  padding: 3px 0;
}

.meta-label {
  color: var(--color-gray-500);
}

.salary-text {
  font-weight: 700;
  color: var(--color-primary);
}

.deadline-text {
  font-weight: 600;
  color: var(--color-danger);
}

.apply-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  background-color: var(--color-primary);
  color: #ffffff;
  padding: 10px 16px;
  border-radius: var(--radius-md);
  font-size: 0.88rem;
  font-weight: 600;
  transition: all var(--transition-fast);
}

.apply-btn:hover {
  background-color: var(--color-primary-dark);
}

.apply-btn svg {
  width: 16px;
  height: 16px;
}

/* Documents */
.documents-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.document-item-card {
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  padding: 18px 22px;
  display: flex;
  align-items: center;
  gap: 20px;
  transition: all var(--transition-fast);
}

.document-item-card:hover {
  border-color: var(--color-primary);
  box-shadow: var(--shadow-sm);
  transform: translateX(4px);
}

.doc-icon-box {
  width: 44px;
  height: 44px;
  border-radius: var(--radius-sm);
  background-color: var(--color-primary-50);
  color: var(--color-primary);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.doc-icon-box svg {
  width: 22px;
  height: 22px;
}

.doc-details {
  flex: 1;
}

.doc-top-row {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 6px;
  font-size: 0.78rem;
}

.doc-number-badge {
  font-weight: 700;
  color: var(--color-primary);
  background-color: var(--color-primary-50);
  padding: 2px 8px;
  border-radius: var(--radius-sm);
}

.doc-type-badge {
  color: var(--color-accent-dark);
  font-weight: 600;
}

.doc-date {
  color: var(--color-gray-400);
}

.doc-title {
  font-size: 0.98rem;
  font-weight: 600;
  color: var(--color-gray-800);
  line-height: 1.4;
}

.doc-download-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  background-color: var(--color-gray-100);
  color: var(--color-gray-700);
  font-size: 0.82rem;
  font-weight: 600;
  padding: 8px 14px;
  border-radius: var(--radius-full);
  transition: all var(--transition-fast);
}

.doc-download-btn:hover {
  background-color: var(--color-primary);
  color: #ffffff;
}

.doc-download-btn svg {
  width: 14px;
  height: 14px;
}

/* Poll Box */
.poll-box-card {
  background-color: #ffffff;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: 32px;
  max-width: 800px;
  box-shadow: var(--shadow-sm);
}

.poll-header {
  margin-bottom: 24px;
}

.poll-badge {
  margin-bottom: 10px;
}

.poll-question {
  font-size: 1.3rem;
  font-weight: 800;
  color: var(--color-primary-dark);
  line-height: 1.35;
  margin-bottom: 8px;
}

.poll-info {
  font-size: 0.88rem;
  color: var(--color-gray-500);
}

.poll-options-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-bottom: 24px;
}

.poll-option-item {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 16px 20px;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  background-color: var(--color-gray-50);
  cursor: pointer;
  transition: all var(--transition-fast);
}

.poll-option-item:hover:not(.voted) {
  border-color: var(--color-primary);
  background-color: #ffffff;
}

.poll-option-item.selected {
  border-color: var(--color-primary);
  background-color: var(--color-primary-50);
}

.radio-circle {
  width: 20px;
  height: 20px;
  border: 2px solid var(--color-gray-400);
  border-radius: var(--radius-full);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  transition: border-color var(--transition-fast);
}

.radio-circle.checked {
  border-color: var(--color-primary);
}

.radio-inner {
  width: 10px;
  height: 10px;
  background-color: var(--color-primary);
  border-radius: var(--radius-full);
}

.poll-option-content {
  flex: 1;
}

.option-title {
  display: block;
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--color-gray-800);
}

.poll-progress-container {
  margin-top: 8px;
  position: relative;
  background-color: var(--color-gray-200);
  height: 18px;
  border-radius: var(--radius-full);
  overflow: hidden;
}

.poll-progress-bar {
  background: linear-gradient(90deg, var(--color-primary), var(--color-accent));
  height: 100%;
  border-radius: var(--radius-full);
  transition: width 0.8s ease;
}

.poll-percent-text {
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
  font-size: 0.72rem;
  font-weight: 700;
  color: var(--color-gray-700);
}

.vote-submit-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background-color: var(--color-primary);
  color: #ffffff;
  padding: 12px 28px;
  border-radius: var(--radius-full);
  font-weight: 700;
  font-size: 0.95rem;
  transition: all var(--transition-fast);
}

.vote-submit-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.vote-submit-btn:not(:disabled):hover {
  background-color: var(--color-primary-dark);
  transform: translateY(-2px);
}

.vote-success-message {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  background-color: var(--color-success-bg);
  color: var(--color-success);
  font-weight: 600;
  padding: 12px 20px;
  border-radius: var(--radius-md);
  font-size: 0.9rem;
}

.vote-success-message svg {
  width: 20px;
  height: 20px;
}

@media (max-width: 992px) {
  .vacancies-grid {
    grid-template-columns: 1fr;
  }
}
</style>
