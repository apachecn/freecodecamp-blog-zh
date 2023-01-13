# JavaScript 中的 try/Catch——如何处理 JS 中的错误

> 原文：<https://www.freecodecamp.org/news/try-catch-in-javascript/>

在编程中，bug 和错误是不可避免的。我的一个朋友称它们为**未知功能**:)。

随你怎么称呼它们，但是我真的相信 bug 是让我们程序员的工作变得有趣的原因之一。

我的意思是，无论你在一夜之间试图调试一些代码时有多么沮丧，我非常肯定当你发现问题是你忽略的一个简单的逗号，或者类似的东西时，你会开怀大笑。虽然，客户报告的错误带来的更多的是皱眉而不是微笑。

也就是说，错误可能是令人讨厌的，也是真正的痛苦。这就是为什么在这篇文章中，我想解释 JavaScript 中叫做 **try / catch** 的东西。

## JavaScript 中的 try/catch 块是什么？

一个 **try / catch** 块主要用于处理 JavaScript 中的错误。当您不希望脚本中的错误破坏您的代码时，您可以使用它。

虽然这看起来像是你可以用一个 if 语句轻松完成的事情，但是 try/catch 给了你很多 if/else 语句所不能提供的好处，其中一些你将在下面看到。

```
try{
//...
}catch(e){
//...
}
```

try 语句允许您测试代码块中的错误。

catch 语句允许您处理该错误。例如:

```
try{ 
getData() // getData is not defined 
}catch(e){
alert(e)
}
```

这就是 try/catch 的基本构造方式。你把你的代码放在 **try 块**中，如果有错误，JavaScript 立即给 **catch** 语句控制权，它就做你说的任何事情。在这种情况下，它会向您发出错误警告。

所有 JavaScript 错误实际上都是包含两个属性的对象:名称(例如，Error、syntaxError 等)和实际的错误消息。这就是为什么当我们提醒 **e** 时，我们会得到类似 **ReferenceError: getData 未定义**的消息。

像 JavaScript 中的其他对象一样，您可以决定以不同的方式访问这些值，例如 **e.name** (ReferenceError)和 **e.message** (getData 未定义)。

但是老实说，这和 JavaScript 的功能并没有什么不同。虽然 JavaScript 会尊重您，将错误记录在控制台中，而不会将警告显示给全世界的人看:)。

那么，try/catch 语句的好处是什么呢？

## 如何使用 try/catch 语句

### `throw`声明

try/catch 的好处之一是它能够显示您自己定制的错误。这叫做 **`(throw error)`** 。

在您不希望 JavaScript 显示这种难看的东西的情况下，您可以使用 **throw 语句**来抛出您的错误(一个异常)。该错误可以是字符串、布尔值或对象。如果有错误，catch 语句将显示您抛出的错误。

```
let num =prompt("insert a number greater than 30 but less than 40")
try { 
if(isNaN(num)) throw "Not a number (☉｡☉)!" 
else if (num>40) throw "Did you even read the instructions ಠ︵ಠ, less than 40"
else if (num <= 30) throw "Greater than 30 (ب_ب)" 
}catch(e){
alert(e) 
}
```

这很好，对吧？但是我们可以更进一步，实际抛出一个 JavaScript 构造函数错误。

基本上，JavaScript 将错误分为六类:

*   **eval error**-eval 函数出现错误。
*   **范围错误** -出现超出范围的数字，例如`1.toPrecision(500)`。`toPrecision`基本上给数字一个十进制数值，比如 1.000，一个数字不能有那个的 500。
*   **ReferenceError** -使用了尚未声明的变量
*   **语法错误**——评估有语法错误的代码时
*   **类型错误**——如果您使用了超出预期类型范围的值:例如`1.toUpperCase()`
*   URI(统一资源标识符)错误——如果你在 URI 函数中使用非法字符，就会抛出一个 URIError。

所以有了这些，我们很容易抛出类似于`throw new Error("Hi there")`的错误。在这种情况下，错误的名称将是**错误**和消息**你好**。您甚至可以创建自己的自定义错误构造函数，例如:

```
function CustomError(message){ 
this.value ="customError";
this.message=message;
}
```

你可以在任何地方用`throw new CustomError("data is not defined")`很容易地使用它。

到目前为止，我们已经了解了 try/catch 以及它如何防止我们的脚本死亡，但这实际上取决于。让我们考虑这个例子:

```
try{ 
console.log({{}}) 
}catch(e){ 
alert(e.message) 
} 
console.log("This should run after the logged details")
```

但是当您尝试它时，即使使用了 try 语句，它仍然不起作用。这是因为 JavaScript 中有两种主要类型的错误(我上面描述的——syntax error 等等——并不是真正的错误类型。你可以把它们称为错误的例子):**解析时错误**和**运行时错误或异常**。

