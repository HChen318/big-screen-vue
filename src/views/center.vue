<template>
  <div id="center">
    <div class="up">
      <!-- <div
        class="bg-color-black item"
        v-for="item in titleItem"
        :key="item.title"
      >
        <p class="colorBlue fw-b" style="text-align: center">
          {{ item.title }}
        </p>
        <div>
          <dv-digital-flop
            :config="item.number"
            style="width: 2.5rem; height: 0.375rem; text-align: center"
          />
        </div>
      </div> -->
      <bottomLeftChart ref="bottomLeftChart" />
      <div class="up-r">
        <dv-border-box-12 style="padding: 0.2rem; height: 3rem">
          <div class="item">
            <div class="label">预灰PH值:</div>
            <div class="val">100</div>
          </div>
          <div class="item">
            <div class="label">混合汁流量</div>
            <div class="val"></div>
          </div>
          <div class="item">
            <div class="label">渗透水温度值:</div>
            <div class="val"></div>
          </div>
        </dv-border-box-12>
      </div>
    </div>
    <div class="down">
      <!-- <div class="percent"> -->
      <!-- <div class="item bg-color-black">
          <span>{{ rate[0].id }}</span>
          <centerChart
            :id="rate[0].id"
            :tips="rate[0].tips"
            :colorObj="rate[0].colorData"
            ref="centerChart1"
          />
        </div>
        <div class="item bg-color-black">
          <span>{{ rate[1].id }}</span>
          <centerChart
            :id="rate[1].id"
            :tips="rate[1].tips"
            :colorObj="rate[1].colorData"
            ref="centerChart2"
          />
        </div>
        <div class="item bg-color-black">
          <span>{{ rate[2].id }}</span>
          <centerChart
            :id="rate[2].id"
            :tips="rate[2].tips"
            :colorObj="rate[2].colorData"
            ref="centerChart3"
          />
        </div> -->
      <!-- <div class="water">
          <dv-water-level-pond :config="water" style="height: 1.5rem" />
        </div>
        -->

      <!-- <bottomLeftChartState style="" ref="bottomLeftChartState" /> -->
      <!-- </div> -->
      <div id="elecChart" class="elec-main"></div>
    </div>
  </div>
</template>
 
<script>
import centerChart from "@/components/echart/center/centerChartRate";
import bottomLeftChart from "@/components/echart/bottom/bottomLeftChart";

