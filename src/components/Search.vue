<template>
  <div class="search-container" @click.stop="">
    <input
      type="text"
      v-model="searchText"
      @click="resetIndex"
      @keydown.up="selectItemUp"
      @keydown.down="selectItemDown"
      @focus="ShowHistory"
      @blur="isShowHistory = false"
    />
    <!-- <button>/</button> -->

    <SearchHistory
      v-show="isShowHistory"
      :searchText="searchText"
      :historyIndex="historyIndex"
      @setIndex="setIndex"
    ></SearchHistory>
  </div>
</template>

<script>
import SearchHistory from "./SearchHistory.vue"

export default {
  name: "Search",
  components: {
    SearchHistory,
  },
  data() {
    return {
      searchText: "",
      // index 0-7
      historyIndex: 0,
      scopeIndex: 0,
      isShowHistory: false,
      isShowScope: false,
    }
  },
  mounted() {
    window.addEventListener("click", () => {
      this.isShowHistory = false
      this.isShowScope = false
    })
  },
  methods: {
    selectItemUp() {
      this.historyReduce()
    },
    selectItemDown() {
      this.historyAdd()
    },
    historyAdd() {
      if (this.historyIndex >= 7) {
        this.historyIndex = 0
      } else {
        this.historyIndex++
      }
    },
    historyReduce() {
      if (this.historyIndex <= 0) {
        this.historyIndex = 7
      } else {
        this.historyIndex--
      }
    },
    scopeAdd() { },
    scopeReduce() { },
    ShowHistory() {
      this.isShowHistory = true
    },
    resetIndex() {
      this.historyIndex = 0
    },
    //接收---
    setIndex(data) {
      this.historyIndex = data
    }
  },
}
</script>

<style lang="less" scoped>
@witch: 350px;
.search-container {
  width: @witch;
  margin: 60px auto;
  .common {
    background-color: transparent;
    border: 1px solid #57606a;
    border-radius: 6px;
    color: #57606a;
  }
  input {
    .common();
    padding-left: 10px;
    width: 100%;
    height: 30px;
    outline: none;
  }
  input:focus {
    background-color: #f6f8fa;
    border-radius: 6px 6px 0 0;
  }
  button {
    .common();
    // margin: 10px;
    height: 26px;
    width: 26px;
  }
}
</style>
