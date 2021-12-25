<template>
  <div class="line">
  </div>
</template>

<script>
export default {
  name: "Line",
  data() {
    return {};
  },
  props: {
    height: Number,
  },
};
</script>

<style lang="scss" scoped>
.line {
  background-color: #e5e9f0;
  width: 10px;
  border-left: 2px solid;
  border-right:2px solid;
  border-color: grey;
  //margin-left: 2px;
  //margin-right: 2px;
}
</style>
