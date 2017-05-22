# 检索算法
作为最基本的计算机编程任务，数据检索已经被研究了很多年。本章只介绍数据检索的一 个方面:如何在列表中查找特定的值。
在列表中查找数据有两种方式:顺序查找和二分查找。顺序查找适用于元素随机排列的列 表;二分查找适用于元素已排序的列表。二分查找效率更高，但是你必须在进行查找之前 花费额外的时间将列表中的元素排序。

## 顺序查找
```
//顺序查找
function seqSearch(arr, data) {
    for(let i = 0, len = arr.length; i < len; i++){
        //console.log(i);
        if(arr[i] === data){
            return i;
        }
    }
    return -1;
}

function disArr(arr) {
    let i,
        str = "";
    for(i = 0; i < arr.length; i++){
        str += arr[i] + " ";
        if(i % 10 === 9){
            console.log(str);
            str = "";
        }
    }
    if(i % 10 != 0){
        console.log("\n");
    }
}
let nums = [];
for(let i = 0; i < 100; i++){
    nums[i] = Math.floor(Math.random() * 101);
}
disArr(nums);
console.log("输入一个要查找的数字: ");
let num = 30;
console.log();
let position = seqSearch(nums, num);
if(position > -1){
    console.log(num + " 在这个数组中的索引位置是 " + position);
}else{
    console.log(num + " 没有出现在这个数组中。");
}
console.log();
disArr(nums);
```

## 查找最小值和最大值
```
//查找最大值和最小值
function findMin(arr) {
    let min = arr[0];
    for(let i = 1; i < arr.length; i++){
        if(arr[i] < min){
            min = arr[i];
        }
    }
    return min;
}
function findMax(arr) {
    let max = arr[0];
    for(let i = 1; i < arr.length; i++){
        if(arr[i] > max){
            max = arr[i];
        }
    }
    return max;
}

let nums = [];
for(let i = 0; i < 100; i++){
    nums[i] = Math.floor(Math.random() * 101);
}
let minValue = findMin(nums);
let maxValue = findMax(nums);
disArr(nums);
console.log();
console.log("最小值是: " + minValue);
console.log();
console.log("最大值是: " + maxValue);
```

## 使用自组织数据
```
//自组织形式的查找
function seqSearch(arr, data) {
    for(let i = 0; i < arr.length; i++){
        if(arr[i] == data){
            if(i > 0){
                swap(arr, i, i-1);
            }
            return true;
        }
    }
    return false;
}

function swap(arr, index, index1) {
    let temp = arr[index];
    arr[index] = arr[index1];
    arr[index1] = temp;
}

let numbers = [5, 1, 7, 4, 2, 10, 9, 3, 6, 8];
console.log(numbers);
for(let i = 1; i <=3; i++){
    seqSearch(numbers, 4);
    console.log(numbers);
}

//更好的自组织形式的查找
function seqSearch(arr, data) {
    for(let i = 0; i < arr.length; i++){
        if(arr[i] === data && i > (arr.length * 0.2)){
            swap(arr, i, 0);
            return true;
        }else if(arr[i] === data){
            return true;
        }
    }
    return false;
}

let nums = [];
for(let i = 0; i < 10; i++){
    nums[i] = Math.floor(Math.random() * 11);
}
disArr(nums);
console.log();
console.log("输入一个要查找的值: ");
let val = 5;
if(seqSearch(nums, val)){
    console.log("找到了元素: " + val);
    console.log();
    disArr(nums);
}else{
    console.log(val + "没有出现在这个数组中。");
}

```

## 二分查找算法
```
//二分查找算法
function binSearch(arr, data) {
    let upperBound = arr.length - 1;
    let lowerBound = 0;
    while(lowerBound <= upperBound){
        let mid = Math.floor((upperBound + lowerBound) / 2);
        //console.log("当前的中间点: " + mid);
        if(arr[mid] < data){
            lowerBound = mid + 1;
        }else if (arr[mid] > data){
            upperBound = mid - 1;
        }else{
            return mid;
        }
    }
    return -1;
}
let nums = new CArray(100);
nums.setData();
console.log(nums.toString());
nums.shellsort1();
console.log();
console.log(nums.toString());
console.log();
console.log("输入一个要查找的值: ");
let val = 15;
let retVal = binSearch(nums.dataStore, val);
if(retVal >= 0){
    console.log("已找到 " + val + " ，所在位置为: " + retVal);
}else{
    console.log(val + " 没有出现在这个数组中。");
}
```

## 重复计数
```
//重复计数
function count(arr, data) {
    let count = 0;
    let position = binSearch(arr, data);
    if(position > -1){
        count++;
        for(let i = position - 1; i > 0; i--){
            if(arr[i] === data){
                count++;
            }else{
                break;
            }
        }
        for(let i = position + 1; i < arr.length; i++){
            if(arr[i] === data){
                count++;
            }else{
                break;
            }
        }
    }
    return count;
}
let nums = new CArray(100);
nums.setData();
console.log(nums.toString());
nums.shellsort1();
console.log();
console.log(nums.toString());
console.log();
console.log();
let val = 15;
let retVal = count(nums.dataStore, val);
console.log("找到了 " + retVal + " 次重复出现的 " + val + "。");
```

