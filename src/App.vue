<template>
  <h1>Sorts</h1>
  <line-list />
  <div id="footer">
    <p id="disclaimer">2021 Amar Tabakovic</p>
  </div>
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
@import url("https://fonts.googleapis.com/css2?family=Inter&display=swap");

body {
  background-color: #2e3440;
}

#app {
  //display: flex;
  //flex-direction: column;
  //align-items: center;
  font-family: Inter, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #e5e9f0;
  margin: 60px 20px;
}

h1 {
  color: white;
  font-size: 27pt;
}

#footer {
  text-align: center;
  margin-left: -40px;
  width: 100%;
  position: fixed;
  bottom: 0;
  background-color: var(--color-background);
}

#disclaimer {
  font-size: 9pt;
}
</style>
