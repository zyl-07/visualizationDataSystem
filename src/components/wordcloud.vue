<template>
    <div>【关键词条】</div>
    <div ref="target" class="w-full h-full"></div>
  </template>
  
  <script setup>
  import * as echarts from "echarts";
  import { ref, onMounted, watch } from "vue";
  import 'echarts-wordCloud'
  const props = defineProps({
    data: {
      type: Object,
      required: true,
    },
  });
  console.log(props.data);
  const target = ref(null);
  let myCharts = null;
  onMounted(() => {
    myCharts = echarts.init(target.value);
    renderChart();
  });
  const RangeRGB = ()=>{
    const r = Math.floor(Math.random()*255)
    const g = Math.floor(Math.random()*255)
    const b = Math.floor(Math.random()*255)
    return   `rgb(${r},${g},${b})`
  }
  const renderChart = () => {
    const options ={
        series:[
            {
                type:'wordCloud',
                sizeRange:[8,46],
                rotationRange:[0,0],
                gridSize:0,
                layoutAnimation:true,
                textStyle:{
                    color:RangeRGB
                },
                emphasis:{
                    textStyle:{
                        fontWeight:'bold',
                        color:'#000'
                    }
                },
                data:props.data.datas
            }
        ]
    }
    myCharts.setOption(options);
  };
  watch(
    () => props.data,
    () => {
      renderChart();
    }
  );
  </script>
  
  <style lang="scss" scoped></style>
  