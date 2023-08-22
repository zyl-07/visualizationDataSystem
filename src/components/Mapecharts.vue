<template>
  <div ref="target" class="w-full h-full bg-opacity-50"></div>
</template>

<script setup>
import * as echarts from "echarts";
import { ref, onMounted, watch } from "vue";
import mapJson from "@/assets/mapData/china.json";
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
  echarts.registerMap("china", mapJson);
  myCharts = echarts.init(target.value);
  renderChart();
});

const renderChart = () => {
  const options = {
    
    timeline: {
      data: props.data.voltageLevel,
      axisType: "category",
      autoPlay: true,
      playInterval: 3000,
      left: "10%",
      right: "10%",
      width: "80%",
      label: {
        normal: {
          textStyle: {
            color: "#DDD",
          },
        },
        emphasis: {
          textStyle: {
            color: "#fff",
          },
        },
      },
      symbolSize: 10,
      lineStyle: {
        color: "#555",
      },
      checkpointStyle: {
        borderColor: "#888",
        borderWidth: 2,
      },
      controlStyle: {
        showNextBtn: true,
        showPrevBtn: true,
        normal: {
          color: "#666",
          borderColor: "#666",
        },
        empahsis: {
          color: "#aaa",
          borderColor: "#aaa",
        },
      },
    },
    baseOption: {
      grid: {
        right: "2%",
        top: "15%",
        bottom: "10%",
        width: "20%",
      },
      geo: {
        show: true, // 显示地理地图。
        map: "china", // 指定使用的地图（在这种情况下，是中国地图）。
        roam: true, // 允许地图漫游和缩放。
        zoom: 0.8, // 设置初始缩放级别为 0.8。
        center: [113.83531246, 34.0267395887], // 设置地图的初始中心点。
        itemStyle: {
          normal: {
            borderColor: "rgba(147, 235, 248, 1)", // 地图元素的边框颜色。
            borderWidth: 1, // 地图元素的边框宽度。
            areaColor: {
              type: "radial", // 定义区域颜色的渐变类型为径向渐变。
              x: 0.5, // 渐变中心的 X 坐标。
              y: 0.5, // 渐变中心的 Y 坐标。
              r: 0.5, // 渐变的半径。
              colorStops: [
                {
                  offset: 0,
                  color: "rgba(147, 235, 248, 0)", // 起始点的颜色（透明）。
                },
                {
                  offset: 1,
                  color: "rgba(147, 235, 248, .2)", // 结束点的颜色（实色）。
                },
              ],
            },
          },
        },
        emphasis:{
            areaColor:'#389BB7',
            borderWidth:0
        }
      },
    },

    options: [],
  };
  props.data.voltageLevel.forEach((item, index) => {
    options.options.push({
      
      title: [
        {
          text: "2019-2023年度数据统计",
          left: 0,
          top: 0,
          textStyle: {
            color: "#ccc",
            fontSize: 30,
          },
        },
        {
          id: "statistic",
          text: item + "年数据统计情况",
          right: 0,
          top: "4%",
          textStyle: {
            color: "#ccc",
            fontSize: 20,
          },
        },
      ],
      xAxis: {
        type: "value",
        scale: true,
        position: "top",
        spliteLine: {
          show: false,
        },
        axisTick: {
          show: false,
        },
        axisLabel: {
          margin: 2,
          textStyle: {
            color: "#aaa",
          },
        },
      },
      yAxis: {
        type: "category",

        axisLine: {
          show: true,
          lineStyle: {
            color: "#ddd",
          },
        },
        axisTick: {
          show: false,
        },
        axisLabel: {
          interval: 0,
          textStyle: {
            color: "#ddd",
          },
        },
        data: props.data.categoryData[item].map((item) => item.name),
      },
      series: [
        {
          type: "bar",
          zlevel: 1.5,
          itemStyle: {
            normal: {
              color: props.data.colors[index],
            },
          },
          data: props.data.categoryData[item].map((item) => item.value),
        },
        {
            type:'effectScatter',
            coordinateSystem:'geo',
            data:props.data.topData[item],
            symbolSize:function(val){
                return val[2]/5
            },
            showEffectOn:'render',
            rippleEffect:{
                brushType:'stroke'
            },
            label:{
                normal:{
                    show:true,
                    formatter:'{b}',
                    position:'right',
                    
                }
            },
            itemStyle:{
                color:props.data.colors[index],
                shadowBlur:5,
                shadowColor:props.data.colors[index]
            }
        }
      ],
    });
  });
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
