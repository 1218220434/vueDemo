<template>
  <div class="hello">
    <div class="d-flex jc-between ai-center line px-5 py-3">
      <div class="d-flex jc-between ai-center logo">
        <span> </span>
        <p>笔试 DEMO</p>
      </div>
      <div class="d-flex jc-between ai-center user">
        <span>|</span>
        <el-dropdown>
          <div class="the-user">张</div>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>个人信息</el-dropdown-item>
            <el-dropdown-item divided>退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "vdemo",
};
</script>

<style scoped lang="scss">
.el-menu.el-menu--horizontal {
  border: none;
}
.el-menu--horizontal > .el-menu-item.is-active {
  border-bottom: 2px solid #10c694;
  color: #303133;
}
.logo {
  span {
    width: 30px;
    height: 30px;
    margin-right: 10px;
    border-radius: 50%;
    background-image: linear-gradient(to right, #56ab2f, #a8e063);
  }
  p {
    font-size: 18px;
    color: rgb(250, 250, 250);
  }
}
.line {
  background-color: #263238;
  border-bottom: 1px solid gainsboro;
}
.user {
  span {
    color: white;
    font-size: 20px;
    margin-right: 20px;
  }
  .the-user {
    cursor: pointer;
    width: 30px;
    height: 30px;
    text-align: center;
    line-height: 30px;
    color: #aaaaaa;
    background-color: #eeeeee;
    border-radius: 50%;
  }
}
</style>
