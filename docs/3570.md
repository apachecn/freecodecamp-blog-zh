# 用例子解释 JavaScript 中的闭包

> 原文：<https://www.freecodecamp.org/news/closures-in-javascript-explained-with-examples/>

# 什么是闭包？

闭包是函数和声明该函数的词法环境(作用域)的组合。闭包是 Javascript 的一个基本而强大的属性。本文讨论了关于闭包的“如何”和“为什么”:

### **例子**

```
//we have an outer function named walk and an inner function named fly

function walk (){

  var dist = '1780 feet';

  function fly(){
    console.log('At '+dist);
  }

  return fly;
}

var flyFunc = walk(); //calling walk returns the fly function which is being assigned to flyFunc
//you would expect that once the walk function above is run
//you would think that JavaScript has gotten rid of the 'dist' var

flyFunc(); //Logs out 'At 1780 feet'
//but you still can use the function as above 
//this is the power of closures
```

### **另一个例子**

```
function by(propName) {
    return function(a, b) {
        return a[propName] - b[propName];
    }
}

const person1 = {name: 'joe', height: 72};
const person2 = {name: 'rob', height: 70};
const person3 = {name: 'nicholas', height: 66};

const arr_ = [person1, person2, person3];

const arr_sorted = arr_.sort(by('height')); // [ { name: 'nicholas', height: 66 }, { name: 'rob', height: 70 },{ name: 'joe', height: 72 } ]
```

闭包会“记住”它被创建的环境。这个环境由创建闭包时在作用域内的所有局部变量组成。

```
function outside(num) {
  var rememberedVar = num; // In this example, rememberedVar is the lexical environment that the closure 'remembers'
  return function inside() { // This is the function which the closure 'remembers'
    console.log(rememberedVar)
  }
}

var remember1 = outside(7); // remember1 is now a closure which contains rememberedVar = 7 in its lexical environment, and //the function 'inside'
var remember2 = outside(9); // remember2 is now a closure which contains rememberedVar = 9 in its lexical environment, and //the function 'inside'

remember1(); // This now executes the function 'inside' which console.logs(rememberedVar) => 7
remember2(); // This now executes the function 'inside' which console.logs(rememberedVar) => 9 
```

闭包是有用的，因为它让你“记住”数据，然后让你通过返回的函数对数据进行操作。这允许 javascript 模拟其他编程语言中的私有方法。私有方法对于限制对代码的访问以及管理全局命名空间非常有用。

## 私有变量和方法

闭包也可以用来封装私有数据/方法。看一下这个例子:

```
const bankAccount = (initialBalance) => {
  const balance = initialBalance;

  return {
    getBalance: function() {
      return balance;
    },
    deposit: function(amount) {
      balance += amount;
      return balance;
    },
  };
};

const account = bankAccount(100);

account.getBalance(); // 100
account.deposit(10); // 110
```

在这个例子中，我们不能从`bankAccount`函数之外的任何地方访问`balance`，这意味着我们刚刚创建了一个私有变量。封闭在哪里？嗯，想想`bankAccount()`回来了。它实际上返回一个内部有一堆函数的对象，然而当我们调用`account.getBalance()`时，函数能够“记住”它对`balance`的初始引用。这就是闭包的强大之处，函数“记住”它的词法范围(编译时范围)，即使函数是在词法范围之外执行的。

### 模拟块范围的变量。

Javascript 没有块范围变量的概念。也就是说，当在 forloop 内部定义一个变量时，这个变量在 forloop 外部也是可见的。那么闭包如何帮助我们解决这个问题呢？让我们来看看。

```
 var funcs = [];

    for(var i = 0; i < 3; i++){
        funcs[i] = function(){
            console.log('My value is ' + i);  //creating three different functions with different param values.
        }
    }

    for(var j = 0; j < 3; j++){
        funcs[j]();             // My value is 3
                                // My value is 3
                                // My value is 3
    }
```

因为变量 I 没有块范围，所以它在所有三个函数中的值都用循环计数器更新了，并产生了恶意值。闭包可以帮助我们解决这个问题，它创建了函数创建时所处环境的快照，保存了它的状态。

```
 var funcs = [];

    var createFunction = function(val){
	    return function() {console.log("My value: " + val);};
    }

    for (var i = 0; i < 3; i++) {
        funcs[i] = createFunction(i);
    }
    for (var j = 0; j < 3; j++) {
        funcs[j]();                 // My value is 0
                                    // My value is 1
                                    // My value is 2
    }
```

javascript es6+的最新版本有一个名为 let 的新关键字，可以用来给变量一个 blockscope。还有许多函数(forEach)和完整的库(lodash.js)专门用于解决上述问题。它们当然可以提高你的生产力，但是当你试图创造一些大的东西时，了解所有这些问题仍然是非常重要的。