export default {
  components: {
    // centerChart
    // centerChart1,
    // centerChart2

    bottomLeftChart,
  },
  data() {
    return {
      titleItem: [
        {
          title: "二氧化碳排放",
          number: {
            number: [120],
            toFixed: 1,
            content: "{nt}",
            style: {
              fontSize: 15,
              fill: "#3de7c9",
            },
          },
        },
        {
          title: "二氧化硫排放",
          number: {
            number: [18],
            toFixed: 1,
            content: "{nt}",
            style: {
              fontSize: 15,
              fill: "#3de7c9",
            },
          },
        },
        {
          title: "一氧化碳排放",
          number: {
            number: [2],
            toFixed: 1,
            content: "{nt}",
            style: {
              fontSize: 15,
              fill: "#3de7c9",
            },
          },
        },
        {
          title: "硫化氢排放",
          number: {
            number: [14],
            toFixed: 1,
            content: "{nt}",
            style: {
              fontSize: 15,
              fill: "#3de7c9",
            },
          },
        },
        {
          title: "二硫化碳排放",
          number: {
            number: [106],
            toFixed: 1,
            content: "{nt}",
            style: {
              fontSize: 15,
              fill: "#3de7c9",
            },
          },
        },
        {
          title: "污水排放",
          number: {
            number: [100],
            toFixed: 1,
            content: "{nt}",

            style: {
              fontSize: 15,
              fill: "#3de7c9",
            },
          },
        },
      ],
      ranking: {
        data: [
          {
            name: "新乡",
            value: 80,
          },
        ],
        waitTime: 4000,
        carousel: "single",
        unit: "件",
      },
      config: {
        header: ["主办单位", "数量"],
        data: [
          ['<span style="color:#9fe6b8;">海淀区政府</span>', "23"],
          ['<span style="color:#9fe6b8;">市发展改革委</span>', "26"],
          ['<span style="color:#9fe6b8;">市民政局</span>', "35"],
          ['<span style="color:#9fe6b8;">市城市管理委</span>', "39"],
          ['<span style="color:#9fe6b8;">市规划自然资源委</span>', "39"],
          ['<span style="color:#9fe6b8;">市住房城乡建设委</span>', "45"],
          ['<span style="color:#9fe6b8;">市公安局</span>', "53"],
          ['<span style="color:#9fe6b8;">市教委</span>', "55"],
          ['<span style="color:#9fe6b8;">市卫生健康委</span>', "60"],
          ['<span style="color:#9fe6b8;">市交通委</span>', "153"],
        ],
        rowNum: 5, //表格行数
        headerHeight: 35,
        headerBGC: "#0f1325", //表头
        oddRowBGC: "#0f1325", //奇数行
        evenRowBGC: "#171c33", //偶数行
        index: true,
        columnWidth: [30, 150, 50],
        align: ["center"],
        waitTime: 3000,
        carousel: "page",
      },
      water: {
        data: [5, 45],
        shape: "roundRect",
        formatter: "{value}%",
        waveNum: 3,
      },
      // 通过率和达标率的组件复用数据
      rate: [
        {
          id: "渗透水流量",
          tips: 22,
          colorData: {
            textStyle: "#3fc0fb",
            series: {
              color: ["#00bcd44a", "transparent"],
              dataColor: {
                normal: "#03a9f4",
                shadowColor: "#97e2f5",
              },
            },
          },
        },
        {
          id: "渗透水温度",
          tips: 67,
          colorData: {
            textStyle: "#3fc0fb",
            series: {
              color: ["#00bcd44a", "transparent"],
              dataColor: {
                normal: "#03a9f4",
                shadowColor: "#97e2f5",
              },
            },
          },
        },
        {
          id: "混合汁流量",
          tips: 78,
          colorData: {
            textStyle: "#67e0e3",
            series: {
              color: ["#faf3a378", "transparent"],
              dataColor: {
                normal: "#ff9800",
                shadowColor: "#fcebad",
              },
            },
          },
        },
      ],
    };
  },

  mounted() {
    // this.fetchProposalSubmit(); //获取-数字板数据
    // this.fetchProposalReplace(); //获取-代提百分比
    // this.fetchUnitSum(); //获取-承办单位-排行榜
    // this.setTimer();
    this.initElecCharts();
  },
  methods: {
    setTimer() {
      this.timer = setInterval(() => {
        this.fetchProposalSubmit(); //获取-数字板数据
        this.fetchProposalReplace(); //获取-代提百分比
        this.fetchUnitSum(); //获取-承办单位-排行榜
        this.fetchState(); //获取-状态
      }, 3000);
    },
    async fetchState() {
      const { data } = await this.$http.get("getDataByName?name=STATE_SUM");

      let status = data.status;
      let dataList = JSON.parse(data.data);

      if (status === 200) {
        this.$refs.bottomLeftChartState.refresh(dataList);
      }
    },
    async fetchProposalSubmit() {
      const { data } = await this.$http.get("getDataByName?name=PROPOSAL_SUM");

      let status = data.status;
      let titleData = JSON.parse(data.data);

      var titleRes = new Array();
      if (status === 200) {
        for (var i = titleData.length - 1; i >= 0; i--) {
          var item = {
            title: "",
            number: {
              number: [],
              toFixed: 0,
              content: "{nt}",
              style: {
                fontSize: 15,
                fill: "#3de7c9",
              },
            },
          };
          item.title = titleData[i].X0;

          item.number.number.push(titleData[i].COUNT);
          titleRes.push(item);
        }
        this.titleItem = titleRes;
      }
    },
    async fetchProposalReplace() {
      const { data } = await this.$http.get("getDataByName?name=SUBMIT_SUM");

      let status = data.status;
      let dataList = JSON.parse(data.data);
      if (status === 200) {
        this.rate[0].id = dataList[0].X0;
        this.rate[1].id = dataList[1].X0;
        let submitSumMe = dataList[0].COUNT + 0;
        let submitSumNotMe = dataList[1].COUNT + 0;
        let count = submitSumMe + submitSumNotMe;
        submitSumMe = parseFloat((submitSumMe / count) * 100).toFixed(2);
        submitSumNotMe = parseFloat((submitSumNotMe / count) * 100).toFixed(2);

        this.$refs.centerChart1.refresh(submitSumMe);
        this.$refs.centerChart2.refresh(submitSumNotMe);
      }
    },
    async fetchUnitSum() {
      const { data } = await this.$http.get(
        "getDataByName?name=UNIT_HANDLE_SUM"
      );

      let status = data.status;
      let dataList = JSON.parse(data.data);

      let resultList = new Array();

      if (status === 200) {
        for (var i = dataList.length - 1; i >= 0; i--) {
          var item = ["组件1", "dev-1"];
          let colorCode = "";
          if (i % 2 == 0) {
            colorCode = '<span style="color:#9fe6b8;">';
          } else {
            colorCode = '<span style="color:#67e0e3;">';
          }
          item[0] = colorCode + dataList[i].X0 + "</span>";
          item[1] = colorCode + dataList[i].COUNT + "</span>";
          resultList.push(item);
        }
        this.config.data = resultList;
        this.$refs["unit"].updateRows(resultList);
      }
    },
    initElecCharts() {
      let xData = ["1#压榨机", "2#压榨机", "3#压榨机", "4#压榨机", "5#压榨机"];
      let lastYearData = [3, 20, 62, 34, 55, 65, 33];
      let thisYearData = [11, 38, 23, 39, 66, 66, 79];
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
                show: false,
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
                show: false,
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
      this.chart = this.$echarts.init(document.getElementById("elecChart"));
      this.chart.setOption(option);
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