<template>
  <div id="center">
    <div class="up">
      <centerChart ref="centerChart" />

      <div class="up-r">
        <dv-border-box-12 style="padding: 0.2rem; height: 3rem">
          <div class="item">
            <div class="label">预灰PH值:</div>
            <div class="val">{{ yuHuiObj.ph }}</div>
          </div>
          <div class="item">
            <div class="label">混合汁流量</div>
            <div class="val">{{ yuHuiObj.flow }}</div>
          </div>
          <div class="item">
            <div class="label">渗透水温度值:</div>
            <div class="val">{{ yuHuiObj.temperature }}</div>
          </div>
        </dv-border-box-12>
      </div>
    </div>
    <div class="down">
      <div id="elecChart" class="elec-main"></div>
    </div>
  </div>
</template>
 
<script>
import centerChart from "@/components/echart/center/centerLeftChart";
import { baseUrl } from "@/common/utils";
export default {
  components: {
    centerChart,
  },
  data() {
    return {
      chart: null,
      yuHuiObj: {},
    };
  },

  mounted() {
    this.initElecCharts();
    this.fetchChartData();
    this.fetchyuhui();
    this.setTimer();
  },
  methods: {
    setTimer() {
      this.timer = setInterval(() => {
        this.initElecCharts();
        this.fetchChartData();
        this.fetchyuhui();
      }, 1000);
    },
    async fetchChartData() {
      const { data } = await this.$http.get(
        `${baseUrl}/api/getDataByName/?e=1&n=GET_YZ_SJJ`
      );
      const status = data.code;
      const chartData = data.data;

      if (status === 0) {
        this.$refs.centerChart.refresh(chartData);
      }
    },
    initElecCharts() {
      this.chart = this.$echarts.init(document.getElementById("elecChart"));
      this.getChartOptionLine();
    },
    async getChartOptionLine() {
      let lastYearData = [];
      let thisYearData = [];
      const { data = {} } = await this.$http.get(
        `${baseUrl}/api/getDataByName/?e=1&n=GET_YZ_YZJ`
      );
      if (data.code === 0) {
        data.data.forEach((ele) => {
          if (ele[0]) {
            lastYearData.push(ele[0].currents);
            thisYearData.push(ele[0].speed);
          }
        });
      }

      let xData = ["1#压榨机", "2#压榨机", "3#压榨机", "4#压榨机", "5#压榨机"];

      let timeLineData = [1];
      let legend = ["电流", "转速"];
      let textColor = "#fff";
      let lineColor = "rgba(255,255,255,0.2)";
      let colors = [
        {
          borderColor: "rgba(227,161,96,1)",
          start: "rgba(227,161,96,0.8)",
          end: "rgba(227,161,96,0.3)",
        },
        {
          borderColor: "rgba(0,222,255,1)",
          start: "rgba(0,222,255,0.3)",
          end: "rgba(0,222,255,0.8)",
        },
      ];
      let borderData = [];
      let scale = 2;
      borderData = xData.map(() => {
        return scale;
      });
      let option = {
        baseOption: {
          timeline: {
            show: false,
            top: 0,
            data: [],
          },
          legend: {
            top: "0",
            // right: '5%',
            itemWidth: 8,
            itemHeight: 8,
            icon: "horizontal",
            textStyle: {
              color: "#ffffff",
              fontSize: 10,
            },
            data: legend,
          },
          grid: [
            {
              show: false,
              left: "5%",
              top: "10%",
              bottom: "8%",
              containLabel: true,
              width: "37%",
            },
            {
              show: false,
              left: "51%",
              top: "10%",
              bottom: "8%",
              width: "0%",
            },
            {
              show: false,
              right: "2%",
              top: "10%",
              bottom: "8%",
              containLabel: true,
              width: "37%",
            },
          ],
          xAxis: [
            {
              type: "value",
              inverse: true,
              axisLine: {
                show: false,
              },
              axisTick: {
                show: false,
              },
              position: "top",
              axisLabel: {
                show: true,
                color: textColor,
              },
              splitLine: {
                show: true,
                lineStyle: {
                  color: lineColor,
                },
              },
            },
            {
              gridIndex: 1,
              show: false,
            },
            {
              gridIndex: 2,
              axisLine: {
                show: false,
              },
              axisTick: {
                show: false,
              },
              position: "top",
              axisLabel: {
                show: true,
                color: textColor,
              },
              splitLine: {
                show: true,
                lineStyle: {
                  color: lineColor,
                },
              },
            },
          ],
          yAxis: [
            {
              type: "category",
              inverse: true,
              position: "right",
              axisLine: {
                show: true,
                lineStyle: {
                  color: lineColor,
                },
              },
              axisTick: {
                show: false,
              },
              axisLabel: {
                show: false,
              },
              data: xData,
            },
            {
              gridIndex: 1,
              type: "category",
              inverse: true,
              position: "left",
              axisLine: {
                show: false,
              },
              axisTick: {
                show: false,
              },
              axisLabel: {
                show: true,
                padding: [0, 0, 0, 0],
                textStyle: {
                  color: "#ffffff",
                  fontSize: 11,
                },
                align: "center",
              },
              data: xData.map(function (value) {
                return {
                  value: value,
                  textStyle: {
                    align: "center",
                  },
                };
              }),
            },
            {
              gridIndex: 2,
              type: "category",
              inverse: true,
              position: "left",
              axisLine: {
                show: true,
                lineStyle: {
                  color: lineColor,
                },
              },
              axisTick: {
                show: false,
              },
              axisLabel: {
                show: false,
              },
              data: xData,
            },
          ],
          series: [],
        },
        options: [],
      };

      option.baseOption.timeline.data.push(timeLineData[0]);
      option.options.push({
        series: [
          {
            name: "电流",
            type: "bar",
            barWidth: 10,
            stack: "1",
            itemStyle: {
              normal: {
                color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                  {
                    offset: 0,
                    color: colors[0].start,
                  },
                  {
                    offset: 1,
                    color: colors[0].end,
                  },
                ]),
              },
            },
            label: {
              normal: {
                show: true,
              },
            },
            data: lastYearData,
            animationEasing: "elasticOut",
          },
          {
            name: "电流",
            type: "bar",
            barWidth: 10,
            stack: "1",
            itemStyle: {
              normal: {
                color: colors[0].borderColor,
              },
            },
            data: borderData,
          },
          {
            name: "转速",
            type: "bar",
            stack: "2",
            barWidth: 10,
            xAxisIndex: 2,
            yAxisIndex: 2,
            itemStyle: {
              normal: {
                color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                  {
                    offset: 0,
                    color: colors[1].start,
                  },
                  {
                    offset: 1,
                    color: colors[1].end,
                  },
                ]),
              },
            },
            label: {
              normal: {
                show: true,
              },
            },
            data: thisYearData,
            animationEasing: "elasticOut",
          },
          {
            name: "转速",
            type: "bar",
            xAxisIndex: 2,
            yAxisIndex: 2,
            barWidth: 10,
            stack: "2",
            itemStyle: {
              normal: {
                color: colors[1].borderColor,
              },
            },
            data: borderData,
          },
        ],
      });

      this.chart.setOption(option);
    },
    async fetchyuhui() {
      const { data = {} } = await this.$http.get(
        `${baseUrl}/api/getDataByName/?e=1&n=GET_YZ_HHZ`
      );
      this.yuHuiObj = data.data;
    },
  },
};
</script>

