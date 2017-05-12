# 图和图算法
图是表示物件与物件之间的关系的数据结构

## 创建图
```
class Vertex {
    constructor(label) {
        this.label = label;
    }
}
class Graph {
    constructor(v) {
        this.vertices = v;
        this.marked = [];
        this.edges = 0;
        this.adj = [];
        this.vertexList = [];
        this.edgeTo = [];
        for (let i = 0; i < this.vertices; i++) {
            this.adj[i] = [];
            this.marked[i] = false;
            this.adj[i].push("");
        }
    }

    addEdge(v, w) {
        if (this.adj[v].indexOf(w) === -1) {
            this.adj[v].push(w);
        }
        if (this.adj[w].indexOf(v) === -1) {
            this.adj[w].push(v);
        }
        this.edges++;
    }

    showGraph() {
        let str = "";
        for (let i = 0; i < this.vertices; i++) {
            str += i + " ->";
            for (let j = 0; j < this.vertices; j++) {
                if (this.adj[i][j] != undefined) {
                    (j < (this.adj[i].length - 1)) ? str += this.adj[i][j] + " " : str += this.adj[i][j];
                }
            }
            if (i < (this.vertices - 1)) {
                str += "\n";
            }
        }
        console.log(str);
    }

    save(file) {
        let str = "";
        for (let i = 0; i < this.vertices; i++) {
            str += i + " ->";
            for (let j = 0; j < this.vertices; j++) {
                if (this.adj[i][j] != undefined) {
                    (j < (this.adj[i].length - 1)) ? str += this.adj[i][j] + " " : str += this.adj[i][j];
                }
            }
            if (i < (this.vertices - 1)) {
                str += "\n";
            }
        }
        fs.writeFileSync(file, str);
    }

    //深度优先搜索
    dfs(v) {
        this.marked[v] = true;
        //用于输出的if语句在这里不是必须的
        if (this.adj[v] != undefined) {
            console.log("Visited vertex: " + v);
        }
        if (Object.prototype.toString.call(this.adj[v]) === '[object Array]') {
            this.adj[v].map((w) => {
                if (!this.marked[w] && typeof w != 'string') {
                    this.edgeTo[w] = v;
                    this.dfs(w);
                }
            })
        }
    }

    //广度优先搜索
    bfs(s) {
        let queue = [];
        this.marked[s] = true;
        queue.push(s);
        while (queue.length > 0) {
            let v = queue.shift(); //从队首移除
            if (typeof(v) != "string") {
                console.log("Visited vertex: " + v);
            }
            if (Object.prototype.toString.call(this.adj[v]) === '[object Array]') {
                this.adj[v].map((w) => {
                    if (!this.marked[w] && typeof w != 'string') {
                        this.edgeTo[w] = v;
                        this.marked[w] = true;
                        queue.push(w);
                    }
                })
            }
        }
    }

    pathTo(v, source) {
        if (!this.hashPathTo(v)) {
            return undefined;
        }
        let path = [];
        path.push(v);
        let i = v;
        while (i != source) {
            i = this.edgeTo[i];
            path.push(i);
        }
        return path;
    }

    hashPathTo(v) {
        return this.marked[v];
    }

    topSort() {
        let stack = [];
        let visited = [];
        for (let i = 0; i < this.vertices; i++) {
            visited[i] = false;
        }
        for (let i = 0; i < this.vertices; i++) {
            if (visited[i] == false) {
                this.topSortHelper(i, visited, stack);
            }
        }
        for (let i = 0, len = stack.length; i < len; i++) {
            if (stack[i] !== undefined && stack[i] !== false) {
                console.log(this.vertexList[stack[i]]);
            }
        }
    }

    topSortHelper(v, visited, stack) {
        visited[v] = true;
        if (Object.prototype.toString.call(this.adj[v]) === '[object Array]') {
            this.adj[v].map((w) => {
                if (!visited[w]) {
                    this.topSortHelper(visited[w], visited, stack);
                }
            });
        }
        stack.push(v);
    }
}
```

## 使用方法
### 深度优先搜索
```
//深度优先搜索
let g = new Graph(5);
g.addEdge(0, 1);
g.addEdge(0, 2);
g.addEdge(1, 3);
g.addEdge(2, 4);
g.showGraph();
g.dfs(0);
```

### 广度优先搜索
```
//广度优先搜索
let g = new Graph(5);
g.addEdge(0, 1);
g.addEdge(0, 2);
g.addEdge(1, 3);
g.addEdge(2, 4);
g.showGraph();
g.bfs(0);
```

### 广度优先搜索查找最短路径
```
//广度优先搜索查找最短路径
let g = new Graph(5);
g.addEdge(0, 1);
g.addEdge(0, 2);
g.addEdge(1, 3);
g.addEdge(2, 4);

let vertex = 4;
g.bfs(0);
let paths = g.pathTo(vertex);
let str = "";
while (paths.length > 0) {
    if (paths.length > 1) {
        str += paths.pop() + '-';
    } else {
        str += paths.pop();
    }
}
console.log(str);
```

