<template>
  <div>
    <div v-show="showLoading" class="loading-container">
      <a-icon type="loading" />
    </div>
    <div v-show="!showLoading" :id="elemId" :style="{width:'100%',height:'450px'}">
    
    </div>
  </div>
</template>
<script>
export default {
    name: 'EchartsBar',
    props:{
        title:{
            type:String,
            default:''
        },
        subTitle:{
            type:String,
            default:''
        },
        type:{
            type:String,
            default:''
        },
        elemId:{
            type:String,
            default:''
        },
        xData:{
            type:Array,
            default: function () {
                return []
            }
        },
        seriesData:{
            type:Array,
            default: function () {
                return []
            }
        },
        legend:{
            type:Array,
            default: function () {
                return []
            }
        },
        showLoading:{
            type:Boolean,
            default:false
        },
        chartConfig:{
            type:Object,
            default:()=> { }
        }
    },
    data(){
        return{
            chart: null
        }
    },
    watch: {
        seriesData(newVal){
            this.chart && this.chart.dispose()
            this.$nextTick(()=>{
                this.initChart()
            })
        },
        showLoading(newVal){
            console.log(newVal)
        }
    },
    beforeDestroy() {
        if(!this.chart){
            return
        }
        this.chart.dispose()
        this.chart = null
    },
    mounted() {
        this.$nextTick(()=>{
            setTimeout(()=>{
                this.initChart()
            },100)
        })
    },
    methods: {
        initChart(){
            this.chart = this.$echarts.init(document.getElementById(this.elemId))
            this.chart.setOption({
                tooltip:{ //是否显示提示框组件，包括提示框浮层和 axisPointer。
                    trigger:'axis', //axis 坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用。
                    axisPointer:{ // 坐标轴指示器，坐标轴触发有效
                      type:'shadow' // 默认为直线，可选为：line | shadow
                    }
                },
                title: {                //图形title
                        left: 'center',
                        text: '',
                    },
                legend:{
                    // data:this.legend
                    data:['底部','空白','总额']
                },
                grid:{ // 屏幕位置
                    // top:'-40%', // 因为总额是透明的，占位置，所以要加top
                    left:'',
                    right:'',
                    bottom:'',
                    containLabel: true, //是否包含坐标轴的刻度标签 true为显示
                    borderWidth:1     //网格的边框线宽。
                },
                xAxis:[
                    {
                        type:'category', //category 适用于离散的类目数据
                        // data:this.xData
                        data:['周一','周二','周三','周四','周五','周六','周日']
                    }
                ],
                yAxis:[
                    {
                        type:'value',
                        // scale:true,
                        //name:'价格'，
                        // min:0,
                        // nameLocation:'center',            //坐标轴名称显示位置。
                        // nameGap: 30,//与轴线间距
                        // boundaryGap:[0.2,0.2],
                        // axisLabel:{
                        //     formatter:this.chartConfig.axisLabel.formatter ? this.chartConfig.axisLabel.formatter : '{value}'
                        // },
                        // show:false,
                        // axisLine:{ // y轴
                        //     show:false
                        // },
                        // axisTick:{ // y轴刻度线
                        //     show:false
                        // },
                        // splitLine:{ // 网格线
                        //     show:false
                        // }

                    }
                ],
                // dataZoom: [{                           //区域缩放，从而能自由关注细节的数据信息
                //     type: 'inside',
                //     start: 0,
                //     end: 100
                // }, {
                //     start: 0,
                //     end: 100,
                //     handleIcon: 'M10.7,11.9v-1.3H9.3v1.3c-4.9,0.3-8.8,4.4-8.8,9.4c0,5,3.9,9.1,8.8,9.4v1.3h1.3v-1.3c4.9-0.3,8.8-4.4,8.8-9.4C19.5,16.3,15.6,12.2,10.7,11.9z M13.3,24.4H6.7V23h6.6V24.4z M13.3,19.6H6.7v-1.4h6.6V19.6z',
                //     handleSize: '80%',
                //     handleStyle: {
                //         color: '#fff',
                //         shadowBlur: 3,
                //         shadowColor: 'rgba(0, 0, 0, 0.6)',
                //         shadowOffsetX: 2,
                //         shadowOffsetY: 2
                //     }
                // }],
                series:[ //每个系列通过 type 决定自己的图表类型
                    {
                        name: '底部',
                        type: 'bar',
                        barMinWidth:10,
                        barMaxWidth:30, // 柱状图宽度
                        stack:'bar', // 统一命名时柱形图成叠加状态
                        label:{
                            show:true,
                            position:'inside'
                        },
                        itemStyle:{
                            color:function(params){
                                    var colorList = ['rgb(73,147,247)','rgb(73,147,247)','rgb(73,147,247)','rgb(73,147,247)','rgb(187,187,187)','rgb(187,187,187)','rgb(187,187,187)']
                                    return colorList[params.dataIndex]
                            },
                            // 鼠标悬停时：
                            emphasis:{
                                shadowBlur:10,
                                shadowOffsetX:0,
                                shadowColor:'rgba(0,0,0,0.5)'
                            }
                        },
                        data: [20, 32, 41, 44, 39, 33, 32]
                    },
                    {
                        name: '空白',
                        type: 'bar',
                        stack: 'bar',
                        barMinWidth:10,
                        barMaxWidth:30, // 柱状图宽度
                        itemStyle:{
                            color:'#ffffff',
                            borderColor:'#4876ff' // 边框色
                        },
                        data: [12, 13, 11, 14, 9, 23, 21]
                    },
                    {
                        name: '总额',
                        type: 'bar',
                        stack: 'bar',
                        barMinWidth:10,
                        barMaxWidth:30, // 柱状图宽度
                        label:{
                            show:true,
                            position:'insideBottom',
                            formatter:'{c}',
                            textStyle:{
                                color:'#000'
                            }
                        },
                        itemStyle:{
                            color:'rgba(128,128,128,0)'
                           
                        },
                        data: [32, 45, 52, 58, 48, 46, 53]
                    },
                ]
            })
        }
    },
}
</script>