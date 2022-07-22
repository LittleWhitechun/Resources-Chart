<template>
  <div id="app">
    <div id="container">
      <div class="map">
        <div class="map__title">
          <div class="map__title-item">
            <div class="map__title-item__title">监控平台接入总量</div>
            <div class="map__title-item__count">200</div>
          </div>
          <div style="float:left;width: 1px;height: 47px; background: #0F4AB8;"></div>
          <div class="map__title-item">
            <div class="map__title-item__title">中央</div>
            <div class="map__title-item__count">10</div>
          </div>
          <div class="map__title-item">
            <div class="map__title-item__title">省</div>
            <div class="map__title-item__count">32</div>
          </div>
          <div class="map__title-item">
            <div class="map__title-item__title">市</div>
            <div class="map__title-item__count">17</div>
          </div>
        </div>
        <ChinaMap :width="1210" :height="960" :scaleRatio="scaleRatio" />
      </div>
      <div class="curve">
        <div class="select-row">
          <el-date-picker v-model="selectData.date" type="date" placeholder="选择日期" class="date-select" @change="changeCurve">
          </el-date-picker>
          <el-select v-model="selectData.level" placeholder="请选择" class="level-select" @change="changeCurve">
            <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
            </el-option>
          </el-select>
        </div>
        <CurveChart :width="492" :height="289" />
        <CurveChart :width="492" :height="289" title="实时数据流" count="10000条" />
        <CurveChart :width="492" :height="289" title="实时状态流" count="0.5Gbps" />
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import ChinaMap from './components/ChinaMap'
import CurveChart from './components/CurveChart.vue'
export default {
  name: 'App',
  components: {
    HelloWorld,
    ChinaMap,
    CurveChart,
  },
  mounted() {
    this.setScale()
    window.addEventListener('resize', () => {
      this.setScale()
    })
  },
  data() {
    return {
      selectData: {
        date: '',
        level: '中央'
      },
      options: [
        { value: '中央', label: '中央' },
        { value: '地方', label: '地方' },
      ],
      scaleRatio:1
    }
  },
  methods: {
    setScale() {
      let designWidth = 1726;
      let designHeight = 1041;
      let scale = document.documentElement.clientWidth / document.documentElement.clientHeight < designWidth / designHeight ?
        (document.documentElement.clientWidth / designWidth) :
        (document.documentElement.clientHeight / designHeight);
      document.querySelector('#container').style.transform = `scale(${scale}) translate(-50%)`
      this.scaleRatio = scale
    },
    changeCurve(){

    }
  },

}
</script>

<style>
body {
  overflow: hidden;
  background-color: #f0f2f5;
}

#app {
  margin-top: 0 !important;
  padding: 0;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  /* background-color: aqua; */
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

#container {
  padding: 0;
  display: flex;
  gap: 24px;
  margin: 0;
  width: 1726px;
  height: 1041px;
  transform-origin: 0 0;
  position: absolute;
  left: 50%;
  /* top:50%; */
  overflow: hidden;
}

.map__title {
  display: inline-flex;
  justify-content: space-between;
  align-items: center;
  width: 1160px;
  height: 85px;
  background: linear-gradient(135deg, #E8F0FF 0%, #C7D9F9 100%);
  border-radius: 2px;
  padding: 0 25px;
}

.map__title-item__title {
  width: 129px;
  height: 16px;
  font-size: 16px;
  font-family: SourceHanSansCN-Normal, SourceHanSansCN;
  font-weight: 400;
  color: #333333;
  line-height: 24px;
  text-align: left
}

.map__title-item__count {
  width: 48px;
  height: 37px;
  font-size: 26px;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #0F4AB8;
  line-height: 37px;
  text-align: left
}

.curve {
  display: flex;
  flex-flow: column;
  /* gap: 24px; */
  /* justify-content: space-between; */
  gap: 20px
}

.date-select {
  width: 236px;
  height: 32px;
  background: #FFFFFF;
  border-radius: 2px;
  /* border: 1px solid #D9D9D9; */
}

.level-select {
  width: 236px;
  height: 32px;
  background: #FFFFFF;
  border-radius: 2px;
  /* border: 1px solid #D9D9D9; */
}
.select-row{
  width: 492px;
  display: flex;
  justify-content: space-between;
}
</style>
