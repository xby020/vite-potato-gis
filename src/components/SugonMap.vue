<template>
  <div class="w-full h-full" id="map"></div>
</template>

<script setup lang="ts">
import AMapLoader from '@amap/amap-jsapi-loader';
import '@amap/amap-jsapi-types';
import { useMapStore } from '@/store/modules/map';
import { createGeoFence, flyTo, getDistrict } from '@/utils/gis/gis';

const mapStore = useMapStore();
const route = useRoute();

onMounted(async () => {
  AMapLoader.load({
    key: 'bdb0638a28f478220a73b5db9014654b',
    version: '2.0',
    plugins: ['AMap.DistrictSearch', 'AMap.Weather'],
    Loca: {
      version: '2.0.0'
    }
  }).then(async (AMapInstance: typeof AMap) => {
    // set global loca instance
    //@ts-ignore
    window.Loca = Loca;
    window.AMap = AMapInstance;

    // map
    const hanyangInfo: any = await getDistrict('汉阳区');
    console.log(hanyangInfo);
    const boundary = hanyangInfo[0].boundaries[0].map((item: any) => {
      return [item.lng, item.lat];
    });

    // set global map instance
    /**
     * 创建地图实例
     */
    window.map = new AMapInstance.Map('map', {
      viewMode: '3D',
      zoom: 12,
      center: [114.21859, 30.554287],
      pitch: 55,
      mapStyle: 'amap://styles/9f34436ac1b8ea3b0427ebae81bd34a5',
      showBuildingBlock: true
    });
    // set map ready
    window.map.on('complete', async () => {
      window.loca.animate.start();
      mapStore.mapReady = true;
    });
    //@ts-ignore
    window.loca = new Loca.Container({
      map: window.map
    });
  });
});
</script>

<style scoped></style>
