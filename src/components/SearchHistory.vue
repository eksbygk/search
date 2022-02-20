<template>
  <div class="container">
    <ul>
      <li
        v-for="(item, index) in historyList"
        :key="index"
        :class="[listIndex === index ? 'active' : '']"
        @mouseover="activeItem(index)"
        @click="jumpToSearch(item)"
      >
        {{ item }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "SearchHistory",
  props: {
    historyList: Array,
    historyIndex: Number,
  },
  computed: {
    listIndex: {
      get: function () {
        return this.historyIndex - 1
      },
      set: function (value) {
        this.$emit("setHistoryIndex", value + 1)
      }
    },
  },
  methods: {
    activeItem(index) {
      this.listIndex = index
    },
    jumpToSearch(item) {
      this.$emit("jumpToSearch", item)
    },
  }
}
</script>

<style lang="less" scoped>
@import '../scss/list';
</style>
