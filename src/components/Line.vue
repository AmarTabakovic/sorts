<template>
  <div class="line"></div>
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
  background-color: var(--color-line);
  width: 5px;
  border-left: 2px solid;
  border-right: 2px solid;
  border-color: var(--color-line-list);
}
</style>