## 查找文本数据
```
const fs = require('fs');
//查找文本数据
function seqSearch(arr, data){
    for(let i = 0; i < arr.length; i++){
        if(arr[i] === data){
            return i;
        }
    }
    return -1;
}
const words = fs.readFileSync("big.txt", "utf-8").replace(/[,.]/g, "").split(" ");
let word = "rhetoric";
let start = new Date().getTime();
let position = seqSearch(words, word);
let stop = new Date().getTime();
let elapsed = stop - start;
if(position >= 0){
    console.log("单词 " + word + " 被找的位置在: " + position + "。");
    console.log("顺序查找消耗了 " + elapsed + " 毫秒。");
}else{
    console.log(word + " 这个单词没有出现在这个文件内容中。");
}
```

## 二分查找法查找文本
```
//二分查找字符
function insertionsort(arr) {
    let temp, inner;
    for (let outer = 1; outer <= arr.length-1; ++outer) {
        temp = arr[outer];
        inner = outer;
        while (inner > 0 && (arr[inner-1] >= temp)) {
            arr[inner] = arr[inner-1];
            --inner; }
        arr[inner] = temp;
    }
}

let words = fs.readFileSync("big.txt", "utf-8").replace(/[,.]/g, "").split(" ");
insertionsort(words);
let word = "rhetoric";
let start = new Date().getTime();
let position = seqSearch(words, word);
let stop = new Date().getTime();
let elapsed = stop - start;
if(position >= 0){
    console.log("单词 " + word + " 被找的位置在: " + position + "。");
    console.log("顺序查找消耗了 " + elapsed + " 毫秒。");
}else{
    console.log(word + " 这个单词没有出现在这个文件内容中。");
}
```

## 顺序查找匹配最后一个元素
```
//顺序查找匹配最后一个元素
function seqSearch(arr, data) {
    for(let i = arr.length; i--;){
        if(arr[i] === data){
            return i;
        }
    }
    return -1;
}
let nums = [];
for(let i = 0; i < 100; i++){
    nums[i] = Math.floor(Math.random() * 101);
}
disArr(nums);
console.log("输入一个要查找的数字: ");
let num = 30;
console.log();
let position = seqSearch(nums, num);
if(position > -1){
    console.log(num + " 在这个数组中的索引位置是 " + position);
}else{
    console.log(num + " 没有出现在这个数组中。");
}
```

## 比较顺序查找与二分查找耗时
```
const CArray = require("CArray");
//比较顺序查找与二分查找耗时
//1000000数据
let numElements = 1000000;
let nums = new CArray(numElements);
nums.setData();
let start = new Date().getTime();
let position = seqSearch(nums.dataStore, 100);
let stop = new Date().getTime();
let elapsed = stop - start;
console.log("1000000数据顺序查找消耗了 " + elapsed + " 毫秒。");

nums.clear();
nums.setData();
qSort(nums.dataStore);
start = new Date().getTime();
position = binSearch(nums.dataStore, 100);
stop = new Date().getTime();
elapsed = stop - start;
console.log("1000000数据二分查找消耗了 " + elapsed + " 毫秒。");

//10000000数据
numElements = 10000000;
nums = new CArray(numElements);
nums.setData();
start = new Date().getTime();
position = seqSearch(nums.dataStore, 100);
stop = new Date().getTime();
elapsed = stop - start;
console.log("10000000数据顺序查找消耗了 " + elapsed + " 毫秒。");

nums.clear();
nums.setData();
qSort(nums.dataStore);
start = new Date().getTime();
position = binSearch(nums.dataStore, 100);
stop = new Date().getTime();
elapsed = stop - start;
console.log("10000000数据二分查找消耗了 " + elapsed + " 毫秒。");

```

## 查找次小元素，第三小元素和第四小元素
```
//查找次小元素，第三小元素和第四小元素
function findMin(arr) {
    let nArr = qSort(arr);
    let minArr = [];
    let arrLen = 4;
    for(let i = 0, len = nArr.length; i < len; i++){
        let minLength = minArr.length;
        if(!minLength){
            minArr.push(nArr[i]);
        }else{
            if(minLength < arrLen && minArr[minLength - 1] < nArr[i]){
                minArr.push(nArr[i]);
            }
        }
    }
    return minArr;
}
let nums = [];
for(let i = 0; i < 10; i++){
    nums[i] = Math.floor(Math.random() * 11);
}
console.log(nums);
let minValue = findMin(nums);
console.log(minValue);
```

参考自《数据结构与算法 JavasScript描述》