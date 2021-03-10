<template>
  <div class="search">
    <div class="d-flex jc-between ai-center">
      <div class="logo">学生管理</div>

      <el-autocomplete
        size="small"
        popper-class="my-autocomplete"
        v-model="state"
        :fetch-suggestions="querySearch"
        placeholder="请输入内容"
        @select="handleSelect"
      >
        <i
          class="el-icon-caret-bottom el-input__icon"
          slot="suffix"
          @click="handleIconClick"
        >
        </i>
        <i slot="prefix" class="el-input__icon el-icon-search"></i>
        <template slot-scope="{ item }">
          <div class="name">{{ item.value }}</div>
          <span class="addr">{{ item.desc }}</span>
        </template>
      </el-autocomplete>
      <div class="bnt-nol d-flex jc-between ai-center px-3">
        <span @click="dialogFormVisible = true">新建学生</span>
        <i slot="prefix" class="el-input__icon el-icon-plus"></i>
      </div>
    </div>
    <el-dialog
      title="新建学生"
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
        <el-form-item label="班级" prop="region" :label-width="formLabelWidth">
          <el-select v-model="ruleForm.region" placeholder="请选择班级">
            <el-option
              v-for="(item, index) in arr"
              :key="index"
              :label="item"
              :value="index"
            ></el-option>
          </el-select>
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
        <el-form-item label="学生性别" prop="sex" :label-width="formLabelWidth">
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
        <el-form-item label="学生分数" prop="num" :label-width="formLabelWidth">
          <el-input
            v-model="ruleForm.num"
            autocomplete="off"
            placeholder="请填写学生分数"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')"
            >立即创建</el-button
          >
          <el-button @click="resetForm('ruleForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "search",
  data() {
    return {
      input4: "",
      restaurants: [],
      state: "",
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
    };
  },
  created() {},
  mounted() {
    this.restaurants = this.loadAll();
  },
  methods: {
    querySearch(queryString, cb) {
      var restaurants = this.restaurants;
      var results = queryString
        ? restaurants.filter(this.createFilter(queryString))
        : restaurants;
      // 调用 callback 返回建议列表的数据
      cb(results);
    },
    createFilter(queryString) {
      return (restaurant) => {
        return (
          restaurant.value.toLowerCase().indexOf(queryString.toLowerCase()) ===
          0
        );
      };
    },
    loadAll() {
      return [
        { value: "条目1", desc: "描述1" },
        { value: "条目2", desc: "描述2" },
        { value: "条目3", desc: "描述3" },
      ];
    },
    handleSelect(item) {
      console.log(item);
    },
    handleIconClick(ev) {
      console.log(ev);
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.dialogFormVisible = false;
          this.$message({
            message: "创建成功",
            type: "success",
          });
          this.$emit("func", this.ruleForm);
        } else {
          this.$message.error("请核对错误信息");
          return false;
        }
      });
    },
  },
};
</script>
<style scoped lang='scss'>
.my-autocomplete {
  li {
    line-height: normal;
    padding: 7px;

    .name {
      text-overflow: ellipsis;
      overflow: hidden;
    }
    .addr {
      font-size: 12px;
      color: #b4b4b4;
    }

    .highlighted .addr {
      color: #ddd;
    }
  }
}
.logo {
  font-size: 20px;
  font-weight: 400;
}
.bnt-nol {
  cursor: pointer;
  border-radius: 3px;
  background-color: #10c694;
  color: white;
}
.el-input {
  width: 300px;
}
</style>
