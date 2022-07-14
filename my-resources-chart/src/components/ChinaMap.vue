<template>
  <div>
    <div id="chinaMap" :style="{ width: `${width}px`, height: `${height}px` }">
    </div>
    <!-- <div id="chinaMap" :style="{ width: `60vw`, height: `90vh` }">
    </div> -->
  </div>

</template>

<script>
import icon1 from '../assets/icon/指令流.png'
import icon2 from '../assets/icon/数据流.png'
import icon3 from '../assets/icon/状态流.png'
import icon4 from '../assets/icon/故障@2x.png'
import icon5 from '../assets/icon/正常@2x.png'
import icon6 from '../assets/icon/未接入@2x.png'

const echarts = require('echarts')
const china = require('echarts/map/json/china.json')
echarts.registerMap('china', china)

export default {
  name: 'ChinaMap',
  props: {
    width:{
      default:1000
    },
    height:{
      default:1000
    }
  },
  data() {
    return {
      streamData: [],
      streamColor: {
        '指令流': 'red',
        '数据流': 'green',
        '状态流': '#F01CED',
      },
      pointColor: {
        '故障': 'red',
        '正常': 'green',
        '未接入': 'blue'
      },
      pointIcon: {
        '故障': icon4,
        '正常': icon5,
        '未接入': icon6
      },
      myChart: null,
      streamOption: {
        backgroundColor:'#fff',
        geo: {
          show: true,
          map: 'china',
          roam: false,
          label: {
            normal: {
              show: false,
              textStyle: {
                color: '#5F608A',
                fontSize: 12,
                fontFamily: 'SourceHanSansCN-Medium',
                fontWeight: 500,
              }
            },
            emphasis: {
              color: '#5F608A',
              show: false
            }
          },
          itemStyle: {
            normal: {
              show: true,
              areaColor: '#fff',
              borderColor: '#9EA4DF',
              borderWidth: 0.75,

              shadowColor: `#051B39 `,
              shadowBlur: 0.75,
              shadowOffsetX: 0,
              shadowOffsetY: 1
            },
            emphasis: {
              show: false,
              areaColor: '#fff',
            }
          }
        },
        series: [],
        legend: [{
          orient: 'vertical',
          top: 'bottom',
          left: 'left',
          padding:[0,0,140,40],
          data: [{
            name: '指令流',
            icon: `image://${icon1}`
          },
          {
            name: '数据流',
            icon: `image://${icon2}`
          }, {
            name: '状态流',
            icon: `image://${icon3}`
          }
          ],
          selectedMode: 'multiple',
          textStyle: {
            fontSize: 12
          },
          itemHeight: 10,
          itemWidth: 20,
        }, {
          orient: 'vertical',
          top: 'bottom',
          left: '10%',
          padding:[0,0,140,40],
          data: [{
            name: '故障',
            icon: `image://${icon4}`
          }, {
            name: '正常',
            icon: `image://${icon5}`
          }, {
            name: '未接入',
            icon: `image://${icon6}`
          }],
          selectedMode: 'multiple',
          textStyle: {
            fontSize: 12
          },
          itemHeight: 12,
          itemWidth: 12,
        }]
      }
    }
  },
  created() {

  },
  mounted() {
    this.getPointData()
    this.getStreamData()
    this.drawLine()
  },
  destroyed() {
    this.myChart.dispose()
    window.removeEventListener('resize', this.myChart.resize())
  },
  methods: {
    resize() {

    },
    drawLine() {

      let container = document.getElementById('chinaMap')
      let myChart = echarts.init(container)
      myChart.setOption(this.streamOption)
      this.myChart = myChart
      // let chartResize = () => {
      //   container.style.width = 100 + '%'
      //   container.style.height = window.innerHeight * 0.7 + 'px'
      // }
      window.addEventListener('resize', this.myChart.resize())
    },
    getStreamData() {
      const resData = [{
        fromName: `北京`,
        toName: `包头`,
        value: 30,
        coords: [[101.4038, 36.8207], [109.642836, 19.327065]],
        type: '指令流'
      }, {
        fromName: `北京`,
        toName: `包头`,
        value: 30,
        coords: [[124.213495, 51.9124], [109.642836, 19.327065]],
        type: '数据流'
      }, {
        fromName: `北京`,
        toName: `包头`,
        value: 30,
        coords: [[121.48, 31.22], [109.642836, 19.327065]],
        type: '状态流'
      }, {
        fromName: `北京`,
        toName: `包头`,
        value: 30,
        coords: [[101.4038, 36.8207], [119.3, 26.08]],
        type: '指令流'
      },
      {
        fromName: `北京`,
        toName: `包头`,
        value: 30,
        coords: [[101.4038, 36.8207], [113.3, 40.12]],
        type: '指令流'
      },
      {
        fromName: `北京`,
        toName: `包头`,
        value: 30,
        coords: [[101.4038, 36.8207], [98.289152, 39.77313]],
        type: '指令流'
      },
      {
        fromName: `北京`,
        toName: `包头`,
        value: 30,
        coords: [[121.979603, 39.627114], [119.3, 26.08]],
        type: '指令流'
      }]
      const streamSerie = {
        type: `lines`,
        smooth: true,
        symbol: 'circle',
        coordinateSystem: `geo`,
        label: {
          show: true,
          position: 'right',
          formatter: '{b}'
        },
        effect: {
          show: true,
          period: 10,
          trailLength: 0.1,
          color: `#fff`,
          symbolSize: 9,
          symbol:'arrow',
        },
        lineStyle: {
          normal: {
            color: {
              type: `linear`,
              x: 0,
              y: 0,
              x2: 0,
              y2: 1,
              colorStops: [{
                offset: 0, color: `red` // 0% 处的颜色
              }, {
                offset: 1, color: `rgba(255, 255, 255, 0.95)` // 100% 处的颜色
              }],
              globalCoord: false // 缺省为 false
            },
            width: 3.5,
            curveness: 0.451
          },
        },
      }
      this.streamData = resData

      for(let type of ['指令流','数据流','状态流']){
        const stream = JSON.parse(JSON.stringify(streamSerie))
        stream.name = type
        stream.lineStyle.normal.color.colorStops[0].color = this.streamColor[type]
        stream.effect.color = this.streamColor[type]
        stream.data = this.streamData.filter(i => i.type == type)
        this.streamOption.series.push(stream)
      }
    },
    getPointData() {
      const resData = [{
        name: '北京',
        coord: [116.16, 39.92],
        type: '正常'
      }, {
        name: '上海',
        coord: [121.48, 31.22],
        type: '故障'
      }, {
        name: '招远',
        coord: [120.38, 37.35, 142],
        type: '未接入'
      }]
      const pointSerie = {
        type: 'scatter',
        coordinateSystem: 'geo',
        symbolSize: 18,
        label: {
          show: true,
          formatter: '{b}',
          position: 'bottom',
          textStyle: {
            fontSize: 12,
            color: '#000',
            fontWeight:700
          }
        }
      }
      for (let type of ['正常', '故障', '未接入']) {
        const point = JSON.parse(JSON.stringify(pointSerie))
        point.name = type
        point.data = resData.filter(i => i.type == type).map(i => {
          return {
            name: i.name,
            value: [...i.coord, 1],
            type: i.type
          }
        })
        point.symbol = `image://${this.pointIcon[type]}`
        point.itemStyle = {
          normal: {
            color: this.pointColor[type]
          }
        }
        this.streamOption.series.push(point)
      }
    }
  }
}
</script>

<style scoped>

</style>
