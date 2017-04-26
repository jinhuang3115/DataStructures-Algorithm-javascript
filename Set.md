# 集合
集合(set)是一种包含不同元素的数据结构。集合中的元素称为成员。集合的两个最重 要特性是:首先，集合中的成员是无序的;其次，集合中不允许相同成员存在。

## 创建集合
```
class Set {
    constructor(){
        this.dataStore = [];
    }
    add(data){
        if(this.dataStore.indexOf(data) < 0){
            this.dataStore.push(data);
            return true;
        }else{
            return false;
        }
    }
    remove(data){
        let pos = this.dataStore.indexOf(data);
        if (pos > -1){
            this.dataStore.splice(pos, 1);
            return true;
        }else{
            return false;
        }
    }
    size(){
        return this.dataStore.length;
    }
    union(set){
        let temSet = new Set();
        for(let i = 0, len = this.dataStore.length; i < len; i++){
            temSet.add(this.dataStore[i]);
        }
        for(let i = 0, len = set.dataStore.length; i < len; i++){
            if(!temSet.contains(set.dataStore[i])){
                temSet.dataStore.push(set.dataStore[i]);
            }
        }
        return temSet;
    }
    intersect(set){
        let tempSet = new Set();
        for(let i = 0, len = this.dataStore.length; i < len; i++){
            if(set.contains(this.dataStore[i])){
                tempSet.add(this.dataStore[i]);
            }
        }
        return tempSet;
    }
    subset(set){
        if(this.size() > set.size()){
            return false;
        }else{
            this.dataStore.forEach(function(member){
                if(!set.contains(member)){
                    return false;
                }
            });
        }
    }
    difference(set){
        let tempSet = new Set();
        for(let i = 0, len = this.dataStore.length; i < len; i++){
            if(!set.contains(this.dataStore[i])){
                tempSet.add(this.dataStore[i])
            }
        }
        return tempSet;
    }
    show(){
        return this.dataStore.sort();
    }
    contains(data){
        if (this.dataStore.indexOf(data) > -1){
            return true;
        }else{
            return false;
        }
    }
}
let names = new Set();
names.add('David');
names.add('Jennifer');
names.add('Cynthia');
names.add('Mike');
names.add('Raymond');
if(names.add('Mike')){
    console.log('Milk added');
}else{
    console.log("Can't add Mike, must already be in set");
}
console.log(names.show());
let removed = 'Mike';
if(names.remove(removed)){
    console.log(removed + ' removed.');
}else{
    console.log(removed + ' not removed');
}
names.add('Clayton');
console.log(names.show());
removed = "Alisa";
if(names.remove(removed)){
    console.log(removed + " removed.");
}else{
    console.log(removed + ' not removed.');
}
```

### union
```
//union
let cis = new Set();
cis.add('Mike');
cis.add('Clayton');
cis.add('Jennifer');
cis.add('Raymond');
let dmp = new Set();
dmp.add('Raymond');
dmp.add('Cynthia');
dmp.add('Jonathan');
let it = new Set();
it = cis.union(dmp);
console.log(it.show());
```

### intersect
```
//intersect
let cis = new Set();
cis.add('Mike');
cis.add('Clayton');
cis.add('Jennifer');
cis.add('Raymond');
let dmp = new Set();
dmp.add('Raymond');
dmp.add('Cynthia');
dmp.add('Bryan');
let inter = cis.intersect(dmp);
console.log(inter.show());
```

### subset
```
//subset
let it = new Set();
it.add("Cynthia");
it.add("Clayton");
it.add("Jennifer");
it.add("Danny");
it.add("Jonathan");
it.add("Terrill");
it.add("Raymond");
it.add("Mike");
let dmp = new Set();
dmp.add("Cynthia");
dmp.add("Raymond");
dmp.add("Jonathan");
dmp.add("Shirley");
if (dmp.subset(it)) {
    console.log("DMP is a subset of IT.");
}
else {
    console.log("DMP is not a subset of IT.");
}
```

### difference
```
//difference
let cis = new Set();
let it = new Set();
cis.add("Clayton");
cis.add("Jennifer");
cis.add("Danny");
it.add("Bryan");
it.add("Clayton");
it.add("Jennifer");
let diff = new Set();
diff = cis.difference(it);
console.log("[" + cis.show() + "] difference [" + it.show()
    + "] -> [" + diff.show() + "]");
```

