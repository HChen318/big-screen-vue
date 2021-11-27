<template>
  <div class="bottom-left-left">
    <div id="bottomLeftChartRight" style="width: 100%; height: 2.5rem"></div>
    <ul class="overview-list" style="width: 100%;">
      <li>
        <span class="label">30T炉蒸汽流量: </span>
        <span class="value">{{ boilerData.steam.flow }}</span>
      </li>
      <li>
        <span class="label">30T炉蒸汽温度: </span>
        <span class="value">{{ boilerData.steam.temperature }}</span>
      </li>
      <li>
        <span class="label">30T炉蒸汽压力: </span>
        <span class="value">{{ boilerData.steam.pressure }}</span>
      </li>
      <li>
        <span class="label">30T炉给水流量: </span>
        <span class="value">{{ boilerData.water.flow }}</span>
      </li>
      <li>
        <span class="label">30T炉炉膛压力: </span>
        <span class="value">{{ boilerData.furnace.pressure }}</span>
      </li>
      <li>
        <span class="label">30T炉汽泡水位: </span>
        <span class="value">{{ boilerData.water.height }}</span>
      </li>
      <li>
        <span class="label">30T烟氧含量: </span>
        <span class="value">{{ boilerData.smoke.oxygen_content }}</span>
      </li>
      <li>
        <span class="label">30T炉炉膛温度: </span>
        <span class="value">{{ boilerData.furnace.temperature }}</span>
      </li>
      <li>
        <span class="label">30T排烟温度: </span>
        <span class="value">{{ boilerData.smoke.temperature }}</span>
      </li>
    </ul>
    <ul class="emission-list" style="">
        <li
          class="emission-item"
          v-for="item in titleItem"
          :key="item.title"
        >
            <span class="colorBlue fw-b">{{ item.title }}: </span>
            <span class="colorGrass fw-b">{{item.number}}</span>
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
      boilerData: {
        steam: {
          flow: 30.0,
          temperature: 100.0,
          pressure: 10.0,
          time: "2021-11-27 00:00:00",
        },
        water: {
          flow: 30.0,
          height: 8.0,
          time: "2021-11-27 00:00:00",
        },
        furnace: {
          pressure: -0.5,
          temperature: 10.0,
          time: "2021-11-27 00:00:00",
        },
        smoke: {
          oxygen_content: 4.0,
          temperature: 180.0,
          time: "2021-11-27 00:00:00",
        },
      },
      titleItem: [
        {
          key: 'oxygen_content',
          title: "二氧化碳排放",
          number: 120,
        },
        {
          key: 'oxygen_content',
          title: "二氧化硫排放",
          number: 18
        },
        {
          key: 'oxygen_content',
          title: "一氧化碳排放",
          number: 2
        },
        {
          key: 'oxygen_content',
          title: "硫化氢排放",
          number: 14,
        },
        {
          key: 'oxygen_content',
          title: "二硫化碳排放",
          number: 106
        },
        {
          key: 'oxygen_content',
          title: "污水排放",
          number: 100
        },
      ],
    };
  },
  mixins: [echartMixins],
  mounted() {
    this.draw();
  },
  methods: {
    refresh(data) {
      let option = this.chart.getOption();
      option.series[0].data.value = data?.steam?.temperature || 0;
      this.chart.setOption(option);
      this.drawEmissionView(data);
    },
    draw() {
      // 基于准备好的dom，初始化echarts实例
      this.chart = this.$echarts.init(
        document.getElementById("bottomLeftChartRight")
      );

      let option = {
        series: [
          {
            type: "gauge",
            min: 0,
            max: 100,
            splitNumber: 10,
            radius: "70%",
            axisLine: {
              lineStyle: {
                color: [[1, "#08cceb"]],
                width: 3,
              },
            },
            splitLine: {
              distance: -16,
              length: 16,
              lineStyle: {
                color: "#08cceb",
              },
            },
            axisTick: {
              distance: -12,
              length: 10,
              lineStyle: {
                color: "#08cceb",
              },
            },
            axisLabel: {
              distance: -40,
              color: "#08cceb",
              fontSize: 14,
            },
            anchor: {
              show: true,
              size: 16,
              itemStyle: {
                borderColor: "#00deff",
                borderWidth: 2,
              },
            },
            pointer: {
              offsetCenter: [0, "10%"],
              icon: "path://M2090.36389,615.30999 L2090.36389,615.30999 C2091.48372,615.30999 2092.40383,616.194028 2092.44859,617.312956 L2096.90698,728.755929 C2097.05155,732.369577 2094.2393,735.416212 2090.62566,735.56078 C2090.53845,735.564269 2090.45117,735.566014 2090.36389,735.566014 L2090.36389,735.566014 C2086.74736,735.566014 2083.81557,732.63423 2083.81557,729.017692 C2083.81557,728.930412 2083.81732,728.84314 2083.82081,728.755929 L2088.2792,617.312956 C2088.32396,616.194028 2089.24407,615.30999 2090.36389,615.30999 Z",
              length: "105%",
              itemStyle: {
                color: "#00deff",
              },
            },
            detail: {
              valueAnimation: true,
              precision: 1,
              fontSize: 20,
            },
            title: {
              color: "#fff",
              offsetCenter: [0, "70%"],
            },
            data: [
              {
                value: 58.46,
                name: "MPa",
              },
            ],
          },
        ],
      };
      this.chart.setOption(option);
    },
    async drawEmissionView(data) {
      this.titleItem.forEach(e => {
          e.number = data?.smoke?.[e.key] || 0;
      });
    },
  },
  destroyed() {
    window.onresize = null;
  },
};
</script>

<style lang="scss" scoped>
.bottom-left-left {
    // display: flex;
    // flex-direction: column;
    #bottomLeftChartRight {
        // flex: 3;
    }
    .overview-list {
    //   flex: 1;
      display: flex;
      padding: 0.125rem;
      flex-wrap: wrap;
      margin-bottom: 0.1rem;
      li {
        width: 33.33%;
        font-size: 0.155rem;
        color: #d3d6dd;
        margin-bottom: 0.1rem;
      }
    }
    .emission-list {
    //   flex: 1;
      display: flex;
      padding: 0.125rem;
      flex-wrap: wrap;
      li {
        width: calc(33.33% - 0.1rem);
        margin-right: 0.1rem;
        padding: 0.1rem;
        background-color: #1b2031;
        border-radius: 0.0625rem;
        text-align: center;
        margin-bottom: 0.1rem;
      }
    }
}
</style>