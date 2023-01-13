# JavaScript 闭包教程——闭包和词法范围如何在 JS 中工作

> 原文：<https://www.freecodecamp.org/news/javascript-closure-lexical-scope/>

在 JavaScript 中，人们经常混淆闭包和词法范围。

词法范围是闭包的重要组成部分，但它本身不是闭包。

闭包是一个高级概念，也是技术访谈的一个常见话题。

在试图理解闭包之前，您应该对函数有一个基本的了解。

读完这篇文章后，我希望我能帮助你了解以下内容:

*   词法范围和闭包的区别。
*   为什么闭包需要词法范围。
*   面试过程中如何举一个封闭的例子？

## JavaScript 中的词法作用域是什么？

词法作用域描述嵌套(也称为“子”)函数如何访问父作用域中定义的变量。

```
const myFunction = () => {
     let myValue = 2;
     console.log(myValue);

     const childFunction = () => {
          console.log(myValue += 1);
     }

     childFunction();
}

myFunction();
```

在这个例子中，`childFunction`可以访问在`myFunction`的父作用域中定义的变量`myValue`。

`childFunction`的词法作用域允许访问父作用域。

## JavaScript 中的闭包是什么？

[w3Schools.com](https://www.w3schools.com/js/js_function_closures.asp)给出了一个关于封闭的伟大定义:

> 闭包是一个可以访问父作用域的函数，即使父函数已经关闭。

让我们注意逗号前的句子的第一部分:

> ...可以访问父作用域的函数

那是描述词法范围！

但是我们需要定义的第二部分来给出一个闭包的例子...

> ...即使在父函数关闭之后。

让我们来看一个闭包的例子:

```
const myFunction = () => {
     let myValue = 2;
     console.log(myValue);

     const childFunction = () => {
          console.log(myValue += 1);
     }

     return childFunction;
}

const result = myFunction();
console.log(result);
result();
result();
result();
```

复制上面的示例代码并尝试一下。

让我们来分析一下正在发生的事情...

在这个版本中，`myFunction`返回`childFunction`而不是调用它。

因此，当`result`设置为等于`myFunction()`时，`myFunction`内的控制台语句被记录，而`childFunction`内的语句不被记录。

`childFunction`不号召行动。

相反，它被返回并保存在`result`中。

此外，我们需要意识到`myFunction`在被调用后已经关闭。

控制台中带有`console.log(result)`的那一行应该显示出`result`现在保存的是匿名函数值，即`childFunction`。

现在，当我们调用`result()`时，我们正在调用分配给`childFunction`的匿名函数。

作为`myFunction`的子函数，这个匿名函数可以访问`myFunction` *中的`myValue`变量，即使它已经关闭了！*

我们现在创建的闭包允许我们在每次调用`result()`时继续增加`myValue`变量的值。

## 用闭包慢慢来

有充分的理由认为闭包是一个先进的概念。

即使一步一步地分解什么是闭包，这个概念也需要时间来理解。

不要急于理解，如果一开始没有意义也不要苛责自己。

当你完全理解了闭包，你可能会觉得自己就像 [Neo 看到矩阵](https://www.google.com/search?q=neo+sees+the+matrix&source=lnms&tbm=isch&sa=X&ved=2ahUKEwiG1MaN1rPxAhUNCM0KHQJWCtAQ_AUoAXoECAEQAw&biw=1762&bih=886)一样。您将看到新的代码可能性，并意识到它们一直就在那里！

我将从我的 YouTube 频道留给你一个关于闭包的教程。我会更深入一点，并提供一些闭包的例子来构建本文的讨论。

[https://www.youtube.com/embed/1S8SBDhA7HA?feature=oembed](https://www.youtube.com/embed/1S8SBDhA7HA?feature=oembed)

Javascript Closure Tutorial