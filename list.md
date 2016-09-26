
#list 类型
列表类型例如待办事项、购物清单这样的简单的数据结构

以下是创建的一个简单的list数据结构

```
  function List() {
    this.dataStore = [];//元素数组
    this.pos = 0;//初始位置
    this.listSize = 0;//初始元素长度
  }
  List.prototype = {
    constructor: this,
    //返回长度
    length: function () {
      return this.listSize;
    },
    //清除列表
    clear: function () {
      delete this.dataStore;
      this.dataStore = [];
      this.listSize = this.pos = 0;
    },
    //返回列表数据
    toString: function () {
      return this.dataStore;
    },
    //检测元素包含关系
    contains: function(element){
      for (var i = 0, len = this.dataStore.length; i < len; i++){
        if (this.dataStore[i] == element){
          return false;
        }
      }
      return false;
    },
    //获取当前元素
    getElement: function () {
      return this.dataStore[this.pos];
    },
    //插入元素到特定元素后面
    insert: function (element, after) {
      var insertPos = this.find(after);
      if (insertPos > -1){
        this.dataStore.splice(insertPos + 1, 0, element);
        ++ this.listSize;
        return true;
      }else {
        return false;
      }
    },
    //插入元素
    append: function (element) {
      this.dataStore[this.listSize++] = element;
    },
    //移除特定元素
    remove: function (element) {
      var findAt = this.find(element);
      if (findAt > -1) {
        this.dataStore.splice(findAt, 1);
        --this.listSize;
        return true;
      } else {
        return false;
      }
    },
    //查找特定元素位置
    find: function (element) {
      for (var i = 0, len = this.dataStore.length; i++;) {
        if (this.dataStore[i] == element) {
          return i;
        }
      }
      return -i;
    },
    //位置移动到最前端
    front: function () {
      this.pos = 0;
    },
    //位置移动到最末位置
    end: function () {
      this.pos = this.listSize - 1;
    },
    //位置前移一个单位
    prev: function () {
      if (this.pos > 0){
        -- this.pos;
      }
    },
    //位置后移一个单位
    next: function () {
      if(this.pos < (this.listSize - 1)){
        ++this.pos;
      }
    },
    //返回当前位置
    currPos: function () {
      return this.pos;
    },
    //移动到指定位置
    moveTo: function (position) {
      this.pos = position;
    }
  };
  ```
参考自《数据结构与算法 JavasScript描述》
