<!-- 
標準的vue寫法:
<script setup></script> 
<templete></templete>
<style scope></style>

語法範例1:
  嵌入純文字:   {{ 變數名稱 }}
  嵌入屬性:     <div v-bing:屬性名稱="變數名稱">內文</div>
                <div :屬性名稱="變數名稱">內文</div>
  嵌入HTML標籤: <div v-html="變數名稱">

  常用變化:
    文字變換大寫 {{ 變數名稱.toUpperCase() }}
    搭配三元運算  :color="布林值變數?'red':'green'"

語法範例2:
  判斷式: <div v-if="變數名稱 >=< 2000"></div>
          <div v-else-if="判斷式"></div>
          <div v-else></div>

  迴圈:   <div v-for="變數 in 陣列變數">陣列中有 {{變數}}</div>
          <div v-for="(變數, 序數) in 陣列變數">陣列中第{{序數}}個是{{變數}}</div>
          <div v-for="(value, key) in 物件變數">物件中有{{key}}的值是{{value}}</div>

語法範例3:
  事件處理:  <button v-on:click="方法變數"></button>
            <button @click="方法變數"></button>
            <button @mouseover="方法變數"></button>
    修飾字:  <button @click.once="方法變數"></button> 僅觸發一次
            <a @click.prevent="方法變數" href="網址"></a> 停止預設行為(a 標籤點擊不會導向)

語法範例4:
  響應式: 
  <script set up>
    let 變數名稱 = ref("內容");
    let 方法名稱 = function(){變數名稱.value =  "新內容"};
  </script>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
  <templete>
   <div @click="方法名稱">{{ 變數名稱 }}</div>
  </templete>

  輸入元件響應:
  <script set up>
    let 變數名稱 = ref("內容");
    let gender = ref(null);
    let array = ref([]);
  </script>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
  <templete>
    <input type="text" v-model="變數名稱"></input>
    <span> {{變數名稱}} </span>

    <input type="radio" value="F" v-model="gender">
    <input type="radio" value="M" v-model="gender">
    <span> 性別: {{gender}} </span>

    <input type="checkbox" value="A" v-model="array">
    <input type="checkbox" value="B" v-model="array">
    <input type="checkbox" value="C" v-model="array">
    <span> 選擇的有: {{array}} </span>

    <select v-model="gender">
      <option value="" selected disabled>請選擇</option>
      <option value="M">男</option>
      <option value="F">女</option>
    </select>
    <span> 性別: {{gender}} </span>

  </templete>

  元件之間的傳遞:
  父元件標籤給予屬性，子元件的script中以 defineProps(["屬性名稱"])
  
  父元件.vue
  <script setup>
    import 子元件 from '子元件.vue'
  </script>
  <templete>
    <子元件 屬性名稱 = "屬性">
  </templete>

  子元件.vue
  <script setup>
    defineProps(["屬性名稱"])
  </script>
  <templete>
    <div> 傳遞進來的屬性為 {{ 屬性名稱 }} </div>
  </templete>


  自訂事件傳遞
  在子元件中使用 "$emit('自訂事件名稱')"觸發父元件事件

  父元件.vue
  <script setup>
    import 子元件 from '子元件.vue';
    let 函式名稱 = function(){};
  </script>
  <templete>
    <子元件 @自訂事件名稱 = "函式名稱">
  </templete>

  子元件.vue
  <templete>
    <button @click="$emit('自訂事件名稱')"> 點擊傳遞事件給父元素 </button>
  </templete>
  或是
  <script setup>
    let emit = defineEmits(["自訂事件名稱"])
    let 函式名稱 = function(){emit("自訂事件名稱")};
  </script>
  <templete>
    <button @click="函式名稱"> 點擊傳遞事件給父元素 </button>
  </templete>

  常用生命週期
  onMounted()
  onUpdated()
  onUnmounted()
