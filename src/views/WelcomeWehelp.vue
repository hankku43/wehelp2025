<template>
    <n-config-provider :theme="isDark ? darkTheme : null">
        <div :class="{ 'mode-dark': isDark }">
            <!-- Header -->
            <header ref="headerRef" class="site-header">
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
                            <!-- 左邊頭像 + 文字 -->
                            <div class="intro-left">
                                <div class="profile-block">
                                    <div class="avatar-wrap">
                                        <div class="avatar"></div>
                                    </div>
                                    <h2 class="name">古亦弘</h2>
                                </div>
                                <div class="intro-text">
                                    <p class="short">
                                        於國立臺灣大學森林系碩士畢業，自2024年起投入<strong>全端工程師職涯</strong>，於金融業負責B2E<strong>網頁服務開發與維護</strong>。<br />
                                        熟悉<strong>JSP</strong>、<strong>Vue3</strong>等前端，以及<strong>Java
                                            Servlet</strong>、<strong>JDBC</strong>、
                                        <strong>Spring-Boot</strong>等後端技術，並具<strong>DB2</strong>、<strong>Oracle資料庫</strong>及<strong>版本控制</strong>操作經驗。
                                    </p>
                                </div>
                            </div>

                            <!-- 右邊影片 -->
                            <div class="video-preview" @mouseenter="isHovering = true" @mouseleave="isHovering = false">
                                <iframe v-if="isHovering" :src="previewSrc" frameborder="0"
                                    allow="autoplay; encrypted-media" allowfullscreen></iframe>
                                <img v-else :src="thumbnail" alt="影片縮圖" />
                                <!-- 點擊遮罩層 -->
                                <div class="click-overlay" @click="openModal"></div>
                                <div class="play-icon">▶</div>
                            </div>
                        </div>
                    </n-card>

                    <!-- Modal -->
                    <n-modal v-model:show="showModal" style="width: 1120px; height: 630px;">
                        <div class="modal-video-wrap">
                            <iframe :src="fullVideoSrc" frameborder="0" allow="autoplay; encrypted-media"
                                allowfullscreen></iframe>
                        </div>
                    </n-modal>
                </section>

                <!-- Q&A Cards -->
                <section class="qa-section">
                    <div v-if="currentIndex === null || isMobile" class="qa-grid">
                        <n-card v-for="(qa, index) in processedQas" :key="index" class="qa-card" @click="openQA(index)"
                            hoverable>
                            <template #header>{{ qa.question }}</template>
                            <div v-html="isMobile ? qa.shortAnswerMobile : qa.shortAnswerPc" />
                        </n-card>
                    </div>

                    <div v-else-if="currentIndex !== null" class="qa-focus" ref="qaFocus">
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
                                <LazySwiper :currentIndex="currentIndex" :qas="qas" :isMobile="false" :isDark="isDark"
                                    @scroll="() => scrollTo('qa-focus')" />
                            </template>
                            <template #fallback>
                                <div class="loading-fallback">
                                    <div class="qa-spinner"></div>
                                    <p>內容載入中...</p>
                                </div>
                            </template>
                        </Suspense>
                    </div>

                    <n-modal v-model:show="showQAModalRef" style="width: 100%; max-width: 100%; height: 100%;"
                        :class="{ 'qa-modal': true, 'mode-dark': isDark }" mask-closable @close="closeQA">
                        <div class="qa-modal-wrap">
                            <Suspense>
                                <template #default>
                                    <LazySwiper :currentIndex="currentIndex" :qas="qas" :isMobile="true"
                                        :isDark="isDark" @close="closeQA" />
                                </template>
                                <template #fallback>
                                    <div class="loading-fallback">
                                        <div class="qa-spinner"></div>
                                        <p>內容載入中...</p>
                                    </div>
                                </template>
                            </Suspense>
                        </div>
                    </n-modal>
                </section>

                <!-- Footer -->
                <footer class="site-footer">
                    <div>
                        © 2025 古亦弘 •
                        <span class="social-icon">💼</span>
                        <a href="https://pda.104.com.tw/profile/share/eVHTjap6lkYwZJnYR0BQEZLmNB29r05D">104 Resume</a> •
                        <a href="https://hankku43.github.io/resume.pdf">One Page Resume</a> •
                        <a href="mailto:hank.ku43@gmail.com">Email Me</a> •
                        <span class="social-icon">📧</span>
                        <a href="https://www.flaticon.com/free-icons/gaming" title="gaming icons">Gaming icons created
                            by
                            riajulislam - Flaticon</a>
                    </div>
                </footer>
            </main>

        </div>
    </n-config-provider>