闭包有许多特殊的应用，在创建大型 javascript 程序时非常有用。

1.  模拟私有变量或封装
2.  进行异步服务器端调用
3.  创建块范围的变量。

### 模拟私有变量。

与许多其他语言不同，Javascript 没有允许您在对象中创建封装的实例变量的机制。在构建大中型程序时，拥有公共实例变量会导致很多问题。然而使用闭包，这个问题可以得到缓解。

与前面的例子非常相似，您可以构建返回对象文字的函数，这些函数的方法可以访问对象的局部变量，而不会暴露它们。从而使它们实际上是私有的。

闭包还可以帮助您管理全局名称空间，以避免与全局共享数据的冲突。通常所有的全局变量都是在你的项目中的所有脚本之间共享的，这肯定会给你在构建大中型程序时带来很多麻烦。这就是库和模块作者使用闭包来隐藏整个模块的方法和数据的原因。这被称为模块模式，它使用一个直接调用的函数表达式，该表达式只向外界导出某些功能，从而大大减少了全局引用的数量。

这里有一个模块框架的简短示例。

```
var myModule = (function() = {
    let privateVariable = 'I am a private variable';

    let method1 = function(){ console.log('I am method 1'); };
    let method2 = function(){ console.log('I am method 2, ', privateVariable); };

    return {
        method1: method1,
        method2: method2
    }
}());

myModule.method1(); // I am method 1
myModule.method2(); // I am method 2, I am a private variable
```

闭包对于捕获包含在“记忆”环境中的私有变量的新实例很有用，这些变量只能通过返回的函数或方法来访问。

## 向量

向量可能是 Clojure 中最简单的集合类型。你可以把它想象成 Javascript 中的一个数组。让我们定义一个简单的向量:

```
(def a-vector [1 2 3 4 5])
;; Alternatively, use the vector function:
(def another-vector (vector 1 2 3 4 5))
;; You can use commas to separate items, since Clojure treats them as whitespace.
(def comma-vector [1, 2, 3, 4, 5])
```

你会看到它使用了方括号，就像 JS 中的数组一样。因为 Clojure 和 JS 一样是动态类型的，所以 vector 可以保存任何类型的元素，包括其他 vector。

```
(def mixed-type-vector [1 "foo" :bar ["spam" 22] #"^baz$"])
```

### 向向量添加项目

您可以使用`conj`将项目添加到向量中。你也可以使用`into`添加到列表中，但是注意`into`是用来合并两个向量的，所以它的两个参数都必须是向量，使用`into`比使用`conj`要慢。

```
(time (conj [1 2] 3))
; => "Elapsed time: 0.032206 msecs"
;    [1 2 3]
(time (into [1] [2 3]))
; => "Elapsed time: 0.078499 msecs"
;    [1 2 3]
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

我想到了！

### 从向量中检索项目

您可以使用`get`从向量中检索项目。这相当于在许多命令式语言中使用括号符号来访问数组中的项。vector 中的项目索引为 0，从左侧开始计数。

```
var arr = [1, 2, 3, 4, 5];
arr[0];
// => 1
```

在 Clojure 中，应该这样写:

```
(def a-vector [1 2 3 4 5])
(get a-vector 0)
; => 1
```

如果你给它一个不在数组中的索引，你也可以给`get`一个默认值。

```
;; the list doesn't have 2147483647 elements, so it'll return a string instead.
(get a-vector 2147483646 "sorry, not found!")
; => "sorry, not found!"
```

### 将其他集合转换为向量

非向量数据结构可以使用`vec`函数转换成向量。对于 hashmaps，这会产生一个包含成对的键和值的 2D 向量。

```
(vec '(1 2 3 4 5))
; => [1 2 3 4 5]
(vec {:jack "black" :barry "white"})
; => [[:jack "black"] [:barry "white"]]
```

### 何时使用向量？

如果需要集合，几乎在所有情况下都应该使用 vector，因为它们具有最短的随机访问时间，这使得从 vector 中检索项目变得容易。注意向量是有序的。如果顺序不重要，用一套可能更好。还要注意，向量是为追加项目而设计的；如果您需要预先计划项目，您可能需要使用列表。

## 关于闭包的更多信息:

*   [六分钟学会 JavaScript 闭包](https://www.freecodecamp.org/news/learn-javascript-closures-in-n-minutes/)
*   JavaScript 中闭包的基本指南
*   [发现 VueJS 中闭包的威力](https://www.freecodecamp.org/news/closures-vuejs-higher-order-functions-emojipicker-f10d3c249a12/)
*   [通过邮寄一个包来解释 JavaScript 闭包](https://www.freecodecamp.org/news/javascript-closures-explained-by-mailing-a-package-4f23e9885039/)