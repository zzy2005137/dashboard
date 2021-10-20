<template>
  <div class="container">
    <el-table
      :data="tableActual.tableData"
      style="width: 100%"
      @current-change="handleCurrentChange"
      @row-click="rowClick"
      stripe="true"
    >
      <el-table-column
        v-for="(item, index) in tableActual.tableColumn"
        :key="index"
        :prop="item.prop"
        :label="item.label"
        :width="item.width"
      >
        <template #default="scope">
          <el-input
            v-if="scope.row.edit"
            size="mini"
            v-model="scope.row[item.prop]"
          ></el-input>
          <span v-else>{{ scope.row[item.prop] }}</span>
        </template>
      </el-table-column>

      <el-table-column prop="operation" label="Operation" width="180">
        <template #default="scope">
          <el-button
            v-if="scope.row.edit"
            size="mini"
            @click="handleSave(scope.$index, scope.row)"
            type="success"
            plain
            >Save</el-button
          >
          <el-button
            v-else
            size="mini"
            @click="handleEdit(scope.$index, scope.row)"
            type="primary"
            plain
            >Edit</el-button
          >

          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)"
            >Delete</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <div class="add" style="margin-top: 20px">
      <el-button @click="handleAdd()">Add </el-button>
    </div>
  </div>
</template>

<script>
// import { ElMessage } from "element-plus"

export default {
  name: "myTable",
  data() {
    return {
      tableActual: {
        tableColumn: [
          {
            prop: "name",
            label: "Name"
          },
          {
            prop: "T1",
            label: "T1"
          },
          {
            prop: "M1",
            label: "M1"
          },
          {
            prop: "T2",
            label: "T2"
          },
          {
            prop: "M2",
            label: "M2"
          },
          {
            prop: "T3",
            label: "T3"
          },
          {
            prop: "M3",
            label: "M3"
          },
          {
            prop: "T4",
            label: "T4"
          },
          {
            prop: "M4",
            label: "M4"
          },
          {
            prop: "T5",
            label: "T5"
          },
          {
            prop: "M5",
            label: "M5"
          },
          {
            prop: "T6",
            label: "T6"
          },
          {
            prop: "M6",
            label: "M6"
          },
          {
            prop: "d",
            label: "d"
          }
        ],
        tableData: [
          {
            name: "R1",
            Mi: 200,
            M1: 200,
            M2: 124,
            M3: 89,
            M4: 61,
            M5: 37,
            M6: null,
            Tb: new Date("2021-07-04").toLocaleDateString(),
            Te: new Date("2021-07-17").toLocaleDateString(),
            T1: 1.5,
            T2: 1.5,
            T3: 2,
            T4: 1,
            T5: 0.5,
            T6: null,
            d: 9,
            edit: false
          },
          {
            name: "R2",
            Mi: 1000,
            M1: 804,
            M2: 712,
            M3: 683,
            M4: null,
            M5: null,
            M6: null,
            Tb: new Date("2021-07-08").toLocaleDateString(),
            Te: new Date("2021-07-18").toLocaleDateString(),
            T1: 1.25,
            T2: 4,
            T3: 2.25,
            T4: null,
            T5: null,
            T6: null,
            d: 8,
            edit: false
          },
          {
            name: "R3",
            Mi: 500,
            M1: 500,
            M2: 500,
            M3: 500,
            M4: 302,
            M5: 281,
            M6: null,
            Tb: new Date("2021-07-05").toLocaleDateString(),
            Te: new Date("2021-07-21").toLocaleDateString(),
            T1: 1,
            T2: 4.75,
            T3: 4,
            T4: 1.75,
            T5: 1,
            T6: null,
            d: 7,
            edit: false
          },
          {
            name: "R4",
            Mi: 400,
            M1: 0,
            M2: 0,
            M3: 0,
            M4: 0,
            M5: 0,
            M6: 0,
            Tb: new Date("2021-07-10").toLocaleDateString(),
            Te: new Date("2021-08-03").toLocaleDateString(),
            T1: 0,
            T2: 0,
            T3: 0,
            T4: 0,
            T5: 0,
            T6: 0,
            d: 3,
            edit: false
          },
          {
            name: "R5",
            Mi: 1000,
            M1: 1000,
            M2: 1000,
            M3: 529,
            M4: 337,
            M5: null,
            M6: null,
            Tb: new Date("2021-07-08").toLocaleDateString(),
            Te: new Date("2021-07-21").toLocaleDateString(),
            T1: 3,
            T2: 1.25,
            T3: 2.15,
            T4: 1.75,
            T5: null,
            T6: null,
            d: 5,
            edit: false
          }
        ]
      },
      edit: false
      // showData: [],
    };
  },
  methods: {
    handleCurrentChange() {
      // console.log("selection change")
    },
    handleEdit(index, row) {
      if (!this.checkSave()) {
        // ElMessage.warning("please save first")
        this.$message.warning("please save first");
      } else {
        // this.edit = !this.edit
        row.edit = true;
        console.log(row);
      }
    },
    handleSave(index, row) {
      //   ElMessage.success("save successfully")
      this.$message.success("save successfully");
      row.edit = false;
      this.$emit("updateData", this.tableActual.tableData);
    },
    handleDelete(index, row) {
      //   ElMessage.error("delete a row ")
      this.$message.error("delete a row");
      this.tableActual.tableData.splice(index, 1);
    },
    handleAdd() {
      if (!this.checkSave()) {
        // ElMessage.warning("please save first")
        this.$message.warning("please save first");
      } else {
        let newRow = {
          date: "",
          name: "",
          address: "",
          edit: true
        };
        this.tableActual.tableData.push(newRow);
      }
    },
    checkSave() {
      for (let i of this.tableActual.tableData) {
        if (i.edit == true) {
          return false;
        }
      }
      return true;
    },
    rowClick(row) {
      // this.edit = !this.edit
      // ElMessage.success("select a row")
      this.$emit("changeSelect", row);
    }
  },
  created() {
    // this.showData = this.tableData
    this.$emit("updateData", this.tableActual);
  }
};
</script>

<style scoped></style>
