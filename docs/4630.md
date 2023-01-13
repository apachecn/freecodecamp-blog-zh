# 7 分钟深刻理解谄媚

> 原文：<https://www.freecodecamp.org/news/deeply-understand-currying-in-7-minutes/>

埃里克·埃利奥特杰出的[创作软件](https://medium.com/javascript-scene/the-rise-and-fall-and-rise-of-functional-programming-composable-software-c2d91b424c8c)系列最初让我对函数式编程感到兴奋。这是必读的。

在这个系列的某个地方，他提到了*阿谀奉承*。计算机科学和数学都同意这个定义:

> Currying 将多参数函数转换为一元(单参数)函数。

Curried 函数接受许多参数**，一次一个**。所以如果你有

```
greet = (greeting, first, last) => `${greeting}, ${first} ${last}`;

greet('Hello', 'Bruce', 'Wayne'); // Hello, Bruce Wayne 
```

正确的奉承给你

```
curriedGreet = curry(greet);

curriedGreet('Hello')('Bruce')('Wayne'); // Hello, Bruce Wayne 
```

这个三参数函数变成了三个一元函数。当您提供一个参数时，会弹出一个新的函数，期待下一个参数。

![dog-properly-currying-a-function-1](img/8733dc364614cf46c2fa0626e8f4495e.png)

## 合适吗？

我说“适当地迎合”是因为一些`curry`函数在使用上更加灵活。Currying 在理论上很棒，但是在 JavaScript 中为每个参数调用一个函数会很累。

[Ramda 的](https://ramdajs.com/) `curry`函数让你像这样调用`curriedGreet`:

```
// greet requires 3 params: (greeting, first, last)

// these all return a function looking for (first, last)
curriedGreet('Hello');
curriedGreet('Hello')();
curriedGreet()('Hello')()();

// these all return a function looking for (last)
curriedGreet('Hello')('Bruce');
curriedGreet('Hello', 'Bruce');
curriedGreet('Hello')()('Bruce')();

// these return a greeting, since all 3 params were honored
curriedGreet('Hello')('Bruce')('Wayne');
curriedGreet('Hello', 'Bruce', 'Wayne');
curriedGreet('Hello', 'Bruce')()()('Wayne'); 
```

请注意，您可以选择在一次注射中给出多个参数。这种实现在编写代码时更有用。

如上所述，你可以永远不带参数地调用这个函数，它总是返回一个需要剩余参数的函数。

## 这怎么可能？

埃利奥特先生分享了一个与拉姆达非常相似的实现。这是密码，或者他恰当地称之为魔咒:

```
const curry = (f, arr = []) => (...args) =>
  ((a) => (a.length === f.length ? f(...a) : curry(f, a)))([...arr, ...args]); 
```

## 嗯…？

![that-code-is-hard-to-read-cmm](img/71eb81e18f5588781c4b871656b9fb8b.png)

是的，我知道...它简洁得令人难以置信，让我们一起重构和欣赏它。

## 这个版本是一样的

我还在 Chrome 开发工具中添加了一些`debugger`语句来检查它。

```
curry = (originalFunction, initialParams = []) => {
  debugger;

  return (...nextParams) => {
    debugger;

    const curriedFunction = (params) => {
      debugger;

      if (params.length === originalFunction.length) {
        return originalFunction(...params);
      }

      return curry(originalFunction, params);
    };

    return curriedFunction([...initialParams, ...nextParams]);
  };
}; 
```

打开您的[开发者工具](https://developers.google.com/web/tools/chrome-devtools/)并跟随！

## 我们开始吧！

将`greet`和`curry`粘贴到您的控制台中。然后进入`curriedGreet = curry(greet)`开始疯狂。

### 在第二行暂停

![1*8_q3YkOu6fDzIEMY65lqUg](img/4de17bf9a2674e06197c12a7b0a5886e.png)

检查我们的两个参数，我们看到`originalFunction`是`greet`和`initialParams`默认为空数组，因为我们没有提供它。移动到下一个断点，哦，等等…就这样。

没错。刚刚返回一个需要 3 个参数的新函数。在控制台中键入`curriedGreet`来看看我在说什么。

当你玩够了，让我们变得更疯狂一点，做
`sayHello = curriedGreet('Hello')`。

### 在第 4 行暂停

![1*FXCJQi5Numlbis5d_bGsjw](img/265485cc1eae267b0a83d120304c691e.png)

在继续之前，在您的控制台中键入`originalFunction`和`initialParams`。注意，即使我们在一个全新的函数中，我们仍然可以访问这两个参数。这是因为从父函数返回的函数享有其父函数的作用域。

#### 现实生活中的遗传

在父函数传递之后，他们把参数留给他们的孩子使用。有点像现实生活中的继承。

`curry`最初被赋予`originalFunction`和`initialParams`，然后返回一个“子”函数。那两个变量还没有被[处理掉](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)，因为也许那个孩子需要它们。如果他没有，*那么*这个范围就会被清理掉，因为当没有人提到你的时候，那就是你真正死亡的时候。

### 好的，回到第 4 行…

![1*_TFVnxtqgisi1i0q_K3dUQ](img/4444ecc87a7613f055d6dab5dbbcfe64.png)

检查`nextParams`并看到它是`['Hello']`…一个数组？但是我想我们说的是`curriedGreet(‘Hello’)`，不是`curriedGreet(['Hello'])`！

正确:我们用`'Hello'`调用了`curriedGreet`，但是由于 [rest 语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator#Rest_syntax_%28parameters%29)，我们已经将 `'Hello'`变成了`['Hello']`。

### Y THO？！

`curry`是一个通用函数，可以提供 1 个、10 个或 10，000，000 个参数，因此它需要一种方法来引用所有这些参数。像这样使用 rest 语法可以捕获一个数组中的每个参数，这使得`curry`的工作更加容易。

让我们跳到下一条`debugger`语句。

### 现在是 6 号线，但是等一下。

您可能已经注意到，第 12 行实际上运行在第 6 行的`debugger`语句之前。如果没有，靠近点看。我们的程序在第 5 行定义了一个名为`curriedFunction`的函数，在第 12 行使用它，然后*然后*我们点击第 6 行的`debugger`语句。还有`curriedFunction`是用什么调用的？

```
[...initialParams, ...nextParams]; 
```

优优。看第 5 行的`params`，你会看到`['Hello']`。`initialParams`和`nextParams`都是数组，所以我们使用方便的[扩展操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator#Syntax)将它们展平并组合成一个数组。

这是好事发生的地方。

![1*pM31i2QVNxVUiqj9aZzVvg](img/ea6285aca2a38259394ee6fe8d2b40a7.png)

第 7 行说“如果`params`和`originalFunction`长度相同，用我们的参数调用`greet`，我们就完成了。”这提醒了我…

### JavaScript 函数也有长度

这就是`curry`的神奇之处！这就是它决定是否需要更多参数的方式。

在 JavaScript 中，函数的`.length`属性告诉你*它需要多少个参数*。

```
greet.length; // 3

iTakeOneParam = (a) => {};
iTakeTwoParams = (a, b) => {};

iTakeOneParam.length; // 1
iTakeTwoParams.length; // 2 
```

如果我们提供的参数和预期的参数匹配，我们就很好，只需将它们交给原始函数并完成工作！

### 那是 baller？

但是在我们的例子中，参数和函数长度是*而不是*相同。我们只提供了`‘Hello’`，所以`params.length`是 1，`originalFunction.length`是 3，因为`greet`期望 3 个参数:`greeting, first, last`。

### 那么接下来会发生什么？

既然那个`if`语句的计算结果是`false`，代码将跳到第 10 行，并重新调用我们的主`curry`函数。它再次接收`greet`，这次是`'Hello'`，再次开始疯狂。

那是[递归](https://developer.mozilla.org/en-US/docs/Glossary/Recursion)，朋友们。

本质上是一个无限循环的自我调用、渴求参数的函数，直到它们的客人满了才会停止。最热情的款待。

![1*AZKiupYSanV5iJSQWrtUwg](img/bb1a8b69103d6ce91dbb716ebe23085a.png)

### 回到第二行

参数和之前一样，只是这次`initialParams`是`['Hello']`。再次跳过退出循环。在控制台中键入我们的新变量，`sayHello`。这是另一个函数，仍然期待更多的参数，但我们越来越热…

让我们用`sayHelloToJohn = sayHello('John')`打开暖气。

我们又在 4 号线里面了，`nextParams`就是`['John']`。跳到第 6 行的下一个调试器，检查`params`:是`['Hello', 'John']`！？

![1*pej6yZ-vGvA2T9LgIIc-vg](img/9ff31d3955e2443a25a2cdae1b584eaa.png)

### 为什么，为什么，为什么？

因为记住，第 12 行说“嘿`curriedFunction`，他上次给了我`'Hello'`，这次给了我`‘John’`。把它们都放在这个数组里`[...initialParams, ...nextParams]`。

![1*Ljvk2BMLxIsbJ09idgStdg](img/10ba39247a50ac40cfd7caa1ba7b2b58.png)

现在`curriedFunction`再次将这些`params`的`length`与`originalFunction`进行比较，由于`2 < 3`我们移动到第 10 行并再次调用`curry`！当然，我们传递`greet`和我们的两个参数`['Hello', 'John']`

![1*EYv9jdo2id8tynbTI5SXYQ](img/44dbe5db361ae11f0eea4797e5a8927e.png)

我们已经很接近了，让我们结束这一切，重新得到完整的问候！

`sayHelloToJohnDoe = sayHelloToJohn('Doe')`

我想我们知道接下来会发生什么。

![1*tMJvls2j9eAjrCGykVN84g](img/4e3e879815e7a67a43ac286d71381d8c.png)![1*NHm1TMo8Tjk7jQVlGGa9zQ](img/ca59b612a11b06c7128d10d152320cf3.png)

## 事情已经完成了

`greet`得到了他的参数，`curry`停止了循环，我们收到了我们的问候:`Hello, John Doe`。

继续使用这个函数。尝试一次提供多个参数或不提供参数，想怎么疯狂就怎么疯狂。看看在返回你期望的输出之前`curry`要递归多少次。

```
curriedGreet('Hello', 'John', 'Doe');
curriedGreet('Hello', 'John')('Doe');
curriedGreet()()('Hello')()('John')()()()()('Doe'); 
```

非常感谢[埃里克·埃利奥特](https://medium.com/@_ericelliott)把这个介绍给我，更感谢你和我一起欣赏`curry`。下次见！

更多类似的内容，请查看[yazeedb.com](https://yazeedb.com)！