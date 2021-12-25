<template>
    <h1>Sorts</h1>
  <line-list />
</template>

<script>
import LineList from "./components/LineList.vue";

export default {
  name: "App",
  components: {
    LineList,
  },
};
</script>

<style lang="scss">
body {
  background-color: #2e3440;
}

#app {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #e5e9f0;
  margin-top: 60px;
}
</style>
