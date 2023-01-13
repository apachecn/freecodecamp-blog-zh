# 发现一流功能的威力

> 原文：<https://www.freecodecamp.org/news/discover-the-power-of-first-class-functions-fd0d7b599b69/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

在 JavaScript 中，函数是一级对象，这意味着它们可以是:

*   存储在变量、对象或数组中
*   作为参数传递给函数
*   从函数返回

### 存储功能

函数可以三种方式存储:

*   存储在变量中:`let fn = function doSomething() {}`
*   存储在一个对象中:`let obj = { doSomething : function(){} }`
*   存储在数组中:`arr.push(function doSomething() {})`

在第一个和第三个例子中，我使用了一个命名函数表达式。

函数表达式将函数定义为更大表达式的一部分。这一行代码不是以`function`开头的。

### 作为一个参数

在下一个例子中，函数`doSomething`被作为参数发送给`doAction()`。

```
doAction(function doSomething(){});
```

`doSomething`是回调。

回调是作为参数传递给另一个函数的函数。

#### 高阶函数

> 高阶函数是将另一个函数作为输入、返回一个函数或两者都做的函数。

您可以在[探索功能 JavaScript](https://www.amazon.com/dp/B07PBQJYYG) 一书中找到更多信息。

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)