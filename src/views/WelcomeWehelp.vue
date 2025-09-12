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
                    <n-card class="glass">
                        <div class="intro-grid">
                            <div class="avatar-wrap">
                                <div class="avatar">A</div>
                            </div>
                            <div>
                                <h2 class="name">古亦弘</h2>
                                <p class="short">原本在生技領域進修與研究，後來轉職進入金融業擔任軟體工程師，累積了一年使用 Java MVC 進行全端開發 的工作經驗。</p>
                                <p class="motivation">

                                </p>
                            </div>
                        </div>
                    </n-card>
                </section>

                <!-- Q&A Cards -->
                <section class="qa-section">
                    <div v-if="currentIndex === null" class="qa-grid">
                        <n-card v-for="(qa, index) in qas" :key="index" class="qa-card" @click="openQA(index)"
                            hoverable>
                            <template #header>{{ qa.question }}</template>
                            <div v-html="qa.answer.slice(0, 60) + '...'" />
                        </n-card>
                    </div>

                    <div v-else class="qa-focus" ref="qaFocus">
                        <div class="qa-top-bar">
                            <n-button text @click="closeQA" class="back-btn">
                                <n-icon class="back-icon">
                                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                                        stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                        <polyline points="15 18 9 12 15 6"></polyline>
                                    </svg>
                                </n-icon>
                                返回
                            </n-button>
                        </div>

                        <!-- Swiper 懶加載 -->
                        <Suspense>
                            <template #default>
                                <LazySwiper :currentIndex="currentIndex" :qas="qas"
                                    @scroll="() => scrollTo('qa-focus')" />
                            </template>
                            <template #fallback>
                                <div class="loading-fallback">內容載入中...</div>
                            </template>
                        </Suspense>

                    </div>
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
import { ref, defineAsyncComponent } from "vue";
import { darkTheme } from 'naive-ui'
import { NCard, NButton } from "naive-ui"

const isDark = ref(false)
const intro = ref(null)
const qaFocus = ref(null)
const LazySwiper = defineAsyncComponent(() => import("../components/SwiperSection.vue"));
const currentIndex = ref(null);

const scrollTo = (id) => {
    if (id === 'intro') {
        intro.value?.scrollIntoView({ behavior: 'smooth' })
    } else if (id === 'qa-focus') {
        qaFocus.value?.scrollIntoView({ behavior: 'auto' })
    }
}



function openQA(index) {
    currentIndex.value = index;
}

function closeQA() {
    currentIndex.value = null
}



