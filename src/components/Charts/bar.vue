<template>
  <div class="wrap">
    <div id="myChart" :style="{ width: '80%', height: '300px' }"></div>
    <!-- <el-button @click="showProps">check</el-button>
    <el-button @click="updateChart">update</el-button> -->
  </div>
</template>

<script>
// 引入 echarts 核心模块，核心模块提供了 echarts 使用必须要的接口。
import * as echarts from "echarts/core";
// 引入柱状图图表，图表后缀都为 Chart
import { BarChart } from "echarts/charts";
// 引入提示框，标题，直角坐标系，数据集，内置数据转换器组件，组件后缀都为 Component
import {
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  DatasetComponentOption,
  TransformComponent
} from "echarts/components";
// 标签自动布局，全局过渡动画等特性
import { LabelLayout, UniversalTransition } from "echarts/features";
// 引入 Canvas 渲染器，注意引入 CanvasRenderer 或者 SVGRenderer 是必须的一步
import { CanvasRenderer } from "echarts/renderers";

// 注册必须的组件
echarts.use([
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  TransformComponent,
  BarChart,
  LabelLayout,
  UniversalTransition,
  CanvasRenderer
]);

export default {
  props: ["tableData", "test"],
  name: "chart",
  data() {
    return {
      chartInstance: null,
      value: 5
    };
  },
  methods: {
    showProps() {
      console.log(this.tableData);
      // this.value += 5
    },
    updateChart() {}
  },
  watch: {
    tableData: {
      deep: true,
      handler() {
        // this.chartInstance.setOption(this.option)
        console.log("tableData changed");
        // this.showProps()
        this.chartInstance.setOption(this.option);
      }
    },
    value() {
      console.log("value changed");
    }
    // test: {
    //   deep: true,
    //   handler() {
    //     console.log("test changed")
    //   },
    // },
  },
  computed: {
    option() {
      let data = this.tableData.map(e => {
        return e.name;
      });

      let value = this.tableData.map(e => {
        return Number(e.quantity);
      });

      let option = {
        title: {
          text: "ECharts 示例"
        },
        tooltip: {},
        xAxis: {
          data: data
        },
        yAxis: {},
        series: [
          {
            name: "销量",
            type: "bar",
            data: value
          }
        ]
      };
      return option;
    }
  },
  mounted() {
    var myChart = echarts.init(document.getElementById("myChart"));
    // 绘制图表
    myChart.setOption(this.option);
    // myChart.resize()
    this.chartInstance = myChart;
  }
};
</script>

<style scoped>
.mychart {
  text-align: center;
}
</style>
