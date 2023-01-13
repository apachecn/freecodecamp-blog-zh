# JavaScript 中作用域的介绍

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-scope-in-javascript-cbd957022652/>

范围定义了变量的生存期和可见性。变量在声明它们的范围之外是不可见的。

JavaScript 有模块作用域、函数作用域、块作用域、词法作用域和全局作用域。

### 全球范围

在任何函数、块或模块范围之外定义的变量都具有全局范围。可以从应用程序的任何地方访问全局范围内的变量。

当一个模块系统被启用时，创建全局变量变得更加困难，但是我们仍然可以这样做。通过在 HTML 中定义一个变量，在任何函数之外，可以创建一个全局变量:

```
<script>
  let GLOBAL_DATA = { value : 1};
</script>

console.log(GLOBAL_DATA);
```

当没有模块系统时，创建全局变量要容易得多。在任何文件中任何函数外声明的变量都是全局变量。

全局变量在应用程序的整个生命周期中都是可用的。

创建全局变量的另一种方法是在应用程序的任何地方使用`window`全局对象:

```
window.GLOBAL_DATA = { value: 1 };
```

此时，`GLOBAL_DATA`变量随处可见。

```
console.log(GLOBAL_DATA)
```

你可以想象这些做法是不好的做法。

### 模块范围

在模块之前，在任何函数之外声明的变量都是全局变量。在模块中，在任何函数外声明的变量都是隐藏的，除非显式导出，否则其他模块无法使用。

导出使函数或对象对其他模块可用。在下一个例子中，我从`sequence.js`模块文件中导出一个函数:

```
// in sequence.js
export { sequence, toList, take };
```

导入使其他模块中的函数或对象可用于当前模块。

```
import { sequence, toList, toList } from "./sequence";
```

在某种程度上，我们可以把一个模块想象成一个自执行函数，它把导入数据作为输入，并返回导出数据。

### 功能范围

函数作用域意味着函数中定义的参数和变量在函数内的任何地方都是可见的，但在函数外是不可见的。

考虑下一个自动执行的函数，叫做 IIFE。

```
(function autoexecute() {
    let x = 1;
})();

console.log(x);
//Uncaught ReferenceError: x is not defined
```

IIFE 代表立即调用的函数表达式，是一个在其定义后立即运行的函数。

用`var`声明的变量只有函数作用域。更有甚者，用`var`声明的变量被提升到其作用域的顶部。这样就可以在声明之前访问它们。看看下面的代码:

```
function doSomething(){
  console.log(x);
  var x = 1;
}

doSomething(); //undefined
```

对于`let`，这种情况不会发生。用`let`声明的变量只能在其定义后访问。

```
function doSomething(){
  console.log(x);
  let x = 1;
}

doSomething();
//Uncaught ReferenceError: x is not defined
```

用`var`声明的变量可以在同一个范围内被多次重新声明。以下代码就可以了:

```
function doSomething(){
  var x = 1
  var x = 2;
  console.log(x);
}

doSomething();
```

用`let`或`const`声明的变量不能在同一个范围内重新声明:

```
function doSomething(){
  let x = 1
  let x = 2;
}
//Uncaught SyntaxError: Identifier 'x' has already been declared
```

也许我们甚至不必关心这个，因为`var`已经开始变得过时了。

### 块范围

块范围用花括号定义。由`{`和`}`隔开。

用`let`和`const`声明的变量可以有块范围。只能在定义它们的块中访问它们。

考虑下一个强调`let`块范围的代码:

```
let x = 1;
{ 
  let x = 2;
}
console.log(x); //1
```

相比之下，`var`声明没有块范围:

```
var x = 1;
{ 
  var x = 2;
}
console.log(x); //2
```

没有块范围的另一个常见问题是在循环中使用像`setTimeout()`这样的异步操作。下面的循环代码将数字 5 显示五次。

```
(function run(){
    for(var i=0; i<5; i++){
        setTimeout(function logValue(){
            console.log(i);         //5
        }, 100);
    }
})();
```

带有`let`声明的`for`循环语句为每次迭代的块范围创建一个新的变量 locale。下一个循环代码显示`0 1 2 3 4 5`。

```
(function run(){
  for(let i=0; i<5; i++){
    setTimeout(function log(){
      console.log(i); //0 1 2 3 4
    }, 100);
  }
})();
```

### 词汇范围

词法范围是内部函数访问定义它的外部范围的能力。

[考虑下一个代码](https://jsfiddle.net/cristi_salcescu/pcg6fab7/):

```
(function autorun(){
    let x = 1;
    function log(){
      console.log(x);
    };

    function run(fn){
      let x = 100;
      fn();
    }

    run(log);//1
})();
```

`log`函数是一个闭包。它从父函数`autorun()`中引用`x`变量，而不是从`run()`函数中引用。

闭包函数可以访问创建它的作用域，但不能访问执行它的作用域。

`autorun()`的局部函数作用域是`log()`函数的词法作用域。

### 范围链

每个作用域都有一个到父作用域的链接。当使用一个变量时，JavaScript 沿着作用域链向下查找，直到找到请求的变量或者到达全局作用域，这是作用域链的末端。

[看下一个例子](https://jsfiddle.net/cristi_salcescu/udq46asp/):

```
let x0 = 0;
(function autorun1(){
 let x1 = 1;

 (function autorun2(){
   let x2 = 2;

   (function autorun3(){
     let x3 = 3;

     console.log(x0 + " " + x1 + " " + x2 + " " + x3);//0 1 2 3
    })();
  })();
})();
```

内部函数`autorun3()`可以访问局部变量`x3`。它还可以从外部函数访问`x1`和`x2`变量以及`x0`全局变量。

如果找不到变量，它将在严格模式下返回一个错误。

```
"use strict";
x = 1;
console.log(x)
//Uncaught ReferenceError: x is not defined
```

在非严格模式下，简称为“草率模式”，它会做一件坏事，创建一个全局变量。

```
x = 1;
console.log(x); //1
```

### 结论

在全局作用域中定义的变量在应用程序中随处可见。

在模块中，在任何函数外声明的变量都是隐藏的，除非显式导出，否则其他模块无法使用。

函数作用域意味着函数中定义的参数和变量在函数中的任何地方都是可见的

用`let`和`const`声明的变量有块范围。`var`没有阻止范围。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)