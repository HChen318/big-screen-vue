<template>
  <div id="centreRight1">
    <div class="bg-color-black">
      <div class="d-flex pt-2 pl-2">
        <span style="color: #5cd9e8">
          <icon name="chart-line"></icon>
        </span>
        <div class="d-flex">
          <span class="fs-xl text mx-2">蒸发罐气鼓统计</span>
        </div>
      </div>
      <div class="d-flex content">
        <div class="left">
          <div class="top">
            <dv-border-box-13>
              <div class="warrper">
                <div class="item">
                  <div class="label">混合汁箱液位:</div>
                  <div class="val">{{ overviewData.hhz }}</div>
                </div>
                <div class="item">
                  <div class="label">中和汁箱液位:</div>
                  <div class="val">{{ overviewData.zhz }}</div>
                </div>
                <div class="item">
                  <div class="label">沉淀池液位:</div>
                  <div class="val">{{ overviewData.cdc }}</div>
                </div>
                <div class="item">
                  <div class="label">桔水罐液位:</div>
                  <div class="val">{{ overviewData.jsg }}</div>
                </div>
                <div class="item">
                  <div class="label">清汁箱液位:</div>
                  <div class="val">{{ overviewData.qzx }}</div>
                </div>
                <div class="item">
                  <div class="label">冷水箱液位:</div>
                  <div class="val">{{ overviewData.lsx }}</div>
                </div>
                <div class="item">
                  <div class="label">热水箱液位:</div>
                  <div class="val">{{ overviewData.rsx }}</div>
                </div>
              </div>
            </dv-border-box-13>
          </div>

          <div class="bottom">
            <dv-border-box-13>
              <div class="warrper-b">
                <div class="item">
                  <div class="label">一次加热温度:</div>
                  <div class="val">{{ jiarewendu.heating1 }}</div>
                </div>
                <div class="item">
                  <div class="label">二次加热温度:</div>
                  <div class="val">{{ jiarewendu.heating2 }}</div>
                </div>
              </div>
            </dv-border-box-13>
          </div>
        </div>
        <div class="right">
          <div class="r-top">
            <dv-border-box-13 class="r-border-top">
              <div class="fs-xl text mx-2">蒸发罐汽鼓</div>
              <!-- <dv-scroll-board :config="config" ref="log" /> -->
              <div class="header">
                <span class="item" v-for="item in config.header" :key="item">{{
                  item
                }}</span>
              </div>
              <div class="zq-content">
                <div v-for="(item, key) in config.data" :key="key">
                  <span v-for="subItem in item" :key="subItem">{{
                    subItem
                  }}</span>
                </div>
              </div>
            </dv-border-box-13>
          </div>
          <div class="r-bottom">
            <dv-border-box-13>
              <div class="warrper-b">
                <div class="item" v-for="(item, i) in jiejingArr" :key="i">
                  <span
                    >{{ item.number }}#甲膏结晶罐真空度:
                    {{ item.vacuums }}</span
                  >
                  <span
                    >{{ item.number }}#甲膏结晶罐气鼓温度:
                    {{ item.temperature }}</span
                  >
                </div>
              </div>
            </dv-border-box-13>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { baseUrl } from "@/common/utils";