-->
<template>
  <n-config-provider :theme="isDark ? darkTheme : null">
    <div :class="{ 'mode-dark': isDark }">

      <!-- Header -->
      <header class="site-header">
        <div class="left">
          <div class="brand">古亦弘 • WeHelp 深度學習課程申請</div>
        </div>
        <div class="right">
          <label>
            <span class="switch-label">{{ isDark ? '深色模式' : '淺色模式' }}</span>
            <n-switch v-model:value="isDark" size="large" aria-label="切換深色/淺色模式">
              <template #checked-icon>
                <span class="switch-icon">🌙</span>
              </template>
              <template #unchecked-icon>
                <span class="switch-icon">☀️</span>
              </template>
            </n-switch>

          </label>
        </div>
      </header>

      <!-- Hero -->
      <section class="hero">
        <div class="hero-inner">
          <h1 class="hero-title">歡迎 WeHelp Academy 蒞臨</h1>
          <p class="hero-sub">申請 WeHelp 深度學習課程｜自我介紹｜報名表</p>

          <div class="hero-scroll-btn" @click="scrollTo('intro')">
            <span>開始閱讀</span>
            <svg class="arrow-down" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none"
              stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <line x1="12" y1="5" x2="12" y2="19"></line>
              <polyline points="19 12 12 19 5 12"></polyline>
            </svg>
          </div>

        </div>
        <div class="hero-visual" aria-hidden="true">
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
                <h2 class="name">古亦弘</h2>
                <p class="short">我是從生技領域轉職至金融業軟體工程，擁有JAVA MVC網頁服務全端工程師1年工作經驗。</p>
                <p class="motivation">我想參加 WeHelp 深度學習訓練。期望在 6 個月密集實作中提升大型專案協作能力，以及加強深度學習模型在前端或後端的整合技能。</p>
              </div>
            </div>
          </n-card>
        </section>

        <!-- Q&A Cards -->
        <section class="qa-list">
          <n-grid :cols="1" :x-gap="24" :y-gap="24" responsive>
            <n-grid-item v-for="(q, idx) in qas" :key="idx">
              <n-card class="glass qa-card" hoverable>
                <n-collapse>
                  <n-collapse-item :title="q.question">
                    <div class="qa-answer">
                      <p v-html="q.answer"></p>
                    </div>
                  </n-collapse-item>
                </n-collapse>
              </n-card>
            </n-grid-item>
          </n-grid>
        </section>

        <!-- Footer -->
        <footer class="site-footer">
          <div>
            © 2025 古亦弘 •
            <a href="#">GitHub</a> •
            <a href="#">Email</a> •
            <span class="social-icon">💼</span>
            <span class="social-icon">📧</span>
          </div>
        </footer>
      </main>
    </div>
  </n-config-provider>
</template>

<script setup>
import { ref } from 'vue'
import { darkTheme } from 'naive-ui'

const isDark = ref(false)
const intro = ref(null)

const scrollTo = (id) => {
  if (id === 'intro') {
    intro.value?.scrollIntoView({ behavior: 'smooth' })
  }
}

// Q&A data
const qas = [
  {
    question: '目前的職業背景，若是剛畢業，畢業的科系為何？',
    answer: `我目前是一名前端全端工程師，畢業於國立臺灣大學森林系（學士與碩士皆為森林科學背景）。雖然本科與碩士專業為生技與植物研究，但我自學前端程式，完成多個專案與互動式小遊戲，成功轉職為 JAVA 全端工程師，具備跨領域問題解決能力與實務經驗。`
  },
  {
    question: '如果參與這個訓練，會怎麼安排學習時間？',
    answer: `我會採取系統化與有回饋的學習方式：<br>1) 平日：每日投入約 4 小時專注學習。<br>2) 假日：每日投入約 6 小時練習與專案實作。<br>3) 每週末：與同儕進行一次 code review，檢視程式品質並討論改進方案，以持續提升技術能力與合作效率。`
  },
  {
    question: '請描述一件產生明顯負面情緒的經歷，如何處理該情緒？',
    answer: `在專案中曾因溝通不良而導致功能延遲，當時感到焦慮。我先冷靜分析問題、整理需求與流程，並主動提出每日短會追蹤進度。最終，我學會拆解任務、建立清晰 API 與工作優先順序，不僅解決了專案瓶頸，也提升團隊協作效率。`
  },
  {
    question: '最想使用自己開發的深度學習模型解決什麼問題？',
    answer: `我希望開發一個自動化資料標註模型，降低人工標註成本並提升資料品質，藉此支援機器學習與 AI 應用，讓跨領域資料整合與分析更高效。`
  },
  {
    question: '若終究無法達到 OpenAI 的程度，為何要學習基礎模型的實作？',
    answer: `基礎模型的實作能讓我深入理解資料處理、訓練流程與模型評估。即便無法達到前沿水準，掌握底層能力仍可應用於實務產品，並為後續進階開發奠定穩固基礎。`
  },
  {
    question: '從上次提出申請至今，多做了哪些努力？',
    answer: `我完成了以下進修與專案：<br>• 開發 3 個前端專案，涵蓋 UI/UX 設計與互動效果。<br>• 熟練 Vue 3 Composition API 與前端測試流程。<br>• 練習 PyTorch 模型訓練，掌握資料前處理與模型驗證。<br>• 持續探索 AI 工具，提升跨領域整合能力與問題解決能力。`
  },
  {
    question: '其他想要對我們說的事情？',
    answer: `感謝 WeHelp 提供這次機會。我希望能加入重視實作與回饋的環境，與優秀同儕共同成長，持續拓展技術視野，並將跨領域的學習與實務能力應用於實際專案中。`
  },
]
</script>

