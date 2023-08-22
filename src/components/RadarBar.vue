<template>
  <div>【云端风险示警】</div>
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
const renderChart = () => {
  const options = {
    //雷达图的坐标系配置
    radar: {
      name: {
        textStyle: {
          color: "#05D5ff",
          fontSize: 14,
        },
    },
        shape: "polygon",
        center: ["50%", "50%"],
        radius: "80%",
        startAngle: 120,
        axisLine: {
          lineStyle: {
            color: "rgba(5,213,255,.8)",
          },
        },
        splitLine: {
          show: true,
          lineStyle: {
            width: 1,
            color: "rgba(5,213,255,.8)",
          },
        },
        indicator:props.data.risks.map(item=>({
            name:item.name,
            max:100
        })),
        splitArea:{
            show:false,

        }
        
    },
    //坐标极点
    polar: {
        center:['50%','50%'],
        radius:'0%'
    },
    //坐标角度
    angleAxis: {
        min:0,
        interval:5,
        clockwise:false
    },
    //径向轴
    radiusAxis: {
        min:0,
        interval:10,
        splitLine:{
            show:true,

        }
    },
    series: {
        type:'radar',
        symbol:'circle',
        symbolSize:10,
        itemStyle:{
            normal:{
                color:'#05D5FF'
            }
        },
        areaStyle:{
            normal:{
                color:'#05D5FF',
                opacity:0.5
            }
        },
        lineStyle:{
            width:2,
            color:'#05D5ff',

        },
        label:{
            normal:{
                show:true,
                color:'#FFF',
                
            }
        },
        data:[
            {
                value:props.data.risks.map(item=>item.value)
            }
        ]
    },
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