// Q&A data
const qas = [
    {
        question: '申請動機',
        answer: '因為生活規劃與地點遷移的考量，我離開了前一份工作，來到竹南展開新生活，也開始尋找能持續精進的學習資源。從研究所畢業後，我對機器學習開始產生興趣。在服兵役期間，我唯一帶進去的書就是 Luis Serrano 的《白話機器學習》中文譯本。此外，當時正好台大李宏毅教授在 YouTube 發佈了他的【生成式 AI 導論】課程影片，這些資源讓我更加確信，深度學習是我未來職涯最想投入的方向。線上的課程雖然很多，但要不是價格過高，就是內容零散。因此當我看到 WeHelp 推出的深度學習課程時，覺得格外振奮，這正是我期待已久的機會。'
    },
    {
        question: '目前的職業背景，若是剛畢業，畢業的科系為何？',
        answer: `我目前待業中，前一份工作是在 國泰人壽 IT 部門 擔任 Java 全端工程師，負責 B2E 系統的維護與開發。主要技術包含 Java、JSP、JDBC、Servlet、SQL、Spring Boot 與 Vue。<br/>
    我畢業於 國立臺灣大學森林系（學士與碩士），專業背景偏向生技與植物研究。但在研究所時期，我開始自學程式，並製作了前端作品，進而獲得國泰人壽 Java 培訓生的資格。經過考核後順利轉職成為軟體工程師。這段經歷讓我確信自己對軟體開發的熱情與能力。`
    },
    {
        question: '如果參與這個訓練，會怎麼安排學習時間？',
        answer: `第一階段與第二階段<br>維持每週44小時以上的學習時間<br>1) 平日：每日投入 8 小時以上。依課程內容規劃學習與實作，並在每日21:00前完成進度報告撰寫<br>2) 假日：依課程內容投入 4 ~ 8 小時z複習、練習、實作並參與群組的問題討論。 <br> 第三階段<br>維持每週50小時以上的學習時間。依當週進度調整於平日約9小時專注練習或專案開發，同樣在每日21:00前完成進度報告撰寫，假日視情況安排5小時以上的時間開發專案與關注群組討論。原則上會安排星期日做休息放鬆，調適心情維持高度的學習效率。`,
    },
    {
        question: '請描述一件產生明顯負面情緒的經歷，如何處理該情緒？',
        answer: ``
    },
    {
        question: '最想使用自己開發的深度學習模型解決什麼問題？',
        answer: `如果設定一個初階的目標，我希望能利用模型幫助我「作詞作曲」。彈吉他是我的興趣之一，我常常將心情寫成歌曲，但經常卡在歌詞不順或和弦不理想的瓶頸。如果有 AI 能提供更自然的歌詞建議、並針對旋律自動給出和弦參考，將大幅提升我的創作體驗。<br/>
    若是放眼更長遠的未來，我會希望能將深度學習應用於 生物技術或醫療產業。例如在學術時期，我對蛋白質序列與抗體親和力的預測有興趣，如果能透過 AI 建模輔助研究，將會是非常有價值的應用。`
    },
    {
        question: '若終究無法達到 OpenAI 的程度，為何要學習基礎模型的實作？',
        answer: `基礎模型的實作能幫助我真正理解資料處理、訓練流程與模型評估。即便自己訓練的模型無法像 ChatGPT 那樣擬人化，也可能更適合某些專門的產業需求。在我看來，「好模型」不是看它多聰明，而是能否解決特定問題。學習基礎模型的實作，將是我邁向 AI 工程師職涯不可或缺的基礎。`
    },
    {
        question: '從上次提出申請至今，多做了哪些努力？',
        answer: `我曾經申請過「網站開發」課程，但當時因服兵役，無法在課程前期投入足夠時間。去年五月初退伍後，我利用兩個月自學彭彭老師的 YouTube 課程，學習 HTML/CSS 與 JavaScript，並獨立完成一個以原生 JavaScript 開發的打地鼠遊戲。這份成果獲得國泰人壽 IT 單位的肯定，使我得以進入 JAVA 培訓計畫，接受高強度的全薪訓練，並成功通過考核成為正式員工，負責 B2E JAVA 網頁服務的開發與維護。培訓期間除了上班時間的課程外，我也持續利用下班與假日進行練習與專案開發，這些努力都是為了實現轉職軟體工程師的目標。而現在，我已經準備好全力投入深度學習的訓練！`
    },
    {
        question: '其他想要對我們說的事情？',
        answer: `我非常珍惜能這次申請的機會。無論是過去自學的經驗，還是轉職的培訓過程，都讓我更確信自己適合透過密集訓練進步。我相信自己具專注投入與快速吸收的能力，非常希望能參加這次的課程中，培養更多能力並完成更有挑戰性的專案。`
    },
]
</script>

<style>
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

/* Intro Card */
.intro-card .glass {
    background: rgba(255, 255, 255, 0.12);
    backdrop-filter: blur(10px);
    border-radius: 16px;
    transition: all 0.4s ease;
}

.mode-dark .intro-card .glass {
    background: rgba(20, 30, 50, 0.4);
    border: 1px solid rgba(255, 255, 255, 0.05);
}

.intro-grid {
    display: flex;
    gap: 40px;
    align-items: center;
}

.avatar-wrap {
    flex-shrink: 0;
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
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
    transition: all 0.4s ease;
}

.avatar:hover {
    transform: scale(1.05);
    filter: brightness(1.1);
}

