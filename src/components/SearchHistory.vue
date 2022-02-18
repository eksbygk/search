<template>
  <div class="history">
    <ul>
      <li
        v-for="(item, index) in historyList"
        :key="index"
        :class="[historyIndex === index + 1 ? 'active' : '']"
        @mouseover="activeItem(index)"
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
      historyList: ["1好好", "ooo2", "哦哦哦", "从前有座山", "Hello World", "帕拉", 900],
    }
  },
  computed: {
    parentIndex: {
      get: function () {
        return this.historyIndex
      },
      set: function (value) {
        this.$emit("setIndex", value);
      }
    }
  },
  methods: {
    activeItem(index) {
      this.parentIndex = index + 1
    },
  }
}
</script>

<style lang="less" scoped>
.history {
  width: 362px;
  color: #57606a;
  font-size: 12px;
  ul {
    overflow: hidden;
    border: 1px solid #57606a;
    border-top: 0;
    border-bottom: 0;
    border-radius: 0 0 6px 6px;
    li {
      padding-left: 10px;
      height: 35px;
      line-height: 35px;
      background-color: #fff;
      border-bottom: 1px solid #d0d7de;
      cursor: pointer;
    }
    li:nth-last-child(1) {
      border-bottom: 0;
    }
    .active {
      color: #fff;
      background-color: #0969da;
    }
  }
}
</style>
