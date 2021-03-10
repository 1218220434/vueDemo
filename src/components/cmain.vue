<template>
  <div class="cmain p-5">
    <Search @func="getMsgFormSon" />

    <div class="d-flex jc-between ai-center my-3">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-sizes="[2, 4]"
        :page-size="4"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      >
      </el-pagination>
      <div @click="allboxshow()" class="flor">
        <i class="el-icon-sort mr-2"></i>折叠所有条目
      </div>
    </div>

    <div class="my-2">
      <div class="card mt-2 mb-5" v-for="(item, index) in dataArr" :key="index">
        <Titleheader :theObj="item.theObj" />
        <div class="formbox" v-if="item.theObj.tablesite">
          <!--这里的name 和 css 类名第一个字段要一样-->
          <div class="box" v-show="item.boxshow">
            <el-table :data="item.tableData" border style="width: 100%">
              <el-table-column
                align="center"
                prop="subj"
                label="学号"
                width="80"
              >
              </el-table-column>
              <el-table-column
                align="center"
                prop="temp"
                label="姓名"
                width="160"
              />

              <el-table-column
                align="center"
                prop="answer"
                label="性别"
                width="160"
              />

              <el-table-column
                align="center"
                prop="scan"
                label="身高"
                width="160"
              />

              <el-table-column
                align="center"
                prop="allocation"
                label="成绩"
                width="160"
              />

              <el-table-column align="center" label="管理">
                <template slot-scope="scope">
                  <el-button
                    @click="handleView(scope.row, 0)"
                    type="primary"
                    plain
                    size="small"
                    >查看</el-button
                  >
                  <el-button
                    @click="handleView(scope.row, 1)"
                    type="success"
                    plain
                    size="small"
                    >编辑</el-button
                  >
                  <el-button
                    type="danger"
                    @click="handleDelate(scope.$index, item.id)"
                    plain
                    size="small"
                    >删除</el-button
                  >
                </template>
              </el-table-column>
            </el-table>
          </div>
          <el-dialog
            :title="editbool ? '编辑学生' : '查看'"
            :visible.sync="dialogFormVisible"
            center
            width="30%"
          >
            <el-form
              :model="ruleForm"
              :rules="rules"
              ref="ruleForm"
              label-width="100px"
              class="demo-ruleForm"
            >
              <el-form-item
                label="学号"
                prop="region"
                :label-width="formLabelWidth"
              >
                <el-input
                  v-model="ruleForm.region"
                  autocomplete="off"
                  placeholder="请填写学生学号"
                ></el-input>
              </el-form-item>
              <el-form-item
                label="学生名字"
                prop="name"
                :label-width="formLabelWidth"
              >
                <el-input
                  v-model="ruleForm.name"
                  autocomplete="off"
                  placeholder="请填写学生名字"
                ></el-input>
              </el-form-item>
              <el-form-item
                label="学生性别"
                prop="sex"
                :label-width="formLabelWidth"
              >
                <el-input
                  v-model="ruleForm.sex"
                  autocomplete="off"
                  placeholder="请填写学生性别"
                ></el-input>
              </el-form-item>
              <el-form-item
                label="学生身高"
                prop="height"
                :label-width="formLabelWidth"
              >
                <el-input
                  v-model="ruleForm.height"
                  autocomplete="off"
                  placeholder="请填写学生身高"
                ></el-input>
              </el-form-item>
              <el-form-item
                label="学生分数"
                prop="num"
                :label-width="formLabelWidth"
              >
                <el-input
                  v-model="ruleForm.num"
                  autocomplete="off"
                  placeholder="请填写学生分数"
                ></el-input>
              </el-form-item>
              <el-form-item v-if="editbool">
                <el-button type="primary" @click="submitForm('ruleForm')"
                  >确定修改</el-button
                >
              </el-form-item>
              <el-form-item v-else>
                <el-button type="primary" @click="dialogFormVisible = false"
                  >确定</el-button
                >
              </el-form-item>
            </el-form>
          </el-dialog>
          <div @click="item.boxshow = true">
            <div v-show="!item.boxshow" class="elsebox py-2">
              <div class="radius">
                <i class="el-icon-caret-bottom"></i>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Search from "./search";
