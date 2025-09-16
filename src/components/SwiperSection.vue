<template>
    <Swiper :pagination="true" :navigation="!isMobile" :modules="modules" :autoHeight="true"
        :class="isMobile ? 'm-qa-swiper' : 'qa-swiper'" @swiper="onSwiperReady">
        <SwiperSlide v-for="(qa, index) in qas" :key="index">
            <div class="qa-focus-main">
                <h3 class="qa-question">{{ qa.question }}</h3>
                <button v-if="isMobile" class="qa-modal-close" @click="closeModalself">✕</button>
                <div class="qa-content">
                    <div class="qa-answer" v-html="qa.answer"></div>
                    <div class="qa-extra" :class="{
                        'timeline-mode': qa.type === 'timeline',
                        'modal-mode': qa.type === 'modal'
                    }">
                        <!-- 圖片模式 -->
                        <div v-if="qa.type === 'image' && qa.images?.length && !isMobile" class="qa-images">
                            <img v-for="(img, idx) in qa.images" :key="idx" :src="img" alt="QA Image"
                                class="qa-image" />
                        </div>

                        <!-- Timeline 模式 -->
                        <n-timeline v-else-if="qa.type === 'timeline' && !isMobile" :horizontal="isHorizontal"
                            class="qa-timeline">
                            <n-timeline-item v-for="(item, idx) in qa.timeline" :key="idx" :type="item.type"
                                :title="item.title" :content="item.content" :time="item.time"
                                :line-type="item.lineType" />
                        </n-timeline>

                        <!-- Modal 圖片/ICON 模式 -->
                        <div v-else-if="qa.type === 'modal'" class="qa-icon-wrapper">
                            <img v-if="qa.icon" :src="qa.icon" alt="icon" class="qa-icon"
                                @click="openModal(qa.iframeSrc)" />
                        </div>

                        <!-- ECharts 模式 -->
                        <div v-else-if="qa.type === 'chart' && qa.chartOptions && !isMobile" class="qa-chart-wrapper">
                            <v-chart :option="qa.chartOptions" autoresize />
                        </div>
                    </div>
                </div>
            </div>
        </SwiperSlide>
    </Swiper>

    <teleport to="body">
        <div v-if="showModal" class="custom-modal-overlay" @click.self="closeModal">
            <div class="custom-modal">
                <div class="custom-modal-header">
                    <button class="custom-modal-close" @click="closeModal">✕</button>
                </div>
                <div class="iframe-wrapper">
                    <iframe v-if="modalSrc" :src="modalSrc" frameborder="0"></iframe>
                </div>
            </div>
        </div>
    </teleport>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import { Pagination, Navigation } from 'swiper/modules';
import 'swiper/css';
import 'swiper/css/pagination';
import 'swiper/css/navigation';
import { NTimeline, NTimelineItem } from 'naive-ui';
import { use } from "echarts/core";
import { CanvasRenderer } from "echarts/renderers";
import { BarChart, PieChart } from "echarts/charts";
import { GridComponent, TooltipComponent, LegendComponent, TitleComponent } from "echarts/components";
import VChart from "vue-echarts";
use([CanvasRenderer, BarChart, PieChart, GridComponent, TooltipComponent, LegendComponent, TitleComponent]);

const isHorizontal = ref(false);
onMounted(() => {
    isHorizontal.value = window.innerWidth <= 1000;
})

const emit = defineEmits(["scroll", "close"])
const props = defineProps({
    currentIndex: Number,
    qas: Array,
    isMobile: Boolean,
    isDark: Boolean
})

props.qas.forEach((qa) => {
    if (qa.type === 'chart' && qa.data) {
        qa.data = reactive(qa.data);
    }
})

const modules = [Pagination, Navigation];

const onSwiperReady = (swiper) => {
    if (typeof props.currentIndex === "number") {
        swiper.slideTo(props.currentIndex, 0);
    }
    emit('scroll');

    // 初始更新一次
    updateBulletsLight(swiper);
    swiper.on('slideNextTransitionStart', () => updateBulletsLight(swiper));
    swiper.on('slidePrevTransitionStart', () => updateBulletsLight(swiper));
    swiper.on('slideChange', () => updateBulletsLight(swiper));


}

