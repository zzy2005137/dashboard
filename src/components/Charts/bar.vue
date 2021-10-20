<template>
  <div class="wrap">
    <div id="myChart" :style="{ width: '100%', height: '350px' }"></div>
    <!-- <el-button @click="handleCheck">check</el-button>
    <el-button @click="handleOne">One</el-button> -->
    <!-- <el-button @click="updateChart">update</el-button> -->
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

// 注册必须的组件npm
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
  props: ["tableActual", "tablePlan", "currentRow"],
  name: "chart",
  data() {
    return {
      chartInstance: null,
      value: 5
    };
  },
  methods: {
    handleCheck() {
      // this.chartInstance.setOption(this.option);
    },
    handleOne() {
      let ms = [];
      let e = this.currentRow;
      ms.push((e.M1 / e.Mi) * 100);
      ms.push(Math.round((e.M2 / e.Mi) * 100));
      ms.push(Math.round((e.M3 / e.Mi) * 100));
      ms.push(Math.round((e.M4 / e.Mi) * 100));
      ms.push(Math.round((e.M5 / e.Mi) * 100));
      ms.push(Math.round((e.M6 / e.Mi) * 100));
      console.log(ms);

      return ms;
    },
    updateChart() {}
  },
  watch: {
    currentRow: {
      deep: true,
      handler() {
        // this.chartInstance.setOption(this.option)
        console.log("currentRow changed === from bar.vue");
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
      let data = this.handleOne();

      let option = {
        title: {
          text: "关键工序点完成度"
        },
        tooltip: {},
        xAxis: {
          data: ["P1", "P2", "P3", "P4", "P5", "P6"],
          axisTick: {
            alignWithLabel: true
          }
        },
        yAxis: {
          type: "value",
          max: 100,

          axisLabel: {
            formatter: "{value} %"
          }
        },
        series: [
          {
            name: "完成进度",
            type: "bar",
            data: data,
            showBackground: true,
            backgroundStyle: {
              color: "rgba(180, 180, 180, 0.2)"
            },
            barWidth: "60%"
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
