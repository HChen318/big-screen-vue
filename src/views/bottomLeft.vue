<template>
  <div id="bottomLeft">
    <div class="bg-color-black">
      <div class="d-flex pt-2 pl-2">
        <span style="color: #5cd9e8">
          <icon name="chart-bar"></icon>
        </span>
        <div class="d-flex">
          <span class="fs-xl text mx-2">锅炉统计</span>
        </div>
      </div>
      <div class="bottom-main">
        <bottomLeftChartLeft ref="bottomLeftChartLeft"/>
        <bottomLeftChartRight ref="bottomLeftChartRight" />
      </div>
    </div>
  </div>
</template>

<script>
import bottomLeftChartLeft from "@/components/echart/bottom/bottomLeftChartLeft";
import bottomLeftChartRight from "@/components/echart/bottom/bottomLeftChartRight";
export default {
  data() {
    return {
      timer: null
    };
  },
  components: {
    bottomLeftChartLeft,
    bottomLeftChartRight
  },
  mounted() {
    this.changeTiming();
  },
  methods: {
    changeTiming() {
      this.timer = setInterval(() => {
        this.fetchBoiler30TData(); //获取-状态
        this.fetchBoiler50TData(); //获取-状态
      }, 3000);
    },
    async fetchBoiler30TData() {
      const { data } = await this.$http.get("getDataByName/?e=1&n=GET_DL_30T");

      const status = data.status;
      const boiler30TData = JSON.parse(data.data);

      if (status === 200) {
        this.$refs.bottomLeftChartState.refresh(boiler30TData);
      }
    },
    async fetchBoiler50TData() {
      const { data } = await this.$http.get("getDataByName/?e=1&n=GET_DL_50T");

      const status = data.status;
      const boiler50TData = JSON.parse(data.data);

      if (status === 200) {
        this.$refs.bottomRightChartState.refresh(boiler50TData);
      }
    },
  },
  beforeDestroy() {
    this.timer && clearInterval(this.timer);
  }
};
</script>

<style lang="scss">
#bottomLeft {
  padding: 0.3rem 0.2rem;
  height: 6.5rem;
  min-width: 3.75rem;
  border-radius: 0.0625rem;
  .bg-color-black {
    height: 6.0625rem;
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
}
.bottom-main {
  display: flex;
  justify-content: space-between;
}
</style>