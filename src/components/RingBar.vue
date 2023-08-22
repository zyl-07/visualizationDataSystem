<template>
  <div>【大区风险处理】</div>
  <div ref="target" class="w-full h-full"></div>
</template>

<script setup>
import * as echarts from "echarts";
import { ref, onMounted, watch } from "vue";
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
const getSeriesData = () => {
  const series = [];
  //上
  props.data.abnormals.map((item, index) => {
    series.push({
      name: item.name,
      type: "pie",
      clockWise: false,
      hoverAnimation: false,
      radius: [78 - index * 15 + "%", 68 - index * 15 + "%"],
      center: ["50%", "50%"],
      label: {
        show: false,
      },
      data: [
        {
          value: item.value,
          name: item.name,
        },
        {
          value: 1000,
          itemStyle: {
            color: "rgba(0,0,0,0)",
            borderWidth: 0,
          },
          tooltip: {
            show: false,
          },
          hoverAnimation: false,
        },
      ],
    });
  
  //下
  series.push({
      name: item.name,
      type: "pie",
      clockWise: false,
      slient:true,
      z:1, 
      hoverAnimation: false,
      radius: [78 - index * 15 + "%", 68 - index * 15 + "%"],
      center: ["50%", "50%"],
      label: {
        show: false,
      },
      data:[
        {
            value:7.5,
            itemStyle:{
                color:'rgba(3,31,62)',
                borderWidth:0
            },
            tooltip:{
                show:false,
               
            },
            hoverAnimation:false
        },
        {
            value:2.5,
            itemStyle:{
                color:'rgba(0,0,0,0)',
                borderWidth:0
            },
            tooltip:{
                show:false,
                
            },
            hoverAnimation:false
        }
      ]
    })
});
  return series;
};
const renderChart = () => {
  const options = {
    legend: {
      icon: "circle",
      top: "14%",
      left: "60%",
      data: props.data.abnormals.map((item) => item.name),
      width: -5,
      itemWidth: 10,
      itemHeight: 10,
      itemGap: 6, //间距
      textStyle: {
            fontSize: 12,
            lineHeight: 5,
            color: "#fff",
      },
    },
    tooltip: {
      show: true,
      trigger: "item",
      formatter: "{a}<br>{b}:{c}({d}%)",
    },
    yAxis: [
      {
        type: "category",
        inverse: true,
        axisLine:{
            show:false
        }
      },
    ],
    xAxis: [
      {
        show: false,
      },
    ],
    series: getSeriesData(),
  };
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
