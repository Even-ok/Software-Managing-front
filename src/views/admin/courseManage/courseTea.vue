<template>
  <el-card>
    <v-btn @click="handleAdd" dark> 添加责任教师</v-btn>
    <el-table
      :data="
        dutyTeaList.filter(
          (data) =>
            !search ||
            data.prefix.toLowerCase().includes(search.toLowerCase()) ||
            data.name.toLowerCase().includes(search.toLowerCase()) ||
            data.year.toLowerCase().includes(search.toLowerCase()) ||
            data.semester.toLowerCase().includes(search.toLowerCase()) ||
            data.t_id.toLowerCase().includes(search.toLowerCase()) ||
            data.t_name.toLowerCase().includes(search.toLowerCase())
        )
      "
      style="width: 100%"
    >
      <el-table-column label="课程编号" prop="prefix" sortable />
      <el-table-column label="课程名称" prop="name" sortable />
      <el-table-column label="开课学年" prop="year" sortable />
      <el-table-column label="开课学期" prop="semester" sortable />
      <el-table-column label="教师id" prop="t_id" sortable />
      <el-table-column label="教师名称" prop="t_name" sortable />
      <el-table-column align="right">
        <template #header>
          <el-input v-model="search" size="mini" placeholder="Type to search" />
        </template>
        <template #default="scope">
          <el-button size="mini" @click="handleEdit(scope.row)">更改</el-button>
          <el-button size="mini" @click="handleDelete(scope.row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <el-dialog :visible.sync="addCourseDialog" title="添加责任教师" center>
      <el-form ref="form" label-width="80px" style="margin-left: 5%">
        <el-form-item label="课程编号" required>
          <el-select v-model="prefix">
            <el-option
              v-for="item in courseList"
              :key="item.prefix"
              :label="item.prefix + item.name"
              :value="item.prefix"
            >
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="开课学年" required>
          <el-input v-model="year"></el-input>
        </el-form-item>
        <el-form-item label="开课学期" required>
          <el-select v-model="semester">
            <el-option
              v-for="item in semesterList"
              :key="item"
              :label="item"
              :value="item"
            >
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="责任教师" required>
          <el-select v-model="t_id">
            <el-option
              v-for="item in teaList"
              :key="item.id"
              :label="item.id + item.name"
              :value="item.id"
            >
            </el-option>
          </el-select>
        </el-form-item>
      </el-form>

      <div slot="footer" class="dialog-footer">
        <el-button @click="addCourseDialog = false">取消</el-button>
        <el-button type="primary" @click="addCourse()">确定</el-button>
      </div>
    </el-dialog>

    <el-dialog :visible.sync="updataTeaDialog" title="更改课程信息" center>
      <el-form ref="form" label-width="80px" style="margin-left: 5%">
        <el-form-item label="课程编号" required>
          <el-input v-model="prefix" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="开课学年" required>
          <el-input v-model="year"></el-input>
        </el-form-item>
        <el-form-item label="开课学期" required>
          <el-select v-model="semester">
            <el-option
              v-for="item in semesterList"
              :key="item"
              :label="item"
              :value="item"
            >
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="责任教师" required>
          <el-select v-model="t_id">
            <el-option
              v-for="item in teaList"
              :key="item.id"
              :label="item.id + item.name"
              :value="item.id"
            >
            </el-option>
          </el-select>
        </el-form-item>
      </el-form>

      <div slot="footer" class="dialog-footer">
        <el-button @click="updataTeaDialog = false">取消</el-button>
        <el-button type="primary" @click="updateTea()">确定</el-button>
      </div>
    </el-dialog>
  </el-card>
</template>

<script scoped>
// import axios from "axios";
export default {
  data() {
    return {
      search: "",

      addCourseDialog: false,
      updataTeaDialog: false,
      courseList: [],

      semesterList: ["春季", "秋季"],
      teaList: [],

      c_id: "",
      prefix: "",
      year: "",
      semester: "",
      t_id: "",
      dutyTeaList: [
        {
          prefix:"001",
          name:"Course1",
          year:2022,
          semester:"1",
          t_id:"001",
          t_name:"Name1",
        }
      ],
    };
  },
  methods: {
    handleEdit(row) {
      this.c_id = row.c_id;
      this.prefix = row.prefix;
      this.year = row.year;
      this.semester = row.semester;
      this.t_id = row.t_id;
      this.updataTeaDialog = true;
    },
    updateTea() {
      //更改责任教师
    },
    handleDelete(row) {
      this.$confirm("确认删除该课程吗?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.deleteCourse(row);
          this.$message({
            type: "success",
            message: "删除成功!",
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "取消删除操作",
          });
        });
    },
    deleteCourse(row) {
      //删除课程
      console.log(row);
    },
    getAllCourse() {
      //获得所有课程
    },
    getAllDutyTea() {
      //已经设置的责任教师
    },
    getAllTea() {
      //获得所有教师
    },
    handleAdd() {
      this.c_id = "";
      this.prefix = "";
      this.year = "";
      this.semester = "";
      this.t_id = "";
      this.addCourseDialog = true;
    },

    addCourse() {
      //开课
    },
  },

  mounted() {
    this.getAllCourse();
    this.getAllDutyTea();
    this.getAllTea();
  },
};
</script>
