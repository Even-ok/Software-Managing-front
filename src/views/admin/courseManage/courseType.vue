<template>
  <div>
    <v-row dense>
      <v-col cols="12"
        ><v-btn dark @click="handleAdd()"> 添加课程 </v-btn></v-col
      >
      <v-col v-for="(item, i) in courseTypeData" :key="i" cols="3">
        <v-card>
          <v-img
            class="white--text align-end"
            height="100px"
            src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
          >
          </v-img>

          <v-card-title> {{ item.prefix }} </v-card-title>
          <v-card-subtitle>{{ item.name }}</v-card-subtitle>

          <v-card-actions>
            <v-btn color="orange" text @click="handleDelete(item)">
              删除
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col></v-row
    >
    <el-dialog :visible.sync="courseAddDialog" title="新增课程" center>
      <v-form lazy-validation>
        <v-text-field
          v-model="form.prefix"
          :counter="20"
          label="课程id"
          filled
          :required="true"
        ></v-text-field>
        <v-text-field
          v-model="form.name"
          :counter="20"
          label="课程名称"
          filled
          c
        ></v-text-field>
      </v-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="addCourse">确定</el-button>
        <el-button type="primary" @click="courseAddDialog = false"
          >取消</el-button
        >
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      courseAddDialog: false,
      courseTypeData: [
        {
          prefix: "001",
          name: "Course1",
        },
        {
          prefix: "003",
          name: "Course2",
        },
        {
          prefix: "003",
          name: "Course3",
        },
      ],
      form: { name: "", prefix: "" },
    };
  },
  methods: {
    handleDelete(item) {
      this.$confirm("确认删除吗?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.deleteCourse(item);
          this.$message({
            type: "success",
            message: "删除成功!",
          });
          document.execCommand("Refresh");
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "取消删除操作",
          });
        });
    },
    handleAdd() {
      this.courseAddDialog = true;
    },
    addCourse() {},
    isExist() {
      //课程是否存在
      var ret =
        this.courseTypeData.indexOf(this.form.prefix) &&
        this.courseTypeData.indexOf(this.form.name);
      return ret > 0;
    },
    deleteCourse() {},
    getCourseType() {},
  },
  mounted() {
    this.getCourseType();
  },
};
</script>

<style scoped>
</style>