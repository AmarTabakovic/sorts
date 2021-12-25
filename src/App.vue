<template>
  <div id="header">
    <dark-theme-switcher></dark-theme-switcher>
  </div>
  <div id="main">
    <h1>
      <a href=""> Sorts</a>
    </h1>

    <line-list />
  </div>
  <div id="footer">
    <p id="disclaimer">{{ new Date().getFullYear() }} Amar Tabakovic</p>
  </div>
</template>

<script>
import LineList from "./components/LineList.vue";
import DarkThemeSwitcher from "./components/DarkThemeSwitcher.vue";

export default {
  name: "App",
  components: {
    LineList,
    DarkThemeSwitcher,
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Inter&display=swap");
@import "./css/variables.css";

body {
  background-color: var(--color-background);
}

#header {
  text-align: right;
}

#app {
  font-family: Inter, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: var(--color-font);
  margin: 60px 40px;
}

#main {
  display: flex;
  flex-direction: column;
  align-items: center;
}

h1 {
  color: var(--color-font);
  font-size: 27pt;

  a {
    list-style: none;
    text-decoration: none;
    color: inherit;
  }
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