const updateBulletsLight = (swiper) => {
    if (props.isDark) {
        const bullets = swiper.pagination.el.querySelectorAll('.swiper-pagination-bullet');
        const active = swiper.pagination.el.querySelector('.swiper-pagination-bullet-active');

        const activeCenter = active.offsetLeft + active.offsetWidth / 2;
        const maxDist = 100; // 光線衰減範圍

        bullets.forEach((bullet, index) => {
            const bulletCenter = bullet.offsetLeft + bullet.offsetWidth / 2;
            const dist = Math.abs(bulletCenter - activeCenter);

            // 計算亮度
            const value = Math.max(0, 1 - dist / maxDist);
            const baseColor = props.isDark ? '124,140,255' : '102,178,255';
            bullet.style.backgroundColor = `rgba(${baseColor},${value})`;
            bullet.style.opacity = 0.3 + 0.7 * value;

            // 延遲流動效果：距離越遠，延遲越大
            const delay = Math.min(dist / maxDist, 1) * 0.2; // 最大延遲 0.2s
            bullet.style.transitionDelay = `${delay}s`;
        });
    } else {
        const bullets = swiper.pagination.el.querySelectorAll('.swiper-pagination-bullet');
        bullets.forEach(bullet => {
            bullet.style.backgroundColor = '';
            bullet.style.opacity = '';
            bullet.style.transitionDelay = '';
        });
    }
}

const showModal = ref(false);
const modalSrc = ref('');

const openModal = (src) => {
    modalSrc.value = src;
    showModal.value = true;
}

const closeModal = () => {
    showModal.value = false;
    modalSrc.value = '';
}

const closeModalself = () => {
    emit('close');
}

</script>


<style scope>
.qa-swiper {
    width: 100%;
    padding-top: 2rem;
    padding-bottom: 2rem;
}

/* bullet 基本樣式 */
.swiper-pagination-bullet {
    width: 20px;
    height: 4px;
    border-radius: 2px;
}

.mode-dark .swiper-pagination-bullet {
    background-color: rgba(124, 140, 255, 0.2);
    border: 1px solid rgba(124, 140, 255, 0.5);
    transition: background-color 0.3s ease, opacity 0.3s ease;
    cursor: pointer;
}


.mode-dark .swiper-pagination-bullet-active {
    border: none;
    box-shadow: 0 0 8px 3px rgba(124, 140, 255, 0.7);
    background-color: #7C8CFF;
}

.swiper-button-prev,
.swiper-button-next {
    color: #40a0ffc9;
    transition: color 0.3s;
}

.swiper-button-prev:hover,
.swiper-button-next:hover {
    color: rgb(18, 84, 150);
}

.mode-dark .swiper-button-prev,
.mode-dark .swiper-button-next {
    color: rgba(255, 255, 255, 0.7);
    transition: color 0.3s;
}

.mode-dark .swiper-button-prev:hover,
.mode-dark .swiper-button-next:hover {
    color: #4DA6FF;
}

.m-qa-swiper {
    background-color: var(--card-bg-light);
}

.mode-dark .m-qa-swiper {
    background-color: var(--card-bg-dark);
}

.qa-focus-main {
    align-items: flex-start;
    min-height: 500px;
    padding: 0 70px
}

.m-qa-swiper .qa-focus-main {
    align-items: flex-start;
    min-height: 500px;
    padding: 0 20px;
    animation: slideHint 0.6s ease-in-out forwards;
}


@keyframes slideHint {
    0% {
        transform: translateX(0);
    }

    25% {
        transform: translateX(10px);
    }

    50% {
        transform: translateX(-10px);
    }

    75% {
        transform: translateX(10px);
    }

    100% {
        transform: translateX(0);
    }
}

.qa-modal-close {
    position: absolute;
    top: -30px;
    right: -5px;
    border: none;
    background: transparent;
    font-size: 2.5rem;
    cursor: pointer;
    z-index: 10;
    color: var(--subtext-light);
}

.mode-dark .qa-modal-close {
    color: var(--subtext-dark);
}


.qa-question {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-weight: 700;
    font-size: 1.25rem;
    color: var(--text-light);
    margin-bottom: 1rem;
}


.mode-dark .qa-question {
    color: var(--subtext-dark);
}

.qa-content {
    display: flex;
    gap: 2rem;
    align-items: flex-start;
    flex-direction: row;
}

.qa-content:has(.timeline-mode) {
    gap: 6rem;
}

