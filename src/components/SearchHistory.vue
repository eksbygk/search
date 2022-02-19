<template>
  <div class="container">
    <ul>
      <li
        v-for="(item, index) in historyList"
        :key="index"
        :class="[historyIndex === index ? 'active' : '']"
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
  created() {
    this.$emit("historyLength", this.historyList.length)
  },
  computed: {
    parentIndex: {
      get: function () {
        return this.historyIndex
      },
      set: function (value) {
        this.$emit("setHistoryIndex", value)
      }
    },
    historyLength: {
      get() {
        return this.historyList.length - 1
      },
      set(value) {
        this.$emit("historyLength", value + 1)
      }
    },
    // historyLength() {
    //   return this.historyList.length
    // },
  },
  methods: {
    activeItem(index) {
      this.parentIndex = index
    },
  }
}
</script>

<style lang="less" scoped>
@import '../scss/list';
</style>
