# 排序算法
对计算机中存储的数据执行的两种最常见操作是排序和检索，自从计算机产业伊始便是如 此。这也意味着排序和检索在计算机科学中是被研究得最多的操作。本书讨论的许多数据 结构，都对排序和查找算法进行了专门的设计，以使对其中的数据进行操作时更简洁高效。

## 创建数组测试平台
```
class CArray {
    constructor(numElements) {
        this.dataStore = [];
        this.pos = 0;
        this.gaps = [701, 301, 132, 57, 23, 10, 4, 1];
        this.numElements = numElements;
        for (let i = 0; i < numElements; i++) {
            this.dataStore[i] = i;
        }
    }

    setData() {
        for (let i = 0, len = this.numElements; i < len; i++) {
            this.dataStore[i] = Math.floor(Math.random() * (this.numElements + 1));
        }
    }

    clear() {
        for (let i = 0, len = this.numElements; i < len; i++) {
            this.dataStore[i] = 0;
        }
    }

    insert(element) {
        this.dataStore[this.pos++] = element;
    }

    toString() {
        let restr = "";
        for (let i = 0, len = this.dataStore.length; i < len; i++) {
            restr += this.dataStore[i] + " ";
            if (i > 0 & i % 10 === 0) {
                restr += "\n";
            }
        }
        return restr;
    }

    //冒泡排序法
    bubbleSoret() {
        let numElements = this.dataStore.length;
        let temp;
        for (let outer = numElements; outer >= 2; outer--) {
            for (let inner = 0; inner <= outer - 1; inner++) {
                if (this.dataStore[inner] > this.dataStore[inner + 1]) {
                    this.swap(this.dataStore, inner, inner + 1);
                }
            }
        }
    }

    swap(arr, index1, index2) {
        let temp = arr[index1];
        arr[index1] = arr[index2];
        arr[index2] = temp;
    }

    //选择排序法
    selectionStore() {
        let min, temp;
        for (let outer = 0; outer < this.dataStore.length - 2; outer++) {
            min = outer;
            for (let inner = outer + 1; inner <= this.dataStore.length - 1; inner++) {
                if (this.dataStore[inner] < this.dataStore[min]) {
                    min = inner;
                }
                this.swap(this.dataStore, outer, min);
            }
        }
    }

    //插入排序法
    insertionSort() {
        let temp, inner;
        for (let outer = 1; outer <= this.dataStore.length - 1; outer++) {
            temp = this.dataStore[outer];
            inner = outer;
            while (inner > 0 && (this.dataStore[inner - 1] >= temp)) {
                this.dataStore[inner] = this.dataStore[inner - 1];
                inner--;
            }
            this.dataStore[inner] = temp;
        }
    }

    //希尔排序
    shellsort() {
        for (let g = 0, glen = this.gaps.length; g < glen; g++) {
            for (let i = this.gaps[g], len = this.dataStore.length; i < len; i++) {
                let temp = this.dataStore[i];
                let j;
                for (j = i; j >= this.gaps[g] && this.dataStore[j - this.gaps[g]] > temp; j -= this.gaps[g]) {
                    this.dataStore[j] = this.dataStore[j - this.gaps[g]];
                }
                this.dataStore[j] = temp;
            }
            //  console.log(this.dataStore.toString(), this.gaps[g]);
        }
    }
//动态间隔序列希尔排序
    shellsort1() {
        let N = this.dataStore.length;
        let h = 1;
        while (h < N / 3) {
            h = 3 * h + 1;
        }
        while (h >= 1) {
            for (let i = h; i < N; i++) {
                for (let j = i; j >= h && this.dataStore[j] < this.dataStore[j - h]; j -= h) {
                    this.swap(this.dataStore, j, j - h);
                }
            }
            h = (h - 1) / 3;
        }
    }

    setGaps(arr) {
        this.gaps = arr;
    }

    //归并排序
    mergeSort(arr) {
        if (this.dataStore.length < 2) {
            return;
        }
        let step = 1;
        let left, right;
        while (step < this.dataStore.length) {
            left = 0;
            right = step;
            while (right + step <= this.dataStore.length) {
                this.mergeArrays(this.dataStore, left, left + step, right, right + step);
                left = right + step;
                right = left + step;
            }
            if (right < this.dataStore.length) {
                this.mergeArrays(this.dataStore, left, left + step, right, this.dataStore.length);
            }
            step *= 2;
        }
    }

    mergeArrays(arr, startLeft, stopLeft, startRight, stopRight) {
        let rightArr = new Array(stopRight - startRight + 1);
        let leftArr = new Array(stopLeft - startLeft + 1);
        let k = startRight;
        for (let i = 0; i < (rightArr.length - 1); i++) {
            rightArr[i] = arr[k];
            ++k;
        }
        k = startLeft;
        for (let i = 0; i < (leftArr.length - 1); i++) {
            leftArr[i] = arr[k];
            ++k;
        }
        rightArr[rightArr.length - 1] = Infinity;
        leftArr[leftArr.length - 1] = Infinity;
        let m = 0;
        let n = 0;
        for (let k = startLeft; k < stopRight; ++k) {
            if (leftArr[m] <= rightArr[n]) {
                arr[k] = leftArr[m];
                m++;
            } else {
                arr[k] = rightArr[n];
                n++;
            }
        }
        // console.log("left array - ", leftArr);
        //console.log("right arr - ", rightArr);
    }
}
//快速排序
function qSort(arr) {
    if (arr.length == 0) {
        return [];
    }
    let left = [];
    let right = [];
    let pivot = arr[0];
    for (var i = 1; i < arr.length; i++) {
        // console.log(" 基准值:" + pivot + " 当前元素:" + arr[i]);
        if (arr[i] < pivot) {
            //  console.log(" 移动 " + arr[i] + " 到左边 ");
            left.push(arr[i]);
        } else {
            //console.log(" 移动 " + arr[i] + " 到右边 ");
            right.push(arr[i]);
        }
    }
    return qSort(left).concat(pivot, qSort(right));
}
let numElements = 10;
let myNums = new CArray(numElements);
myNums.setData();
console.log(myNums.toString());
myNums.insertionSort();
console.log();
console.log(myNums.toString());
```

