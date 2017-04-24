# 字典
字典是一种以键 - 值对形式存储数据的数据结构，就像电话号码簿里的名字和电话号码一 样。

##  创建字典
```
const fs = require('fs');
class Dictionary{
    constructor(){
        this.datastore = [];
    }
    add(key, value){
        this.datastore[key] = value;
    }
    find(key){
        return this.datastore[key];
    }
    remove(key){
        delete this.datastore[key];
    }
    showAll(){
        let list = Object.keys(this.datastore).sort();
        for(let key in list){
            console.log(list[key] + ' -> ' + this.datastore[list[key]]);
        }
    }
    count(){
        let n = 0;
        for (let key in this.datastore){
            ++n;
        }
        return n;
    }
    clear(){
        for(let key in this.datastore){
            delete this.datastore[key];
        }
    }
}

let pbook = new Dictionary();
pbook.add("Raymond","123");
pbook.add("David", "345");
pbook.add("Cynthia", "456");
pbook.add("Mike", "723");
pbook.add("Jennifer", "987");
pbook.add("Danny", "012");
pbook.add("Jonathan", "666");
console.log('Number of entries: ' + pbook.count());
console.log("David's extension: " + pbook.find('David'));
pbook.remove('David');
pbook.showAll();
pbook.clear();
console.log('Number of entries: ' + pbook.count());
```

### 当数组的key为字符串时length失效
```
let nums = [];
nums[0] = 1;
nums[1] = 2;
console.log(nums.length);
let pbooks = [];
pbooks['David'] = 1;
pbooks['Jennifer'] = 2;
console.log(pbooks.length);
```

##  字典的应用
### 电话号码簿
```
//电话号码簿
class PhoneDictionary{
    constructor(){
        this.datastore = new Dictionary();
        let phones = fs.readFileSync("phoneList.txt", "utf-8").split("\n");
        for(let i = 0, len = phones.length; i < len; i++){
            let dic = phones[i].split(" ");
            let name = dic[0];
            let num = dic[1];
            this.datastore.add(name, num);
        }
    }
    find(name){
        console.log(this.datastore.find(name));
    }
    showAll(){
        this.datastore.showAll();
    }
    add(name, num){
        this.datastore.add(name, num);
    }
    remove(name){
        this.datastore.remove(name);
    }
    clearAll(){
        this.datastore.clear();
    }
}

let phoneDictionary = new PhoneDictionary();
phoneDictionary.showAll();
console.log();
phoneDictionary.find('Cynthia');
phoneDictionary.add('Alan', '891234567');
console.log();
phoneDictionary.showAll();
phoneDictionary.remove('Mike');
console.log();
phoneDictionary.showAll();
phoneDictionary.clearAll();
console.log();
phoneDictionary.showAll();
```

### 显示文本单词出现次数
```
//show word times
function showWordTimes(word){
    let words = word.split(" ");
    let dic = new Dictionary();
    for(let i = 0, len = words.length; i < len; i++){
        let val = dic.find(words[i]);
        !val ? val = 1 : val++;
        dic.add(words[i], val);
    }
    dic.showAll();
}
showWordTimes('the brown fox jumped over the blue fox');
```
参考自《数据结构与算法 JavasScript描述》