### 存储方式从函数组替换为链表
```
const LList = require('linkedList');
class Set {
    constructor(){
        this.dataStore = new LList();
    }
    add(data){
        let dataStore = this.dataStore;
        let currentNode = dataStore.find(data);
        if(currentNode.element === 'head'){
            dataStore.insert(data, dataStore.findLast().element);
            return true;
        }else{
            return false;
        }
    }
    remove(data){
        let item = this.dataStore.find(data);
        if (item.element !== 'head'){
            this.dataStore.remove(item.element);
            return true;
        }else{
            return false;
        }
    }
    size(){
        return this.dataStore.count();
    }
    union(set){
        let temSet = new Set();
        for(let i = 0, len = this.dataStore.count(); i < len; i++){
            this.dataStore.advance(1);
            temSet.add(this.dataStore.currentNode.element);
        }
        for(let i = 0, len = set.dataStore.count(); i < len; i++){
            set.dataStore.advance(1);
            if(!temSet.contains(set.dataStore.currentNode.element)){
                temSet.add(set.dataStore.currentNode.element);
            }
        }
        return temSet;
    }
    intersect(set){
        let tempSet = new Set();
        for(let i = 0, len = this.dataStore.count(); i < len; i++){
            this.dataStore.advance(1);
            if(set.contains(this.dataStore.currentNode.element)){
                tempSet.add(this.dataStore.currentNode.element);
            }
        }
        return tempSet;
    }
    subset(set){
        if(this.size() > set.size()){
            return false;
        }else{
            for(let i = 0, len = this.dataStore.count(); i < len; i++){
                this.dataStore.advance(1);
                if(!set.contains(this.dataStore.currentNode.element)){
                    return false;
                }
            }
        }
        return true;
    }
    difference(set){
        let tempSet = new Set();
        for(let i = 0, len = this.dataStore.count(); i < len; i++){
            this.dataStore.advance(1);
            if(!set.contains(this.dataStore.currentNode.element)){
                tempSet.add(this.dataStore.currentNode.element);
            }
        }
        return tempSet;
    }
    show(){
        return this.dataStore.getAll();
    }
    contains(data){
        if(this.dataStore.find(data).element !== 'head'){
            return true;
        }else{
            return false;
        }
    }
}
```

### higher & lower
```
class Set {
    constructor(){
        this.dataStore = [];
    }
    add(data){
        if(this.dataStore.indexOf(data) < 0){
            this.dataStore.push(data);
            return true;
        }else{
            return false;
        }
    }
    remove(data){
        let pos = this.dataStore.indexOf(data);
        if (pos > -1){
            this.dataStore.splice(pos, 1);
            return true;
        }else{
            return false;
        }
    }
    size(){
        return this.dataStore.length;
    }
    union(set){
        let temSet = new Set();
        for(let i = 0, len = this.dataStore.length; i < len; i++){
            temSet.add(this.dataStore[i]);
        }
        for(let i = 0, len = set.dataStore.length; i < len; i++){
            if(!temSet.contains(set.dataStore[i])){
                temSet.dataStore.push(set.dataStore[i]);
            }
        }
        return temSet;
    }
    intersect(set){
        let tempSet = new Set();
        for(let i = 0, len = this.dataStore.length; i < len; i++){
            if(set.contains(this.dataStore[i])){
                tempSet.add(this.dataStore[i]);
            }
        }
        return tempSet;
    }
    subset(set){
        if(this.size() > set.size()){
            return false;
        }else{
            this.dataStore.forEach(function(member){
                if(!set.contains(member)){
                    return false;
                }
            });
        }
    }
    difference(set){
        let tempSet = new Set();
        for(let i = 0, len = this.dataStore.length; i < len; i++){
            if(!set.contains(this.dataStore[i])){
                tempSet.add(this.dataStore[i])
            }
        }
        return tempSet;
    }
    show(){
        return this.dataStore.sort();
    }
    contains(data){
        if (this.dataStore.indexOf(data) > -1){
            return true;
        }else{
            return false;
        }
    }
    higher(element){
        let arr = [];
        for(let i = 0, len = this.dataStore.length; i < len; i++){
            if(this.dataStore[i] > element){
                arr.push(this.dataStore[i]);
            }
        }
        return Math.min.apply(null, arr);
    }
    lower(element){
        let arr = [];
        for(let i = 0, len = this.dataStore.length; i < len; i++){
            if(this.dataStore[i] < element){
                arr.push(this.dataStore[i]);
            }
        }
        return Math.max.apply(null, arr);
    }
}  
let names = new Set();
names.add(1);
names.add(2);
names.add(3);
names.add(4);
names.add(5);
console.log(names.higher(3));
console.log(names.lower(3));
```

参考自《数据结构与算法 JavasScript描述》