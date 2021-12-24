<template>
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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