</template>

<script setup>
import { ref, defineAsyncComponent, onMounted, onBeforeUnmount, computed } from "vue";
import { darkTheme } from 'naive-ui';
import { NCard, NButton } from "naive-ui";
const isDark = ref(false);
const intro = ref(null);
const qaFocus = ref(null);
const LazySwiper = defineAsyncComponent(() => import("../components/SwiperSection.vue"));
const currentIndex = ref(null);
const isHovering = ref(false);
const showModal = ref(false);
const videoId = "lk7c0fFAxrA";
import thumb from '@/assets/video-thumb.png';
const thumbnail = thumb;
const previewSrc = `https://www.youtube.com/embed/${videoId}?autoplay=1&mute=1&controls=0&loop=1&playlist=${videoId}`;
const fullVideoSrc = `https://www.youtube.com/embed/${videoId}?autoplay=1`;
const isMobile = ref(false);
const showQAModalRef = ref(false)

const updateIsMobile = () => {
    isMobile.value = window.innerWidth <= 640;
};
const scrollTo = (id) => {
    if (id === 'intro') {
        intro.value?.scrollIntoView({ behavior: 'smooth' });
    } else if (id === 'qa-focus') {
        qaFocus.value?.scrollIntoView({ behavior: 'auto' });
    }
}

function openModal() {
    showModal.value = true;
}

function openQA(index) {
    currentIndex.value = index;
    if (!isMobile.value) return;
    showQAModalRef.value = true;
}

function closeQA() {
    currentIndex.value = null;
    showQAModalRef.value = false;
}

const headerRef = ref(null);
let lastScroll = 0;
let ticking = false;
const handleScroll = () => {
    const currentScroll = window.scrollY;
    if (currentScroll > lastScroll && currentScroll > 50) {
        headerRef.value.style.top = '-48px'
    } else {
        headerRef.value.style.top = '0'
    } lastScroll = currentScroll
    ticking = false
}

const onScroll = () => {
    if (isMobile.value) {
        if (!ticking) {
            window.requestAnimationFrame(handleScroll)
            ticking = true
        }
    }
}

onMounted(() => {
    // 初始化 header scroll
    window.addEventListener('scroll', onScroll);

    // 初始化 isMobile
    updateIsMobile();
    window.addEventListener('resize', updateIsMobile);
})

onBeforeUnmount(() => {
    window.removeEventListener('scroll', onScroll);
    window.removeEventListener('resize', updateIsMobile);
})

