<template>
    <Swiper :pagination="true" :navigation="true" :modules="modules" :autoHeight="true" class="qa-swiper"
        @swiper="onSwiperReady">
        <SwiperSlide v-for="(qa, index) in qas" :key="index">
            <div class="qa-focus-main">
                <h3 class="qa-quesion">{{ qa.question }}</h3>
                <div v-html="qa.answer" class="qa-answer" />
            </div>
        </SwiperSlide>
    </Swiper>
</template>

<script setup>
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Pagination, Navigation } from 'swiper/modules'
import 'swiper/css'
import 'swiper/css/pagination'
import 'swiper/css/navigation'

const emit = defineEmits(["scroll"])
const props = defineProps({
    currentIndex: Number,
    qas: Array
})

const modules = [Pagination, Navigation];

const onSwiperReady = (swiper) => {
    console.log("Swiper ready", swiper)
    if (typeof props.currentIndex === "number") {
        swiper.slideTo(props.currentIndex, 0)
    }
    emit('scroll');
}

</script>


<style scope>
.qa-focus-main {
    align-items: flex-start;
    min-height: 500px;
    padding: 0 50px;
}

.qa-quesion {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-weight: 700;
    font-size: 1.25rem;
    color: var(--text-light);
}

.mode-dark .qa-quesion {
    color: var(--subtext-dark);
}

.qa-answer {
    font-family: Inter, "Noto Sans TC", sans-serif;
    font-size: 1rem;
    font-weight: 400;
    color: var(--subtext-light);
    line-height: 1.65;
    letter-spacing: 0.5px;
}

.mode-dark .qa-answer {
    color: var(--subtext-dark);
}

.qa-swiper {
    width: 100%;
    padding-top: 2rem;
    padding-bottom: 2rem;
}
</style>