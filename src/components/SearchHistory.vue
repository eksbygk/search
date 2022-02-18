<template>
  <div class="history">
    <ul>
      <li
        v-for="(item, index) in historyList"
        :key="index"
        :class="[historyIndex === index + 1 ? 'active' : '']"
      >
        {{ item }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "SearchHistory",
  props: ["searchText", "historyIndex"],
  data() {
    return {
      historyList: [1, 2, 3, 4, 5, 6, 7],
    }
  },
}
</script>

<style lang="less" scoped>
.history {
  width: 282px;
  ul {
    border: 1px solid #57606a;
    border-top: 0;
    border-bottom: 0;
    border-radius: 0 0 6px 6px;
    li {
      background-color: #fff;
      // border: 1px solid #d0d7de;
    }

    .active {
      color: #fff;
      background-color: #0969da;
    }
  }
}
</style>
