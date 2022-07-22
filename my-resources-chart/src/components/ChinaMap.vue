<template>
  <div>
    <span class="tooltiptext" ref="toolTip">
      <div style="color:#4E85D7;font-weight: 800;">
        <i class="el-icon-s-order"></i><span>关联信息</span>
      </div>
      <div v-html="tooltipText"></div>
    </span>
    <div ref="chinaMap" id="chinaMap" :style="{ width: `${width}px`, height: `${height}px` }">

    </div>
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
    width: {
      default: 1000
    },
    height: {
      default: 1000
    },
    scaleRatio: {
      default: 1
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
        backgroundColor: '#fff',
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
        // legend:[]
        legend: [{
          orient: 'vertical',
          top: 'bottom',
          left: 'left',
          padding: [0, 0, 140, 40],
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
          padding: [0, 0, 140, 40],
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
      },
      tooltipText: ''
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
      this.$nextTick(() => {
        if (this.$refs.chinaMap) {
          // this.$refs.chinaMap.style.width = `${this.width * this.scaleRatio}px`
          // this.$refs.chinaMap.style.height = `${this.height * this.scaleRatio}px`
          // this.$refs.chinaMap.style.transform = `scale(${1 / this.scaleRatio})`
          // this.$refs.chinaMap.style.transformOrigin = 'left top'
          this.myChart.resize()
        }
      })
    },
    drawLine() {
      let container = document.getElementById('chinaMap')
      let myChart = echarts.init(container)
      myChart.setOption(this.streamOption)
      myChart.on('click', e => {
        if (!e.data) {
          this.$refs.toolTip.style.visibility = 'hidden'
          return
        }
        // console.log(e.fromName)
        if (e.data.fromName) {
          this.tooltipText = `${e.data.fromName} => ${e.data.toName} <br> ${e.data.type}`
        }
        // console.log('scale:',this.scaleRatio)
        if (e.data.name) {
          this.tooltipText = `${e.data.name} <br> ${e.data.type}`
        }
        this.$refs.toolTip.style.visibility = 'visible'
        this.$refs.toolTip.style.left = `${e.event.offsetX - this.$refs.toolTip.offsetWidth /2}px`
        this.$refs.toolTip.style.top = `${(e.event.offsetY + this.$refs.chinaMap.getBoundingClientRect().top / this.scaleRatio - this.$refs.toolTip.offsetHeight - 15)}px`
        console.log(this.$refs.toolTip.offsetHeight)
      })
      this.myChart = myChart
      window.addEventListener('resize', this.resize)
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
          symbol: 'arrow',
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
                offset: 0, color: `red`
              }, {
                offset: 1, color: `rgba(255, 255, 255, 0.95)`
              }],
              globalCoord: false
            },
            width: 3.5,
            curveness: 0.451
          },
        },
      }
      this.streamData = resData

      for (let type of ['指令流', '数据流', '状态流']) {
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
            fontWeight: 700
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
#container{
  padding: 0 !important;
}
.tooltiptext {
  visibility: hidden;
  width: 220px;
  height: 100px;
  color: #000;
  text-align: center;
  padding: 5px 0;
  border-radius: 6px;

  background: #FFFFFF;
  box-shadow: 0px 4px 10px 0px rgba(5, 27, 57, 0.4000);
  border: 1px solid #9EA4DF;

  /* 定位 */
  position: fixed;
  z-index: 1;

  font-size: 14px;
  font-family: SourceHanSansCN-Regular, SourceHanSansCN;
  font-weight: 400;
  line-height: 21px;
}

.tooltiptext::after {
  content: " ";
  position: absolute;
  top: 100%;
  /* 提示工具底部 */
  left: 50%;
  margin-left: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: rgb(255, 255, 255) transparent transparent transparent;
}
</style>
