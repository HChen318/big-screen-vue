<template>
  <div>
    <div id="bottomLeftChartState" style="width: 5.35rem; height: 6.25rem;"></div>
  </div>
</template>

<script>
import echartMixins from "@/utils/resizeMixins";
export default {
  data() {
    return {
      chart: null,
    };
  },
  mixins: [echartMixins],
  mounted() {
    this.draw();
  },
  methods: {
    draw() {
      // 基于准备好的dom，初始化echarts实例
      this.chart = this.$echarts.init(
        document.getElementById("bottomLeftChartState")
      );

      let option = {
        animation: true,
          legend: {
              align: "left",
              right: '10%',
              top:'2%',
              type:'plain',
              textStyle:{
                  color:'#7ec7ff',
                  fontSize:16
              },
              itemGap:25,
              itemWidth:18,
          },
        grid: {
              top: '12%',
              left: '10%',
              right: '10%',
              bottom: '20%'
        },
        xAxis: {
          data: ['消防烟感','摄像机','环境烟感','路灯','地磁'],
          axisLine: {
            show: false //隐藏X轴轴线
          },
          axisTick: {
            show: false //隐藏X轴轴线
          },
          splitLine: {
            show: true,
            lineStyle: {
              color: "rgba(77, 128, 254, 0.2)",
              width: 2
            }
          },
          axisLabel: {
            show: true,
            margin: 14,
            fontSize: 16,
            textStyle: {
              color: "#65D5FF" //X轴文字颜色
            }
          }
        },
        yAxis: [
          {
            type: "value",
            gridIndex: 0,
            min: 0,
            max: 100,
            interval: 25,
            // splitNumber: 4,
            splitLine: {
              show: true,
              lineStyle: {
                color: "rgba(77, 128, 254, 0.2)",
                width: 2
              }
            },
            axisTick: {
              show: false
            },
            axisLine: {
              show: true,
              lineStyle: {
                color: "rgba(77, 128, 254, 0.2)"
              }
            },
            axisLabel: {
              show: true,
              margin: 14,
              fontSize: 16,
              textStyle: {
                color: "#65D5FF"
              }
            }
          }
        ],
        series: [
          {
            name: "设备在线率",
            type: "bar",
            barWidth: 16,
            itemStyle: {
              normal: {
                color: new this.$echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  {
                    offset: 0,
                    color: "rgba(146, 225, 255, 1)"
                  },
                  {
                    offset: 1,
                    color: "rgba(0, 151, 251, 1)"
                  }
                ])
              }
            },
            data: [21,43,56,32,64],
            z: 10,
            zlevel: 0
          },
          {
            // 分隔
            type: "pictorialBar",
            itemStyle: {
              normal: {
                color: "#0F375F"
              }
            },
            symbolRepeat: "fixed",
            symbolMargin: 6,
            symbol: "rect",
            symbolClip: true,
            symbolSize: [18, 2],
            symbolPosition: "start",
            symbolOffset: [1,1],
            data:  [21,43,56,32,64],
            width: 2,
            z: 0,
            zlevel: 1
          },
          {
            name: "外框",
            type: "bar",
            barGap: "-110%", // 设置外框粗细
            data: [100, 100, 100, 100, 100, 100, 100],
            barWidth: 16,
            itemStyle: {
              normal: {
                color: "transparent", // 填充色
                // barBorderRadius: 0, //圆角半径
                label: {
                  // 标签显示位置
                  show: false,
                  position: "top" // insideTop 或者横向的 insideLeft
                }
              }
            },
            z: 0
          },
          {
            name: "背影",
            type: "line",
            smooth: true, //平滑曲线显示
            showAllSymbol: false, //显示所有图形。
            symbolSize: 0,
            lineStyle: {
              width: 0
            },
            areaStyle: {
              color: "rgba(0, 151, 251, 0.1)"
            },
            data:  [21,43,56,32,64],
            z: 5
          }
        ],
        dataZoom: [
          {
            type: "slider",
            show: false,
            xAxisIndex: [0],
            endValue: 4,
            startValue: 0
          }
        ]
      }

      this.chart.setOption(option);
    },
    refresh(data) {
      let option = this.chart.getOption();
      var showList = new Array();
      var hiddenList = new Array();
      var wordList = new Array();
      // let coutSum = data[0];
      let temp = 0;
      for (let index = 0; index < data.length; index++) {
        const element = data[index];
        wordList.push(element.X0);
        showList.push(element.COUNT);
        if (index > 1) {
          temp += data[index - 1].COUNT;
        }
        hiddenList.push(temp);
      }

      option.xAxis[0].data = wordList;
      option.series[0].data = hiddenList;
      option.series[1].data = showList;

      // console.log(wordLisst)
      // console.log(hiddenList)
      // console.log(showList)
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