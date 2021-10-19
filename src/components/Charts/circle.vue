<template>
  <div id="circle-wrapper">
    <div id="main" :style="{ width: '200px', height: '200px' }"></div>

    <!-- <el-button @click="handleCheck">check</el-button> -->
  </div>
</template>

<script>
import * as echarts from "echarts/core";
import { GaugeChart } from "echarts/charts";
import { CanvasRenderer } from "echarts/renderers";

echarts.use([GaugeChart, CanvasRenderer]);

export default {
  props: ["currentRow"],
  data() {
    return {
      circleInstance: null
    };
  },
  methods: {
    handleCheck() {},
    getTimeSpan(date1, date2) {
      //date1:小日期   date2:大日期
      var sdate = new Date(date1);
      var now = new Date(date2);
      var days = now.getTime() - sdate.getTime();
      var day = parseInt(days / (1000 * 60 * 60 * 24));
      return day + 1;
    },
    getProgress() {
      //处理currentRow
      let ts = [];
      let e = this.currentRow;
      ts.push(e.T1);
      ts.push(e.T2);
      ts.push(e.T3);
      ts.push(e.T4);
      ts.push(e.T5);
      ts.push(e.T6);

      let span = this.getTimeSpan(e.Tb, e.Te);
      let curTime = 0;
      ts.forEach(e => {
        curTime += e;
      });

      let value = ((curTime / span) * 100).toFixed(1);
      console.log(value);

      return value;
    }
  },
  watch: {
    currentRow: {
      deep: true,
      handler() {
        // this.chartInstance.setOption(this.option)
        console.log("currentRow changed ===from circle.vue");

        this.circleInstance.setOption(this.option);
      }
    }
  },
  computed: {
    option() {
      let value = this.getProgress();
      const gaugeData = [
        {
          value: value,
          name: "Commonly",
          title: {
            offsetCenter: ["0%", "0%"]
          },
          detail: {
            valueAnimation: true,
            offsetCenter: ["0%", "5%"]
          }
        }
      ];
      let option = {
        series: [
          {
            type: "gauge",
            startAngle: 90,
            endAngle: -270,
            data: gaugeData,
            itemStyle: {
              color: "#F72C5B"
            },
            pointer: {
              show: false
            },
            progress: {
              show: true,
              overlap: false,
              roundCap: true,
              clip: false,
              itemStyle: {
                borderWidth: 0,
                borderColor: "#464646"
              }
            },
            axisLine: {
              lineStyle: {
                width: 20
              }
            },
            splitLine: {
              show: false,
              distance: 0,
              length: 12
            },
            axisTick: {
              show: false
            },
            axisLabel: {
              show: false
              // distance: 50
            },

            title: {
              show: false,
              fontSize: 20
            },
            detail: {
              width: 50,
              height: 14,
              fontSize: 30,
              color: "auto",
              formatter: "{value}%"
            }
          }
        ]
      };

      return option;
    }
  },
  mounted() {
    var chartDom = document.getElementById("main");
    var myChart = echarts.init(chartDom);
    myChart.setOption(this.option);
    this.circleInstance = myChart;
  }
};
</script>

<style scoped>
#main {
  margin-top: 2.5rem;
  margin-left: 2rem;
}
</style>
