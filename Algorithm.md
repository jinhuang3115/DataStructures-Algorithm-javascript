# 高级算法
动态规划和贪心算法

## 动态规划

### 递归实现斐波纳切数列
```
//递归斐波纳切数列
function recurFib(n) {
    if (n < 2) {
        return n;
    } else {
        return recurFib(n - 1) + recurFib(n - 2);
    }
}
```

### 动态规划实现斐波纳切数列
```
//动态规划斐波纳切数列
function dynFib(n) {
    let val = [];
    for (let i = 0; i <= n; i++) {
        val[i] = 0;
    }
    if (n === 1 || n === 2) {
        return 1;
    } else {
        val[1] = 1;
        val[2] = 2;
        for (let i = 3; i <= n; i++) {
            val[i] = val[i - 1] + val[i - 2];
        }
        return val[n - 1];
    }
}
```

### 非数组版本动态规划斐波纳切数列
```
//非数组版本动态规划斐波纳切函数
function iterFib(n) {
    let last = 1;
    let nextLast = 1;
    let result = 1;
    for (let i = 2; i < n; i++) {
        result = last + nextLast;
        nextLast = last;
        last = result;
    }
    return result;
}
```

### 统计动态规划和递归版本实现斐波纳切数列耗时
```
let start = new Date().getTime();
console.log(recurFib(50));
let stop = new Date().getTime();
console.log("递归计算耗时 - " + (stop - start) + "毫秒");//180864
console.log();
start = new Date().getTime();
console.log(iterFib(50));
stop = new Date().getTime();
console.log("动态规划耗时 - " + (stop - start) + "毫秒");//13
```

### 寻找最长公共子串
```
//寻找最长公共子串
function lcs(word1, word2) {
    let max = 0;
    let index = 0;
    let lcsarr = new Array(word1.length + 1);
    for (let i = 0; i <= word1.length + 1; i++) {
        lcsarr[i] = new Array(word2.length + 1);
        for (let j = 0; j <= word2.length + 1; j++) {
            lcsarr[i][j] = 0;
        }
    }
    for (let i = 0; i <= word1.length; i++) {
        for (let j = 0; j <= word2.length; j++) {
            if (i === 0 || j === 0) {
                lcsarr[i][j] = 0;
            } else {
                if (word1[i - 1] === word2[j - 1]) {
                    lcsarr[i][j] = lcsarr[i - 1][j - 1] + 1;
                } else {
                    lcsarr[i][j] = 0;
                }
            }
            if (max < lcsarr[i][j]) {
                max = lcsarr[i][j];
                index = i;
            }
        }
    }
    let str = "";
    if (max === 0) {
        return "";
    } else {
        for (let i = index - max; i < max; i++) {
            str += word2[i];
        }
        return str;
    }
}

console.log(lcs("abbcc", "dbbcc"));
```

### 背包问题：递归解决方案和动态规划解决方案
```
function max(a, b) {
    return (a > b) ? a : b;
}
//递归方案
function knapsack(capacity, size, value, n) {
    if (n === 0 || capacity === 0) {
        return 0;
    }
    if (size[n - 1] > capacity) {
        return knapsack(capacity, size, value, n - 1);
    } else {
        return max(value[n - 1] +
            knapsack(capacity - size[n - 1], size, value, n - 1),
            knapsack(capacity, size, value, n - 1));
    }
}
//动态规划方案
function dKnapsack(capacity, size, value, n) {
    let K = [];
    for (let i = 0; i <= capacity + 1; i++) {
        K[i] = [];
    }
    for (let i = 0; i <= n; i++) {
        let str = "";
        for (let w = 0; w <= capacity; w++) {
            if (i === 0 || w === 0) {
                K[i][w] = 0;
            } else if (size[i - 1] <= w) {
                K[i][w] = max(value[i - 1] + K[i - 1][w - size[i - 1]], K[i - 1][w]);
            } else {
                K[i][w] = K[i - 1][w];
            }
            str += K[i][w] + " ";
        }
        console.log(str);
        console.log();
    }
    return K[n][capacity];
}


let value = [4, 5, 10, 11, 13];
let size = [3, 4, 7, 8, 9];
let capacity = 16;
let n = 5;
console.log(dKnapsack(capacity, size, value, n));
console.log(knapsack(capacity, size, value, n));
```

## 贪心算法
### 找零问题
```
//贪心算法
function makeChange(origAmt, coins) {
    let remainAmt = 0;
    if (origAmt % 0.25 < origAmt) {
        coins[3] = parseInt(origAmt / 0.25, 10);
        remainAmt = origAmt % 0.25;
        origAmt = remainAmt;
    }
    if (origAmt % 0.1 < origAmt) {
        coins[2] = parseInt(origAmt / 0.1, 10);
        remainAmt = origAmt % 0.1;
        origAmt = remainAmt;
    }
    if (origAmt % 0.05 < origAmt) {
        coins[1] = parseInt(origAmt / 0.05, 10);
        remainAmt = origAmt % 0.05;
        origAmt = remainAmt;
    }
    coins[0] = parseInt(origAmt / 0.01, 10);
}

function showChange(coins) {
    if (coins[3] > 0) {
        console.log("25 美分的数量 - " + coins[3] + " - " + coins[3] * 0.25);
    }
    if (coins[2] > 0) {
        console.log("10 美分的数量 - " + coins[2] + " - " + coins[2] * 0.1);
    }
    if (coins[1] > 0) {
        console.log("5 美分的数量 - " + coins[1] + " - " + coins[1] * 0.05);
    }
    if (coins[0] > 0) {
        console.log("1 美分的数量 - " + coins[0] + " - " + coins[0] * 0.01);
    }
}


let origAmt = 0.30;
let coins = [];
makeChange(origAmt, coins);
showChange(coins);
```

### 贪心算法解决背包问题
```
//贪心算法解决背包问题
function ksack(values, weights, capacity) {
    let load = 0;
    let i = 0;
    let w = 0;
    while (load < capacity && i < 4) {
        if (weights[i] <= (capacity - load)) {
            w += values[i];
            load += weights[i];
        } else {
            let r = (capacity - load) / weights[i];
            w += r * values[i];
            load += weights[i];
        }
        i++;
    }
    return w;
}

let items = ["A", "B", "C", "D"];
let values = [50, 140, 60, 60];
let weights = [5, 20, 10, 12];
let capacity = 30;
console.log(ksack(values, weights, capacity));
```

### 暴力方式解决最长公共子串
```
//暴力方式解决最长公共子串
function recurlcs(word1, word2) {
    let str = "";
    let len1 = word1.length;
    let len2 = word2.length;
    for (let i = len1; i--;) {
        for (let j = 0; j < (len1 - 1); j++) {
            str = word1.substr(j, i);
            if (word2.indexOf(str) > -1) {
                return str;
            }
        }
    }
    return "";
}

console.log(recurlcs("abbccggc", "dbbccdabcdefga"));
```