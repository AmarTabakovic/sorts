<template>
  <div id="line-list">
    <my-line
      v-for="index in lineList"
      :height="index"
      :key="index"
      v-model="lineList[index]"
    >
      {{ index }}
    </my-line>
  </div>
  <p v-on:click="selectionSort">Selection sort</p>
</template>

<script>
import MyLine from "./Line.vue";

export default {
  name: "LineList",
  components: {
    MyLine,
  },
  data() {
    return {
      listSize: 42,
    };
  },
  computed: {
    lineList: function () {
      return this.randomizeArray();
    },
  },
  methods: {
    randomizeArray() {
      var list = [];
      for (var i = 0; i < this.listSize; i++) {
        var numberToAdd = 1 + Math.floor(Math.random() * this.listSize);
        while (list.includes(numberToAdd)) {
          numberToAdd = 1 + Math.floor(Math.random() * this.listSize);
        }
        list.push(numberToAdd);
      }
      return list;
    },
    selectionSort() {
      for (var i = 0; i < this.listSize - 1; i++) {
        var min = i;
        for (var j = i + 1; j < this.listSize; j++) {
          if (this.lineList[j] < this.lineList[min]) {
            min = j;
          }
        }

        if (min != i) {
          var temp = this.lineList[i];
          this.lineList[i] = this.lineList[min];
          this.lineList[min] = temp;
        }

        console.log("Selection sort");
        console.log(this.lineList);
      }
    },
  },
};
</script>

<style scoped lang="scss">
#line-list {
  height: 600px;
  width: 1000px;
  background-color: grey;
  display: flex;
  flex-direction: row;
  align-items: flex-end;
}
</style>
