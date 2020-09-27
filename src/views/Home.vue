<template>
  <div class="home" id="pdf">
    <div class="echarts" id="main"></div>
    <div>哎呦，不错哦</div>
    <button @click="exportPdf">导出pdf</button>
  </div>
</template>

<script>
// @ is an alias to /src
import echarts from "echarts";
import html2canvas from "html2canvas";
import JsPDF from "jspdf";

export default {
  name: "Home",
  components: {},
  methods: {
    init() {
      var echarts = require("echarts");

      // 基于准备好的dom，初始化echarts实例
      var myChart = echarts.init(document.getElementById("main"));
      // 绘制图表
      myChart.setOption({
        title: {
          text: "ECharts 入门示例",
        },
        tooltip: {},
        xAxis: {
          data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"],
        },
        yAxis: {},
        series: [
          {
            name: "销量",
            type: "bar",
            data: [5, 20, 36, 10, 10, 20],
          },
        ],
      });
    },

    exportPdf() {
      console.log("exportPdf");
      html2canvas(document.querySelector("#pdf")).then((canvas) => {
        let contentWidth = canvas.width;
        let contentHeight = canvas.height;
        let pageHeight = (contentWidth / 592.28) * 841.89;
        let leftHeight = contentHeight;
        let position = 0;
        let imgWidth = 595.28;
        let imgHeight = (592.28 / contentWidth) * contentHeight;
        let pageData = canvas.toDataURL("image/jpeg", 1.0);
        let PDF = new JsPDF("", "pt", "a4");
        if (leftHeight < pageHeight) {
          PDF.addImage(pageData, "JPEG", 0, 0, imgWidth, imgHeight);
        } else {
          while (leftHeight > 0) {
            PDF.addImage(pageData, "JPEG", 0, position, imgWidth, imgHeight);
            leftHeight -= pageHeight;
            position -= 841.89;
            if (leftHeight > 0) {
              PDF.addPage();
            }
          }
        }
        PDF.save('test' + '.pdf')
      });
    },
  },

  mounted() {
    this.init();
  },
};
</script>

<style scoped>
.echarts {
  width: 500px;
  height: 200px;
  background: pink;
}
</style>
