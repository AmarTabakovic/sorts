<template>
  <div id="line" :style="cssVariables">
    {{ height }}
  </div>
</template>

<script>
export default {
  name: "Line",
  data() {
    return {};
  },
  computed: {
    cssVariables() {
      return {
        "--height": this.height,
      };
    },
  },
  props: {
    height: Number,
  },
};
</script>

<style lang="scss" scoped>
#line {
  background-color: white;
  height: calc(var(--height) * 14px);
  width: 20px;
  margin-left: 2px;
  margin-right: 2px;
}
</style>
