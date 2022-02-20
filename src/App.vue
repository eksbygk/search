<template>
  <div id="app">
    <Search @search="goSearch"></Search>

    <div class="show">
      {{ searchData }}
    </div>
  </div>
</template>

<script>
import Search from "./components/Search.vue"

export default {
  name: "App",
  components: {
    Search,
  },
  data() {
    return {
      searchData: '',
    }
  },
  methods: {
    goSearch(data) {
      this.searchData = data
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
body {
  background-color: #24292f;
}
li {
  list-style: none;
}
.show {
  padding: 50px;
  width: 100%;
  font-size: 28px;
  text-align: center;
  color: #fff;
}
</style>
