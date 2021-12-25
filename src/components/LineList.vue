<template>
  <div id="line-list">
    <my-line
      v-for="(line, index) in lineList"
      :height="line"
      :key="index"
      :style="{ height: `${line * 5.3}px` }"
    >
    </my-line>
  </div>
  <div id="links-list">
    <div id="algorithms-list">
      <a v-on:click="selectionSort">Selection sort</a>
      <a v-on:click="insertionSort">Insertion sort</a>
      <a v-on:click="bubbleSort">Bubble sort</a>
      <a v-on:click="quickSort">Quick sort</a>
      <a v-on:click="mergeSort(0, listSize - 1)">Merge sort</a>
      <!------ Work in Progress ------>
      <!--<a v-on:click="radixSort">Radix sort</a>-->
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
      listSize: 111,
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
    async insertionSort() {
      var i = 1;
      while (i < this.listSize) {
        var j = i;
        while (j > 0 && this.lineList[j - 1] > this.lineList[j]) {
          var temp = this.lineList[j];
          this.lineList[j] = this.lineList[j - 1];
          this.lineList[j - 1] = temp;
          j--;
        }
        i++;
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
    // Credits to: https://stackabuse.com/quicksort-in-javascript/
    async quickSort() {
      var stack = [];

      stack.push(0);
      stack.push(this.listSize - 1);

      while (stack[stack.length - 1] >= 0) {
        var end = stack.pop();
        var start = stack.pop();

        var pivotIndex = await this.partition(this.lineList, start, end);

        if (pivotIndex - 1 > start) {
          stack.push(start);
          stack.push(pivotIndex - 1);
        }

        if (pivotIndex + 1 < end) {
          stack.push(pivotIndex + 1);
          stack.push(end);
        }
      }
    },
    async partition(arr, start, end) {
      const pivotValue = arr[end];
      let pivotIndex = start;
      for (let i = start; i < end; i++) {
        if (arr[i] < pivotValue) {
          [arr[i], arr[pivotIndex]] = [arr[pivotIndex], arr[i]];
          pivotIndex++;
          await this.sleep();
        }
      }

      [arr[pivotIndex], arr[end]] = [arr[end], arr[pivotIndex]];
      return pivotIndex;
    },
    async mergeSort(l, r) {
      if (l >= r) {
        return;
      }
      var m = l + parseInt((r - l) / 2);

      await this.mergeSort(l, m);
      await this.mergeSort(m + 1, r);
      await this.merge(l, m, r);
    },
    // Credits to: https://www.geeksforgeeks.org/merge-sort/
    async merge(l, m, r) {
      var n1 = m - l + 1;
      var n2 = r - m;

      var L = new Array(n1);
      var R = new Array(n2);

      for (var i = 0; i < n1; i++) {
        L[i] = this.lineList[l + i];
      }
      for (var j = 0; j < n2; j++) {
        R[j] = this.lineList[m + 1 + j];
      }

      var x = 0;
      var y = 0;
      var k = l;

      while (x < n1 && y < n2) {
        if (L[x] <= R[y]) {
          this.lineList[k] = L[x];
          x++;
        } else {
          this.lineList[k] = R[y];
          y++;
        }
        k++;
        await this.sleep();
      }

      while (x < n1) {
        this.lineList[k] = L[x];
        x++;
        k++;
      }

      while (y < n2) {
        this.lineList[k] = R[y];
        y++;
        k++;
      }
    },
    async returnMax() {
      var max = this.lineList[0];
      for (var i = 1; i < this.listSize; i++) {
        if (this.listSize[i] > max) {
          max = this.listSize[i];
        }
      }
      return max;
    },
    countSort(exp) {
      var out = [];
      var count = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0];

      for (var i = 0; i < this.listSize; i++) {
        count[(this.lineList[i] / exp) % 10]++;
      }

      for (var j = 1; j < 10; j++) {
        count[j] += count[j - 1];
      }

      for (var k = this.listSize - 1; k >= 0; k--) {
        out[count[(this.lineList[k] / exp) % 10] - 1] = this.lineList[k];
        count[(this.lineList[k] / exp) % 10]--;
      }

      for (var l = 0; l < this.listSize; l++) {
        this.lineList[l] = out[l];
      }
    },
    async radixSort() {
      for (var exp = 1; this.returnMax() / exp > 0; exp *= 10) {
        this.countSort(exp);
        await this.sleep();
      }
    },
    randomizeArray() {
      this.lineList = this.returnRandomizedArray(this.listSize);
    },
    sleep() {
      return new Promise((resolve) => setTimeout(resolve, 25));
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
  //justify-content: space-between;
  flex-wrap: wrap;

  * {
    flex: 0 0 33.333333%;
    margin-bottom: 25px;
  }
}

a {
  color: #d8dee9;
  &:hover {
    cursor: pointer;
  }
}
</style>