import imgBook from '@/assets/book.png'
import imgYT from '@/assets/youtube.png'
import imgGame from '@/assets/gamepad.png'
// Q&A data
const qas = [
    {
        type: 'image',
        question: '申請動機',
        answer: '<p>因為生活規劃與地點遷移的考量，我離開了前一份工作，來到竹南展開新生活，也開始尋找能持續精進的學習資源。由於竹南缺乏相關的實體課程，我只能透過線上資源自學，因此像WeHelp<strong>線上訓練</strong>的模式正好符合我的需求。</p><p>從研究所畢業後，我對機器學習開始產生興趣。在服兵役期間，我唯一帶進去的書就是<q>Luis Serrano的《白話機器學習》中文譯本</q>。</p><p>此外，當時正好台大李宏毅教授在YouTube發佈了他的<q>【生成式人工智慧導論】課程影片</q>，接觸過這些資源後，讓我更加確信，<strong>深度學習是我未來職涯最想投入的方向</strong>。</p><p>線上的課程雖然很多，但要不是價格過高，就是內容零散。因此，當我看到WeHelp推出的深度學習課程時，覺得格外振奮，這正是我期待已久的機會。</p><p>WeHelp主打<strong>任務導向、非手把手教學，這完全貼合我的學習模式</strong>。比起不斷的閱讀或聽課，我一直以來都習慣動手做，從實作中學習。也因此，我認為WeHelp的課程非常適合我。</p></p>',
        images: [imgBook, imgYT]
    },
    {
        type: 'timeline',
        question: '目前的職業背景，若是剛畢業，畢業的科系為何？',
        answer: `<p>我目前待業中，前一份工作是在<strong>國泰人壽程式設計科</strong>擔任<strong>Java全端工程師</strong>，負責<strong>B2E 系統網頁服務</strong>的維護與開發。主要使用的語言是<strong>Java</strong>、<strong>JavaScript</strong>、<strong>SQL</strong>，採用的技術或框架包含<strong>JSP</strong>、<strong>JDBC</strong>、<strong>Servlet</strong>、<strong>Spring Boot</strong>與<strong>Vue</strong>。</p><p>我畢業於<strong>國立臺灣大學森林系</strong>（學士與碩士），求學時專業背景偏向<strong>植物</strong>與<strong>分子生物技術</strong>和<strong>研究</strong>。但在畢業並退伍後，我開始自學前端基礎程式(<strong>HTML</strong>、<strong>CSS</strong>、<strong>JavaScript</strong>)，並製作了我的<strong>個人網頁</strong>，包含<strong>自我介紹</strong>和一個<strong>網頁式打地鼠遊戲</strong>，進而獲得<strong>國泰金控Java培訓生</strong>的資格。經過考核後順利轉職成為<strong>軟體工程師</strong>。這段經歷讓我確信自己對軟體開發的熱情與能力。</p>`,
        timeline: [
            { type: "success", title: "臺大森林系", content: "碩士畢業", time: "2023-12" },
            { type: "warning", title: "兵役", content: "退伍", time: "2024-05" },
            { type: "info", title: "自學前端語言", content: "完成個人網頁", time: "2024-06" },
            { type: "success", title: "獲選國泰培訓", content: "JAVA實習生", time: "2024-07" },
            { type: "success", title: "通過考核", content: "程設科專員", time: "2024-11" }
        ]
    },
    {
        type: 'chart',
        question: '如果參與這個訓練，會怎麼安排學習時間？',
        answer: `<p><p><strong>第一階段與第二階段：</strong><br>維持每週<strong>44個小時以上</strong>的學習時間。<br>1) <strong>平日：</strong>每日投入<strong>8小時以上</strong>，依課程內容規劃學習與實作，並在每日<strong>21:00</strong>前完成進度報告撰寫。<br>2) <strong>假日：</strong>依課程內容投入<strong>4小時以上</strong>複習、練習、實作，並參與群組的問題討論。<br><br><strong>第三階段：</strong><br>維持每週<strong>50小時以上</strong>的學習時間，依當週進度調整。<br/>1) 平日：每日<strong>9 小時</strong>專注練習或專案開發，同樣在每日<strong>21:00</strong>前完成進度報告撰寫。<br/>2) 假日：視情況安排<strong>5小時以上</strong>的時間開發專案與關注群組討論。<br/>如果能跟上進度，會安排星期日做休息放鬆，調適心情，以維持高度的學習效率。</p>`,
        chartOptions: {
            tooltip: { trigger: 'item' },
            legend: { show: false }, // 隱藏外部 legend
            grid: {
                left: '5%',
                right: '55%',
                top: 20,
                bottom: 60,
                containLabel: true
            },
            xAxis: {
                type: 'category',
                data: ['第一、二階段', '第三階段'],
                axisLabel: { fontSize: 10 }
            },
            yAxis: {
                type: 'value',
                axisLabel: { fontSize: 14 }
            },
            series: [
                // 左側柱狀圖
                {
                    name: '平日',
                    type: 'bar',
                    data: [8, 9],
                    barWidth: '35%',
                    itemStyle: { color: '#4E79A7' } // 藍
                },
                {
                    name: '假日',
                    type: 'bar',
                    data: [6, 8],
                    barWidth: '35%',
                    itemStyle: { color: '#F28E2B' } // 橘
                },
                // 右側圓餅圖
                {
                    name: '每週總學習時數',
                    type: 'pie',
                    radius: '35%',
                    center: ['75%', '55%'],
                    data: [
                        { value: 44, name: '第一、二階段', itemStyle: { color: '#76B7B2' } },
                        { value: 50, name: '第三階段', itemStyle: { color: '#E15759' } }
                    ],
                    label: {
                        show: true,
                        position: 'inside',        // 文字在扇形內
                        formatter: '{b}\n{c} 小時', // 類別 + 值
                        textStyle: {
                            lineHeight: 20,
                            fontSize: 10  //文字大小要放這裡
                        },
                        color: '#000',
                        fontWeight: 'bold'
                    },
                    labelLine: { show: false }     // 不顯示外部指示線
                }
            ],
            title: [
                {
                    text: '每日學習時數',
                    left: '25%',
                    top: 0,
                    textAlign: 'center',
                    textStyle: { fontSize: 16, color: '#7c7c7c' }
                },
                {
                    text: '每週總學習時數',
                    left: '75%',
                    top: 0,
                    textAlign: 'center',
                    textStyle: { fontSize: 16, color: '#7c7c7c' }
                }
            ]
        }
    },
    {
        question: '請描述一件產生明顯負面情緒的經歷，如何處理該情緒？',
        answer: `<p>我自認為對自己的情緒有很高的認識，能在情緒發生的當下快速察覺，並積極處理，不讓情緒累積到壓垮自己。</p><p>對我來說，負面情緒有很多種，包括生氣、難過、挫折、愧疚，有時候甚至會同時出現。其中印象最深刻的一次，是在我高中時期。當時我母親正在對我哥哥大發雷霆，正好對我產生誤會。母親大多數時間都非常溫柔，可能正因為這種反差，當下我感到非常委屈，同時伴隨著相當大的驚嚇。那種負面情感突然襲來，至今我已忘記當時的事件細節，但仍深深記得母親咆哮的畫面。</p><p>即使感到委屈，我知道當務之急是讓母親冷靜下來，因此我強忍著淚水，用冷靜的聲音對她說：「媽，事情不是你想的這樣，你先冷靜聽我說。」她聽到後瞬間冷靜下來。在我們解開誤會後，我沒有讓事情就這麼結束，我知道必須處理自己的情緒。於是我向母親表達了感受到的受傷、委屈、生氣與驚嚇，透過真誠的溝通，我們相互理解，情緒才有了出口。</p><p>至今，面對各種負面情緒，我已能以正向態度處理。生氣可能源於找不到解決方法，悲傷是因為沒有把握好失去的事物，不論何種負面情緒，都是提醒自己還未成為理想的自己。因此當負面情緒出現，我會去接受、感受並處理，讓下次遇到類似事情時能做出更正確的選擇。這就是我面對並處理負面情緒的態度與方式。</p>`
    },
    {
        question: '最想使用自己開發的深度學習模型解決什麼問題？',
        answer: `<p>先說一個可能比較務實的目標，我希望能利用模型幫助我<strong>作詞作曲</strong>。我在大學一、二年級期間擔任詞曲創作社教學職位，音樂創作一直是我的興趣之一。但在創作過程經常卡在<strong>歌詞不順</strong>或<strong>和弦不理想</strong>的瓶頸。目前大型語言模型對於中文韻腳給的建議不是很和諧，關於音樂生成也不如文章生成這麼厲害，如果有AI模型能<strong>提供更自然的歌詞建議</strong>、並<strong>針對旋律自動給出和弦參考</strong>，將能大幅提升我的創作體驗。<p/><p>若是放眼更長遠的未來，我會希望能將深度學習應用於<strong>生物技術或醫療產業</strong>。例如在學術時期，我需要進行<strong>蛋白質序列對抗體親和力</strong>的預測，如果能透過深度學習輔助研究，勢必能產生有價值的結果。<p/><p>除了相對正經的想法之外，我對於深度學習、人工智慧還有好多好奇的事，例如：<br/>1) 以AlphaGo</strong>為例，如果模型可以藉由大量資料的訓練得到人類思考不出的棋路，那模型如果足夠了解我思考的方式，他能不能為我想學的語言設定一套學習辦法，這個學習辦法可能非常奇特、人類無法理解，但照著做就能很快學會呢？<p/><p>2) 因為我研究所時期是做分子生物的，開始對機器學習模型有一點認識之後，我總感覺這跟生物有那麼一點相似性：模型會有超多的參數、DNA有好多的鹼基；模型訓練過程有增強式學習，生物有天擇壓力，這讓我好奇能不能設計讓模型有壽命？能不能設計讓模型也會做減數分裂進行生育？</p><p>可能之後再回頭來看會覺得這些想法很荒唐吧！但現在的我對於深度學習就是抱持著許多疑問，非常期待能學會訓練自己的模型。</p>`
    },
    {
        question: '若終究無法達到 OpenAI 的程度，為何要學習基礎模型的實作？',
        answer: `<p>我認為原因有兩個：<p/><p>1) 即便自己訓練的模型無法像ChatGPT那樣多功能或擬人化，也可能更適合某些專門的產業需求。就我的認知來說，一個好的模型不是看它多像一個聰明人，而是有沒有<strong>在可接受的成本中，去解決特定問題、滿足特定需求。</strong><p/><p>2) 作為一個軟體工程師，尤其是去年才轉職的軟體工程師，我在工作中不時尋求ChatGPT協助，而我是一個喜歡追根究底的人，探討事物背後的原理；而且我們總不能對經常使用的工具一無所知吧？所以我認為尤其是<strong>軟體工程師更應該學習去實作基礎模型</strong>。<p/><p>如果可以的話，我也希望未來能成為機器學習/AI工程師，基礎模型的實作能幫助我真正理解資料處理、訓練流程與模型評估。學習基礎模型的實作，將是我邁向AI工程師職涯不可或缺的基礎。</p>
`
    },
    {
        type: 'modal',
        question: '從上次提出申請至今，多做了哪些努力？',
        answer: `<p>我去年在服兵役期間曾經申請過<strong>「網站開發」課程</strong>，但我猜想礙於退伍時間是課程開始3週後，因此沒有通過審核。但為了成為<strong>軟體工程師</strong>，去年五月初退伍後，我利用兩個月自學<strong>彭彭老師的YouTube課程</strong>，學習<strong>HTML/CSS</strong>與<strong>JavaScript</strong>，並獨立完成一個以<strong>原生JavaScript</strong>開發的<strong>打地鼠遊戲</strong>，也記錄了開發的一些心得(<a href="https://hankku43.github.io/?tab=1" target="_blank">點此前往閱讀</a>)，可以點選旁邊的遊戲把手圖案進行遊玩。</p><p>這份成果獲得<strong>國泰金控IT單位</strong>的肯定，使我得以進入<strong>JAVA培訓計畫</strong>，接受高強度支薪訓練，並成功<strong>通過考核成為正式員工</strong>。受訓期間除了上班時間的課程外，我也持續利用<strong>下班</strong>與<strong>假日</strong>進行練習與專案開發，這些曾經的努力都是為了實現<strong>轉職軟體工程師</strong>的目標。而現在，我已經準備好全力投入<strong>深度學習</strong>的訓練！</p><p>為了證明我具備<strong>程式開發</strong>的能力，這次我也花了一週的時間從<strong>彭彭老師的YouTube</strong>上複習<strong>Vue3 Composition API</strong>框架，並使用這個框架撰寫這份<strong>報名網頁</strong>。我也特別花費額外的時間設計<strong>攜帶裝置的RWD</strong>，雖然我沒有網頁設計的美術天分，但我盡量從<strong>使用者角度</strong>出發細心開發，希望這個成果能增加我申請此課程的成功率。</p>`,
        icon: imgGame,
        iframeSrc: 'https://hankku43.github.io/game'
    },
    {
        question: '其他想要對我們說的事情？',
        answer: `9月14日中的課程說明會中，彭彭老師有提到，申請深度學習訓練的條件是──<strong>至少「數學」和「寫程式之間」要會一個</strong>。我認為我有符合條件：<p/><p>在數學方面，雖然我大學的必修僅有微積分沒有線性代數，但我一直以來都<strong>對數學有熱忱</strong>，享受解數學題目的快樂。雖然沒有大學程度高等數學訓練經驗，但我也曾<strong>擔任國高中數學家教</strong>，對於基礎的數學能力非常有把握。</p><p>在寫程式方面，雖然我不是資工背景的科系畢業，但我從開始寫程式到現在也<strong>超過1年</strong>，並且也有<strong>軟體工程師的職場經驗</strong>，彭彭老師在說明會中也有提到：「<strong>軟體工程師在工作的時候也不會有人手把手教你，必須自己想辦法解決問題。</strong>」這段話我也有充分的經歷，在工作中確實常常遇到程式使用沒有學過的框架、元件庫，都必須<strong>透過自身努力尋找方向</strong>，以及<strong>積極主動地詢問學長姐</strong>來完成任務，因為有過這樣的工作經歷，我<strong>對於WeHelp這樣的訓練模式也非常有信心</strong>。<p/><p>我非常珍惜能這次申請的機會，所以即使<strong>我清楚對於這些問題的回答才是重點</strong>，但我仍希望能藉由這份報名表展現<strong>我對於此課程申請的重視</strong>。</p><p>無論是過去自學的經驗，還是轉職的培訓過程，都讓我更確信自己適合透過密集訓練進步。我相信自己具備<strong>快速吸收新知並將所學轉換成作品的能力</strong>，非常希望能參加這次的深度學習課程，在課程中培養更多能力並完成有挑戰性的專案。`
    },
]