**解析时错误**是发生在代码内部的错误，基本上是因为引擎不理解代码。

举个例子，从上面来看，JavaScript 不理解你说的 **{{}}** 是什么意思，正因为如此，你的 try / catch 在这里就没有用了(不会起作用)。

另一方面，**运行时错误**是发生在有效代码中的错误，这些是 try/catch 肯定会发现的错误。

```
try{ 
y=x+7 
} catch(e){ 
alert("x is not defined")
} 
alert("No need to worry, try catch will handle this to prevent your code from breaking")
```

信不信由你，上面是有效的代码，try /catch 会适当地处理错误。

### `Finally`声明

**finally** 语句的作用类似于中性接地，即 try/ catch 块的基点或最终接地。使用 finally，您基本上是在说**无论 try/catch 中发生了什么(错误或没有错误)，finally 语句中的这段代码都应该运行**。例如:

```
let data=prompt("name")
try{ 
if(data==="") throw new Error("data is empty") 
else alert(`Hi ${data} how do you do today`) 
} catch(e){ 
alert(e) 
} finally { 
alert("welcome to the try catch article")
}
```

### 嵌套 try 块

您也可以嵌套 try 块，但是像 JavaScript 中的其他嵌套一样(例如 if、for 等)，它往往会变得笨拙和不可读，所以我建议不要这样做。但这只是我。

嵌套 try 块的好处是可以对多个 try 语句只使用一个 catch 语句。尽管您也可以决定为每个 try 块编写一个 catch 语句，如下所示:

```
try { 
try { 
throw new Error('oops');
} catch(e){
console.log(e) 
} finally { 
console.log('finally'); 
} 
} catch (ex) { 
console.log('outer '+ex); 
}
```

在这种情况下，外部 try 块不会有任何错误，因为它没有任何问题。错误来自内部 try 块，它已经在处理自己了(它有自己的 catch 语句)。请考虑以下情况:

```
try { 
try { 
throw new Error('inner catch error'); 
} finally {
console.log('finally'); 
} 
} catch (ex) { 
console.log(ex);
}
```

上面这段代码的工作方式略有不同:错误发生在内部 try 块中，没有 catch 语句，而是使用了 finally 语句。

注意 **try/catch** 可以有三种不同的写法:`try...catch`、`try...finally`、`try...catch...finally`)，但是错误是从这个内部 try 抛出的。

这个内部 try 的 finally 语句肯定会起作用，因为就像我们前面说过的，不管 try/catch 中发生什么，它都会起作用。但是，即使外部 try 没有错误，控制权仍然交给它的 catch 来记录错误。更好的是，它使用了我们在内部 try 语句中创建的错误，因为错误来自那里。

如果我们要为外部尝试创建一个错误，它仍然会显示所创建的内部错误，只是内部错误会捕获自己的错误。

您可以通过注释掉内部捕获来摆弄下面的代码。

```
try { 
try { 
throw new Error('inner catch error');
} catch(e){ //comment this catch out
console.log(e) 
} finally { 
console.log('finally'); 
} 
throw new Error("outer catch error") 
} catch (ex) { 
console.log(ex);
}
```

### 再次抛出错误

catch 语句实际上捕获了所有出现的错误，有时我们可能不希望这样。举个例子，

```
"use strict" 
let x=parseInt(prompt("input a number less than 5")) 
try{ 
y=x-10 
if(y>=5) throw new Error(" y is not less than 5") 
else alert(y) 
}catch(e){ 
alert(e) 
}
```

让我们假设输入的数字小于 5(**“use strict”**的目的是表示代码应该在“严格模式”下执行)。使用**严格模式**，你可以不使用未声明的变量([源](https://www.w3schools.com/))。

我希望 try 语句抛出一个错误 **y 不是...**当 y 的值大于 5 接近不可能时。上面的误差应该对**来说 y 不算少...**而非 **y 未定义**。

在这种情况下，您可以检查错误的名称，如果它不是您想要的，**重新抛出它**:

```
"use strict" 
let x = parseInt(prompt("input a number less than 5"))
try{
y=x-10 
if(y>=5) throw new Error(" y is not less than 5") 
else alert(y) 
}catch(e){ 
if(e instanceof ReferenceError){ 
throw e
}else alert(e) 
} 
```

这将简单地**再次抛出错误**,让另一个 try 语句在这里捕获或中断脚本。当您只想监视特定类型的错误，而由于疏忽而可能发生的其他错误会破坏代码时，这很有用。

## 结论

在本文中，我试图解释以下与 try/catch 相关的概念:

*   什么是 try /catch 语句以及它们何时起作用
*   如何抛出自定义错误
*   finally 语句是什么以及它是如何工作的
*   嵌套 try / catch 语句的工作原理
*   如何重新引发错误

感谢您的阅读。在 twitter 上关注我 [@fakoredeDami](https://twitter.com/fakoredeDami) 。