# 链表
数组不总是组织数据的最佳数据结构。如果你发现数组在实际使用时很慢，就可以考虑使用链表来替代它。

## 创建链表
```
//循环 双向 链表
class Node {
    constructor(element) {
        this.element = element;
        this.next = null;
        this.previous = null;
    }
}

class LList {
    constructor() {
        this.head = new Node('head');
        this.head.next = this.head;
        this.currentNode = this.head;
    }

    find(item) {
        let currNode = this.head;
        while (currNode.element !== item) {
            currNode = currNode.next;
        }
        return currNode;
    }

    insert(newElement, item) {
        let newNode = new Node(newElement);
        let current = this.find(item);
        newNode.next = current.next;
        newNode.previous = current;
        current.next = newNode;
    }

    display() {
        let currNode = this.head;
        while (!(currNode.next == null) && !(currNode.next.element === "head")) {
            console.log(currNode.next.element);
            currNode = currNode.next;
        }
    }

    /*  findPrevious(item){
     let currNode = this.head;
     while(!(currNode.next == null) && (currNode.next.element != item)){
     currNode = currNode.next;
     }
     return currNode;
     }*/
    remove(item) {
        let currNode = this.find(item);
        if (!(currNode.next.element === 'head')) {
            currNode.previous.next = currNode.next;
            currNode.next.previous = currNode.previous;
            currNode.next = null;
            currNode.previous = null;
        }
    }

    findLast() {
        let currNode = this.head;
        while (!(currNode.next.element === 'head')) {
            currNode = currNode.next;
        }
        return currNode;
    }

    dispReverse() {
        let currNode = this.head;
        currNode = this.findLast();
        while (!(currNode.previous == null)) {
            console.log(currNode.element);
            currNode = currNode.previous;
        }
    }

    advance(n) {
        while (n > 0) {
            if(this.currentNode.next.element == "head"){
                this.currentNode = this.currentNode.next.next;
            }else{
                this.currentNode = this.currentNode.next;
            }
            n--;
        }
    }

    back(n) {
        while (n > 0 && !(this.currentNode.element === 'head')) {
            this.currentNode = this.currentNode.previous;
            n--;
        }
    }

    show() {
        console.log(this.currentNode);
    }
    count(){
        let currNode = this.head;
        let i = 0;
        while(!(currNode.next.element === 'head')){
            currNode = currNode.next;
            i++;
        }
        return i;
    }
}

let cities = new LList();
cities.insert("Conway", "head");
cities.insert("Russellville", "Conway");
cities.insert("Carlisle", "Russellville");
cities.insert("Alma", "Carlisle");
cities.display();
console.log();
cities.remove("Carlisle");
cities.display();
console.log();
cities.dispReverse();
console.log();
cities.show();
console.log();
cities.advance(2);
console.log();
cities.show();
cities.back(1);
console.log();
cities.show();
```
## 实现约瑟夫环
```
//约瑟夫环
let suicides = new LList();
for (let i = 0, len = 30; i < len; i++) {
    if (!i) {
        suicides.insert(i + 1, 'head');
    } else {
        suicides.insert(i + 1, i);
    }
}
while (suicides.count() > 2) {
    suicides.advance(4);
    if (suicides.currentNode.previous.element === 'head'){
        suicides.advance(1);
    }
    suicides.remove(suicides.currentNode.previous.element);
    suicides.back(1);
}
suicides.display();
```