// 預先處理一次
function stripHtml(html) {
    return html.replace(/<[^>]+>/g, '');
}
function truncate(text, length) {
    return text.length > length ? text.slice(0, length) + '...' : text;
}

const processedQas = computed(() =>
    qas.map(qa => {
        const plain = stripHtml(qa.answer);
        return {
            ...qa,
            shortAnswerMobile: truncate(plain, 20),
            shortAnswerPc: truncate(plain, 50),
        };
    })
);
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
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-weight: 400;
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
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05), 0 1px 3px rgba(0, 0, 0, 0.05);
}

.hero-scroll-btn:hover {
    transform: translateY(-3px);
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
    box-shadow: 0 1px 1px rgba(255, 255, 255, 0.2), 0 1px 1px rgba(255, 255, 255, 0.15);
}

.mode-dark .hero-scroll-btn:hover {
    transform: translateY(-4px);
    box-shadow: 0 2px 6px rgba(255, 255, 255, 0.25), 0 3px 8px rgba(255, 255, 255, 0.2);
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
    max-width: 1800px;
    margin: 0 auto;
    padding: 140px 40px 80px;
    color: var(--text-light);
    background: linear-gradient(180deg, #f3f6fb, #ffffff);
    border-radius: 12px;
    transition: color 0.5s, background 0.5s;
    position: relative;
    z-index: 1;
    overflow: hidden;
}

.main-content::before {
    content: "";
    position: absolute;
    inset: 0;
    background: radial-gradient(circle at 20% 30%, rgba(120, 161, 255, 0.45), transparent 65%),
        radial-gradient(circle at 80% 70%, rgba(200, 160, 255, 0.4), transparent 80%);
    z-index: -1;
    animation: subtleGlow-light 8s ease-in-out infinite alternate;
}

@keyframes subtleGlow-light {
    0% {
        opacity: 0.4;
    }

    100% {
        opacity: 0.9;
    }
}

.mode-dark .main-content {
    background: linear-gradient(180deg, #1a1c2e, #0f172a, #1e1b4b);
    color: var(--text-dark);
    border-radius: 16px;
    position: relative;
    z-index: 1;
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.6),
        inset 0 0 50px rgba(255, 255, 255, 0.03);
    backdrop-filter: blur(12px);
    overflow: hidden;
}

.mode-dark .main-content::before {
    content: "";
    position: absolute;
    inset: 0;
    background: radial-gradient(circle at 30% 20%, rgba(56, 189, 248, 0.4), transparent 70%),
        radial-gradient(circle at 70% 80%, rgba(168, 85, 247, 0.2), transparent 75%);
    mix-blend-mode: screen;
    z-index: -1;
    animation: subtleGlow 12s ease-in-out infinite alternate;
}

@keyframes subtleGlow {
    0% {
        opacity: 0.5;
    }

    100% {
        opacity: 0.95;
    }
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

.glass.qa-card:hover .n-card__body {
    box-shadow: 0 16px 36px rgba(0, 0, 0, 0.541) !important;
    transform: translateY(-3px);
    transition: all 0.35s ease;
}

/* Intro Card */
.intro-card {
    scroll-margin-top: 64px;
}

.intro-card .glass {
    background: rgba(255, 255, 255, 0.5);
    backdrop-filter: blur(10px);
    border-radius: 16px;
    transition: all 0.4s ease;
    box-shadow: 0 2px 2px rgba(0, 0, 0, 0.1);
}

.mode-dark .intro-card .glass {
    background: rgba(0, 0, 0, 0.2);
    border: 1px solid rgba(255, 255, 255, 0.05);
}

/* Grid 佈局：左文字 + 右影片 */
.intro-grid {
    display: grid;
    grid-template-columns: 60% 40%;
    align-items: center;
}

/* 左邊：頭像 + 文字 */
.profile-block {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 20px;
}

.avatar {
    width: 160px;
    height: 160px;
    border-radius: 50%;
    background-image: url("@/assets/avatar.jpg");
    background-size: cover;
    background-position: center;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
    transition: transform 0.4s ease, filter 0.4s ease;
}

.avatar:hover {
    transform: scale(1.05);
    filter: brightness(1.1);
}

.name {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-size: 36px;
    font-weight: 800;
    letter-spacing: 4.5px;
}

.short {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-size: 18px;
    line-height: 1.9;
    font-weight: 400;
    color: var(--subtext-light);
    max-width: 95%;
}

.mode-dark .short {
    color: var(--subtext-dark);
}

/* 右邊影片 */
.video-preview {
    position: relative;
    aspect-ratio: 16/9;
    border-radius: 12px;
    overflow: hidden;
    cursor: pointer;
    box-shadow: 0 4px 14px rgba(0, 0, 0, 0.2);
    transition: transform 0.3s;
}

.video-preview:hover {
    transform: scale(1.02);
}

.video-preview img,
.video-preview iframe {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.play-icon {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: rgba(0, 0, 0, 0.6);
    color: white;
    font-size: 40px;
    padding: 12px 25.7px;
    border-radius: 50%;
    pointer-events: none;
    z-index: 3;
    transition: opacity 0.3s ease;
    opacity: 1;
}

.video-preview:hover .play-icon {
    opacity: 0;
}

.click-overlay {
    position: absolute;
    inset: 0;
    cursor: pointer;
    background: transparent;
    z-index: 2;
}

/* Modal 影片 */
.modal-video-wrap {
    width: 80vw;
    height: 45vw;
    max-width: 1120px;
    max-height: 630px;
    margin: 0 auto;
}

.modal-video-wrap iframe {
    width: 100%;
    height: 100%;
    border-radius: 8px;
}


/* QA */
.qa-top-bar n-button,
.qa-modal-wrap n-button {
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

.qa-top-bar n-button:hover,
.qa-modal-wrap n-button {
    background: rgba(255, 255, 255, 0.18);
    box-shadow: 0 6px 18px rgba(0, 0, 0, 0.15);
    transform: translateY(-2px);
}

.mode-dark .qa-top-bar n-button,
.mode-dark .qa-modal-wrap n-button {
    color: var(--text-dark);
    background: rgba(20, 30, 50, 0.4);
}

.mode-dark .qa-top-bar n-button:hover,
.mode-dark .qa-modal-wrap n-button:hover {
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
    grid-template-columns: repeat(4, minmax(280px, 1fr));
    gap: 1.5rem;
    margin-top: 2rem;
    max-width: calc(4 * 1fr + 3 * 1.5rem);
    width: 100%;
}

.qa-grid p {
    margin: 0;
}

.qa-card .n-card-header {
    height: 40%;
    align-items: baseline;
}

.qa-card .n-card-n-card-header__main {
    height: 60%;
}

.qa-card {
    cursor: pointer;
    border-radius: 16px;
    overflow: hidden;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    backdrop-filter: blur(8px);
    background: rgba(255, 255, 255, 0.5);
    border: 1px solid rgba(255, 255, 255, 0.1);
    box-shadow: 0 2px 2px rgba(0, 0, 0, 0.1);
}

.mode-dark .qa-card {
    background: rgba(0, 0, 0, 0.2);
    border: 1px solid rgba(255, 255, 255, 0.05);
}

.qa-card:hover {
    transform: translateY(-6px);
    box-shadow: 0 12px 28px rgba(0, 0, 0, 0.25);
    text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.mode-dark .qa-card:hover {
    transform: translateY(-6px);
    box-shadow: 0 2px 6px rgba(255, 255, 255, 0.25);
    text-shadow: 0 1px 2px rgba(255, 255, 255, 0.1);
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
    text-align: justify;
    color: var(--subtext-light);
    line-height: 1.65;
    letter-spacing: 0.5px;
}

.mode-dark .qa-card div {
    color: var(--subtext-dark);
}

/* QA Focus */
.loading-fallback {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 40px;
    color: #555;
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-size: 1.2rem;
}

.mode-dark .loading-fallback {
    color: var(--subtext-dark);
}

.qa-spinner {
    width: 50px;
    height: 50px;
    border: 5px solid #eee;
    border-top: 5px solid #3b82f6;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin-bottom: 16px;
}

@keyframes spin {
    to {
        transform: rotate(360deg);
    }
}

.qa-focus {
    scroll-margin-top: 64px;
    margin-top: 2rem;
    padding: 1rem;
    background: rgba(255, 255, 255, 0.5);
    border-radius: 16px;
    backdrop-filter: blur(6px);
    max-height: 90vh;
    overflow-y: auto;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
}

.mode-dark .qa-focus {
    background: rgba(0, 0, 0, 0.4);
}


/* Footer */
.site-footer {
    margin-top: 64px;
    text-align: center;
    font-size: 14px;
    color: var(--subtext-light);
    transition: color 0.3s;
}

.site-footer a,
.site-footer div {
    color: var(--text-light);
    text-decoration: none;
    transition: color 0.3s;
}

.mode-dark .site-footer a,
.mode-dark .site-footer div {
    color: var(--text-dark);
}

.social-icon {
    margin: 0px 3px;
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
/* 桌機小於1440px */
@media (max-width: 1439px) {
    .qa-grid {
        grid-template-columns: repeat(4, 1fr);
    }
}

/* 桌機小於1280px */
@media (max-width: 1280px) {
    .qa-grid {
        grid-template-columns: repeat(3, 1fr);
    }
}

/* 小桌機 / 大平板 */
@media (max-width: 1024px) {
    .intro-grid {
        flex-direction: column;
        text-align: justify;
    }

    .avatar {
        margin: 0 auto;
    }
}

/* 平板橫向 */
@media (max-width: 960px) {
    .qa-grid {
        grid-template-columns: repeat(2, 1fr);
    }

    .intro-grid {
        display: grid;
        grid-template-columns: 1fr;
    }

    .video-preview {
        max-width: 600px;
        width: 60vw;
        margin: 0 auto;
    }

    .video-preview img,
    .video-preview iframe {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .profile-block {
        flex-direction: column;
        align-items: center;
        text-align: center;
        gap: 0;
    }

    .avatar {
        width: 25vw;
        height: 25vw;
    }

    .name {
        margin-top: 10px;
        margin-bottom: 0px;
    }
}

/* 平板縱向 / 大手機橫向 */
@media (max-width: 768px) {

    .site-header {
        height: 48px;
        padding: 0 16px;
        transition: top 0.3s, height 0.3s;
    }

    .site-header .brand {
        font-size: 16px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }

    .site-header .switch-label {
        display: none;
    }

    .switch-icon {
        font-size: 18px;
    }

    .profile-block {
        align-items: center;
        text-align: center;
    }

    .avatar {
        width: 30vw;
        min-width: 150px;
        height: 30vw;
        min-height: 150px;
    }

    .name {
        font-size: 28px;
    }

    .short {
        font-size: 16px;
        line-height: 1.8;
        max-width: 100%;
    }

    .video-preview {
        max-width: 100%;
        height: auto;
        aspect-ratio: 16/9;
        width: 80vw;
    }

    .video-preview img,
    .video-preview iframe {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

}

/* 大手機縱向 */
@media (max-width: 640px) {
    .hero-title {
        font-size: 32px;
    }

    .hero-sub {
        font-size: 16px;
    }

    .hero-btn {
        width: 90%;
    }

    .main-content {
        padding-left: 0;
        padding-right: 0;
    }

    .intro-card {
        scroll-margin-top: 10px;
    }

    .intro-grid {
        gap: 24px;
    }

    n-grid {
        grid-template-columns: 1fr !important;
    }

    .qa-grid {
        grid-template-columns: 1fr;
        gap: 1rem;
    }

    .qa-card {
        margin-left: 0;
        margin-right: 0;
        border-radius: 12px;
    }

    .qa-card div {
        font-size: 0.95rem;
    }

    .qa-card n-card__header {
        font-size: 1.1rem;
    }

    .qa-card .n-card-header {
        height: 50%;
        align-items: center;
    }

    .qa-card .n-card-n-card-header__main {
        height: 50%;
    }


    /*QA modal*/
    .n-modal-container .n-modal-mask {
        background-color: rgba(0, 0, 0, 0.9) !important;
    }
}

/* 小手機 */
@media (max-width: 480px) {
    .hero-title {
        font-size: 28px;
    }

    .hero-sub {
        font-size: 14px;
    }

    .short {
        font-size: 14px;
        line-height: 1.6;
    }

    .video-preview {
        width: 100%;
        height: auto;
        aspect-ratio: 16/9;
    }

    .qa-card {
        margin-left: 0;
        margin-right: 0;
    }
}
</style>
