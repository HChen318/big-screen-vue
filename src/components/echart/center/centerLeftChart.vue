<template>
  <div>
    <div id="bottomLeftChart" style="width: 8rem; height: 3rem"></div>
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
    refresh(data) {
      const option = this.getChartOption(data);
      this.chart.setOption(option);
    },
    draw() {
      // 基于准备好的dom，初始化echarts实例
      this.chart = this.$echarts.init(
        document.getElementById("bottomLeftChart")
      );
      let option = this.getChartOption();
      this.chart.setOption(option);
    },
    getChartOption(data) {
      let xLabel = ["3.26", "3.27", "3.28", "3.29", "3.30", "3.31"];
      let data1 = ["40", "60", "22", "85", "50", "40"];
      let data2 = ["20", "50", "12", "65", "30", "60"];
      let data3 = ["10", "20", "12", "55", "40", "50"];
      let dataObj = {
        data1,
        data2,
        data3,
      }

      if (data) {
        dataObj = {};
        data.forEach((dev,i)=>{
          const obj = {};
          let curNumber = 1;
          dev.forEach(y=>{
            obj[y.time]=y.currents;
            curNumber = y.number;
          })
          dataObj[curNumber] = Object.values(obj);
          xLabel = Object.keys(obj);
        })
      }
      // console.log(xLabel, dataObj, 2222);
      return {
        tooltip: {
          trigger: "axis",
          backgroundColor: "transparent",
          axisPointer: {
            lineStyle: {
              color: {
                type: "linear",
                x: 0,
                y: 0,
                x2: 0,
                y2: 1,
                colorStops: [
                  {
                    offset: 0,
                    color: "rgba(126,199,255,0)", // 0% 处的颜色
                  },
                  {
                    offset: 0.5,
                    color: "rgba(126,199,255,1)", // 100% 处的颜色
                  },
                  {
                    offset: 1,
                    color: "rgba(126,199,255,0)", // 100% 处的颜色
                  },
                ],
                global: false, // 缺省为 false
              },
            },
          },
        },
        legend: {
          align: "left",
          right: "10%",
          top: "2%",
          type: "plain",
          textStyle: {
            color: "#7ec7ff",
            fontSize: 16,
          },
          itemGap: 25,
          itemWidth: 18,
          icon: "path://M0 2a2 2 0 0 1 2 -2h14a2 2 0 0 1 2 2v0a2 2 0 0 1 -2 2h-14a2 2 0 0 1 -2 -2z",

          data: [
            {
              name: "1#撕解机",
            },
            {
              name: "2#撕解机",
            },
            {
              name: "3#撕解机",
            },
          ],
        },
        grid: {
          top: "20%",
          left: "10%",
          right: "3%",
          bottom: "20%",
        },
        xAxis: [
          {
            type: "category",
            boundaryGap: false,
            axisLine: {
              //坐标轴轴线相关设置。数学上的x轴
              show: true,
              lineStyle: {
                color: "#233653",
              },
            },
            axisLabel: {
              //坐标轴刻度标签的相关设置
              textStyle: {
                color: "#7ec7ff",
                padding: 16,
                fontSize: 14,
              },
              formatter: function (data) {
                return data;
              },
            },
            splitLine: {
              show: true,
              lineStyle: {
                color: "#192a44",
              },
            },
            axisTick: {
              show: false,
            },
            data: xLabel,
          },
        ],
        yAxis: [
          {
            name: "电流",
            nameTextStyle: {
              color: "#7ec7ff",
              fontSize: 16,
              padding: 10,
            },
            min: 0,
            splitLine: {
              show: true,
              lineStyle: {
                color: "#192a44",
              },
            },
            axisLine: {
              show: true,
              lineStyle: {
                color: "#233653",
              },
            },
            axisLabel: {
              show: true,
              textStyle: {
                color: "#7ec7ff",
                padding: 16,
              },
              formatter: function (value) {
                if (value === 0) {
                  return value;
                }
                return value;
              },
            },
            axisTick: {
              show: false,
            },
          },
        ],
        series: [
          {
            name: "1#撕解机",
            type: "line",
            symbol: "circle", // 默认是空心圆（中间是白色的），改成实心圆
            showAllSymbol: true,
            symbolSize: 0,
            smooth: true,
            lineStyle: {
              normal: {
                width: 2,
                color: "rgba(227, 161, 96,1)", // 线条颜色
              },
              borderColor: "rgba(0,0,0,.4)",
            },
            itemStyle: {
              color: "rgba(227, 161, 96,1)",
              borderColor: "#646ace",
              borderWidth: 1,
            },
            tooltip: {
              show: true,
            },
            areaStyle: {
              //区域填充样式
              normal: {
                //线性渐变，前4个参数分别是x0,y0,x2,y2(范围0~1);相当于图形包围盒中的百分比。如果最后一个参数是‘true’，则该四个值是绝对像素位置。
                color: new this.$echarts.graphic.LinearGradient(
                  0,
                  0,
                  0,
                  1,
                  [
                    {
                      offset: 0,
                      color: "rgba(227, 161, 96,.3)",
                    },
                    {
                      offset: 1,
                      color: "rgba(227, 161, 96, 0)",
                    },
                  ],
                  false
                ),
                shadowColor: "rgba(227, 161, 96, 0.5)", //阴影颜色
                shadowBlur: 20, //shadowBlur设图形阴影的模糊大小。配合shadowColor,shadowOffsetX/Y, 设置图形的阴影效果。
              },
            },
            data: dataObj[1],
          },
          {
            name: "2#撕解机",
            type: "line",
            symbol: "circle", // 默认是空心圆（中间是白色的），改成实心圆
            showAllSymbol: true,
            symbolSize: 0,
            smooth: true,
            lineStyle: {
              normal: {
                width: 2,
                color: "rgba(10,219,250,1)", // 线条颜色
              },
              borderColor: "rgba(0,0,0,.4)",
            },
            itemStyle: {
              color: "rgba(10,219,250,1)",
              borderColor: "#646ace",
              borderWidth: 1,
            },
            tooltip: {
              show: true,
            },
            areaStyle: {
              //区域填充样式
              normal: {
                //线性渐变，前4个参数分别是x0,y0,x2,y2(范围0~1);相当于图形包围盒中的百分比。如果最后一个参数是‘true’，则该四个值是绝对像素位置。
                color: new this.$echarts.graphic.LinearGradient(
                  0,
                  0,
                  0,
                  1,
                  [
                    {
                      offset: 0,
                      color: "rgba(10,219,250,.3)",
                    },
                    {
                      offset: 1,
                      color: "rgba(10,219,250, 0)",
                    },
                  ],
                  false
                ),
                shadowColor: "rgba(10,219,250, 0.5)", //阴影颜色
                shadowBlur: 20, //shadowBlur设图形阴影的模糊大小。配合shadowColor,shadowOffsetX/Y, 设置图形的阴影效果。
              },
            },
            data: dataObj[2],
          },          
          {
            name: "3#撕解机",
            type: "line",
            symbol: "circle", // 默认是空心圆（中间是白色的），改成实心圆
            showAllSymbol: true,
            symbolSize: 0,
            smooth: true,
            lineStyle: {
              normal: {
                width: 2,
                color: "rgba(18, 210, 105,1)", // 线条颜色
              },
              borderColor: "rgba(0,0,0,.4)",
            },
            itemStyle: {
              color: "rgba(18, 210, 105,1)",
              borderColor: "#646ace",
              borderWidth: 1,
            },
            tooltip: {
              show: true,
            },
            areaStyle: {
              //区域填充样式
              normal: {
                //线性渐变，前4个参数分别是x0,y0,x2,y2(范围0~1);相当于图形包围盒中的百分比。如果最后一个参数是‘true’，则该四个值是绝对像素位置。
                color: new this.$echarts.graphic.LinearGradient(
                  0,
                  0,
                  0,
                  1,
                  [
                    {
                      offset: 0,
                      color: "rgba(18, 210, 105,.3)",
                    },
                    {
                      offset: 1,
                      color: "rgba(18, 210, 105, 0)",
                    },
                  ],
                  false
                ),
                shadowColor: "rgba(18, 210, 105, 0.5)", //阴影颜色
                shadowBlur: 20, //shadowBlur设图形阴影的模糊大小。配合shadowColor,shadowOffsetX/Y, 设置图形的阴影效果。
              },
            },
            data: dataObj[3],
          },
        ],
      };
    }
  },
  destroyed() {
    window.onresize = null;
  },
};
</script>

<style lang="scss" scoped>
</style>