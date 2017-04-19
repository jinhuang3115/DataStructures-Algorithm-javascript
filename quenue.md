# 队列
队列是一种先进先出(First-In-First-Out，FIFO)的数据结构。队列被用在很多地方，比如 提交操作系统执行的一系列进程、打印任务池等，一些仿真系统用队列来模拟银行或杂货 店里排队的顾客。

## 创建队列
```
class Queue {
    constructor() {
        this.dataStore = [];
    }

    enqueue(element) {
        this.dataStore.push(element);
    }

    dequeue() {
        return this.dataStore.shift();
    }

    front() {
        return this.dataStore[0];
    }

    back() {
        return this.dataStore[this.dataStore.length - 1];
    }

    toString() {
        let retStr = "";
        for (let i = 0, len = this.dataStore.length; i < len; i++) {
            retStr += this.dataStore[i] + "\n";
        }
        return retStr;
    }

    empty() {
        return !this.dataStore.length;
    }

    count() {
        return this.dataStore.length;
    }
}

let q = new Queue();
q.enqueue("Meredith");
q.enqueue("Cynthia");
q.enqueue("Jennifer");
console.log(q.toString());
q.dequeue();
console.log(q.toString());
console.log("Frond of queue: " + q.front());
console.log("Back of queue: " + q.back());
console.log("empty: " + q.empty());
```

##  队列的应用
### 1 方块舞模拟
```
//方块舞模拟
function Dancer(name, sex) {
    this.name = name;
    this.sex = sex;
}
function getDancers(males, females) {
    let names = fs.readFileSync("dancers.txt", "utf-8").split("\n");
    for (let i = 0; i < names.length; ++i) {
        names[i] = names[i].trim();
    }
    for (let i = 0; i < names.length; ++i) {
        let dancer = names[i].split(" ");
        let sex = dancer[0];
        let name = dancer[1];
        if (sex == "F") {
            females.enqueue(new Dancer(name, sex));
        }
        else {
            males.enqueue(new Dancer(name, sex));
        }
    }
}

function dance(males, females) {
    let person = null;
    console.log("The dance partners are: \n");
    while (!females.empty() && !males.empty()) {
        person = females.dequeue();
        console.log("Female dancer is: " + person.name);
        person = males.dequeue();
        console.log(" and the male dancer is: " + person.name);
    }
}

let maleDancers = new Queue();
let femaleDancers = new Queue();
getDancers(maleDancers, femaleDancers);
dance(maleDancers, femaleDancers);
if (maleDancers.count()) {
    console.log("There are " + maleDancers.count() +
        " male dancers waiting to dance.");
}
if (femaleDancers.count()) {
    console.log("There are " + femaleDancers.count() +
        " female dancers waiting to dance.");
}
```

### 2 基数排列
```
//基数排序
function collect(queues, nums) {
    let i = 0;
    for (let digit = 0; digit < 10; ++digit) {
        while (!queues[digit].empty()) {
            nums[i++] = queues[digit].dequeue();
        }
    }
}

function distribute(nums, queues, n, digit) {
    for (let i = 0; i < n; ++i) {
        if (digit === 1) {
            queues[nums[i] % 10].enqueue(nums[i]);
        } else {
            queues[Math.floor(nums[i] / 10)].enqueue(nums[i]);
        }
    }
}

function collect(queues, nums) {
    let i = 0;
    for (let digit = 0; digit < 10; ++digit) {
        while (!queues[digit].empty()) {
            nums[i++] = queues[digit].dequeue();
        }
    }
}
function dispArray(arr) {
    for (let i = 0; i < arr.length; ++i) {
        console.log(arr[i] + " ");
    }
}

let queues = [];
for (let i = 0; i < 10; ++i) {
    queues[i] = new Queue();
}
let nums = [];
for (let i = 0; i < 10; i++) {
    nums[i] = Math.floor(Math.floor(Math.random() * 101));
}
console.log("Before radix sort: ");
dispArray(nums);
distribute(nums, queues, 10, 1);
collect(queues, nums);
distribute(nums, queues, 10, 10);
collect(queues, nums);
console.log("\n\n After radix sort: ");
dispArray(nums);
```