.mode-dark .avatar {
    background: linear-gradient(135deg, #1f2937, #0b0e17);
}

.name {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-size: 32px;
    font-weight: 700;
    margin: 0 0 12px;
}

.short {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-size: 18px;
    font-weight: 400;
    line-height: 1.65;
    color: var(--subtext-light);
    margin-bottom: 12px;
}

.mode-dark .short {
    color: var(--subtext-dark);
}

.motivation {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-size: 18px;
    font-weight: 400;
    line-height: 1.7;
    color: var(--subtext-light);
}

.mode-dark .motivation {
    color: var(--subtext-dark);
}


/* QA */
.qa-top-bar n-button {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-weight: 800;
    font-size: 30px;
    letter-spacing: 0.5px;
    color: var(--text-light);
    background: rgba(255, 255, 255, 0.12);
    backdrop-filter: blur(8px);
    border-radius: 12px;
    padding: 0.5rem 1rem;
    transition: all 0.3s ease;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.qa-top-bar n-button:hover {
    background: rgba(255, 255, 255, 0.18);
    box-shadow: 0 6px 18px rgba(0, 0, 0, 0.15);
    transform: translateY(-2px);
}

.mode-dark .qa-top-bar n-button {
    color: var(--text-dark);
    background: rgba(20, 30, 50, 0.4);
}

.mode-dark .qa-top-bar n-button:hover {
    background: rgba(20, 30, 50, 0.6);
}

.back-btn {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-weight: 500;
    font-size: 15px;
    letter-spacing: 0.5px;
    display: inline-flex;
    align-items: center;
    gap: 6px;
    color: var(--text-light);
    background: rgba(255, 255, 255, 0.12);
    backdrop-filter: blur(8px);
    border-radius: 12px;
    padding: 0.5rem 1rem;
    transition: all 0.3s ease;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.back-btn:hover {
    background: rgba(255, 255, 255, 0.18);
    box-shadow: 0 6px 18px rgba(0, 0, 0, 0.15);
    transform: translateY(-2px);
}

.mode-dark .back-btn {
    color: var(--text-dark);
    background: rgba(20, 30, 50, 0.4);
}

.mode-dark .back-btn:hover {
    background: rgba(20, 30, 50, 0.6);
}

.back-icon svg {
    stroke: currentColor;
}

.qa-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1.5rem;
    margin-top: 2rem;
}

.qa-card {
    cursor: pointer;
    border-radius: 16px;
    overflow: hidden;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    backdrop-filter: blur(8px);
    background: rgba(255, 255, 255, 0.15);
    border: 1px solid rgba(255, 255, 255, 0.1);
}

.mode-dark .qa-card {
    background: rgba(20, 30, 50, 0.4);
    border: 1px solid rgba(255, 255, 255, 0.05);
}

.qa-card:hover {
    transform: translateY(-6px);
    box-shadow: 0 12px 28px rgba(0, 0, 0, 0.25);
    text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.qa-card n-card__header {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-weight: 700;
    font-size: 1.25rem;
    color: var(--text-light);
    margin-bottom: 0.75rem;
    border-bottom: 2px solid rgba(0, 0, 0, 0.08);
    padding-bottom: 0.25rem;
}

.mode-dark .qa-card n-card__header {
    color: var(--text-dark);
    border-bottom-color: rgba(255, 255, 255, 0.1);
}

.qa-card div {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-size: 1rem;
    font-weight: 400;
    color: var(--subtext-light);
    line-height: 1.65;
    letter-spacing: 0.5px;
}

.mode-dark .qa-card div {
    color: var(--subtext-dark);
}

/* QA Focus */
.qa-focus {
    margin-top: 2rem;
    padding: 1rem;
    background: rgba(255, 255, 255, 0.08);
    border-radius: 16px;
    backdrop-filter: blur(6px);
    max-height: 80vh;
    overflow-y: auto;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
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

@media (max-width: 768px) {
    .intro-grid {
        flex-direction: column;
        text-align: center;
    }

    .avatar {
        width: 120px;
        height: 120px;
        font-size: 36px;
        margin-bottom: 16px;
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

    .qa-grid {
        grid-template-columns: 1fr;
        gap: 1rem;
    }

    .qa-card {
        border-radius: 12px;
    }

    .qa-card div {
        font-size: 0.95rem;
    }

    .qa-card n-card__header {
        font-size: 1.1rem;
    }
}

.loading-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(255, 255, 255, 0.8);
    /* 淺色模式半透明背景 */
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    z-index: 2000;
    /* 確保蓋在最上面 */
    backdrop-filter: blur(4px);
    /* 微模糊背景 */
    transition: opacity 0.3s ease;
}

.mode-dark .loading-overlay {
    background: rgba(0, 0, 0, 0.6);
    /* 深色模式背景 */
}

.spinner {
    width: 60px;
    height: 60px;
    border: 6px solid rgba(0, 0, 0, 0.1);
    border-top: 6px solid #3eaf7c;
    /* 亮色綠藍感 (可改成主題色) */
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin-bottom: 1rem;
}

.mode-dark .spinner {
    border: 6px solid rgba(255, 255, 255, 0.1);
    border-top: 6px solid #4ecdc4;
    /* 深色模式用清爽藍綠 */
}

@keyframes spin {
    to {
        transform: rotate(360deg);
    }
}

.loading-overlay p {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-size: 1.1rem;
    font-weight: 500;
    color: #333;
    letter-spacing: 1px;
}

.mode-dark .loading-overlay p {
    color: #eee;
}
</style>
