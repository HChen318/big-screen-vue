<template>
  <div id="centreLeft1">
    <div class="bg-color-black">
      <div class="d-flex pt-2 pl-2">
        <span style="color: #5cd9e8">
          <icon name="chart-bar"></icon>
        </span>
        <div class="d-flex">
          <span class="fs-xl text mx-2">{{title}}数据</span>
          <dv-decoration-3
            style="
              width: 1.25rem;
              height: 0.25rem;
              position: relative;
              top: -0.0375rem;
            "
          />
        </div>
      </div>
      <div class="d-flex jc-center">
        <div
          :id="id"
          style="height: 2.75rem; width: 2.75rem"
        >
        </div>
      </div>
      <!-- 4个主要的数据 -->
      <div class="bottom-data">
        <div class="item-box" v-for="(item, index) in data" :key="index">
          <p class="d-flex">
            {{ item.text }}: {{item.value}}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
      id: {
          type: String,
          default: 'ring'
      },
      title: {
          type: String,
          default: '锅炉'
      },
      data: {
          type: Array,
          default: () => {}
      }
  },
  data() {
    return {
    };
  },
  mounted() {
    let dataArr = [{
        value: 76,
        name: '压力检测'
    }];
    let color = new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [{
            offset: 0,
            color: '#5CF9FE' // 0% 处的颜色
        },
        {
            offset: 0.17,
            color: '#468EFD' // 100% 处的颜色
        },
        {
            offset: 0.9,
            color: '#468EFD' // 100% 处的颜色
        },
        {
            offset: 1,
            color: '#5CF9FE' // 100% 处的颜色
        }
    ]);
    let colorSet = [
        [1, color]
    ];
    let rich = {
        white: {
            fontSize: 22,
            color: '#fff',
            fontWeight: '500',
            padding: [0, 0, 0, 0]
        },
        bule: {
            fontSize: 22,
            fontFamily: 'DINBold',
            color: '#fff',
            fontWeight: '700',
            padding: [-60, 0, 0, 0],
        },
        radius: {
            width: 350,
            height: 80,
            borderWidth: 1,
            borderColor: '#0092F2',
            fontSize: 22,
            color: '#fff',
            backgroundColor: '#1B215B',
            borderRadius: 20,
            textAlign: 'center',
        },
        size: {
            height: 400,
            padding: [100, 0, 0, 0]
        }
    }
    let option = {
        backgroundColor: '#0E1327',
        tooltip: {
            formatter: "{a} <br/>{b} : {c}Pa"
        },

        series: [
            {
                type: 'gauge',
                name: '外层辅助',
                radius: '94%',
                startAngle: '225',
                endAngle: '-45',
                splitNumber: '120',
                pointer: {
                    show: false
                },
                detail: {
                    show: false,
                },
                data: [{
                    value: 1
                }],
                title: {
                    show: true,
                    offsetCenter: [0, 30],
                    textStyle: {
                        color: '#fff',
                        fontStyle: 'normal',
                        fontWeight: 'normal',
                        fontFamily: '微软雅黑',
                        fontSize: 20,
                    }
                },
                axisLine: {
                    show: true,
                    lineStyle: {
                        color: [
                            [1, '#00FFFF']
                        ],
                        width: 2,
                        opacity: 1
                    }
                },
                axisTick: {
                    show: false
                },
                splitLine: {
                    show: true,
                    length: 20,
                    lineStyle: {
                        color: '#051932',
                        width: 0,
                        type: 'solid',
                    },
                },
                axisLabel: {
                    show: false
                }
            },
            {
                type: 'gauge',
                radius: '87%',
                startAngle: '225',
                endAngle: '-45',
                pointer: {
                    show: true
                },
                detail: {
                    formatter: function() {
                        return '{bule|Pa}{white|}' + '{size|' + '}';
                    },
                    rich: rich,
                    "offsetCenter": ['0%', "55%"],
                },
                data: dataArr,
                title: {
                    show: false,
                },
                axisLine: {
                    show: true,
                    lineStyle: {
                        color: colorSet,
                        width: 7,
                        shadowOffsetX: 0,
                        shadowOffsetY: 0,
                        opacity: 1
                    }
                },
                axisTick: {
                    show: false
                },
                splitLine: {
                    show: false,
                    length: 25,
                    lineStyle: {
                        color: '#00377a',
                        width: 2,
                        type: 'solid',
                    },
                },
                axisLabel: {
                    show: false
                },
                animationDuration: 4000,
            },
            {
                name: '灰色内圈', //刻度背景
                type: 'gauge',
                z: 2,
                radius: '77%',
                startAngle: '225',
                endAngle: '-45',
                //center: ["50%", "75%"], //整体的位置设置
                axisLine: { // 坐标轴线
                    lineStyle: { // 属性lineStyle控制线条样式
                        color: [
                            [1, '#018DFF']
                        ],
                        fontSize: 14,
                        width: 2,
                        opacity: 1, //刻度背景宽度
                    }
                },
                splitLine: {
                    show: false
                },
                axisLabel: {
                    show: false
                },
                pointer: {
                    show: false
                },
                axisTick: {
                    show: false
                },
                detail: {
                    show: 0
                }
            },
            {
                name: "白色圈刻度",
                type: "gauge",
                radius: "73%",
                startAngle: 225, //刻度起始
                endAngle: -45, //刻度结束
                min: 0,
                max: 120,
                splitNumber: 6,
                z: 4,
                axisTick: {
                    show: false
                },
                splitLine: {
                    length: 16, //刻度节点线长度
                    lineStyle: {
                        width: 2,
                        color: '#018DFF'
                    } //刻度节点线
                },
                axisLabel: {
                    color: 'rgba(255,255,255,8)',
                    fontSize: 14,
                }, //刻度节点文字颜色
                pointer: {
                    show: false
                },
                axisLine: {
                    lineStyle: {
                        opacity: 0
                    }
                },
                detail: {
                    show: false
                },
                data: [{
                    value: 0,
                    name: ""
                }]
            },
            { //内圆
                type: 'pie',
                radius: '60%',
                center: ['50%', '50%'],
                z: 1,
                itemStyle: {
                    normal: {
                        color: new this.$echarts.graphic.RadialGradient(.5, .5, .8, [{
                                offset: 0,
                                color: '#4978EC'
                            },
                            {
                                offset: .5,
                                color: '#1E2B57'
                            },
                            {
                                offset: 1,
                                color: '#141F3D'
                            }
                        ], false),
                        label: {
                            show: false
                        },
                        labelLine: {
                            show: false
                        }
                    },
                },
                hoverAnimation: false,
                label: {
                    show: false,
                },
                tooltip: {
                    show: false
                },
                data: [100],
                animationType: "scale"
            },
            { //外圆
                type: 'pie',
                radius: '98%',
                center: ['50%', '50%'],
                z: 0,
                itemStyle: {
                    color: '#141C33',
                },
                hoverAnimation: false,
                label: {
                    show: false,
                },
                tooltip: {
                    show: false
                },
                data: [120],
                animationType: "scale"
            }
        ]
    };
    // ring
    this.chart = this.$echarts.init(document.getElementById(this.id));
    this.chart.setOption(option)
  },
  methods: {},
};
</script>

<style lang="scss">
#centreLeft1 {
  padding: 0.2rem;
  height: 5.125rem;
  min-width: 3.75rem;
  border-radius: 0.0625rem;
  .bg-color-black {
    height: 4.8125rem;
    border-radius: 0.125rem;
  }
  .text {
    color: #c3cbde;
  }
  .chart-box {
    margin-top: 0.2rem;
    width: 2.125rem;
    height: 2.125rem;
    .active-ring-name {
      padding-top: 0.125rem;
    }
  }

  .bottom-data {
    .item-box {
      float: right;
      position: relative;
      top: 0;
      width: 45%;
      color: #d3d6dd;
      text-align: center;
      .d-flex {
        font-size: 18px;
        margin-top: 20px;
        text-align: center;
      }
    }
    .text {
        // text-align: center;
    }
  }
}
</style>