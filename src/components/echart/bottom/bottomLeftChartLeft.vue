<template>
  <div>
    <div id="bottomLeftChartLeft" style="width: 6.15rem; height: 3.25rem"></div>
    <ul style="width: 6.15rem; height: 3rem">
        <li>
            <span class="label">30T炉蒸汽流量</span>
            <span class="value">{{boiler.steamFlow}}</span>
        </li>
        <li>
            <span class="label">30T炉蒸汽温度</span>
            <span class="value">{{}}</span>
        </li>
        <li>
            <span class="label">30T炉蒸汽压力</span>
            <span class="value"></span>
        </li>
        <li>
            <span class="label">30T炉给水流量</span>
            <span class="value"></span>
        </li>
        <li>
            <span class="label">30T炉炉膛压力</span>
            <span class="value"></span>
        </li>
        <li>
            <span class="label">30T炉汽泡水位</span>
            <span class="value"></span>
        </li>
        <li>
            <span class="label">30T烟氧含量</span>
            <span class="value"></span>
        </li>
        <li>
            <span class="label">30T炉炉膛温度</span>
            <span class="value"></span>
        </li>
        <li>
            <span class="label">30T排烟温度</span>
            <span class="value"></span>
        </li>
    </ul>
  </div>
</template>

<script>
import echartMixins from "@/utils/resizeMixins";
export default {
  data() {
    return {
      chart: null,
      boilerData: {}
    };
  },
  mixins: [echartMixins],
  mounted() {
    this.draw();
  },
  methods: {
    refresh(data) {
      let option = this.chart.getOption();
      option.series[0].data.value = data;
      this.chart.setOption(option);
    },
    draw() {
      // 基于准备好的dom，初始化echarts实例
      this.chart = this.$echarts.init(
        document.getElementById("bottomLeftChartLeft")
      );

      let option = {
        series: [
          {
            type: 'gauge',
            min: 0,
            max: 100,
            splitNumber: 10,
            radius: '80%',
            axisLine: {
              lineStyle: {
                color: [[1, '#f00']],
                width: 3
              }
            },
            splitLine: {
              distance: -18,
              length: 18,
              lineStyle: {
                color: '#f00'
              }
            },
            axisTick: {
              distance: -12,
              length: 10,
              lineStyle: {
                color: '#f00'
              }
            },
            axisLabel: {
              distance: -50,
              color: '#f00',
              fontSize: 25
            },
            anchor: {
              show: true,
              size: 20,
              itemStyle: {
                borderColor: '#000',
                borderWidth: 2
              }
            },
            pointer: {
              offsetCenter: [0, '10%'],
              icon: 'path://M2090.36389,615.30999 L2090.36389,615.30999 C2091.48372,615.30999 2092.40383,616.194028 2092.44859,617.312956 L2096.90698,728.755929 C2097.05155,732.369577 2094.2393,735.416212 2090.62566,735.56078 C2090.53845,735.564269 2090.45117,735.566014 2090.36389,735.566014 L2090.36389,735.566014 C2086.74736,735.566014 2083.81557,732.63423 2083.81557,729.017692 C2083.81557,728.930412 2083.81732,728.84314 2083.82081,728.755929 L2088.2792,617.312956 C2088.32396,616.194028 2089.24407,615.30999 2090.36389,615.30999 Z',
              length: '115%',
              itemStyle: {
                color: '#000'
              }
            },
            detail: {
              valueAnimation: true,
              precision: 1
            },
            title: {
              offsetCenter: [0, '-50%']
            },
            data: [
              {
                value: 58.46,
                name: 'PLP'
              }
            ]
          },
          {
            type: 'gauge',
            min: 0,
            max: 60,
            splitNumber: 6,
            axisLine: {
              lineStyle: {
                color: [[1, '#000']],
                width: 3
              }
            },
            splitLine: {
              distance: -3,
              length: 18,
              lineStyle: {
                color: '#000'
              }
            },
            axisTick: {
              distance: 0,
              length: 10,
              lineStyle: {
                color: '#000'
              }
            },
            axisLabel: {
              distance: 10,
              fontSize: 25,
              color: '#000'
            },
            pointer: {
              show: false
            },
            title: {
              show: false
            },
            anchor: {
              show: true,
              size: 14,
              itemStyle: {
                color: '#000'
              }
            }
          }
        ]
    };      
      this.chart.setOption(option);
    },
  },
  destroyed() {
    window.onresize = null;
  },
};
</script>

<style lang="scss" scoped>
</style>