export default {
  data() {
    return {
      config: {
        header: ["设备", "压力", "温度", "真空度"],
        data: [
          ["1#", "12", "35", "78"],
          ["2#", "12", "35", "78"],
          ["3#", "12", "35", "78"],
          ["4#", "12", "35", "78"],
          ["5#", "12", "35", "78"],
          ["6#", "12", "35", "78"],
        ],
        rowNum: 7, //表格行数
        headerHeight: 30,
        headerBGC: "#0f1325", //表头
        oddRowBGC: "#0f1325", //奇数行
        evenRowBGC: "#171c33", //偶数行
        index: false,
        columnWidth: [100],
        align: ["left"],
      },
      overviewData: {
        hhz: "",
        zhz: "",
        cdc: "",
        jsg: "",
        qzx: "",
        lsx: "",
        rsx: "",
      },
      jiarewendu: {
        heating1: "",
        heating2: "",
      },
      jiejingArr: [],
    };
  },
  components: {},
  mounted() {
    this.fetchProposalSubmit();
    this.fetchZhengfa();
    this.fetchJiaRe();
    this.changeTiming();
    this.fetchJiejing();
  },
  methods: {
    changeTiming() {
      setInterval(() => {
        this.fetchProposalSubmit();
        this.fetchZhengfa();
        this.fetchJiaRe();
        this.fetchJiejing();
        this.fetchJiejing();
      }, 1000);
    },
    async fetchProposalSubmit() {
      const { data = {} } = await this.$http.get(
        `${baseUrl}/api/getDataByName/?e=1&n=GET_ZL_ZFG`
      );
      let dataList = data.data;
      var dataArr = [];
      dataList.forEach((ele) => {
        if (ele[0]) {
          dataArr.push([
            `#${ele[0].number}`,
            ele[0].pressure,
            ele[0].temperature,
            ele[0].vacuums,
          ]);
        }
      });
      // this.$refs["log"].updateRows(dataArr);
      this.config.data = dataArr;
    },
    async fetchZhengfa() {
      const { data = {} } = await this.$http.get(
        `${baseUrl}/api/getDataByName/?e=1&n=GET_ZL_ZFYW`
      );
      for (const key in data.data) {
        this.overviewData[key] = data.data[key].water_level;
      }
    },
    async fetchJiaRe() {
      const { data = {} } = await this.$http.get(
        `${baseUrl}/api/getDataByName/?e=1&n=GET_ZL_JRWD`
      );
      for (const key in data.data) {
        this.jiarewendu[key] = data.data[key].temperature;
      }
    },
    async fetchJiejing() {
      const { data = {} } = await this.$http.get(
        `${baseUrl}/api/getDataByName/?e=1&n=GET_ZL_JJG`
      );
      console.log(data, "===data");
      const dataArr = [];
      if (data.code === 0) {
        data.data.forEach((ele) => {
          if (ele[0]) {
            dataArr.push({
              number: ele[0].number,
              temperature: ele[0].temperature,
              vacuums: ele[0].temperature,
            });
          }
        });
        this.jiejingArr = dataArr;
      }
    },
  },
};
</script>

<style lang="scss">
#centreRight1 {
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
  .content {
    display: flex;
    .left {
      flex: 1;
      margin-right: 0.1rem;
      .warrper {
        padding: 0.3rem 0.2rem;
      }
      .item {
        display: flex;
        margin-top: 0.2rem;
        .label {
          margin-right: 0.1rem;
        }
      }
      .warrper-b {
        padding: 0.1rem 0.2rem 0.25rem;
      }
    }
    .right {
      flex: 3;
      display: flex;
      flex-direction: column;
      .r-top {
        height: 3rem;
      }
      .r-border-top {
        box-sizing: border-box;
        padding: 0.3rem;
        display: flex;

        .header {
          display: flex;
          justify-content: center;
          margin-top: 0.1rem;
          span {
            flex: 1;
            font-size: 0.22rem;
            display: flex;
            justify-content: center;
          }
        }
        .zq-content {
          margin-top: 0.2rem;
          > div {
            display: flex;
            margin-top: 0.05rem;
            span {
              flex: 1;
              display: flex;
              justify-content: center;
              font-size: 0.16rem;
            }
          }
        }
      }
      .warrper {
        padding: 0.3rem 0.2rem;
      }
      .r-bottom {
        flex: 1;
        .warrper-b {
          padding: 0.3rem 0.2rem;
        }
        .item {
          font-size: 0.24rem;
          margin-top: 0.08rem;
          span {
            margin-left: 0.5rem;
          }
        }
      }
    }
  }
}
</style>