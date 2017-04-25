# 散列(HASH)
散列是一种常用的数据存储技术，散列后的数据可以快速地插入或取用。

##  创建散列
```
const fs = require('fs');
class HashTable {
    constructor() {
        this.table = new Array(137);
        this.values = [];
    }

    simpleHash(data) {
        let total = 0;
        for (let i = 0, len = data.length; i < len; i++) {
            total += data.charCodeAt(i);
        }
        return total % this.table.length;
    }

    betterHash(string) {
        const H = 37;
        var total = 0;
        for (var i = 0; i < string.length; ++i) {
            total += H * total + string.charCodeAt(i);
        }
        total = total % this.table.length;
        if (total < 0) {
            total += this.table.length - 1;
        }
        return parseInt(total);
    }
    
    put(data) {
        var pos = this.betterHash(data);
        this.table[pos] = data;
    }


    get(key) {
        return this.table[this.betterHash(key)];
    }
    showDistro() {
        let n = 0;
        for (let i = 0; i < this.table.length; ++i) {
            if (this.table[i] != undefined) {
                console.log(i + ": " + this.table[i]);
            }
        }
    }
}
let someNames = ["David", "Jennifer", "Donnie", "Raymond",
    "Cynthia", "Mike", "Clayton", "Danny", "Jonathan"];
let hTable = new HashTable();
for (let i = 0, len = someNames.length; i < len; i++) {
    hTable.put(someNames[i]);
}
hTable.showDistro();
```

### 散列化整型键
```
function getRandomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}
function genStuData(arr) {
    for (let i = 0, len = arr.length; i < len; i++) {
        let num = "";
        for (let j = 1; j <= 9; j++) {
            num += Math.floor(Math.random() * 10);
        }
        num += getRandomInt(50, 100);
        arr[i] = num;
    }
}
let numStudents = 10;
let arrSize = 97;
let idLen = 9;
let students = new Array(numStudents);
genStuData(students);
console.log('Student data: \n');
for (let i = 0, len = students.length; i < len; i++) {
    console.log(students[i].substring(0, 8) + " " + students[i].substring(9));
}
console.log("\n\nData distribution: \n");
let hTable = new HashTable();
for (let i = 0, len = students.length; i < len; i++) {
    hTable.put(students[i]);
}
hTable.showDistro();
```

## 碰撞处理
###开链法

```
class HashTable {
    constructor() {
        this.table = new Array(137);
        this.values = [];
    }

    simpleHash(data) {
        let total = 0;
        for (let i = 0, len = data.length; i < len; i++) {
            total += data.charCodeAt(i);
        }
        return total % this.table.length;
    }

    betterHash(string) {
        const H = 37;
        var total = 0;
        for (var i = 0; i < string.length; ++i) {
            total += H * total + string.charCodeAt(i);
        }
        total = total % this.table.length;
        if (total < 0) {
            total += this.table.length - 1;
        }
        return parseInt(total);
    }

    //开链法
    put(key, data) {
        let pos = this.betterHash(key);
        let index = 0;
        if (this.table[pos][index] == undefined) {
            this.table[pos][index] = key;
            this.table[pos][index + 1] = data;

        } else {
            while (this.table[pos][index] != undefined) {
                ++index;
            }
            this.table[pos][index] = data;
        }
    }

    //开链法
    showDistro() {
        let n = 0;
        for (let i = 0, len = this.table.length; i < len; i++) {
            if (this.table[i][1] != undefined) {
                console.log(i + ': ' + this.table[i]);
            }
        }
    }
    //开链法
    get(key) {
        let index = 0;
        let hash = this.betterHash(key);
        if (this.table[hash][index] == key) {
            return this.table[hash].slice(index + 1, this.table[hash].length);
        } else {
            while (this.table[hash][index] != key) {
                index += 2;
            }
            return this.table[hash][index + 1];
        }
        return undefined;
    }

    buildChains() {
        for (let i = 0, len = this.table.length; i < len; i++) {
            this.table[i] = [];
        }
    }
}
//开链法
let hTable = new HashTable();
hTable.buildChains();
let someNames = ["David", "Jennifer", "Donnie", "Raymond",
    "Cynthia", "Mike", "Clayton", "Danny", "Jonathan"];
for (let i = 0, len = someNames.length; i < len; i++) {
    hTable.put(someNames[i]);
}
hTable.showDistro();
```

