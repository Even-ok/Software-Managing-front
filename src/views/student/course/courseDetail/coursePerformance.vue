<template>
  <div>
    <h3
      style="
        display: flex;
        align-items: center;
        color: #ef5350;
        margin-bottom: 5px;
      "
    >
      各项占总成绩比例： 实验:{{ eachScore.report }}% ；测验:{{
        eachScore.exam
      }}% ；出勤:{{ eachScore.attendance }}%
    </h3>

    <v-card style="display: flex; justify-content: center; margin-bottom: 5px">
      <div
        id="myChart"
        style="width: 800px; height: 500px; display: flex; align-items: center"
      />
      <h3 style="display: flex; align-items: center">
        所得成绩占比:{{
          allScore.attendence_score + allScore.ex_score + allScore.exam_score
        }}/100
      </h3>
    </v-card>

    <v-card style="display: flex; justify-content: center; margin-bottom: 5px">
      <v-card-title>
        <h3 style="color: #6666cc">出勤次数:{{ attendScore }}</h3>
      </v-card-title>

      <el-row>
        <el-row>
          <el-col :span="24">
            <el-table
              :data="
                attendData.slice(
                  (currentPageA - 1) * pagesize,
                  currentPageA * pagesize
                )
              "
              :stripe="true"
              style="width: 100%; margin-left: 7%; margin-right: 10%"
            >
              <el-table-column prop="seq" label="序号" width="180">
              </el-table-column>
              <el-table-column prop="title" label="实验" width="180" />

              <el-table-column prop="is_submit" label="是否提交">
                <template slot-scope="scope"
                  ><el-tag
                    :type="scope.row.is_submit === '是' ? 'success' : 'primary'"
                    disable-transitions
                    >{{ scope.row.is_submit }}</el-tag
                  ></template
                ></el-table-column
              >
            </el-table></el-col
          ></el-row
        >
        <el-row>
          <el-col :span="24">
            <el-pagination
              @current-change="handleCurrentChangeA"
              :current-page="currentPageA"
              :page-size="pagesize"
              layout="total, prev, pager, next, jumper"
              :total="attendData.length"
            >
            </el-pagination></el-col></el-row
      ></el-row>
    </v-card>
    <v-card style="display: flex; justify-content: center; margin-bottom: 5px">
      <v-card-title>
        <h3 style="color: #6666cc">报告得分与权重:{{ reportScore }}</h3>
      </v-card-title>

      <el-row>
        <el-row>
          <el-col :span="24">
            <el-table
              :data="
                reportData.slice(
                  (currentPageR - 1) * pagesize,
                  currentPageR * pagesize
                )
              "
              :stripe="true"
              style="width: 100%; margin-left: 7%; margin-right: 10%"
            >
              <el-table-column prop="seq" label="序号" width="180">
              </el-table-column>
              <el-table-column prop="title" label="实验" width="180" />
              <el-table-column prop="weight" label="权重" width="180" />

              <el-table-column prop="score" label="得分" /> </el-table></el-col
        ></el-row>
        <el-row>
          <el-col :span="24">
            <el-pagination
              @current-change="handleCurrentChangeR"
              :current-page="currentPageR"
              :page-size="pagesize"
              layout="total, prev, pager, next, jumper"
              :total="reportData.length"
            >
            </el-pagination></el-col></el-row
      ></el-row>
    </v-card>
    <v-card style="display: flex; justify-content: center; margin-bottom: 5px">
      <v-card-title>
        <h3 style="color: #6666cc">测验得分:{{ examScore }}</h3>
      </v-card-title>

      <el-row>
        <el-row>
          <el-col :span="24">
            <el-table
              :data="
                examData.slice(
                  (currentPageE - 1) * pagesize,
                  currentPageE * pagesize
                )
              "
              :stripe="true"
              style="width: 100%; margin-left: 7%; margin-right: 10%"
            >
              <el-table-column prop="seq" label="序号" width="180">
              </el-table-column>
              <el-table-column prop="title" label="测验" width="180" />
              <el-table-column prop="all_score" label="总分" width="180" />

              <el-table-column
                prop="stu_score"
                label="得分"
              /> </el-table></el-col
        ></el-row>
        <el-row>
          <el-col :span="24">
            <el-pagination
              @current-change="handleCurrentChangeE"
              :current-page="currentPageE"
              :page-size="pagesize"
              layout="total, prev, pager, next, jumper"
              :total="examData.length"
            >
            </el-pagination></el-col></el-row></el-row
    ></v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentPageA: 1,
      currentPageR: 1,
      currentPageE: 1,
      pagesize: 6,
      allScore: {
        attendence_score: 10,
        ex_score: 60,
        exam_score: 15,
      },
      class_id: null,
      now_score: 0,
      attendData: [],
      reportData: [],
      examData: [],
      eachScore: {
        exam: 20,
        report: 70,
        attendance: 10,
      },
      attendScore: 10,
      examScore: 60,
      reportScore: 15,
    };
  },
  methods: {
    handleCurrentChangeA: function (currentPage) {
      this.currentPageA = currentPage;
    },

    handleCurrentChangeR: function (currentPage) {
      this.currentPageR = currentPage;
    },

    handleCurrentChangeE: function (currentPage) {
      this.currentPageE = currentPage;
    },
    getParams: function () {
      this.class_id = JSON.parse(this.$Base64.decode(this.$route.query.info))[
        "class_id"
      ];
    },
    getAllScore() {},

    exChart() {
      var echarts = require("echarts");
      let myChart = echarts.init(document.getElementById("myChart"));
      myChart.clear();
      var option = {
        backgroundColor: "#FFFFFF",

        title: {
          text: "您的成绩",
          left: "center",
          top: 20,
          textStyle: {
            color: "#3366CC",
          },
        },

        tooltip: {
          trigger: "item",
        },

        series: [
          {
            name: "实验名称",
            type: "pie",
            radius: "55%",
            center: ["50%", "50%"],
            data: [
              { value: this.allScore.attendence_score, name: "出勤" },
              { value: this.allScore.ex_score, name: "实验" },
              { value: this.allScore.exam_score, name: "测验" },
            ],
            label: {
              color: "#000000",
            },
            labelLine: {
              lineStyle: {
                color: "#000000",
              },
              smooth: 0.2,
              length: 10,
              length2: 20,
            },
            itemStyle: {
              shadowBlur: 300,
              shadowColor: "rgba(0, 0, 0, 0.5)",
            },

            animationType: "scale",
            animationEasing: "elasticOut",
            animationDelay: function () {
              return Math.random() * 200;
            },
          },
        ],
      };
      myChart.setOption(option);
      myChart.resize();
    },
    getNowScore() {
      this.getAllScore();
      this.getExam();
      this.getAttend();
      this.getAttend();
    },
    getExam() {},
    getReport() {},
    getAttend() {},
    getPercent() {},
  },
  mounted() {
    this.getParams();
    this.getAllScore();
    this.getExam();
    this.getReport();
    this.getAttend();
    this.getPercent();
    setTimeout(() => {
      this.exChart();
    }, 760);
    /* setTimeout(() => {
      this.examChart();
    }, 760);
    setTimeout(() => {
      this.scoreChart();
    }, 760);*/
    setTimeout(() => {
      this.getNowScore();
    }, 760);
  },
};
</script>