import Titleheader from "./titleheader";
function getRandomIntInclusive(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min; //含最大值，含最小值
}
function getNewArr() {
  var arr = [];
  for (let index = 0; index < getRandomIntInclusive(10, 20); index++) {
    arr.push({
      subj: index + 6000,
      temp: ["小何", "小王", "小杨", "小刘", "小李"][
        index < 5 ? index % 5 : index % 2
      ],
      answer: ["男", "女"][index % 2],
      scan: getRandomIntInclusive(150, 180),
      allocation: getRandomIntInclusive(500, 750),
    });
  }
  return arr;
}
function getNewObj(i) {
  return {
    name: `DEMO 学校2020级${i + 1}班`,
    tagarr: ["高中2020级", ["精品班", "普通班"][i % 2]],
    time: "1 月 7 日",
    tagsite: [0, 1, 2][i % 3],
    tagname: ["未开班", "已开班", "已结束"][i % 3],
    elsetagname: ["暂未授课", "正常授课", "已结束授课"][i % 3],
    tablesite: true,
  };
}
export default {
  name: "cmain",
  data() {
    return {
      total: 20,
      currentPage: 1,
      thepagearr: [4, 1],
      allDataArr: [],
      dataArr: [],
      dialogFormVisible: false,
      ruleForm: {
        name: "",
        region: "",
        sex: "",
        height: "",
        num: "",
      },
      rules: {
        region: [{ required: true, message: "请选择班级", trigger: "change" }],
        name: [
          { required: true, message: "请输入学生名字", trigger: "blur" },
          { min: 2, max: 5, message: "长度在 2 到 5 个字符", trigger: "blur" },
        ],
        sex: [
          { required: true, message: "请输入学生性别", trigger: "blur" },
          { min: 1, max: 1, message: "只能为男或者女", trigger: "blur" },
        ],
        height: [
          { required: true, message: "请输入学生身高", trigger: "blur" },
          { min: 3, max: 5, message: "长度在 3 到 5 个数字", trigger: "blur" },
        ],
        num: [
          { required: true, message: "请输入学生分数", trigger: "blur" },
          { min: 2, max: 5, message: "长度在 2 到 5 个数字", trigger: "blur" },
        ],
      },
      formLabelWidth: "120px",
      arr: Array.from({ length: 20 }, (v, k) => k + 1 + "班"),
      editbool: false,
    };
  },
  components: { Search, Titleheader },
  created() {
    for (let i = 0; i < this.total; i++) {
      this.allDataArr.push({
        id: i,
        boxshow: true,
        theObj: getNewObj(i),
        tableData: getNewArr(),
      });
    }

    this.changepage(this.thepagearr[0], this.thepagearr[1]);
  },
  methods: {
    changepage(a, b) {
      this.dataArr = [];
      let num = a * b;
      for (let index = num - a; index < num; index++) {
        this.dataArr.push(this.allDataArr[index]);
      }
    },
    allboxshow() {
      this.dataArr.forEach(function (el) {
        el.boxshow = false;
      });
    },
    handleSizeChange(val) {
      this.thepagearr[0] = val;
      this.changepage(this.thepagearr[0], this.thepagearr[1]);
    },
    handleCurrentChange(val) {
      this.thepagearr[1] = val;
      this.changepage(this.thepagearr[0], this.thepagearr[1]);
    },
    handleView(row, e) {
      this.ruleForm = {
        name: row.temp,
        region: row.subj,
        sex: row.answer,
        height: row.scan,
        num: row.allocation,
      };
      if (e == 0) {
        this.editbool = false;
        this.name = "";
      } else {
        this.editbool = true;
      }
      this.dialogFormVisible = true;
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.dialogFormVisible = false;
          this.$message({
            message: "修改成功",
            type: "success",
          });
        } else {
          this.$message.error("请核对错误信息");
          return false;
        }
      });
    },
    handleDelate(i, e) {
      this.$confirm("此操作将永久删除该学生, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.allDataArr.forEach((el) => {
            if (el.id == e) {
              this.allDataArr[e].tableData.splice(i, 1);
            }
          });
          this.$message({
            type: "success",
            message: "删除成功!",
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    },
    getMsgFormSon(data) {
      this.allDataArr.forEach((el) => {
        if (el.id == data.region) {
          el.tableData.push({
            subj: el.tableData.length + 6000,
            temp: data.name,
            answer: data.sex,
            scan: data.height,
            allocation: data.num,
          });
        }
      });
    },
  },
};
</script>
<style scoped lang='scss'>
.cmain {
  width: 1200px;
}
.btnarr {
  span {
    color: gray;
  }
}
.flor {
  color: rgb(97, 97, 97);
  cursor: pointer;
}
.elsebox {
  border: 1px solid gainsboro;
  font-size: 20px;
  cursor: pointer;
  .radius {
    margin: 0 auto;
    color: rgb(255, 255, 255);
    background-color: rgb(212, 212, 212);
    width: 25px;
    height: 25px;
    text-align: center;
    line-height: 25px;
    border-radius: 50%;
  }
}
.card {
  .formbox {
    .pubrad {
      margin: 0 auto;
      width: 15px;
      height: 15px;
      text-align: center;
      line-height: 15px;
      border-radius: 50%;
      color: white;
      font-size: 12px;
    }
    .green {
      background-color: #0baa8e;
    }
    .gray {
      background-color: #dddddd;
    }
  }
}
</style>
