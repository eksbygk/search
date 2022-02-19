<template>
  <div class="search-container" @click.stop="">
    <input
      type="text"
      v-model="searchText"
      @click="resetIndex"
      @keydown.up="selectItemUp"
      @keydown.down="selectItemDown"
      @focus="ShowHistoryOrScope"
      @blur="hide"
      @input="toggleShow"
    />
    <!-- <button>/</button> -->

    <SearchHistory
      v-show="isShowHistory"
      :searchText="searchText"
      :historyIndex="historyIndex"
      @setHistoryIndex="setHistoryIndex"
      @historyLength="historyLength"
    ></SearchHistory>

    <SearchScope
      v-show="isShowScope"
      :searchText="searchText"
      :scopeIndex="scopeIndex"
      @setScopeIndex="setScopeIndex"
    ></SearchScope>

    <!-- 展示结果 -->
    <!-- <div class="showResults">{{}}</div>s -->
  </div>
</template>

<script>
import SearchHistory from "./SearchHistory.vue"
import SearchScope from "./SearchScope.vue"

export default {
  name: "Search",
  components: {
    SearchHistory,
    SearchScope,
  },
  data() {
    return {
      searchText: "",
      // index 0-7
      historyIndex: 0,
      scopeIndex: 0,
      isShowHistory: false,
      isShowScope: false,
      historyListLength: 0,
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
      if (this.isShowHistory) {
        this.historyReduce()
      }
      if (this.isShowScope) {
        this.scopeReduce()
      }
    },
    selectItemDown() {
      if (this.isShowHistory) {
        this.historyAdd()
      }
      if (this.isShowScope) {
        this.scopeAdd()
      }
    },
    historyAdd() {
      if (this.historyIndex >= this.historyListLength) {
        this.historyIndex = 0
      } else {
        this.historyIndex++
      }
    },
    historyReduce() {
      if (this.historyIndex <= 0) {
        this.historyIndex = this.historyListLength
      } else {
        this.historyIndex--
      }
    },
    scopeAdd() {
      if (this.scopeIndex >= 3) {
        this.scopeIndex = 0
      } else {
        this.scopeIndex++
      }
    },
    scopeReduce() {
      if (this.scopeIndex <= 0) {
        this.scopeIndex = 3
      } else {
        this.scopeIndex--
      }
    },
    ShowHistoryOrScope() {
      this.toggleShow()
    },
    resetIndex() {
      this.historyIndex = 0
      this.scopeIndex = 0
    },
    toggleShow() {
      if (this.searchText === '') {
        this.isShowHistory = true
        this.isShowScope = false
        this.scopeIndex = 0
      } else {
        this.isShowHistory = false
        this.isShowScope = true
        this.historyIndex = 0
      }
    },
    hide() {
      this.isShowHistory = false
      this.isShowScope = false
    },

    //接收---
    setHistoryIndex(data) {
      this.historyIndex = data
    },
    setScopeIndex(data) {
      this.scopeIndex = data
    },
    historyLength(data) {
      this.historyListLength = data
    },
  },
}
</script>

<style lang="less" scoped>
@witch: 450px;
@height: 35px;
.search-container {
  width: @witch;
  margin: 60px auto;
  font-size: 14px;
  .common {
    background-color: transparent;
    border: 1px solid #57606a;
    border-radius: 6px;
    color: #fff;
  }
  input {
    .common();
    padding-left: 10px;
    width: 100%;
    height: @height;
    outline: none;
  }
  input:focus {
    color: #57606a;
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
