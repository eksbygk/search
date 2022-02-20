<template>
  <div class="search-container" @click.stop="">
    <input
      type="text"
      v-model="searchField"
      id="searchInput"
      :class="
        isShowScope || (isShowHistory && historyListLength)
          ? 'focus-radius'
          : 'common-radius'
      "
      @click="resetIndex"
      @keydown.up="selectItemUp"
      @keydown.down="selectItemDown"
      @keyup.enter="goSearch"
      @focus="ShowHistoryOrScope"
      @input="toggleShow"
    />
    <!-- <button>/</button> -->

    <SearchHistory
      v-show="isShowHistory"
      :historyList="historyList"
      :historyIndex="historyIndex"
      @setHistoryIndex="setHistoryIndex"
      @jumpToSearch="clickHistory"
    ></SearchHistory>

    <SearchScope
      v-show="isShowScope"
      :searchField="searchField"
      :scopeIndex="scopeIndex"
      @setScopeIndex="setScopeIndex"
    ></SearchScope>

    <!-- 展示结果 -->
    <!-- <div class="showResults">{{}}</div>s -->
  </div>
</template>

<script>
import SearchHistory from './SearchHistory.vue'
import SearchScope from './SearchScope.vue'

export default {
  name: 'Search',
  components: {
    SearchHistory,
    SearchScope,
  },
  data() {
    return {
      searchField: '',
      // index 0-7
      historyIndex: 0,
      scopeIndex: 0,
      isShowHistory: false,
      isShowScope: false,
      historyList: [],
      historyListLength: 0,
    }
  },
  mounted() {
    window.addEventListener('click', () => {
      this.hide()
    })
    window.addEventListener('blur', () => {
      this.hide()
    })
    this.getSearchHistory()
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
      if (this.searchField === '') {
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

    getSearchHistory() {
      const keywords = localStorage.getItem('keywords')
      const historyList = keywords.split(',')
      this.historyList = historyList
      this.historyListLength = historyList.length
    },
    updateSearchHistoy(data) {
      const searchField = data ? data : this.searchField
      if (searchField !== '') {
        const keywords = localStorage.getItem('keywords')
        let searchHistory
        if (keywords === null || keywords === '') {
          searchHistory = []
        } else {
          searchHistory = keywords.split(',')
        }
        const index = searchHistory.indexOf(searchField)
        if (index >= 0) {
          searchHistory.splice(index, 1)
        }
        if (searchHistory.length >= 7) {
          searchHistory.pop()
        }
        searchHistory.unshift(searchField)
        localStorage.setItem('keywords', searchHistory)
        this.getSearchHistory()
      }
    },
    goSearch() {
      const searchField = this.searchField
      // const searchField = data ? data : this.searchField
      if (searchField === '') {
        location.reload()
      } else {
        // go search
        this.updateSearchHistoy(searchField)
        this.$emit("search", searchField)
        this.searchField = ''
        this.hide()
        document.getElementById('searchInput').blur()
      }
    },

    //接收---
    setHistoryIndex(data) {
      this.historyIndex = data
    },
    setScopeIndex(data) {
      this.scopeIndex = data
    },
    clickHistory(data) {
      this.updateSearchHistoy(data)
      this.searchField = data
      this.goSearch()
      this.hide()
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
    color: #fff;
  }
  .common-radius {
    border-radius: 6px;
  }
  .focus-radius {
    border-radius: 6px 6px 0 0;
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
    // border-radius: 6px 6px 0 0;
  }
  button {
    .common();
    // margin: 10px;
    height: 26px;
    width: 26px;
  }
}
</style>
