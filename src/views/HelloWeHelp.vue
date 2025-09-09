<template>
  <n-config-provider :theme="isDark ? darkTheme : null">
    <!-- Header -->
    <header class="site-header">
      <div class="left"> 
        <div class="brand">你的名字 • WeHelp</div>
      </div>
      <div class="right">
        <n-switch v-model:value="isDark" size="small">
          <template #checked-icon>🌙</template>
          <template #unchecked-icon>☀️</template>
        </n-switch>
      </div>
    </header>

    <!-- Hero -->
    <section class="hero" :class="{ dark: isDark }">
      <div class="hero-inner">
        <h1 class="hero-title">歡迎 — 我是 你的名字</h1>
        <p class="hero-sub">申請 WeHelp Academy｜前端工程師訓練生</p>
        <n-button type="primary" size="large" @click="scrollTo('intro')">開始閱讀</n-button>
      </div>
      <div class="hero-visual" aria-hidden>
        <!-- 裝飾性背景，模仿 Apple 科技感的大面積質感 -->
        <div class="vortex"></div>
      </div>
    </section>

    <!-- Intro -->
    <main class="main-content">
      <section ref="intro" class="intro-card">
        <n-card class="glass" title="個人簡介與申請動機">
          <div class="intro-grid">
            <div class="avatar-wrap">
              <div class="avatar">A</div>
            </div>
            <div>
              <h2 class="name">你的名字</h2>
              <p class="short">我目前是一名前端工程師/應屆畢業生，熱衷於寫乾淨、有可讀性的前端程式碼，擅長 Vue 3 與現代化 UI 實作。</p>
              <p class="motivation">我想參加 WeHelp Academy，因為期望在 6 個月密集實作中提升大型專案協作能力，以及加強深度學習模型在前端或後端的整合技能。</p>
            </div>
          </div>
        </n-card>
      </section>

      <!-- Q&A Cards -->
      <section class="qa-list">
        <n-space vertical size="large" class="qa-space">
          <n-card v-for="(q, idx) in qas" :key="idx" class="glass qa-card" hoverable>
            <n-collapse>
              <n-collapse-item :title="q.question">
                <div class="qa-answer">
                  <p v-html="q.answer"></p>
                </div>
              </n-collapse-item>
            </n-collapse>
          </n-card>
        </n-space>
      </section>

      <!-- Footer -->
      <footer class="site-footer">
        <div>© 2025 你的名字 • <a href="#">GitHub</a> • <a href="#">Email</a></div>
      </footer>
    </main>
  </n-config-provider>
</template>

<script setup>
import { ref } from 'vue'
import { darkTheme } from 'naive-ui'

const isDark = ref(true)
const intro = ref(null)

const scrollTo = (id) => {
  if (id === 'intro') {
    intro.value?.scrollIntoView({ behavior: 'smooth' })
  }
}