<style lang="scss" scoped>
#center {
  display: flex;
  flex-direction: column;
  .up {
    width: 100%;
    display: flex;
    padding: 10px;
    // height: 3.25rem;
    flex: 1;
    // flex-wrap: wrap;
    // justify-content: space-around;
    // .item {
    //   border-radius: 0.0625rem;
    //   padding-top: 0.13rem;
    //   margin-top: 0.1rem;
    //   width: 32%;
    //   height: 0.675rem;
    //   font-size: 0.01rem;
    // }
    .up-r {
      flex: 1;
      .item {
        display: flex;
        font-size: 0.3rem;
        margin-top: 0.5rem;
        .label {
          margin-right: 0.1rem;
        }
      }
    }
  }
  .down {
    flex: 1;
    padding: 0.07rem 0.05rem;
    // padding-bottom: 0;
    width: 100%;
    // display: flex;
    // height: 3rem;
    // margin-top: 0.5rem;
    // justify-content: space-between;
    .bg-color-black {
      border-radius: 0.0625rem;
    }
    .ranking {
      padding: 0.125rem;
      width: 46%;
    }
    .percent {
      width: 100%;
      display: flex;
      justify-content: space-between;
      // flex-wrap: wrap;
      .item {
        width: 50%;
        height: 1.5rem;
        span {
          margin-top: 0.0875rem;
          display: flex;
          justify-content: center;
        }
      }
      .water {
        width: 100%;
      }
    }
    .elec-main {
      width: 100%;
      height: 130px;
    }
  }
}
</style>