### 高级排序
### 希尔排序
```
//希尔排序
let nums = new CArray(10);
nums.setData();
console.log('希尔排序前: \n');
console.log(nums.toString());
console.log('\n 希尔排序中: \n');
nums.shellsort1();
console.log('\n 希尔排序后: \n');
console.log(nums.toString());
```
### 归并排序
```
//归并排序
let nums = new CArray(10);
nums.setData();
console.log(nums.toString());
nums.mergeSort();
console.log(nums.toString());
```

### 快速排序
```
//快速排序
let nums = new CArray(10);
nums.setData();
console.log(qSort(nums.dataStore));
```



### 测试排序效率
```
//排序效率比较
//数据长度100
let numElements = 100;
let nums = new CArray(numElements);
nums.setData();
let start = new Date().getTime();
nums.bubbleSoret();
let stop = new Date().getTime();
let elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行冒泡排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.selectionStore();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行选择排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.insertionSort();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行插入排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.shellsort1();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行动态间隔序列的希尔排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.mergeSort();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行归并排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
qSort(nums.dataStore);
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行快速排序消耗的时间为: " + elapsed + " 毫秒");

//数据长度1000
numElements = 1000;
nums = new CArray(numElements);
nums.setData();
start = new Date().getTime();
nums.bubbleSoret();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行冒泡排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.selectionStore();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行选择排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.insertionSort();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行插入排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.shellsort1();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行动态间隔序列的希尔排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.mergeSort();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行归并排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
qSort(nums.dataStore);
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行快速排序消耗的时间为: " + elapsed + " 毫秒");

//数据长度10000
numElements = 10000;
nums = new CArray(numElements);
nums.setData();
start = new Date().getTime();
nums.bubbleSoret();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行冒泡排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.selectionStore();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行选择排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.insertionSort();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行插入排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.shellsort1();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行动态间隔序列的希尔排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.mergeSort();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行归并排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
qSort(nums.dataStore);
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行快速排序消耗的时间为: " + elapsed + " 毫秒");

//数据长度100000
numElements = 100000;
nums = new CArray(numElements);
nums.setData();
start = new Date().getTime();
nums.bubbleSoret();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行冒泡排序消耗的时间为: " + elapsed + " 毫秒");
numElements = 100000;
nums = new CArray(numElements);
nums.setData();
start = new Date().getTime();
nums.selectionStore();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行选择排序消耗的时间为: " + elapsed + " 毫秒");
numElements = 100000;
nums = new CArray(numElements);
nums.setData();
start = new Date().getTime();
nums.insertionSort();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行插入排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.shellsort1();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行动态间隔序列的希尔排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
nums.mergeSort();
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行归并排序消耗的时间为: " + elapsed + " 毫秒");
nums.clear();
nums.setData();
start = new Date().getTime();
qSort(nums.dataStore);
stop = new Date().getTime();
elapsed = stop - start;
console.log("对 " + numElements + " 个元素执行快速排序消耗的时间为: " + elapsed + " 毫秒");
```

参考自《数据结构与算法 JavasScript描述》