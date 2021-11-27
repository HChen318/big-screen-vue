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
                  <div class="val">100</div>
                </div>
                <div class="item">
                  <div class="label">中和汁箱液位:</div>
                  <div class="val">100</div>
                </div>
                <div class="item">
                  <div class="label">沉淀池液位:</div>
                  <div class="val">100</div>
                </div>
                <div class="item">
                  <div class="label">桔水罐液位:</div>
                  <div class="val">100</div>
                </div>
                <div class="item">
                  <div class="label">清汁箱液位:</div>
                  <div class="val">100</div>
                </div>
                <div class="item">
                  <div class="label">冷水箱液位:</div>
                  <div class="val">100</div>
                </div>
                <div class="item">
                  <div class="label">热水箱液位:</div>
                  <div class="val">100</div>
                </div>
              </div>
            </dv-border-box-13>
          </div>

          <div class="bottom">
            <dv-border-box-13>
              <div class="warrper-b">
                <div class="item">
                  <div class="label">一次加热温度:</div>
                  <div class="val">100</div>
                </div>
                <div class="item">
                  <div class="label">二次加热温度:</div>
                  <div class="val">100</div>
                </div>
              </div>
            </dv-border-box-13>
          </div>
        </div>
        <div class="right">
          <div class="r-top">
            <dv-border-box-13 class="r-border-top">
              <dv-scroll-board :config="config" ref="log" />
            </dv-border-box-13>
          </div>
          <div class="r-bottom">
            <dv-border-box-13>
              <div class="warrper">
                <div class="item">
                  <span>7#甲膏结晶罐真空度</span>
                  <span>7#甲膏结晶罐气鼓温度</span>
                </div>
                <div class="item">
                  <span>8#甲膏结晶罐真空度</span>
                  <span>8#甲膏结晶罐气鼓温度</span>
                </div>
                <div class="item">
                  <span>9#甲膏结晶罐真空度</span>
                  <span>9#甲膏结晶罐气鼓温度</span>
                </div>
                <div class="item">
                  <span>10#甲膏结晶罐真空度</span>
                  <span>10#甲膏结晶罐气鼓温度</span>
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
        rowNum: 8, //表格行数
        headerHeight: 35,
        headerBGC: "#0f1325", //表头
        oddRowBGC: "#0f1325", //奇数行
        evenRowBGC: "#171c33", //偶数行
        index: false,
        columnWidth: [50],
        align: ["left"],
      },
    };
  },
  components: {},
  mounted() {
    this.changeTiming();
  },
  methods: {
    changeTiming() {
      setInterval(() => {
        this.fetchProposalSubmit(); //获取-建议情况
      }, 3000);
    },
    async fetchProposalSubmit() {
      const { data } = await this.$http.get("getDataByName?name=LOGIN_LOG");

      let status = data.status;
      let dataList = JSON.parse(data.data);

      var dataArr = new Array();
      if (status === 200) {
        for (var i = dataList.length - 1; i >= 0; i--) {
          let item = new Array();
          item.push(dataList[i].NAME);
          // item.push(dataList[i].TIME);
          item.push(
            "<span  class='colorGrass'>" + dataList[i].MESSAGE + "</span>"
          );
          dataArr.push(item);
        }
        this.config.data = dataArr;
        this.$refs["log"].updateRows(dataArr);
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
        height: 2.4rem;
      }
      .r-border-top {
        box-sizing: border-box;
        padding: 0.3rem;
      }
      .warrper {
        padding: 0.3rem 0.2rem;
      }
      .r-bottom {
        flex: 1;
        .item {
          font-size: 0.3rem;
          margin-top: 0.15rem;
          span {
            margin-left: 0.3rem;
          }
        }
      }
    }
  }
}
</style>