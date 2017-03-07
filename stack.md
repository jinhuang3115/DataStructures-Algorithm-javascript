# 栈
栈就是和列表类似的一种数据结构，它可用来解决计算机世界里的很多问题。栈是一种高 效的数据结构，因为数据只能在栈顶添加或删除，所以这样的操作很快，而且容易实现。 栈的使用遍布程序语言实现的方方面面，从表达式求值到处理函数调用。

## 创建栈
```
function Stack() {
        this.dataStore = [];
        this.top = 0;
    }

    Stack.prototype = {
        constructor: this,
        push: function (element) {
            this.dataStore[this.top++] = element;
        },
        pop: function () {
            return this.dataStore[--this.top];
        },
        peek: function () {
            return this.dataStore[this.top - 1];
        },
        length: function () {
            return this.top;
        },
        clear: function () {
            this.top = 0;
        }
    };
```
## 栈的应用
### 1. 数制转换
```
//进制转换2-9
    function mulBase(num, base) {
        var s = new Stack();
        var converted = "";
        do {
            s.push(num % base);
            num = Math.floor(num /= base);
        } while (num > 0);

        while (s.length() > 0) {
            converted += s.pop();
        }
        return converted;
    }

    var num = 32;
    var base = 2;
    var newNum = mulBase(num, base);
    console.log(num + " converted to base " + base + " is " + newNum);
    num = 125;
    base = 8;
    var newNum = mulBase(num, base)
    console.log(num + " converted to base " + base + " is " + newNum);
```
### 2. 回文
```
    //判断是否是回文
    function isPalindrome(word) {
        var s = new Stack();
        for (var i = 0; i < word.length; ++i) {
            s.push(word[i]);
        }
        var rword = "";
        while (s.length() > 0) {
            rword += s.pop();
        }
        if (word == rword) {
            return true;
        } else {
            return false;
        }
    }
    var word = "hello";
    if (isPalindrome(word)) {
        console.log(word + " is a palindrome.");
    }
    else {
        console.log(word + " is not a palindrome.");
    }
    word = "racecar";
    if (isPalindrome(word)) {
        console.log(word + " is a palindrome.");
    }
    else {
        console.log(word + " is not a palindrome.");
    }
    word = "1001";
    if (isPalindrome(word)) {
        console.log(word + " is a palindrome.");
    }
    else {
        console.log(word + " is not a palindrome.");
    }
 ```
 ### 3. 阶乘
 ```
 //阶乘
    function fact(n) {
        var s = new Stack();
        while (n > 1) {
            s.push(n--);
        }
        var product = 1;
        while (s.length() > 0) {
            product *= s.pop();
        }
        return product;
    }
    console.log(fact(5));
```
### 4. 检查表达式中是否有括号缺失
```
function checkVaild(expression) {
        var str = expression.split("");
        var stack = new Stack();
        for (var i = 0, len = str.length; i < len; i++) {
            if (str[i] === "(") {
                stack.push(i);
            } else if (str[i] === ")") {
                stack.pop(i);
            }
        }
        if (stack.top !== 0) {
            return stack.peek();
        } else {
            return "通过"
        }
    }
    console.log(checkVaild("2.3 + 23 / 12 + (3.14159×0.24"));
```

### 5. 中缀表达式转后缀表达式
```
function suffixOperator(operator) {
        var operatorSymbol = new Stack();
        var operatorNum = new Stack();
        var char = 0;
        var nums = "";
        
        for (var i = 0, len = operator.length; i < len; i++) {
            char = parseInt(operator.charAt(i), 10);
            if (char >= 0 && char <= 9) {
                if (i === (len - 1)){
                    operatorNum.push(char);
                }
                nums += char;
            }else {
                var currentSymbol = "";
                if (operatorSymbol.peek()){
                    if (operator.charAt(i).indexOf('*') > -1 || operator.charAt(i).indexOf('/') > -1){
                        if (operatorSymbol.peek().indexOf('+') > -1 || operatorSymbol.peek().indexOf('-') > -1){
                            currentSymbol = operatorSymbol.pop();
                            operatorSymbol.push(operator.charAt(i));
                            operatorSymbol.push(currentSymbol);
                        }
                    }else {
                        operatorSymbol.push(operator.charAt(i));
                    }
                }else {
                    operatorSymbol.push(operator.charAt(i));
                }
                operatorNum.push(parseInt(nums));
                nums = "";
                
            }
        }

        var oper = new Stack();
        while(operatorNum.top){
            if (operatorSymbol.peek()){
                if (operatorSymbol.peek().indexOf('*') > -1 || operatorSymbol.peek().indexOf('/') > -1){
                    while(!isNaN(parseInt(oper.peek(), 10)) || oper.peek() === " "){
                        if (oper.peek() === " "){
                            oper.pop();
                        }else {
                            operatorNum.push(oper.pop());
                        }
                    }
                    oper.push(operatorSymbol.pop());
                    oper.push(operatorNum.pop());
                    oper.push(" ");
                    oper.push(operatorNum.pop());
                }else {
                    oper.push(operatorSymbol.pop());
                    oper.push(operatorNum.pop());
                    oper.push(" ");
                    oper.push(operatorNum.pop());
                    
                }
            }else {
                oper.push(operatorNum.pop());
            }
        }
        var expression = "";
        while(oper.top){
            expression += oper.pop();
        }
        console.log(expression)
    }
    var operator = '8+4-6*2';
    suffixOperator(operator);
```

### 6. 佩兹糖果盒
```
 var sweetBox = new Stack();
    sweetBox.push('red');
    sweetBox.push('yellow');
    sweetBox.push('red');
    sweetBox.push('yellow');
    sweetBox.push('red');
    sweetBox.push('white');
    sweetBox.push('yellow');
    sweetBox.push('white');
    sweetBox.push('yellow');
    sweetBox.push('white');
    sweetBox.push('red');
    
    //取出不喜欢的颜色
    function getColor(element, stack) {
        var getColorStack = new Stack();
        var setColorStack = new Stack();
        while (stack.top) {
            if (stack.peek() == element) {
                getColorStack.push(stack.pop());
            } else {
                setColorStack.push(stack.pop());
            }
        }
        while (setColorStack.top) {
            stack.push(setColorStack.pop());
        }
    }
    getColor('red', sweetBox);
```

参考自《数据结构与算法 JavasScript描述》
