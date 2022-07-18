<template>
    <div :style="{ width: `${width}px`, height: `${height}px` }" class="curve-chart">
        <div :style="{ width: `100%`, height: '10%' ,backgroundColor:'#fff',paddingTop:'10px' }" >
            <span class="title">
                {{ title }}
            </span>
            <span class="count">
                {{ count }}
            </span>
        </div>
        <div class="line"></div>
        <div ref="curveChart" :style="{ width: `100%`, height: '90%' }">
        </div>
    </div>
</template>
<script>
const echarts = require('echarts')

export default {
    name: 'CurveChart',
    props: {
        width: {
            default: 500
        },
        height: {
            default: 500
        },
        title: {
            default: '实时指令流',
        },
        count: {
            default: '1.5Gbps'
        }
    },
    data() {
        return {
            curveOption: {
                backgroundColor:'#fff',
                legend: {
                    data: ['态势', '情报', '事件', '检查', '业务'],
                    left:'left',
                    padding:[10,0,0,20]
                },
                tooltip: {
                    trigger: 'axis'
                },
                xAxis: {
                    type: 'category',
                    boundaryGap: false,
                },
                yAxis: {
                    type: 'value'
                },
                series: []
            },
            myChart: null,
            timeFrame: []
        }
    },
    mounted() {
        this.getCurveData()
        this.drawCurve()
    },
    methods: {
        drawCurve() {
            let container = this.$refs.curveChart
            let myChart = echarts.init(container)
            myChart.setOption(this.curveOption)
            this.myChart = myChart
            window.addEventListener('resize', this.myChart.resize())
        },
        getCurveData() {
            const curveSerie = {
                type: 'line',
                smooth: true,
                showSymbol: false
            }

            this.timeFrame = ['2019-01', '2019-02', '2019-03', '2019-04', '2019-05', '2019-06']
            this.curveOption.xAxis.data = this.timeFrame

            const resData = [[820, 932, 901, 934, 1290, 1330, 1320], [720, 882, 691, 634, 590, 830, 710], [720, 932, 801, 934, 1200, 1030, 1120],
            [920, 832, 991, 1034, 1090, 1030, 820], [1020, 782, 591, 734, 690, 530, 510]]

            for (let [idx, type] of ['态势', '情报', '事件', '检查', '业务'].entries()) {
                let serie = JSON.parse(JSON.stringify(curveSerie))
                serie.name = type
                serie.data = resData[idx]
                this.curveOption.series.push(serie)
            }
        }
    }
}
</script>
<style scoped>
.title {
    font-weight: 900;
    color: #000000;
    float: left;
    padding-left: 5%;
    line-height: 20px;
    font-size: 14px;
}

.count {
    float: left;
    color: #E8684A;
    line-height: 20px;
    padding-left: 3%;
    font-size: 14px;
}

.line {
    display: flex;
    align-items: center;
}

.line::before,
.title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: #E9E9E9;
}

.curve-chart{
    border-radius: 10px;
}
</style>