# 字典
树是计算机科学中经常用到的一种数据结构。树是一种非线性的数据结构，以分层的方式 存储数据。

## 创建二叉树
```
class Node {
    constructor(data, left, right) {
        this.data = data;
        this.left = left;
        this.right = right;
        this.count = 1;
    }

    show() {
        return this.data;
    }
}

class BST {
    constructor() {
        this.root = null;
        this.count = 0;
    }

    //插入
    insert(data) {
        let n = new Node(data, null, null);
        if (!this.root) {
            this.root = n;
        } else {
            let current = this.root;
            let parent;
            while (true) {
                parent = current;
                if (data < current.data) {
                    current = current.left;
                    if (!current) {
                        parent.left = n;
                        break;
                    }
                } else {
                    current = current.right;
                    if (!current) {
                        parent.right = n;
                        break;
                    }
                }
            }
        }
    }

    //中序遍历
    inOrder(node) {
        if (node) {
            this.inOrder(node.left);
            console.log(node.show() + " ");
            this.inOrder(node.right);
        }
    }

    //先序遍历
    preOrder(node) {
        if (node) {
            console.log(node.show() + " ");
            this.preOrder(node.left);
            this.preOrder(node.right);
        }
    }

    //后序遍历
    postOrder(node) {
        if (node) {
            this.postOrder(node.left);
            this.postOrder(node.right);
            console.log(node.show() + " ");
        }
    }

    //获取最小值
    getMin() {
        let current = this.root;
        while (!(current.left == null)) {
            current = current.left;
        }
        return current.data;
    }

    //获取最大值
    getMax() {
        let current = this.root;
        while (!(current.right == null)) {
            current = current.right;
        }
        return current.data;
    }

    //查找节点
    find(data) {
        let current = this.root;
        while (current != null) {
            if (current.data == data) {
                return current;
            } else if (data < current.data) {
                current = current.left;
            } else {
                current = current.right;
            }
        }
        return null;
    }

    //删除节点
    remove(data) {
        this.removeNode(this.root, data);
    }

    removeNode(node, data) {
        if (node == null) {
            return null;
        }
        if (data == node.data) {
            //没有子节点
            if (node.left == null && node.right == null) {
                return null;
            }
            //没有左子节点的节点
            if (node.left == null) {
                return node.right;
            }
            //没有右子节点的节点
            if (node.right == null) {
                return node.left;
            }
            //有两个子节点的节点
            let tempNode = this.getSmallest(node.right);
            node.data = tempNode.data;
            node.right = this.removeNode(node.right, tempNode.data);
            return node;
        } else if (data < node.data) {
            node.left = this.removeNode(node.left, data);
            return node;
        } else {
            node.right = this.removeNode(node.right, data);
            return node;
        }
    }

    getSmallest(node) {
        if (node.left == null) {
            return node;
        }
        else {
            return this.getSmallest(node.left);
        }
    }

    //计算数量
    update(data) {
        let grade = this.find(data);
        grade.count++;
        return grade;
    }

    //显示节点数
    displayCount() {
        this.count = 0;
        this.nodeCount(this.root);
        console.log(this.count);
    }

    nodeCount(node) {
        if (node) {
            this.count++;
            this.nodeCount(node.left);
            this.nodeCount(node.right);
        }
    }

    //显示边数
    sideCount() {
        this.count = 0;
        this.nodeCount(this.root);
        console.log(this.count - 1);
    }
}

let nums = new BST();
nums.insert(23);
nums.insert(45);
nums.insert(16);
nums.insert(3);
nums.insert(37);
nums.insert(99);
nums.insert(22);
console.log('InOrder traversal: ');
nums.inOrder(nums.root);
console.log('preOrder traversal: ');
nums.preOrder(nums.root);
console.log('postOrder traversal: ');
nums.postOrder(nums.root);
let min = nums.getMin();
console.log('The minimum value of the BST is: ' + min);
console.log('\n');
let max = nums.getMax();
console.log('The maximum value of the BST is: ' + max);
console.log('\n');
console.log('Enter a value to search for: ');
let value = 17;
let found = nums.find(value);
if (found != null) {
    console.log('Found ' + value + ' in the BST.');
} else {
    console.log(value + ' was not found in the BST.');
}

nums.remove(16);
console.log('InOrder traversal: ');
nums.inOrder(nums.root);
```

### 计数
```

function prArray(arr) {
    //console.log(arr[0].toString() + ' ');
    let str = '';
    for (let i = 1; i < arr.length; ++i) {
        str += arr[i].toString() + ' ';
        if (i % 10 == 0) {
            console.log(str);
            str = '';
        }
    }
}
//生成随机数组
function genArray(length) {
    let arr = [];
    for (let i = 0; i < length; ++i) {
        arr[i] = Math.floor(Math.random() * 101);
    }
    return arr;
}
let grades = genArray(100);

prArray(grades);
let gradeistro = new BST();
for (let i = 0; i < grades.length; ++i) {
    let g = grades[i];
    let grade = gradeistro.find(g);
    gradeistro.insert(g);
    if (grade == null) {
        gradeistro.insert(g);
    } else {
        gradeistro.update(g);
    }
}

let count = 'y';
while (count == 'y') {
    console.log('\n\nEnter a grade: ');
    let g = parseInt(grades.length / 10, 10);
    let aGrade = gradeistro.find(g);
    if (aGrade == null) {
        console.log('No occurrences of ' + g);
    } else {
        console.log('Occurrences of ' + g + ': ' + aGrade.count);
    }
    count = parseInt(grades.length / 10, 10);
}
```

### 显示文本中每个单词出现次数
```
//显示文本中每个单词出现次数
let str = "In this case you need to teach the webpack-generated assets to make requests to the webpack-dev-server even when running on a HTML-page sent by the backend server. On the other side you need to teach your backend server to generate HTML pages that include script tags that point to assets on the webpack-dev-server. In addition to that you need a connection between the webpack-dev-server and the webpack-dev-server runtime to trigger reloads on recompilation.";

class WordSolution {
    constructor(str) {
        let strArr = str.replace(/[.,]/g, "").split(" ");
        this.strBST = new BST();
        for (let i = 0, len = strArr.length; i < len; i++) {
            let strUnit = strArr[i];
            let strWord = this.strBST.find(strUnit);
            if (strWord) {
                this.strBST.update(strUnit);
            } else {
                this.strBST.insert(strUnit);
            }
        }
    }

    showDuplicate() {
        this.getWordDuplicate(this.strBST.root);
    }

    getWordDuplicate(node) {
        if (node) {
            this.getWordDuplicate(node.left);
            console.log(node.show() + " : " + node.count);
            this.getWordDuplicate(node.right);
        }
    }
}

let wordSolution = new WordSolution(str);
wordSolution.showDuplicate();
```

参考自《数据结构与算法 JavasScript描述》