### 3 优先队列
```
//优先队列
function Patient(name, code) {
    this.name = name;
    this.code = code;
}
class PriorityQuene {
    constructor() {
        this.dataStore = [];
    }

    enqueue(element) {
        this.dataStore.push(element);
    }

    dequeue() {
        let priority;
        let inx = 0;
        if (this.dataStore.length) {
            priority = this.dataStore[0].code;
            for (let i = 1, len = this.dataStore.length; i < len; i++) {
                if (this.dataStore[i].code < priority) {
                    priority = this.dataStore[i].code;
                    inx = i;
                }
            }
            return this.dataStore.splice(inx, 1);
        } else {
            return "is empty";
        }

    }

    front() {
        return this.dataStore[0];
    }

    back() {
        return this.dataStore[this.dataStore.length - 1];
    }

    toString() {
        let retStr = "";
        for (let i = 0, len = this.dataStore.length; i < len; i++) {
            retStr += this.dataStore[i].name + " code: " + this.dataStore[i].code + "\n";
        }
        return retStr;
    }

    empty() {
        return !this.dataStore.length;
    }

    count() {
        return this.dataStore.length;
    }
}

let p = new Patient("Smith", 5);
let ed = new PriorityQuene();
ed.enqueue(p);
p = new Patient("Jones", 4);
ed.enqueue(p);
p = new Patient("Fehrenbach", 6);
ed.enqueue(p);
p = new Patient("Brown", 1);
ed.enqueue(p);
p = new Patient("Ingram", 1);
ed.enqueue(p);
console.log(ed.toString());
let seen = ed.dequeue();
console.log("Patient being treated: " + seen[0].name);
console.log("Patients wating to be seen: ");
console.log(ed.toString());
// 下一轮
seen = ed.dequeue();
console.log("Patient being treated: " + seen[0].name);
console.log("Patients waiting to be seen: ");
console.log(ed.toString());
seen = ed.dequeue();
console.log("Patient being treated: " + seen[0].name);
console.log("Patients waiting to be seen: ");
console.log(ed.toString());
seen = ed.dequeue();
console.log("Patient being treated: " + seen[0].name);
console.log("Patients waiting to be seen: ");
console.log(ed.toString());
seen = ed.dequeue();
console.log("Patient being treated: " + seen[0].name);
console.log("Patients waiting to be seen: ");
console.log(ed.toString());
seen = ed.dequeue();
console.log(seen);
```

### 4 双向队列
```
//双向队列
class Deque {
    constructor() {
        this.dataStore = [];
    }

    topEnqueue(element){
        this.dataStore.unshift(element);
    }

    endEenqueue(element) {
        this.dataStore.push(element);
    }

    topDequeue() {
        return this.dataStore.shift();
    }
    endDequeue(){
        return this.dataStore.pop();
    }

    front() {
        return this.dataStore[0];
    }

    back() {
        return this.dataStore[this.dataStore.length - 1];
    }

    toString() {
        let retStr = "";
        for (let i = 0, len = this.dataStore.length; i < len; i++) {
            retStr += this.dataStore[i] + "\n";
        }
        return retStr;
    }

    empty() {
        return !this.dataStore.length;
    }

    count() {
        return this.dataStore.length;
    }
}

let d = new Deque();
d.topEnqueue("Meredith");
d.topEnqueue("Cynthia");
d.endEenqueue("Jennifer");
d.endEenqueue("jack");
console.log(d.toString());

d.topDequeue();
d.endDequeue();
console.log(d.toString());
console.log("Frond of queue: " + d.front());
console.log("Back of queue: " + d.back());
console.log("empty: " + d.empty());
```

### 5 判断是否是回文