const qas = [
  {
    question: '目前的職業背景，若是剛畢業，畢業的科系為何？',
    answer: `我目前是一名前端工程師（或應屆畢業生）。
    我的科系為資訊工程 / 資訊管理。在大學期間我曾經參與校內專案與社團，負責前端介面與部署。`,
  },
  {
    question: '如果參與這個訓練，會怎麼安排學習時間？',
    answer: `我會採用每天固定學習與每週回顧的方式：<br>
    1) 平日：每日 4 小時（包含晨間練習、任務時間）。<br>
    2) 假日：每日 6 小時（專案實作、閱讀）。<br>
    3) 每週末與同儕進行一次 code review 與成果展示。`,
  },
  {
    question: '請描述一件產生明顯負面情緒的經歷，如何處理該情緒？',
    answer: `曾在一個專案中因為溝通不良導致功能延遲，當時感到焦慮與挫折。我先冷靜記錄問題、檢討溝通流程，並主動提出每日短會以同步進度。事後我學會把問題拆解為小任務，並和團隊建立更清晰的 API 與責任邊界。`,
  },
  {
    question: '最想使用自己開發的深度學習模型解決什麼問題？',
    answer: `我希望開發一個能協助自動化資料標註的模型，特別是在影像或語音的前處理上，降低標註成本，提升資料品質。這能加快模型訓練迭代速度，並協助小型研究團隊快速建立資料集。`,
  },
  {
    question: '若終究無法達到 OpenAI 的程度，為何要學習基礎模型的實作？',
    answer: `基礎模型的實作能讓我理解模型訓練、資料處理與評估的關鍵細節。即使無法達到最前沿水準，掌握這些底層能力也能幫助我在應用層面做出有效且可靠的產品，並在團隊中與研究工程師更順暢合作。`,
  },
  {
    question: '從上次提出申請至今，多做了哪些努力？',
    answer: `我在這段時間完成了以下事項：<br>
    • 完成 3 個前端專案（包含 SPA 與響應式設計）。<br>
    • 學習並實作 Vue 3 Composition API、單元測試與 CI/CD 流程。<br>
    • 練習簡單的 PyTorch 模型訓練並整理實驗記錄。`,
  },
  {
    question: '其他想要對我們說的事情？',
    answer: `感謝 WeHelp 提供這樣一個密集且貼近職場的訓練機會。我希望能進入一個重視實作與回饋的環境，與優秀的同儕一起成長。期待能在課程中貢獻我的工程實務經驗。`,
  },
]
</script>

<style scoped>
:root {
  --glass-bg: rgba(255,255,255,0.06);
  --glass-bg-dark: rgba(255,255,255,0.04);
}

* { box-sizing: border-box }
body,html,#app { height: 100%; margin: 0; font-family: Inter, ui-sans-serif, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial }

.site-header{
  position: fixed; top: 0; left: 0; right:0; height:64px; display:flex; align-items:center; justify-content:space-between; padding:0 24px; z-index:60;
  background: linear-gradient(180deg, rgba(0,0,0,0.12), rgba(0,0,0,0.04));
  backdrop-filter: blur(6px);
}
.site-header .brand{ font-weight:600 }

.hero{
  height:100vh; display:flex; align-items:center; justify-content:center; position:relative; overflow:hidden; color:white;
}
.hero.dark{ background: linear-gradient(120deg,#020617,#071330); }
.hero:not(.dark){ background: linear-gradient(120deg,#0f172a,#0b2447); }
.hero-inner{ z-index:4; text-align:center }
.hero-title{ font-size:48px; margin:0 0 12px }
.hero-sub{ margin:0 0 24px; opacity:0.9 }

.hero-visual{ position:absolute; inset:0; z-index:2; pointer-events:none }
.vortex{ position:absolute; width:1100px; height:1100px; right:-200px; top:-200px; background: radial-gradient(circle at 30% 30%, rgba(255,255,255,0.06), transparent 30%), radial-gradient(circle at 70% 70%, rgba(255,255,255,0.03), transparent 30%); transform: rotate(10deg); filter: blur(40px); }

.main-content{ padding:120px 20px 60px; background:transparent; }
.intro-card{ margin-bottom:36px }
.glass{ background: var(--glass-bg); border-radius:14px; box-shadow: 0 8px 30px rgba(2,6,23,0.6); padding:16px; color: #e6eef8 }

.intro-grid{ display:flex; gap:18px; align-items:center }
.avatar-wrap{ width:96px }
.avatar{ width:96px;height:96px;border-radius:50%; background:linear-gradient(135deg,#334155,#1e293b); display:flex;align-items:center;justify-content:center;font-size:34px;color:white }
.name{ margin:0 0 6px }
.short{ margin:0 0 8px; color: #cfe3ff }
.motivation{ margin:0; color:#a9c9ff }

.qa-list{ margin-top:8px }
.qa-space{ width:100% }
.qa-card{ padding:0 }
.qa-answer{ color:#dbeefe; line-height:1.8 }

.site-footer{ margin-top:48px; text-align:center; color:#9fb4d6 }

/* responsive */
@media (max-width:768px){
  .hero-title{ font-size:32px }
  .intro-grid{ flex-direction:row }
}
</style>
