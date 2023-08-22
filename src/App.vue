<template>
  <div class="bg-[url(./src/assets/bg.jpg)] bg-cover bg-center h-screen p-5 bg-blue-300 flex overflow-hidden text-white font-[Electronic]" v-if="data">
    <div class="flex-1 flex-col flex mr-5 bg-opacity-50  bg-slate-800 p-3" >
      <!-- 横向柱状图 -->
        <HorizontalBar class="h-1/3 box-border pb-4"  :data="data.regionData"></HorizontalBar>
      <!-- 雷达图 -->
      <RadarBar class="h-1/3 box-border pd-4" :data="data.riskData"></RadarBar>
      <!-- 关系图 -->
      <Relation class="h-1/3" :data="data.relationData"></Relation>
    </div>
    <div class="w-1/2  mr-5 flex flex-col">
      <TotalData class="bg-opacity-50 bg-slate-800 p-3" :data="data.totalData"></TotalData>
      <Mapecharts class="bg-opacity-50 bg-slate-800 p-3 flex-1 mt-4" :data="data.mapData"></Mapecharts>
    </div>
    <div class="flex flex-1 flex-col bg-opacity-50 bg-slate-800 p-3 " >
      <VericalBar class="h-1/3 box-border pb-4" :data="data.serverData"></VericalBar>
      <RingBar class="h-1/3 box-border pb-4" :data="data.abnormalData"></RingBar>
      <wordcloud class="h-1/3" :data="data.wordCloudData"></wordcloud>
    </div>
  </div>
</template>

<script setup>
import HorizontalBar from './components/HorizontalBar.vue';
import Mapecharts from './components/Mapecharts.vue';
import RadarBar from './components/RadarBar.vue';
import Relation from './components/Relation.vue';
import TotalData from './components/TotalData.vue';
import RingBar from './components/RingBar.vue';
import VericalBar from './components/VericalBar.vue';
import wordcloud from './components/wordcloud.vue';

import {getVisualization} from './api/visualization.js'
import { ref,onMounted, onBeforeMount } from 'vue';
const data = ref(null)
const timer =ref(null)
const loadData = async ()=>{
  data.value = await getVisualization()
  console.log(data.value);
}
onMounted(()=>{
  loadData()
  timer.value = setInterval(
  loadData,5000)
  
})
onBeforeMount(() => {
      if (timer.value !== null) {
        clearInterval(timer.value);
      }
    });

  </script>
<style scoped></style>