<style>
:root {
  --bg-light: #f1f5f9;
  --text-light: #0f1724;
  --subtext-light: #475569;
  --card-bg-light: #ffffff;

  --bg-dark: #020617;
  --text-dark: #e6eef8;
  --subtext-dark: #cbd5e1;
  --card-bg-dark: #071226;
}

/* Reset & Base */
* {
  box-sizing: border-box;
}

body,
html,
#app {
  height: 100%;
  margin: 0;
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
  background-color: var(--bg-light);
  color: var(--text-light);
  transition: background 0.5s, color 0.5s;
}

.mode-dark {
  background-color: var(--bg-dark);
  color: var(--text-dark);
}

/* Header */
.site-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 64px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 40px;
  z-index: 60;
  background: rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(6px);
  box-shadow:
    0 2px 4px rgba(0, 0, 0, 0.08),
    0 8px 16px rgba(0, 0, 0, 0.06);
  color: var(--text-light);
  transition: all 0.3s;
}

.mode-dark .site-header {
  background: rgba(0, 0, 0, 0.18);
  color: var(--text-dark);
  box-shadow:
    0 2px 4px rgba(0, 0, 0, 0.2),
    0 8px 16px rgba(0, 0, 0, 0.15);
}

.site-header .brand {
  font-weight: 600;
  font-size: 18px;
}

.switch-icon {
  display: inline-block;
  transition: transform 0.5s, opacity 0.5s;
}

.n-switch__button:hover .switch-icon {
  transform: rotate(20deg);
}

.site-header .switch-label {
  font-weight: 300;
  font-size: 12px;
  margin-right: 10px;
}

/* Hero */
.hero {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  text-align: center;
  background: linear-gradient(120deg, #f1f5f9, #e2e8f0);
  color: var(--text-light);
  transition: background 0.5s, color 0.5s;
}

.mode-dark .hero {
  background: linear-gradient(120deg, #020617, #071330);
  color: var(--text-dark);
}

.hero-title {
  font-size: 64px;
  margin: 0 0 16px;
  animation: fadeInUp 1s ease forwards;
  opacity: 0;
}

.hero-sub {
  margin: 0 0 32px;
  font-size: 22px;
  opacity: 0.9;
  animation: fadeInUp 1.2s ease forwards;
}

.hero-scroll-btn {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 2rem;
  background: linear-gradient(135deg, #d3e2f5, #e0eaf9);
  font-weight: bold;
  font-size: 1.1rem;
  border-radius: 2rem;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
  /* 調整為更淺的陰影 */
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05), 0 1px 3px rgba(0, 0, 0, 0.05);
}

.hero-scroll-btn:hover {
  transform: translateY(-3px);
  /* 調整 hover 時的陰影，讓效果更輕微 */
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.08), 0 2px 4px rgba(0, 0, 0, 0.08);
}

.arrow-down {
  margin-top: 0.25rem;
  width: 1.5rem;
  height: 1.5rem;
  animation: bounce 1.5s infinite;
}

/* 深色模式的樣式覆寫 */
.mode-dark .hero-scroll-btn {
  background: linear-gradient(135deg, #3796c5b4, #2a6b8694);
  color: #e0eaf9;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2), 0 1px 3px rgba(0, 0, 0, 0.15);
}

.mode-dark .hero-scroll-btn:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.25), 0 4px 6px rgba(0, 0, 0, 0.2);
}

.mode-dark .arrow-down {
  color: #e0eaf9;
}

/* Vortex */
.hero-visual {
  position: absolute;
  inset: 0;
  z-index: 2;
  pointer-events: none;
}

