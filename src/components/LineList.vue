<template>
  <div id="line-list">
    <my-line
      v-for="(line, index) in lineList"
      :height="line"
      :key="index"
      :style="{ height: `${line * 8.3}px` }"
    >
    </my-line>
  </div>
  <div id="links-list">
    <div id="algorithms-list">
      <a v-on:click="selectionSort">Selection sort</a>
      <a v-on:click="selectionSort">Bubble sort</a>
      <a></a>
    </div>
    <a v-on:click="randomizeArray">Randomize array</a>
  </div>
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
      listSize: 72,
      lineList: [],
      isSorting: false,
    };
  },
  created() {
    this.lineList = this.returnRandomizedArray(this.listSize);
  },
  methods: {
    returnRandomizedArray(size) {
      var list = [];
      for (var i = 0; i < this.listSize; i++) {
        var numberToAdd = 1 + Math.floor(Math.random() * size);
        while (list.includes(numberToAdd)) {
          numberToAdd = 1 + Math.floor(Math.random() * size);
        }
        list.push(numberToAdd);
      }
      return list;
    },
    async selectionSort() {
      console.log("Selection sort");
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
        await this.sleep();
      }
    },
    async bubbleSort() {
      for (var i = 0; i < this.listSize - 1; i++) {
        for (var j = 0; j < this.listSize - i - 1; j++) {
          if (this.lineList[j] > this.lineList[j + 1]) {
            var temp = this.lineList[j];
            this.lineList[j] = this.lineList[j + 1];
            this.lineList[j + 1] = temp;
          }
        }
        await this.sleep();
      }
    },
    async quickSort() {},
    randomizeArray() {
      this.lineList = this.returnRandomizedArray(this.listSize);
    },
    sleep() {
      return new Promise((resolve) => setTimeout(resolve, 100));
    },
  },
};
</script>

<style scoped lang="scss">
#line-list {
  margin-top: 50px;
  height: 600px;
  width: 1000px;
  background-color: #434c5e;
  display: flex;
  flex-direction: row;
  align-items: flex-end;
}

#links-list {
  margin-top: 20px;
  width: 1000px;
  display: flex;
  justify-content: space-between;
}

#algorithms-list {
  width: 500px;
  display: flex;
  justify-content: space-between;
}

a {
  color: #d8dee9;
  &:hover {
    cursor: pointer;
  }
}
</style>
