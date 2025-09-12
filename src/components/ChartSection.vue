<template>
    <v-chart :option="option" autoresize style="height:300px;width:100%" />
</template>

<script setup>
import { defineProps } from "vue";
import VChart from "vue-echarts";

defineProps({
    option: Object
});
</script>
