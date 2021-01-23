<template>
    <div class="sales-report">
        <div class="header">电商商品趋势图</div>
        <div class="content">
            <div class="content-title-wraper">月销售增长率</div>
            <div class="content-index-wraper">
                <span class="arrow-up">^</span>
                <span class="percentage">34<span>%</span></span>
                <span class="text">+14,400</span>
            </div>
            <div id="content-charts" />
            <div class="content-circle-wraper">
                <div 
                    :class="['circle', selectedIndex === index ? 'selected' : '']" 
                    v-for="(item, index) in circle" 
                    :key="index"
                    @click="change(index)"  
                />
            </div>
            <div class="content-bottom-wraper">销售趋势</div>
        </div>
        <div class="footer">
            <div class="footer-wraper">
                <div class="left">
                    <div class="footer-title">订单销售额</div>
                    <div class="footer-sub-title">3月累计销售额</div>
                </div>
                <div class="right">
                    <small>￥</small>300,254.00
                </div>
            </div>
            <div class="progress-wraper">
                <div class="progress-bg">
                    <div class="progress-current" :style="{width:`${progress*100}%`}"></div>
                </div>
            </div>
            <div class="footer-text">
                <div>销售增长率</div>
                <div>34%</div>
            </div>
        </div>
    </div>
</template>

<script>
import ECharts from 'echarts'

export default {
    name: '',
    data() {
        return {
            circle: new Array(3),
            selectedIndex: 0,
            progress: 0.35
        }
    },
    props: {

    },
    methods: {
        change( index ){
            this.selectedIndex = index
            this.genChart()
        },
        genChart(){
            // 获取数据源
            const mockData = []
            for (let i = 0; i < 12; i++) {
                let tmp = Math.floor(Math.random() * 150) + 150
                mockData.push(tmp)
            }

            //获取chart对应的dom
            const chartDom = document.getElementById('content-charts')
            const chart = ECharts.init(chartDom)

            //生成渲染参数
            const options = {
                xAxis:{
                    show: false,
                    type: 'category'
                },
                yAxis:{
                    min: 0,
                    max: 350,
                    show: false,
                },
                series:[{
                    data:mockData,
                    type: 'line',
                    smooth: true,
                    areaStyle: {
                        color: 'rgba(75,193,252,0.5)'
                    },
                    lineStyle: {
                        width : 2.5,
                        color: 'rgba(75,193,252,1)'
                    },
                    itemStyle: {
                        color: 'rgba(75,193,252,1)',
                        borderWidth: 3
                    }
                }],
                grid: {
                    top: 0,
                    bottom: 0,
                    left: -30,
                    right: -30
                },
                toolbox: {
                    feature: {
                        saveAsImage: {}
                    }
                },
                tooltip:{
                    trigger: 'axis',
                    axisPointer: {
                        type: 'cross',
                        label: {
                            backgroundColor: '#6a7985'
                        }
                    }

                }
            }

            //渲染图表
            chart.setOption(options)
            // console.log(mockData);
        }
    },
    mounted() {
        this.genChart()
        this.task = setInterval(()=>{
            let index = this.selectedIndex
            index++ 
            if (index > 2) {
                index = 0
            }
            this.change(index)
        },3000)

    },
    // 钩子函数 在vue页面销毁前调用，解决挂载时多生成定时器问题
    destroyed(){
        this.task && clearInterval(this.task)
    }
}

</script>
<style lang='scss' scoped>
    .sales-report{
        /** 采用弹性布局 */
        display: flex;
        /** 竖着排列 */
        flex-direction: column;
        width: 100%;
        height: 100%;
        background-color: #fff;
        box-shadow: 0 2px 8px rgba(4,9,20,.03),0 2px 8px rgba(4,9,20,.03),0 2px 8px rgba(4,9,20,.03),0 2px 8px rgba(4,9,20,.03);
        .header{
            display: flex;
            /** 居中对齐 */
            align-items: center;
            width: 100%;
            height: 50px;
            border-bottom: 1px solid #eee;
            box-sizing: border-box;
            color: rgba(13,27,62,0.7);
            padding-left: 20px;

        }
        .content{
            flex: 1;
            display: flex;
            flex-direction: column;
            width: 100%;
            padding: 0 28px;
            /** 使得border不会额外占用像素 */
            box-sizing: border-box;
            .content-title-wraper{
                padding-top: 28px;
                color: rgb(108, 117, 125);
                font-size: 13px;
            }
            .content-index-wraper{
                display: flex;
                align-items: center;
                margin-top: 35px;
                .arrow-up{
                    color: rgb(58, 196, 125); 
                    font-size: 40px;
                    font-weight: bolder;
                }
                .percentage{
                    font-size: 40px;
                    font-weight: 700;
                    color: #333;
                    margin-left: 15px;
                    span{
                        font-size: 28px;
                        font-weight: 400;
                        color: #999;
                        margin-left: 2px;
                    }
                }
                .text{
                    margin-left: 15px;  
                    color: rgb(58, 196, 125);
                }
            }
            #content-charts{
                flex: 1;
            }
            .content-circle-wraper{
                display: flex;
                align-items: center;
                /** 横向居中 */
                justify-content: center;
                margin-top: 20px;
                .circle{
                    width: 10px;
                    height: 10px;
                    background-color: #fff;
                    border: 3px solid rgb(63, 106, 216);
                    border-radius: 50%;
                    margin: 0 5px;
                    &.selected{
                        width: 12px;
                        height: 12px;
                        border: 5px solid rgb(63, 106, 216);
                    }
                }
            }
            .content-bottom-wraper{
                margin: 10px 0;
                color: #999;
            }
        }
        .footer{
            width: 100%;
            height: 120px;
            border-top: 1px solid #eee;
            box-sizing: border-box;
            .footer-wraper{
                display: flex;
                padding: 14px 14px;
                .left{
                    .footer-title{
                        font-size: 13px;
                        font-weight: 700;
                        color: #333;
                    }
                    .footer-sub-title{
                        font-size: 13px;
                        color: #999;
                    }
                }
                .right{
                    flex: 1;
                    text-align: right;
                    color: rgb(58,196,125);
                    font-size: 25px;
                    font-weight: 700;
                    small{
                    font-weight: 400;

                    }
                }
            }
            .progress-wraper{
                flex: 1;
                display: flex;
                justify-content: center;
                align-items: center;
                padding: 14px;
                .progress-bg{
                    position: relative;
                    width: 100%;
                    height: 7px;
                    background: rgb(233,236,239);
                    border-radius: 3.5px;
                    .progress-current{
                        position: absolute;
                        left: 0;
                        top: 0;
                        height: 7px;
                        background: #3f6ad8;
                        border-radius: 3.5px;
                        &::after{
                            content: "";
                            position: absolute;
                            top: 0;
                            bottom: 0;
                            left: 0;
                            right: 0;
                            width: 100%;
                            border-radius: 3.5px;
                            background: #fff;
                            opacity: 0;
                            animation: progress-active 2s ease infinite; /** 引用动画 */
                        }
                        /* 自定义的动画 */
                        @keyframes progress-active {
                            from {
                                width: 0;
                                opacity: 0;
                            }
                            to {
                                width: 100%;
                                opacity: 0.3;
                            }
                        }
                    }
                }
            }
            .footer-text{
                display: flex;
                justify-content: space-between;
                padding: 0 14px 14px;
                font-size: 13px;
                color: #999;
            }
        }
    }
</style>