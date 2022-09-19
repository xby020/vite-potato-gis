<template>
  <div
    class="w-full h-full flex flex-col items-center justify-center bg-dark-400 pointer-events-auto"
  >
    <vue3-lottie
      animation-link="https://assets7.lottiefiles.com/packages/lf20_qp1q7mct.json"
    ></vue3-lottie>
    <h1
      class="mt-24 text-4xl font-bold tracking-widest text-white animate-pulse"
    >
      加载中，请稍后...
    </h1>
  </div>
</template>

<script setup lang="ts">
import { Vue3Lottie } from 'vue3-lottie';
</script>

<style scoped></style>
