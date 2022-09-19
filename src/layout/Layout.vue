<template>
  <div
    class="w-1920px h-1080px overflow-hidden transform-gpu original-scale"
    :style="scaleStyle"
  >
    <div class="relative w-full h-full bg-dark-300 text-light-50">
      <!-- map -->
      <div class="absolute z-1 top-0 left-0 w-full h-full">
        <sugon-map></sugon-map>
      </div>

      <!-- dark corner -->
      <div
        class="absolute z-2 top-0 left-0 w-full h-full pointer-events-none dark-corner"
      ></div>

      <!-- ui -->
      <div class="absolute z-3 top-0 left-0 w-full h-full pointer-events-none">
        <layout-main v-if="mapReady">
          <router-view></router-view>+
        </layout-main>
      </div>

      <!-- overlay -->
      <div
        class="absolute z-999 top-0 left-0 w-full h-full pointer-events-none"
      >
        <transition
          leave-active-class="transition transform duration-500 ease-in-out"
          leave-from-class="opacity-100"
          leave-to-class="opacity-0"
        >
          <layout-overlay v-if="showOverlay"></layout-overlay>
        </transition>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import LayoutMain from './components/LayoutMain.vue';
import LayoutOverlay from './components/LayoutOverlay.vue';
import { useMapStore } from '@/store/modules/map';
import { useWindowSize } from '@vueuse/core';

// system scale style
const { width, height } = useWindowSize();
const scaleStyle = computed(() => {
  const wr = width.value / 1920;
  const hr = wr;
  return {
    transform: `scale(${wr}, ${hr})`
  };
});

const mapStore = useMapStore();
const { mapReady } = storeToRefs(mapStore);

// show overlay
const showOverlay = ref(false);
onMounted(() => {
  showOverlay.value = true;
});
watch(
  () => mapReady.value,
  (val) => {
    if (val) {
      setTimeout(() => {
        showOverlay.value = false;
      }, 1000);
    }
  }
);
</script>

<style scoped>
.dark-corner {
  box-shadow: 0 0 100px 70px rgba(0, 0, 0, 1) inset;
}
.original-scale {
  transform-origin: 0% 0px;
}
</style>
