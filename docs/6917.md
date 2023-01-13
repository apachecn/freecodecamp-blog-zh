# 什么时候(为什么)应该使用 ES6 箭头功能，什么时候不应该使用

> 原文：<https://www.freecodecamp.org/news/when-and-why-you-should-use-es6-arrow-functions-and-when-you-shouldnt-3d851d7f0b26/>

辛西娅·李

# 什么时候(为什么)应该使用 ES6 箭头功能，什么时候不应该使用

![s2aEYgjNLkSMH5RxJRuZrJWhWcor0gmZC5tb](img/02c6d49e192fd13c598b2e013dd08f24.png)

箭头功能(也叫“胖箭头功能”)无疑是 ES6 比较受欢迎的功能之一。他们引入了一种编写简洁函数的新方法。

下面是一个用 ES5 语法编写的函数:

```
function timesTwo(params) {  return params * 2}function timesTwo(params) {
  return params * 2
}

timesTwo(4);  // 8
```

这是用箭头函数表示的同一个函数:

```
var timesTwo = params => params * 2

timesTwo(4);  // 8
```

短多了！由于隐式返回，我们可以省略花括号和 return 语句(但是只有在没有块的情况下——下面会详细介绍)。

与常规的 ES5 函数相比，理解 arrow 函数的不同行为是很重要的。

### 变化

![c1-i0BPczDkbeDybCAzWCHsEyVFX0Ttg5bpL](img/efd699e9aa8a9ad5c3e8b973390e2016.png)

Variety is the spice of life

您将很快注意到的一件事是 arrow 函数中可用的各种语法。让我们来看看一些常见的问题:

#### 1.没有参数

如果没有参数，可以在`=&`gt；前放一个空括号；。

```
() => 42
```

事实上，你甚至不需要括号！

```
_ => 42
```

#### 2.单参数

对于这些函数，括号是可选的:

```
x => 42  || (x) => 42
```

#### **3。多个参数**

这些函数需要括号:

```
(x, y) => 42
```

#### **4。语句(相对于表达式)**

在其最基本的形式中，*函数表达式*产生一个值，而*函数语句*执行一个动作。

对于 arrow 函数，记住语句需要有花括号是很重要的。一旦花括号出现，你总是需要写`return` 。

以下是 if 语句中使用的 arrow 函数的示例:

```
var feedTheCat = (cat) => {
  if (cat === 'hungry') {
    return 'Feed the cat';
  } else {
    return 'Do not feed the cat';
  }
}
```

#### **5。【块体】**

如果您的函数在一个块中，您还必须使用显式的`return`语句:

```
var addValues = (x, y) => {
  return x + y
}
```

#### **6。对象文字**

如果你返回一个对象文字，它需要用括号括起来。这迫使解释器评估括号内的内容，并返回对象文字。

```
x =>({ y: x })
```

### 语法上匿名

![hS7maItiZiV0IIYACtt0PiD3VStILiS1n4sd](img/5453e808ca8aa753be23c4d21178733a.png)

需要注意的是，箭头函数是匿名的，这意味着它们没有命名。

这种匿名产生了一些问题:

1.  更难调试

当您得到一个错误时，您将无法跟踪函数的名称或者错误发生的确切行号。

2.没有自我引用

如果你的函数需要在任何地方有一个自引用(比如递归，需要解除绑定的事件处理程序)，它就不会工作。

### 主要好处:没有“这个”的束缚

![3Rc2e8J5whHdFrH3IzPckp5GCQ-QtMvEOH1k](img/38f96ba2c5bd29ac4b0d28fc9cd1f10f.png)

Photo by [davide ragusa](https://unsplash.com/@davideragusa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在经典函数表达式中，`this`关键字根据调用它的*上下文*被绑定到不同的值。然而对于 arrow 函数，`this`是*的词汇界限*。这意味着它使用了包含箭头函数的代码中的`this`。

比如看下面的`setTimeout`函数:

```
// ES5
var obj = {
  id: 42,
  counter: function counter() {
    setTimeout(function() {
      console.log(this.id);
    }.bind(this), 1000);
  }
};
```

在 ES5 的例子中，`.bind(this)`需要帮助将`this`上下文传递给函数。否则，默认情况下`this`将是未定义的。

```
// ES6
var obj = {
  id: 42,
  counter: function counter() {
    setTimeout(() => {
      console.log(this.id);
    }, 1000);
  }
};
```

ES6 arrow 函数不能绑定到一个`this`关键字，所以它将在词汇上向上一个作用域，并在定义它的作用域中使用`this`的值。

### 当您不应该使用箭头功能时

在学习了更多关于箭头函数的知识后，我希望你明白它们并不能取代常规函数。

以下是一些您可能不想使用它们的情况:

1.  对象方法

当你呼叫`cat.jumps`的时候，生命的数量并没有减少。这是因为`this`没有绑定到任何东西，并将从其父作用域继承`this`的值。

```
var cat = {
  lives: 9,
  jumps: () => {
    this.lives--;
  }
}
```

2.具有动态上下文的回调函数

如果你需要你的上下文是动态的，箭头函数不是正确的选择。看看下面这个事件处理程序:

```
var button = document.getElementById('press');
button.addEventListener('click', () => {
  this.classList.toggle('on');
});
```

如果我们点击按钮，我们会得到一个类型错误。这是因为`this`没有绑定到按钮，而是绑定到了它的父作用域。

3.当它降低了代码的可读性时

有必要考虑一下我们前面提到的各种语法。有了常规函数，人们就知道会发生什么。使用箭头功能，可能很难直接解读你正在看的东西。

### 何时应该使用它们

箭头函数最适合需要绑定到上下文的任何东西，而不是函数本身。

尽管它们是匿名的，但我也喜欢将它们与`map`和`reduce`等方法一起使用，因为我认为这使我的代码更具可读性。对我来说，利大于弊。

谢谢你看我的文章，如果你喜欢就分享吧！查看我的其他文章，如[我如何构建我的番茄钟应用程序，以及我在这个过程中吸取的教训](https://www.freecodecamp.org/news/how-i-built-my-pomodoro-clock-app-and-the-lessons-i-learned-along-the-way-51288983f5ee/)，以及[让我们揭开 JavaScript 的“新”关键字](https://www.freecodecamp.org/news/demystifying-javascripts-new-keyword-874df126184c/)的神秘面纱。