<template>
  <div>【服务资源占用比】</div>
  <div ref="target" class="w-full h-full"></div>
</template>

<script setup>
import * as echarts from 'echarts'
import { ref,onMounted, watch } from 'vue';
const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
});
console.log(props.data);
const target = ref(null)
let myCharts = null
onMounted(()=>{
    myCharts = echarts.init(target.value)
    renderChart()
})
const renderChart = ()=>{
    const options = {
        //X轴展示数据
        yAxis:{
            show:false,
            type:'value',
            max:function(value){
                return parseInt(value.max*1.2)
            }
        },
        xAxis:{
            type:'category',
            data:props.data.servers.map(item=>item.name),
            axisLine:{
                show:false
            },
            axisTick:{
                show:false
            },
            axisLabel:{
                color:'#9Eb1c8'
            }
        },

        //位置
        grid:{
            top:0,
            left:-26,
            right:0,
            bottom:26,
            containLabel:true
        },
        //核心配置
        series:[
            {
                type:'bar',
                data:props.data.servers.map(item=>({
                    name:item.name,
                    value:item.value
                })),
                
                backgroundStyle:{
                    color:'rgba(255,255,255,0.2)'
                },
                itemStyle:{
                    color:'#5D98CE',
                    barBorderRadius:5,
                    shadowColor:'rgba(0,0,0,0.3)',
                    shadowBlur:5 
                },
                barWidth:12,
                label:{
                    show:true,
                    position:'top',
                    textStyle:{
                        color:'#fff'
                    },
                    formatter:'{c}%'
                }
                

            }
        ]
    }
    myCharts.setOption(options)
}
watch(()=>props.data,()=>{
    renderChart()
})
</script>

<style lang="scss" scoped></style>
