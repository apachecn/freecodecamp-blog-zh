# JavaScript 中的生活:什么是直接调用的函数表达式？

> 原文：<https://www.freecodecamp.org/news/iife-in-javascript-what/>

## **功能语句**

用函数声明创建的函数是一个函数对象，具有函数对象的所有属性、方法和行为。示例:

```
 function statement(item){
    console.log('Function statement example '+ item);
  }
```

## **函数表达式**

函数表达式类似于函数语句，只是在创建匿名函数时可以省略函数名。示例:

```
 var expression = function (item){
    console.log('Function expression example '+ item);
  }
```

## **立即调用函数表达式**

一旦函数被创建，它就调用它自己，不需要显式调用。在下面的例子中，变量 iife 将存储一个由函数执行返回的字符串。

```
 var iife = function (){
    return 'Immediately Invoked Function Expressions(IIFEs) example ';
  }();
  console.log(iife); // 'Immediately Invoked Function Expressions(IIFEs) example '
```

生命之前的陈述应该总是以 a 结尾；否则它会抛出一个错误。

****坏榜样**** :

```
var x = 2 //no semicolon, will throw error
(function(y){
  return x;
})(x); //Uncaught TypeError: 2 is not a function
```

## **为什么使用立即调用函数表达式？**

```
 (function(value){
    var greet = 'Hello';
    console.log(greet+ ' ' + value);
  })('IIFEs');
```

在上面的例子中，当 javascript 引擎执行上述代码时，它会在看到代码时创建全局执行上下文，并在内存中为 IIFE 创建函数对象。当它到达第`46`行时，由于哪个函数被调用，一个新的执行上下文被动态创建，因此 greet 变量进入那个函数执行上下文，而不是进入全局变量，这就是它的独特之处。因此代码是安全的。

#### **更多信息**

*   [维基百科上立即调用的函数表达式](https://en.wikipedia.org/wiki/Immediately-invoked_function_expression)
*   [JavaScript 库中的前导分号是做什么的？](https://stackoverflow.com/questions/1873983/what-does-the-leading-semicolon-in-javascript-libraries-do)