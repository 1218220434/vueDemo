<template>
  <div class="titleheader">
    <div class="headertitle px-3 py-3" :class="{ bob: !theObj.tablesite }">
      <div class="title d-flex jc-between ai-center my-2">
        <div class="maintitle">{{ theObj.name }}</div>
        <div class="d-flex">
          <div
            class="badge mr-1"
            v-for="(item, index) in theObj.tagarr"
            :key="index"
          >
            {{ item }}
          </div>
        </div>
        <div class="time">{{ theObj.time }}</div>
      </div>
      <div class="site d-flex ai-center">
        <div
          class="tag mr-3"
          :class="{
            yellow: theObj.tagsite == 0,
            green: theObj.tagsite == 1,
            grey: theObj.tagsite == 2,
          }"
        >
          {{ theObj.tagname }}
        </div>
        <div class="tagelse">{{ theObj.elsetagname }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "titleheader",
  data() {
    return {};
  },
  props: {
    theObj: Object,
  },
};
</script>
<style scoped lang='scss'>
.headertitle {
  border-left: 1px solid gainsboro;
  border-right: 1px solid gainsboro;
  border-top: 1px solid gainsboro;
  .maintitle {
    min-width: 600px;
    font-weight: 600;
    font-size: 18px;
  }
  .badge {
    background-color: #e6e6e4;
    padding: 2px 4px;
    border-radius: 3px;
  }
  .site {
    .tag {
      padding: 2px 8px;
      border-radius: 3px;
      color: white;
    }
    .yellow {
      background-color: #ffab00;
    }
    .green {
      background-color: #0baa8e;
    }
    .grey {
      background-color: #9e9e9e;
    }
    .tagelse {
      color: rgb(177, 177, 177);
    }
  }
}
.bob {
  border-bottom: 1px solid gainsboro;
}
</style>