### 拓扑排序法
```
//拓扑排序算法
let g = new Graph(6);
g.addEdge(1, 2);
g.addEdge(2, 5);
g.addEdge(1, 3);
g.addEdge(1, 4);
g.addEdge(0, 1);
g.vertexList = ["CS1", "CS2", "Data Structures", "Assembly Language", "Operating System", "Algorithms"];
g.showGraph();
g.topSort();
```

### 测试广度优先算法和深度优先算法速度
```
//测试广度优先算法和深度优先算法速度
let d = new Graph(100);
for(let i = 0; i < 10; i++){
    d.addEdge(0, (i + 1));
    d.addEdge(1, (i + 10));
    d.addEdge((i + 10), (i + 20));
    d.addEdge(3, (i + 30));
    d.addEdge((i + 30), (i + 40));
    d.addEdge((i + 30), (i + 50));
    d.addEdge((i + 50), (i + 60));
    d.addEdge(4, (i + 70));
    d.addEdge((i + 70), (i + 80));
    d.addEdge((i + 80), (i + 90));
}
let timer = new Date().getTime();
d.dfs(0);
console.log("using time: " + (new Date().getTime() - timer));
console.log();
let g = new Graph(100);
for(let i = 0; i < 10; i++){
    g.addEdge(0, (i + 1));
    g.addEdge(1, (i + 10));
    g.addEdge((i + 10), (i + 20));
    g.addEdge(3, (i + 30));
    g.addEdge((i + 30), (i + 40));
    g.addEdge((i + 30), (i + 50));
    g.addEdge((i + 50), (i + 60));
    g.addEdge(4, (i + 70));
    g.addEdge((i + 70), (i + 80));
    g.addEdge((i + 80), (i + 90));
}
timer = new Date().getTime();
g.bfs(0);
console.log("using time: " + (new Date().getTime() - timer));
```

### 存储图数据
```
//存储图
let g = new Graph(100);
for (let i = 0; i < 10; i++) {
    g.addEdge(0, (i + 1));
    g.addEdge(1, (i + 10));
    g.addEdge((i + 10), (i + 20));
    g.addEdge(3, (i + 30));
    g.addEdge((i + 30), (i + 40));
    g.addEdge((i + 30), (i + 50));
    g.addEdge((i + 50), (i + 60));
    g.addEdge(4, (i + 70));
    g.addEdge((i + 70), (i + 80));
    g.addEdge((i + 80), (i + 90));
}
g.save('./graphData.txt');
```

### 读取图数据
```
//读取图文件
let graphData = fs.readFileSync('./graphData.txt', {'encoding': "utf-8"}).split("\n");
let g = new Graph(graphData.length);
graphData.map((val) => {
    let graphs = val.split(" -> ");
    let arr = graphs[1].split(" ");
    arr.map((v) => {
        console.log(graphs[0], v);
        g.addEdge(graphs[0], v);
    })
});
g.showGraph();
```

### 北京二环内地铁线路建模 查找最有路线
```
//北京地铁线路建模
let g = new Graph(33);
g.addEdge(0, 1);
g.addEdge(0, 17);
g.addEdge(0, 18);
g.addEdge(1, 2);
g.addEdge(1, 19);
g.addEdge(2, 3);
g.addEdge(3, 22);
g.addEdge(3, 4);
g.addEdge(4, 5);
g.addEdge(5, 22);
g.addEdge(5, 6);
g.addEdge(6, 7);
g.addEdge(7, 8);
g.addEdge(8, 29);
g.addEdge(8, 9);
g.addEdge(9, 10);
g.addEdge(10, 29);
g.addEdge(10, 11);
g.addEdge(11, 12);
g.addEdge(11, 27);
g.addEdge(12, 13);
g.addEdge(13, 14);
g.addEdge(14, 25);
g.addEdge(14, 15);
g.addEdge(15, 16);
g.addEdge(16, 17);
g.addEdge(16, 17);
g.addEdge(18, 19);
g.addEdge(19, 20);
g.addEdge(19, 23);
g.addEdge(20, 21);
g.addEdge(21, 22);
g.addEdge(22, 30);
g.addEdge(23, 24);
g.addEdge(24, 27);
g.addEdge(25, 26);
g.addEdge(26, 27);
g.addEdge(27, 28);
g.addEdge(28, 29);
g.addEdge(29, 32);
g.addEdge(32, 31);
g.addEdge(31, 30);
g.vertexList = ["西直门", "车公庄", "阜成门", "复兴门", "长椿街", "宣武门", "和平门", "前门", "崇文门",
    "北京站", "建国门", "朝阳门", "东四十条", "东直门", "雍和宫", "安定门", "鼓楼大街", "积水潭", "新街口",
    "平安里", "西四", '灵境胡同', "西单", "北海", "南锣鼓巷", "北新桥", "张自忠路", "东四", "灯市口", "东单",
    "天安门西", "天安门东", "王府井"];
let vertex = 9;
let start = 1;
g.bfs(start);
let paths = g.pathTo(vertex, start);
let str = "";
while (paths.length > 0) {
    if (paths.length > 1) {
        str += paths.pop() + '-';
    } else {
        str += paths.pop();
    }
}
console.log(str);
//广度优先搜索可以得出最优路线，深度优先搜索可以得到接近路线（但不是最优，搜索速度快）
```

参考自《数据结构与算法 JavasScript描述》