```
//判断单词是否是回文
let str = '123321';
function isPalindrome(word){
    let arr = word.split("");
    let deque = new Deque();
    for (let i = 0, len = arr.length; i < len; i++){
        deque.topEnqueue(arr[i]);
    }
    while(deque.count() && (deque.front() === deque.back())){
        deque.topDequeue();
        deque.endDequeue();
    }
    return deque.empty();
}
console.log(isPalindrome(str));
```

### 6 数字高优先级高，优先队列

```
//数字高优先级高，优先队列
function Patient(name, code) {
    this.name = name;
    this.code = code;
}
class PriorityQuene {
    constructor() {
        this.dataStore = [];
    }

    enqueue(element) {
        this.dataStore.push(element);
    }

    dequeue() {
        let priority;
        let inx = 0;
        if (this.dataStore.length) {
            priority = this.dataStore[0].code;
            for (let i = 1, len = this.dataStore.length; i < len; i++) {
                if (this.dataStore[i].code > priority) {
                    priority = this.dataStore[i].code;
                    inx = i;
                }
            }
            return this.dataStore.splice(inx, 1);
        } else {
            return "is empty";
        }

    }

    front() {
        return this.dataStore[0];
    }

    back() {
        return this.dataStore[this.dataStore.length - 1];
    }

    toString() {
        let retStr = "";
        for (let i = 0, len = this.dataStore.length; i < len; i++) {
            retStr += this.dataStore[i].name + " code: " + this.dataStore[i].code + "\n";
        }
        return retStr;
    }

    empty() {
        return !this.dataStore.length;
    }

    count() {
        return this.dataStore.length;
    }
}

let p = new Patient("Smith", 5);
let ed = new PriorityQuene();
ed.enqueue(p);
p = new Patient("Jones", 4);
ed.enqueue(p);
p = new Patient("Fehrenbach", 6);
ed.enqueue(p);
p = new Patient("Brown", 1);
ed.enqueue(p);
p = new Patient("Ingram", 1);
ed.enqueue(p);
console.log(ed.toString());
let seen = ed.dequeue();
console.log("Patient being treated: " + seen[0].name);
console.log("Patients wating to be seen: ");
console.log(ed.toString());
// 下一轮
seen = ed.dequeue();
console.log("Patient being treated: " + seen[0].name);
console.log("Patients waiting to be seen: ");
console.log(ed.toString());
seen = ed.dequeue();
console.log("Patient being treated: " + seen[0].name);
console.log("Patients waiting to be seen: ");
console.log(ed.toString());
seen = ed.dequeue();
console.log("Patient being treated: " + seen[0].name);
console.log("Patients waiting to be seen: ");
console.log(ed.toString());
seen = ed.dequeue();
console.log("Patient being treated: " + seen[0].name);
console.log("Patients waiting to be seen: ");
console.log(ed.toString());
seen = ed.dequeue();
console.log(seen);
```

### 7 患者菜单系统
```
//患者菜单系统
class PatientSystem {
    constructor() {
        this.patientList = new PriorityQuene();
    }

    waiting(people) {
        this.patientList.enqueue(people);
    }

    curing() {
        this.patientList.dequeue();
    }
    showWating(){
        return this.patientList.toString();
    }
}

console.log('*******************');
let patientSystem = new PatientSystem();
//进入候诊
let people = new Patient("Smith", 5);
patientSystem.waiting(people);
//显示就诊名单
console.log(patientSystem.showWating());
//进入候诊
people = new Patient("Jones", 4);
patientSystem.waiting(people);
//显示就诊名单
console.log(patientSystem.showWating());
//进入候诊
people = new Patient("Fehrenbach", 6);
patientSystem.waiting(people);
//显示就诊名单
console.log(patientSystem.showWating());
//患者就诊
patientSystem.curing();
//显示就诊名单
console.log(patientSystem.showWating());
//进入候诊
people = new Patient("Brown", 1);
patientSystem.waiting(people);
//显示就诊名单
console.log(patientSystem.showWating());
//患者就诊
patientSystem.curing();
//显示就诊名单
console.log(patientSystem.showWating());
```

参考自《数据结构与算法 JavasScript描述》