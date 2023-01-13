# 如何在 React 中使用功能组件

> 原文：<https://www.freecodecamp.org/news/a-few-questions-on-functional-components/>

您是否想知道如何在 React 中创建组件？

要回答这个问题，就像创建一个返回类似 HTML 语法的函数一样简单。

```
import React from 'react';

function Counter({n}) {
  return (
    <div>{n}</div>
  );
}

export default Counter;
```

现在让我们看看上面的代码发生了什么。`Counter`是一个将数字转换成 HTML 的函数。而如果你看得更仔细一点，`Counter`就是一个纯函数。没错，就是那种基于输入返回结果并且没有副作用的函数。

这个解释带来了一个新问题。什么是副作用？

简而言之，副作用是对函数外部环境的任何修改，或者从外部环境读取的任何可以改变的信息。

你可能已经注意到我在参数列表中使用了[析构赋值语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)来提取`n`输入数字。这是因为组件接受一个名为“props”的对象作为输入，这个对象具有发送给它们的所有属性。

以下是如何从任何其他组件设置`n`参数:

```
<Counter n={1} />
```

在某种意义上，这个语法可以想象成一个函数调用`Counter({n: 1})`。对不对？

让我们继续我们的旅程。

功能组件可以有状态吗？正如组件名所暗示的，我想存储和更改一个计数器。如果我们在组件中声明一个保存数字的变量会怎么样？行得通吗？

让我们找出答案。

我将从声明函数组件内部的变量开始。

```
import React from 'react';

function Counter() {
  let n = 0;
  return (
    <div>{n}</div>
  );
}

export default Counter;
```

现在让我们添加增加数字并将其记录到控制台的函数。我将使用该函数作为 click 事件的事件处理程序。

```
import React from 'react';

function Counter() {
  let n = 0;

  function increment(){
    n = n + 1;
    console.log(n)
  }

  return (
      <div>
        <span>{n}</span>
        <button onClick={increment}>Increment </button>
      </div>
  );
}

export default Counter;
```

如果我们查看控制台，我们会看到数字实际上是递增的，但这并没有反映在屏幕上。有什么想法吗？

你答对了…我们需要改变数字，但我们也需要在屏幕上重新呈现它。

这里是[反应钩子](https://reactjs.org/docs/hooks-intro.html)的效用函数发挥作用的地方。顺便说一下，这些实用函数被称为钩子，它们以单词“use”开头。我们将使用其中的一个，[使用状态](https://reactjs.org/docs/hooks-state.html)。我还会将“重新呈现”文本记录到控制台，以查看实际调用了多少次`Counter`函数。

```
import React, { useState } from 'react';

function Counter() {
  const [n, setN] = useState(0);

  console.log('re-render');

  function increment(){
    setN(n + 1);
    console.log(n)
  }

  return (
    <div>
        <span>{n}</span>
        <button onClick={increment}>Increment </button>
    </div>
  );
}

export default Counter;
```

让我们看看`useState()`是做什么的。

**`****useState****`****返回什么？**** 它返回一对值:当前状态和更新它的函数。**

**在我们的例子中，`n`是当前状态，`setN()`是更新它的函数。你是否检查过控制台，看看“重新渲染”文本显示了多少次？我会让你自己去发现。**

**我们不仅可以通过设置新值来更新状态，还可以通过提供返回新值的函数来更新状态。**

**在我们的例子中，提供新值的函数将被称为`increment()`。如你所见，`increment()`是一个纯函数。**

```
`import React, { useState } from 'react';

function increment(n){
  return n + 1;
}

function Counter() {
  const [n, setN] = useState(0);

  return (
    <div>
        <span>{n}</span>
        <button 
         onClick={() => setN(increment)}>
           Increment 
        </button>
    </div>
  );
}

export default Counter;`
```

**为了理解`setN(increment)`做什么，让我们阅读文档。**

****传递一个更新函数允许你访问更新器内部的当前状态值。****

**好的，那么用当前的`n`状态调用`increment()`,并用它来计算新的状态值。**

# **最后的想法**

**让我们总结一下我们的发现。**

**在 React 中，我们可以简单地使用返回类似 HTML 语法的函数来定义组件。**

**React Hooks 使我们能够将状态定义到这样的功能组件中。**

**最后但同样重要的是，我们最终消除了组件中的`this`伪参数。也许你已经注意到了`this`在你意想不到的时候通过改变上下文变得令人讨厌。这点不用担心。我们不打算在功能组件中使用`this`。**

**如果你已经做到这一步，你也可以看看我的书。**

**[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为[****最佳函数式编程书籍之一！****](https://bookauthority.org/books/best-functional-programming-books)******

关于将函数式编程技术应用于 React 的更多信息，请看一下 ****[函数式 React](https://www.amazon.com/dp/B088FZQ1XN) 。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[把你的反馈发给我](https://twitter.com/cristi_salcescu)。