.vortex {
  position: absolute;
  width: 1600px;
  height: 1600px;
  right: -400px;
  top: -400px;
  background: radial-gradient(circle at 30% 30%, rgba(120, 160, 255, 0.30), transparent 40%),
    radial-gradient(circle at 70% 70%, rgba(200, 160, 255, 0.3), transparent 50%),
    radial-gradient(circle at 50% 50%, rgba(255, 180, 220, 0.3), transparent 55%);
  transform: rotate(0deg);
  filter: blur(80px);
  animation: rotateVortex 40s linear infinite;
}

.mode-dark .vortex {
  background: radial-gradient(circle at 30% 30%, rgba(255, 255, 255, 0.15), transparent 35%),
    radial-gradient(circle at 70% 70%, rgba(180, 200, 255, 0.12), transparent 40%),
    radial-gradient(circle at 50% 50%, rgba(255, 200, 255, 0.20), transparent 50%);
}

/* Main Content */
.main-content {
  max-width: 1400px;
  margin: 0 auto;
  padding: 140px 40px 80px;
  color: var(--text-light);
  transition: color 0.5s;
}

.mode-dark .main-content {
  color: var(--text-dark);
}

/* Glass Card */
.glass {
  background: transparent;
  transition: all 0.4s;
}

.glass .n-card__body {
  background: var(--card-bg-light) !important;
  color: var(--text-light) !important;
  border-radius: 12px;
  transition: all 0.4s;
}

.mode-dark .glass .n-card__body {
  background: var(--card-bg-dark) !important;
  color: var(--text-dark) !important;
  border: 1px solid rgba(255, 255, 255, 0.04) !important;
}

.glass.qa-card:hover .n-card__body {
  box-shadow: 0 16px 36px rgba(0, 0, 0, 0.541) !important;
  transform: translateY(-3px);
  transition: all 0.35s ease;
}

/* Intro Grid */
.intro-grid {
  display: flex;
  gap: 40px;
  align-items: center;
}

.avatar {
  width: 140px;
  height: 140px;
  border-radius: 50%;
  background: linear-gradient(135deg, #4f5d75, #2b3a67);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 42px;
  color: white;
  transition: background 0.4s;
}

.mode-dark .avatar {
  background: linear-gradient(135deg, #1f2937, #0b0e17);
}

.name {
  margin: 0 0 8px;
  font-size: 32px;
}

.short,
.motivation {
  margin: 0 0 12px;
  font-size: 18px;
  color: var(--subtext-light);
  transition: color 0.4s;
}

.mode-dark .short,
.mode-dark .motivation,
.mode-dark .qa-answer,
.mode-dark .site-footer {
  color: var(--subtext-dark);
}

/* QA */
.qa-list {
  margin-top: 60px;
}

.qa-answer {
  line-height: 1.9;
  font-size: 16px;
  color: var(--subtext-light);
  transition: color 0.3s;
}

/* Footer */
.site-footer {
  margin-top: 64px;
  text-align: center;
  font-size: 14px;
  color: var(--subtext-light);
  transition: color 0.3s;
}

.site-footer a {
  color: var(--text-light);
  text-decoration: none;
  transition: color 0.3s;
}

.mode-dark .site-footer a {
  color: var(--text-dark);
}

.social-icon {
  margin-left: 6px;
  transition: transform 0.3s, color 0.3s;
}

.social-icon:hover {
  transform: scale(1.2);
  color: #ffbb33;
}

/* Animations */
@keyframes rotateVortex {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}

@keyframes fadeInUp {
  0% {
    opacity: 0;
    transform: translateY(20px);
  }

  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes bounce {

  0%,
  20%,
  50%,
  80%,
  100% {
    transform: translateY(0);
  }

  40% {
    transform: translateY(5px);
  }

  60% {
    transform: translateY(3px);
  }
}

/* Responsive */
@media (max-width:1024px) {
  .intro-grid {
    flex-direction: column;
    text-align: center;
  }

  .avatar {
    margin: 0 auto;
  }
}

@media (max-width:640px) {
  .hero-title {
    font-size: 32px;
  }

  .hero-sub {
    font-size: 16px;
  }

  .hero-btn {
    width: 90%;
  }

  .intro-grid {
    gap: 24px;
  }

  .avatar {
    width: 120px;
    height: 120px;
    font-size: 36px;
    margin: 0 auto;
  }

  n-grid {
    grid-template-columns: 1fr !important;
  }
}
</style>
