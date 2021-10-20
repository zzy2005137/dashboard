<template>
  <div id="circle-wrapper">
    <h2>进度计算值</h2>
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
      ts.push(Number(e.T1));
      ts.push(Number(e.T2));
      ts.push(Number(e.T3));
      ts.push(Number(e.T4));
      ts.push(Number(e.T5));
      ts.push(Number(e.T6));

      let span = this.getTimeSpan(e.Tb, e.Te);
      let curTime = 0;
      ts.forEach(e => {
        curTime += e;
      });
      console.log(curTime);
      console.log(span);

      let value = ((curTime / span) * 100).toFixed(1);
      // console.log(value);

      //get bias
      let d = e.d;
      let planValue = ((d / span) * 100).toFixed(1);
      let color = this.getColor(value - planValue);

      return { value: value, color: color };
    },
    getColor(bias) {
      const max = 20;
      const min = -10;
      if (bias > 20) {
        return "#006633"; //green
      } else if (bias < -10) {
        return "#CC0033"; //red
      } else {
        return "#FFCC33"; //yellow
      }
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
      let result = this.getProgress();
      const gaugeData = [
        {
          value: result.value,
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
              color: result.color
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
  /* margin-top: 0.5rem; */
  margin-left: 2rem;
}
h2 {
  color: gray;
  font-size: 1.5rem;
  margin-left: 4rem;
}
</style>
