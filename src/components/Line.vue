<template>
  <div class="line" />
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
  border-left: 1px solid;
  border-right: 1px solid;
  border-color: var(--color-line-list);
}
</style>
