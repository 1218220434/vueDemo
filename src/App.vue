<template>
  <div id="app" class="main">
    <Header />
    <div class="d-flex"><Cmenu /><Cmain /></div>
    <el-backtop><i class="el-icon-caret-top"></i></el-backtop>
  </div>
</template>

<script>
import Header from "./components/header.vue";
import Cmenu from "./components/cmenu.vue";

import Cmain from "./components/cmain.vue";

export default {
  name: "App",
  components: {
    Header,
    Cmenu,
    Cmain,
  },
};
</script>

<style lang="scss">
body {
  font-size: 16px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.goTop {
  height: 100vh;
  overflow-x: hidden;
}
</style>
