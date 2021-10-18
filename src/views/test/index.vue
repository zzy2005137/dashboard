<template>
  <div class="test-container">
    <my-chart class="container" :tableData="tableData"></my-chart>
    <my-table class="container" @updateData="updateData"></my-table>
  </div>
</template>

<script>
import myTable from "@/components/Table/index.vue";
import myChart from "@/components/Charts/bar.vue";
// import myProgress from "./components/progress.vue"
export default {
  name: "App",
  components: {
    myTable,
    myChart
    //   myProgress,
  },
  data() {
    return {
      tableData: []
    };
  },
  methods: {
    updateData(tableData) {
      this.tableData = tableData;
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
</style>
