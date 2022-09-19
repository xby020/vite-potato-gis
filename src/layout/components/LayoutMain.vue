<template>
  <div
    class="w-full h-full flex flex-col justify-start items-center bg-dark-bg"
  >
    <!-- header -->
    <div
      class="relative w-full h-120px flex-none flex justify-center items-center pt-24px"
    >
      <div class="w-full h-full"></div>
    </div>

    <div class="w-full flex-auto">
      <div class="w-full h-full">
        <slot name="default"></slot>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { getWeather } from '@/utils/gis/gis';
import dayjs from 'dayjs';
import Dayjs from 'dayjs';
import zhCN from 'dayjs/locale/zh-cn';

dayjs.locale(zhCN);

const weather = reactive({
  temperature: 27,
  desc: '',
  wind: ''
});

onMounted(async () => {
  try {
    const weatherResult = await getWeather('汉阳');
    const { forecasts } = weatherResult;
    weather.temperature = forecasts[0].dayTemp;
    weather.desc = forecasts[0].dayWeather;
    weather.wind = `${forecasts[0].dayWindDir}风 ${forecasts[0].dayWindPower}级`;
  } catch (err) {
    window.$message.error('获取天气失败');
  }
});

// title
const title = ref(import.meta.env.VITE_APP_TITLE);

// date
const time = ref(Dayjs().format('HH:mm:ss'));
const date = ref(Dayjs().format('YYYY-MM-DD'));
const week = ref(Dayjs().format('dddd'));

const clock = ref<NodeJS.Timer>();

onMounted(() => {
  clock.value = setInterval(() => {
    time.value = Dayjs().format('HH:mm:ss');
    date.value = Dayjs().format('YYYY-MM-DD');
    week.value = Dayjs().format('dddd');
  }, 1000);
});

onUnmounted(() => {
  clearInterval(clock.value);
});
</script>

<style scoped></style>
