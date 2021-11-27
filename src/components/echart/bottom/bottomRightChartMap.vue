<template>
  <!-- 绘制流水线 -->
  <div>
    <!-- style="width: 11.25rem; height: 6rem" -->
    <canvas id="canvas" width="790" height="400"></canvas>
  </div>
</template>

<script>
import "../../../../node_modules/echarts/map/js/province/beijing.js";
export default {
  data() {
    return {
      chart: null,
      index: -1,
      tempCanvas: null,
      tempCtx: null,
      ctx: null,
      offset: 100,
    };
  },
  mounted() {
    this.initCanvas();
  },
  methods: {
    initCanvas() {
      this.tempCanvas = document.getElementById("canvas");
      // console.dir(this.tempCanvas);
      let ctx = this.tempCanvas.getContext("2d");

      this.setupCanvas(ctx, this.tempCanvas);
      let offset = 100;
      let canvas = this.tempCanvas
      function  createGradient(ctx, x0, y0, x1, y1) {
        let grd = ctx.createLinearGradient(x0, y0, x1, y1);
        grd.addColorStop(0, "#9a12b3");
        grd.addColorStop(1, "#19b5fe");
        return grd;
      }
      function animate() {
        ctx.fillStyle = 'rgba(255, 255, 255, 0)';
        // console.log(canvas.width, canvas.height)
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        ctx.lineWidth = 3;
        ctx.save();
        ctx.beginPath();
        ctx.moveTo(100, 100);
        ctx.lineTo(680, 100);
        ctx.lineTo(680, 300);
        ctx.lineTo(580, 260);
        ctx.lineTo(100, 260);
        ctx.strokeStyle = "gray";
        ctx.lineJoin = "round";
        // 绘制线
        ctx.stroke();
        ctx.beginPath();
        ctx.moveTo(680, 300);
        ctx.lineTo(580, 320);
        ctx.lineTo(320, 320);
        ctx.strokeStyle = "gray";
        ctx.lineJoin = "round";
        // 绘制线
        ctx.stroke();
        ctx.beginPath();
        ctx.rect(offset, 0, 150, 500);
        ctx.clip();
        ctx.beginPath();
        ctx.moveTo(100, 100);
        ctx.lineTo(680, 100);
        ctx.lineTo(680, 300);
        ctx.lineTo(580, 260);
        ctx.lineTo(100, 260);
        ctx.lineWidth = 4;
        ctx.strokeStyle = createGradient(
          ctx,
          offset,
          0,
          offset + 150,
          0
        );
        ctx.lineCap = "round";
        ctx.lineJoin = "round";
        ctx.stroke();
        // 绘制线
        ctx.stroke();
        ctx.beginPath();
        ctx.moveTo(680, 300);
        ctx.lineTo(580, 320);
        ctx.lineTo(320, 320);
        ctx.strokeStyle = "gray";
        ctx.lineJoin = "round";
        ctx.restore();
        offset += 2;
        if (offset > 600) {
          offset = 100;
        }
        requestAnimationFrame(animate);
      }
      animate();
    },
    setupCanvas(ctx, canvas) {
      let width = canvas.width,
        height = canvas.height,
        dpr = window.devicePixelRatio || 1.0;
      if (dpr != 1.0) {
        canvas.style.width = width + "px";
        canvas.style.height = height + "px";
        canvas.height = height * dpr;
        canvas.width = width * dpr;
        ctx.scale(dpr, dpr);
      }
    }
  },
  destroyed() {
    window.onresize = null;
  },
};
</script>

<style lang="scss" scoped>
</style>