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
      <a v-on:click="!isSorting && !isSorted ? heapSort() : ''">Heap sort</a>
      <a v-on:click="!isSorting && !isSorted ? mergeSort() : ''">Merge sort</a>
      <a v-on:click="!isSorting && !isSorted ? quickSort() : ''">Quick sort</a>
      <a v-on:click="!isSorting && !isSorted ? radixSort() : ''">Radix sort</a>
      <a v-on:click="!isSorting && !isSorted ? gnomeSort() : ''">Gnome sort</a>
      <a v-on:click="!isSorting && !isSorted ? cocktailShakerSort() : ''"
        >Cocktail shaker sort</a
      >
      <a v-on:click="!isSorting && !isSorted ? oddEvenSort() : ''"
        >Odd-Even sort</a
      >
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
      let list = [];
      for (let i = 0; i < this.listSize; i++) {
        let numberToAdd = 1 + Math.floor(Math.random() * size);
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
      for (let i = 0; i < this.listSize - 1; i++) {
        let min = i;
        for (let j = i + 1; j < this.listSize; j++) {
          if (this.lineList[j] < this.lineList[min]) {
            min = j;
          }
        }

        if (min != i) {
          let temp = this.lineList[i];
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
      let i = 1;
      while (i < this.listSize) {
        let j = i;
        while (j > 0 && this.lineList[j - 1] > this.lineList[j]) {
          let temp = this.lineList[j];
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
    async heapify(n, node) {
      let largest = node;
      let l = 2 * node + 1;
      let r = 2 * node + 2;

      if (l < n && this.lineList[l] > this.lineList[largest]) {
        largest = l;
      }

      if (r < n && this.lineList[r] > this.lineList[largest]) {
        largest = r;
      }

      if (largest != node) {
        let temp = this.lineList[node];
        this.lineList[node] = this.lineList[largest];
        this.lineList[largest] = temp;
      }

      this.heapify(n, largest);
    },
    async heapSort() {
      this.isSorting = true;
      for (let i = Math.floor(this.listSize / 2) - 1; i >= 0; i--) {
        this.heapify(this.listSize, i);
        await this.sleep();
      }

      for (let i = this.listSize - 1; i > 0; i--) {
        let temp = this.lineList[0];
        this.lineList[0] = this.lineList[i];
        this.lineList[i] = temp;

        this.heapify(i, 0);
        await this.sleep();
      }
      this.isSorting = false;
      this.isSorted = true;
    },
    async bubbleSort() {
      this.isSorting = true;
      for (let i = 0; i < this.listSize - 1; i++) {
        for (let j = 0; j < this.listSize - i - 1; j++) {
          if (this.lineList[j] > this.lineList[j + 1]) {
            let temp = this.lineList[j];
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
      let stack = [];

      stack.push(0);
      stack.push(this.listSize - 1);

      while (stack[stack.length - 1] >= 0) {
        let end = stack.pop();
        let start = stack.pop();

        let pivotIndex = await this.partition(this.lineList, start, end);

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
      let m = l + parseInt((r - l) / 2);

      await this.mergeSortRec(l, m);
      await this.mergeSortRec(m + 1, r);
      await this.merge(l, m, r);
    },
    async merge(l, m, r) {
      let n1 = m - l + 1;
      let n2 = r - m;

      let L = new Array(n1);
      let R = new Array(n2);

      for (let i = 0; i < n1; i++) {
        L[i] = this.lineList[l + i];
      }
      for (let i = 0; i < n2; i++) {
        R[i] = this.lineList[m + 1 + i];
      }

      let i = 0;
      let j = 0;
      let k = l;

      while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
          this.lineList[k] = L[i];
          i++;
        } else {
          this.lineList[k] = R[j];
          j++;
        }
        k++;
        await this.sleep();
      }

      while (i < n1) {
        this.lineList[k] = L[i];
        i++;
        k++;
      }

      while (j < n2) {
        this.lineList[k] = R[j];
        j++;
        k++;
      }
    },
    // Credits to: https://www.geeksforgeeks.org/radix-sort/
    async countSort(exp) {
      let out = new Array(this.listSize);
      let count = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0];

      for (let i = 0; i < this.listSize; i++) {
        count[Math.floor(this.lineList[i] / exp) % 2]++;
      }

      for (let i = 1; i < 10; i++) {
        count[i] += count[i - 1];
      }

      for (let i = this.listSize - 1; i >= 0; i--) {
        out[count[Math.floor(this.lineList[i] / exp) % 2] - 1] =
          this.lineList[i];
        count[Math.floor(this.lineList[i] / exp) % 2]--;
      }

      for (let i = 0; i < this.listSize; i++) {
        this.lineList[i] = out[i];
        await this.sleep();
      }
    },
    async radixSort() {
      this.isSorting = true;
      let max = this.listSize;
      for (let exp = 1; Math.floor(max / exp) > 0; exp *= 2) {
        await this.countSort(exp);
        await this.sleep();
      }
      this.isSorting = false;
      this.isSorted = true;
    },
    async gnomeSort() {
      this.isSorting = true;
      let i = 0;
      while (i < this.listSize) {
        if (i == 0 || this.lineList[i] >= this.lineList[i - 1]) {
          i++;
        } else {
          let temp = this.lineList[i];
          this.lineList[i] = this.lineList[i - 1];
          this.lineList[i - 1] = temp;
          i--;
          await this.sleep();
        }
      }
      this.isSorting = false;
      this.isSorted = true;
    },
    async cocktailShakerSort() {
      this.isSorting = true;
      let swapped = true;
      let start = 0;
      let end = this.listSize;
      while (swapped) {
        swapped = false;
        for (let i = 0; i < end - 1; ++i) {
          if (this.lineList[i] > this.lineList[i + 1]) {
            let temp = this.lineList[i];
            this.lineList[i] = this.lineList[i + 1];
            this.lineList[i + 1] = temp;
            swapped = true;
            await this.sleep();
          }
        }
        if (swapped == false) {
          break;
        }

        end--;

        for (let i = end - 1; i >= start; i--) {
          if (this.lineList[i] > this.lineList[i + 1]) {
            let temp = this.lineList[i];
            this.lineList[i] = this.lineList[i + 1];
            this.lineList[i + 1] = temp;
            swapped = true;
            await this.sleep();
          }
        }

        start++;
      }
      this.isSorting = false;
      this.isSorted = true;
    },
    async oddEvenSort() {
      this.isSorting = true;

      let sorted = false;

      while (!sorted) {
        sorted = true;
        for (let i = 1; i < this.listSize - 1; i += 2) {
          if (this.lineList[i] > this.lineList[i + 1]) {
            let temp = this.lineList[i];
            this.lineList[i] = this.lineList[i + 1];
            this.lineList[i + 1] = temp;
            sorted = false;
            await this.sleep();
          }
        }
        for (let i = 0; i < this.listSize - 1; i += 2) {
          if (this.lineList[i] > this.lineList[i + 1]) {
            let temp = this.lineList[i];
            this.lineList[i] = this.lineList[i + 1];
            this.lineList[i + 1] = temp;
            sorted = false;
            await this.sleep();
          }
        }
        await this.sleep();
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
