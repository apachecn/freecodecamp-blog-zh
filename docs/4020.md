# JavaScript 中的回调函数是什么？

> 原文：<https://www.freecodecamp.org/news/what-is-a-callback-function-in-javascript/>

本文简要介绍了 JavaScript 编程语言中回调函数的概念和用法。

## **函数是对象**

我们需要知道的第一件事是，在 Javascript 中，函数是一级对象。因此，我们可以像处理其他对象一样处理它们，比如将它们赋给变量并将它们作为参数传递给其他函数。这很重要，因为后一种技术允许我们在应用程序中扩展功能。

## **回调函数**

一个 ****回调函数**** 是一个作为参数传递给另一个函数*的函数，在稍后的时间被“回调”。接受其他函数作为参数的函数称为 ****高阶函数**** ，它包含当*回调函数被执行时*的逻辑。这两者的结合使我们能够扩展我们的功能。*

为了说明回调，让我们从一个简单的例子开始:

```
function createQuote(quote, callback){ 
  var myQuote = "Like I always say, " + quote;
  callback(myQuote); // 2
}

function logQuote(quote){
  console.log(quote);
}

createQuote("eat your vegetables!", logQuote); // 1

// Result in console: 
// Like I always say, eat your vegetables!
```

在上面的例子中，`createQuote`是高阶函数，它接受两个参数，第二个参数是回调。`logQuote`函数被用来作为我们的回调函数传入。当我们执行`createQuote`函数 *(1)* 时，请注意，当我们将其作为参数传入时，*没有将*括号附加到`logQuote`上。这是因为我们不想马上执行回调函数，我们只想将函数定义传递给更高阶的函数，以便稍后执行。

此外，我们需要确保如果我们传入的回调函数需要参数，我们在执行回调 *(2)* 时提供这些参数。在上面的例子中，那将是`callback(myQuote);`语句，因为我们知道`logQuote`期望传入一个报价。

此外，我们可以将匿名函数作为回调函数传入。下面对`createQuote`的调用会产生与上面例子相同的结果:

```
createQuote("eat your vegetables!", function(quote){ 
  console.log(quote); 
});
```

顺便提一下，你不需要*让*使用单词“callback”作为你的参数名，Javascript 只需要知道它是正确的参数名。基于上面的例子，下面的函数将以完全相同的方式运行。

```
function createQuote(quote, functionToCall) { 
  var myQuote = "Like I always say, " + quote;
  functionToCall(myQuote);
}
```

## **为什么要使用回调函数？**

大多数时候，我们都在创建以 ****同步**** 方式运行的程序和应用程序。换句话说，我们的一些操作只有在前面的操作完成之后才开始。通常，当我们从其他来源请求数据时，比如一个外部 API，我们并不总是知道*何时*我们的数据会被返回。在这些情况下，我们希望等待响应，但我们不希望在获取数据时整个应用程序陷入停顿。在这些情况下，回调函数就派上了用场。

让我们来看一个模拟向服务器发出请求的示例:

```
function serverRequest(query, callback){
  setTimeout(function(){
    var response = query + "full!";
    callback(response);
  },5000);
}

function getResults(results){
  console.log("Response from the server: " + results);
}

serverRequest("The glass is half ", getResults);

// Result in console after 5 second delay:
// Response from the server: The glass is half full!
```

在上面的例子中，我们向服务器发出一个模拟请求。5 秒钟后，响应被修改，然后我们的回调函数`getResults`被执行。要看到这一点，您可以将上述代码复制/粘贴到浏览器的开发工具中并执行它。

同样，如果你已经熟悉了`setTimeout`，那么你一直都在使用回调函数。传入上面例子的`setTimeout`函数调用的匿名函数参数也是回调！因此，该示例的原始回调实际上是由另一个回调执行的。如果可以的话，注意不要嵌套太多的回调，因为这可能会导致所谓的“回调地狱”！顾名思义，这不是一件愉快的事情。