.qa-answer {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-size: 1rem;
    font-weight: 400;
    text-align: justify;
    color: var(--subtext-light);
    line-height: 1.65;
    letter-spacing: 0.5px;
    flex: 1 1 45%;
    min-width: 200px;
}

.mode-dark .qa-answer {
    color: var(--subtext-dark);
}

/* 圖片容器 */
.qa-images {
    display: flex;
    flex-wrap: wrap;
    /* 多張圖片換行 */
    gap: 1rem;
    /* 圖片間距 */
    max-width: 600px;
    /* 容器最大寬度 */
}

/* 單張圖片設定 */
.qa-image {
    width: calc(100% / 2 - 0.5rem);
    max-width: 300px;
    height: auto;
    object-fit: contain;
    border-radius: 8px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

/* timeline 模式下至少 30% */
.qa-extra.timeline-mode {
    flex: 0 0 20%;
    max-width: 20%;
}

.qa-extra.modal-mode {
    flex: 0 0 30%;
    max-width: 30%;
    display: flex;
    justify-content: center;
    align-items: center;
}

.qa-extra.chart-mode,
.qa-chart-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
}

.qa-chart-wrapper {
    width: 600px;
    height: 500px;
}

.qa-icon {
    cursor: pointer;
    width: 100px;
    height: 100px;
    object-fit: contain;
}

.qa-icon-wrapper {
    position: relative;
    display: inline-block;
    cursor: pointer;
    text-align: center;
}

.qa-icon-wrapper::after {
    content: "點擊圖案開始遊戲";
    position: absolute;
    top: 110%;
    left: 50%;
    transform: translateX(-50%);
    font-size: 14px;
    font-family: Inter, "Noto Sans TC", sans-serif;
    color: #7c7c7c;
    white-space: nowrap;
}

/* 遮罩 */
.custom-modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
}

/* Modal */
.custom-modal {
    position: relative;
    width: 510px;
    height: 570px;
    background: #fff;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
}

.custom-modal-header {
    width: 100%;
    height: 40px;
    background-color: rgba(218, 90, 40, 0.5);
}

/* 關閉按鈕 */
.custom-modal-close {
    position: absolute;
    top: 5px;
    right: 10px;
    border: none;
    background: transparent;
    font-size: 1.5rem;
    cursor: pointer;
    z-index: 10;
}

.iframe-wrapper {
    width: 100%;
    height: 100%;
    box-sizing: border-box;
}

.iframe-wrapper iframe {
    width: 100%;
    height: 100%;
    border: none;
    border-radius: 8px;
}


@media screen and (max-width: 1120px) {
    .qa-extra .qa-images {
        flex-direction: column;
    }

    .qa-image {
        width: auto;
    }
}

/* 窄螢幕響應式 */
@media screen and (max-width: 1000px) {
    .qa-content {
        flex-direction: column;
    }

    .qa-extra {
        margin: auto;
    }

    .qa-images {
        height: 200px;
    }

    .qa-image {
        display: block;
        height: 100%;
        width: auto;
    }

    .qa-extra.timeline-mode {
        flex: none;
        max-width: 150%;
    }

    .qa-extra.n-timeline {
        max-width: 150%;
        position: relative;
        left: 50%;
        transform: translateX(-50%);
    }

    .qa-content:has(.timeline-mode) {
        overflow: visible;
        height: 400px;
        position: relative;
    }


    .qa-content:has(.qa-icon-wrapper) {
        gap: 0em;
    }

    .qa-icon-wrapper::after {
        font-size: 0px;
    }
}

@media screen and (max-width: 800px) {
    .qa-timeline {
        overflow-x: auto;
    }
}

/* 窄螢幕改上下排列 */
@media screen and (max-width: 640px) {

    .qa-answer,
    .qa-extra,
    .qa-extra.timeline-mode,
    .qa-extra.modal-mode {
        flex: unset;
        max-width: 100%;
    }

    .custom-modal {
        width: 100%;
        height: 100%;
        border-radius: 0;
    }

    .iframe-wrapper iframe {
        width: 100%;
        height: 100%;
    }

    .qa-content:has(.timeline-mode) {
        gap: 0rem;
    }

    .qa-chart-wrapper {
        width: 600px;
        height: 300px;
    }

    .qa-icon {
        padding-bottom: 20px;
    }
}
</style>