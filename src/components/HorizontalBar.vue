<template>
    <div>
        【大区数据信息】
    </div>
    <div class="w-full h-full" ref="target"></div>
</template>

<script setup>
import {onMounted, ref, watch} from 'vue'
import * as echarts from 'echarts'
const props =  defineProps({
    data:{
        type:Object,
        reqiured:true
    }
})

//构建echarts实例
const target = ref(null)
let myCharts = null
onMounted(()=>{
    myCharts = echarts.init(target.value)
    renderChart()
})
//options
const renderChart = ()=>{
    const options = {
        //X轴展示数据
        xAxis:{
            show:false,
            type:'value',
            max:function(value){
                return parseInt(value.max*1.2)
            }
        },
        yAxis:{
            type:'category',
            data:props.data.regions.map(item=>item.name),
            inverse:true,
            axisLine:{
                show:false
            },
            axisTick:{
                show:false
            },
            axisLabel:{
                color:'#91b1c8'
            }
        },

        //位置
        grid:{
            top:0,
            left:0,
            right:0,
            bottom:0,
            containLabel:true
        },
        //核心配置
        series:[
            {
                type:'bar',
                data:props.data.regions.map(item=>({
                    name:item.name,
                    value:item.value
                })),
                
                showBackground:true,
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
                    position:'right',
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

<style  scoped>

</style>