### 线性探测法
```
class HashTable {
    constructor() {
        this.table = new Array(137);
        this.values = [];
    }

    simpleHash(data) {
        let total = 0;
        for (let i = 0, len = data.length; i < len; i++) {
            total += data.charCodeAt(i);
        }
        return total % this.table.length;
    }

    betterHash(string) {
        const H = 37;
        var total = 0;
        for (var i = 0; i < string.length; ++i) {
            total += H * total + string.charCodeAt(i);
        }
        total = total % this.table.length;
        if (total < 0) {
            total += this.table.length - 1;
        }
        return parseInt(total);
    }

    //线性探测法
    put(key, data) {
        let pos = this.betterHash(key);
        if (this.table[pos] == undefined) {
            this.table[pos] = key;
            this.values[pos] = data;
        } else {
            while (this.table[pos] != undefined) {
                pos++;
            }
            this.table[pos] = key;
            this.values[pos] = data;
        }
    }

    //开链法
    showDistro() {
        let n = 0;
        for (let i = 0, len = this.table.length; i < len; i++) {
            if (this.table[i][1] != undefined) {
                console.log(i + ': ' + this.table[i]);
            }
        }
    }

    //线性探测法
    showDistro() {
        let n = 0;
        for (let i = 0, len = this.table.length; i < len; i++) {
            if (this.table[i] != undefined) {
                console.log(i + ': ' + this.table[i]);
            }
        }
    }

    //线性探测法
    get(key) {
        let hash = -1;
        hash = this.betterHash(key);
        if (hash > -1) {
            for (let i = hash; this.table[hash] != undefined; i++) {
                if (this.table[hash] == key) {
                    return this.values[hash];
                }
            }
        }
        return undefined;
    }
}
```

##散列应用
### 线性探测法创建字典
```
//使用线性探测法创建字典
class Dictionary {
    constructor() {
        this.table = new HashTable();
    }

    put(filePath) {
        let dics = fs.readFileSync(filePath, "utf-8").split("\n");
        for (let i = 0, len = dics.length; i < len; i++) {
            let datas = dics[i].split(" ");
            let key = datas[0];
            let value = datas[1];
            this.table.put(key, value);
        }
        this.table.showDistro();
    }

    get(key) {
        console.log(this.table.get(key));
    }
}
let dic = new Dictionary();
dic.put('dictionary.txt');
console.log();
dic.get('b');
```

### 开链法创建字典
```
//使用开链法创建字典
class Dictionary {
    constructor() {
        this.table = new HashTable();
        this.table.buildChains();
    }

    put(filePath) {
        let dics = fs.readFileSync(filePath, "utf-8").split("\n");
        for (let i = 0, len = dics.length; i < len; i++) {
            let datas = dics[i].split(" ");
            for (let j = 1, jlen = datas.length; j < jlen; j++) {
                let key = datas[0];
                let value = datas[j];
                this.table.put(key, value);
            }
        }
        this.table.showDistro();
    }

    get(key) {
        console.log(this.table.get(key));
    }
}
let dic = new Dictionary();
dic.put('dictionary.txt');
console.log();
dic.get('b');
```

### 开链法显示文本单词出现次数
```
//show word times
function showWordTimes(word) {
    let words = word.split(" ");
    let table = new HashTable();
    table.buildChains();
    for (let i = 0, len = words.length; i < len; i++) {
        table.put(words[i], i);
    }
    table.showUnitCount();
}

showWordTimes('the brown fox jumped over the blue fox');
```
参考自《数据结构与算法 JavasScript描述》