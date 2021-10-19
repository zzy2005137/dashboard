<template>
  <div class="test-container">
    <div class="charts">
      <bar-chart
        class="bar"
        :tableActual="tableActual"
        :tablePlan="tablePlan"
        :array="array"
        :currentRow="currentRow"
      ></bar-chart>
      <circle-chart class="circle" :currentRow="currentRow" />
    </div>

    <h2 class="table-header">各关键工序点的监控数据</h2>
    <my-table
      class="container"
      @updateData="updateData"
      @changeSelect="changeRow"
    ></my-table>
    <h2 class="table-header">五种外协加工任务实例的计划进度</h2>
    <el-table :data="tablePlan.tableData" style="width: 100%" stripe="true">
      <el-table-column
        v-for="(item, index) in tablePlan.tableColumn"
        :key="index"
        :prop="item.prop"
        :label="item.label"
        :width="item.width"
      >
        <template #default="scope">
          {{ scope.row[item.prop] }}
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import myTable from "@/components/Table/index.vue";
import barChart from "@/components/Charts/bar.vue";
import circleChart from "@/components/Charts/circle.vue";

export default {
  name: "App",
  components: {
    myTable,
    barChart,
    circleChart
    //   myProgress,
  },
  data() {
    return {
      tableData: [],
      tablePlan: {
        tableColumn: [
          {
            prop: "name",
            label: "Name"
          },
          {
            prop: "Mi",
            label: "Mi"
          },
          {
            prop: "Tb",
            label: "Tb"
          },
          {
            prop: "Te",
            label: "Te"
          },
          {
            prop: "T1",
            label: "T1"
          },
          {
            prop: "T2",
            label: "T2"
          },
          {
            prop: "T3",
            label: "T3"
          },
          {
            prop: "T4",
            label: "T4"
          },
          {
            prop: "T5",
            label: "T5"
          },
          {
            prop: "T6",
            label: "T6"
          }
        ],
        tableData: [
          {
            name: "R1",
            Mi: 200,
            Tb: new Date("2021-07-04").toLocaleDateString(),
            Te: new Date("2021-07-17").toLocaleDateString(),
            T1: 2,
            T2: 1.5,
            T3: 5.5,
            T4: 3,
            T5: 2,
            T6: null,
            edit: false
          },
          {
            name: "R2",
            Mi: 1000,
            Tb: new Date("2021-07-08").toLocaleDateString(),
            Te: new Date("2021-07-18").toLocaleDateString(),
            T1: 2,
            T2: 5.75,
            T3: 3.25,
            T4: null,
            T5: null,
            T6: null,

            edit: false
          },
          {
            name: "R3",
            Mi: 500,
            Tb: new Date("2021-07-05").toLocaleDateString(),
            Te: new Date("2021-07-21").toLocaleDateString(),
            T1: 1,
            T2: 5,
            T3: 4,
            T4: 3,
            T5: 4,
            T6: null,
            edit: false
          },
          {
            name: "R4",
            Mi: 400,
            Tb: new Date("2021-07-10").toLocaleDateString(),
            Te: new Date("2021-08-03").toLocaleDateString(),
            T1: 1,
            T2: 7.25,
            T3: 4.5,
            T4: 5.75,
            T5: 4.5,
            T6: 2,
            edit: false
          },
          {
            name: "R5",
            Mi: 1000,
            Tb: new Date("2021-07-08").toLocaleDateString(),
            Te: new Date("2021-07-21").toLocaleDateString(),
            T1: 3.25,
            T2: 1.5,
            T3: 5.5,
            T4: 3.75,
            T5: null,
            T6: null,
            edit: false
          }
        ]
      },
      tableActual: {
        tableColumn: [],
        tableData: []
      },
      array: [1, 2, 3],
      currentRow: {}
    };
  },
  methods: {
    updateData(tableActual) {
      this.tableActual = tableActual;
    },
    changeRow(row) {
      // console.log(row);
      this.currentRow = row;
    }
  }
};
</script>

<style scoped>
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.test-container {
  margin: 30px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

.table-header {
  text-align: center;
  margin: 2rem;
  color: gray;
}
.charts {
  display: flex;
}
.bar {
  display: inline-block;
  width: 50%;
}
.cirle {
  display: block;
  width: 50%;
  margin: 3rem;
}
</style>
