<template>
  <div class="container">
    <el-table
      :data="tableData"
      style="width: 100%"
      @current-change="handleCurrentChange"
      @row-click="selectRow"
      stripe="true"
    >
      <el-table-column
        v-for="(item, index) in tableColumn"
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

      <el-table-column prop="operation" label="Operation">
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
      tableData: [
        {
          name: "A",
          quantity: 5,
          edit: false
        },
        {
          name: "B",
          quantity: 10,
          edit: false
        },
        {
          name: "C",
          quantity: 30,
          edit: false
        },
        {
          name: "D",
          quantity: 15,
          edit: false
        }
      ],
      tableColumn: [
        {
          prop: "name",
          label: "Name",
          width: "180"
        },
        {
          prop: "quantity",
          label: "Quantity",
          width: "180"
        }
      ],
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
      this.$emit("updateData", this.tableData);
    },
    handleDelete(index, row) {
      //   ElMessage.error("delete a row ")
      this.$message.error("delete a row");
      this.tableData.splice(index, 1);
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
        this.tableData.push(newRow);
      }
    },
    checkSave() {
      for (let i of this.tableData) {
        if (i.edit == true) {
          return false;
        }
      }
      return true;
    },
    selectRow(row) {
      // this.edit = !this.edit
      // ElMessage.success("select a row")
    }
  },
  created() {
    // this.showData = this.tableData
    this.$emit("updateData", this.tableData);
  }
};
</script>

<style scoped></style>
