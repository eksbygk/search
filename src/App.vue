<template>
  <div id="app">
    <Search></Search>
  </div>
</template>

<script>
import Search from "./components/Search.vue"

export default {
  name: "App",
  components: {
    Search,
  },
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
</style>
