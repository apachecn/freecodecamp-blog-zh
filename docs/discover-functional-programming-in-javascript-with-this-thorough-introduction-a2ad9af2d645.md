# 通过这篇全面的介绍，探索 JavaScript 中的函数式编程

> 原文：<https://www.freecodecamp.org/news/discover-functional-programming-in-javascript-with-this-thorough-introduction-a2ad9af2d645/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

JavaScript 是第一种将函数式编程带入主流的语言。它有一流的函数和闭包。它们为函数式编程模式开辟了道路。

# 一流的功能

函数是一级对象。函数可以存储在变量、对象或数组中，作为参数传递给其他函数或从函数返回。

```
//stored in variable
function doSomething(){
}

//stored in variable
const doSomething = function (){ };

//stored in property
const obj = { 
   doSomething : function(){ } 
}

//passed as an argument
process(doSomething);

//returned from function
function createGenerator(){
  return function(){
  }
}
```

## 希腊字母的第 11 个

**λ是用作值的函数。**

在 JavaScript 中，函数是一级对象，所以所有函数都可以作为值使用。所有函数都可以是 lambdas，有名字也可以没有名字。我实际上建议支持命名函数。

# 函数数组工具箱

## [基础工具箱](https://jsfiddle.net/lorinoata/s5b9m6ut/)

`filter()` 根据决定应该保留哪些值的谓词函数从列表中选择值。

```
const numbers = [1,2,3,4,5,6];
function isEven(number){
  return number % 2 === 0;
}
const evenNumbers = numbers.filter(isEven);
```

谓词函数是以一个值为输入，根据该值是否满足条件返回`true` / `false`的函数。`isEven()`是谓语功能。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)