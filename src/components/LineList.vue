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
      <a v-on:click="!isSorting && !isSorted ? bubbleSort() : ''"
        >Bubble sort</a
      >
      <a v-on:click="!isSorting && !isSorted ? selectionSort() : ''"
        >Selection sort</a
      >
      <a v-on:click="!isSorting && !isSorted ? insertionSort() : ''"
        >Insertion sort</a
      >
      <a v-on:click="!isSorting && !isSorted ? mergeSort() : ''">Merge sort</a>
      <a v-on:click="!isSorting && !isSorted ? quickSort() : ''">Quick sort</a>
      <!------ Work in Progress ------>
      <a v-on:click="!isSorting && !isSorted ? radixSort() : ''">Radix sort</a>

      <a v-on:click="!isSorting && !isSorted ? gnomeSort() : ''">Gnome sort</a>
    </div>
    <a v-on:click="!isSorting ? randomizeArray() : ''">Randomize array</a>
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
      isSorted: false,
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
      this.isSorted = false;
      return list;
    },
    async selectionSort() {
      this.isSorting = true;
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
      this.isSorting = false;
      this.isSorted = true;
    },
    async insertionSort() {
      this.isSorting = true;
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
      this.isSorting = false;
      this.isSorted = true;
    },
    async bubbleSort() {
      this.isSorting = true;
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
      this.isSorting = false;
      this.isSorted = true;
    },
    // Credits to: https://stackabuse.com/quicksort-in-javascript/
    async quickSort() {
      this.isSorting = true;
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
      this.isSorting = false;
      this.isSorted = true;
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
    // Credits to: https://www.geeksforgeeks.org/merge-sort/
    async mergeSort() {
      this.isSorting = true;
      await this.mergeSortRec(0, this.listSize - 1);
      this.isSorting = false;
      this.isSorted = true;
    },
    async mergeSortRec(l, r) {
      if (l >= r) {
        return;
      }
      var m = l + parseInt((r - l) / 2);

      await this.mergeSortRec(l, m);
      await this.mergeSortRec(m + 1, r);
      await this.merge(l, m, r);
    },
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
    // Credits to: https://www.geeksforgeeks.org/radix-sort/
    async countSort(exp) {
      var i;
      var out = new Array(this.listSize);
      var count = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0];

      for (i = 0; i < this.listSize; i++) {
        count[Math.floor(this.lineList[i] / exp) % 10]++;
      }

      for (i = 1; i < 10; i++) {
        count[i] += count[i - 1];
      }

      for (i = this.listSize - 1; i >= 0; i--) {
        out[count[Math.floor(this.lineList[i] / exp) % 10] - 1] =
          this.lineList[i];
        count[Math.floor(this.lineList[i] / exp) % 10]--;
      }

      for (i = 0; i < this.listSize; i++) {
        this.lineList[i] = out[i];
        await this.sleep();
      }
    },
    async radixSort() {
      this.isSorting = true;
      var max = this.listSize;
      for (var exp = 1; Math.floor(max / exp) > 0; exp *= 10) {
        await this.countSort(exp);
        await this.sleep();
      }
      this.isSorting = false;
      this.isSorted = true;
    },
    async gnomeSort() {
      this.isSorting = true;
      var i = 0;
      while (i < this.listSize) {
        if (i == 0 || this.lineList[i] >= this.lineList[i - 1]) {
          i++;
        } else {
          var temp = this.lineList[i];
          this.lineList[i] = this.lineList[i - 1];
          this.lineList[i - 1] = temp;
          i--;
          await this.sleep();
        }
      }
      this.isSorting = false;
      this.isSorted = true;
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
  background-color: var(--color-line-list);
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
  flex-wrap: wrap;

  * {
    flex: 0 0 33.333333%;
    margin-bottom: 25px;
  }
}

a {
  color: var(--color-font);
  &:hover {
    cursor: pointer;
  }
}
</style>
