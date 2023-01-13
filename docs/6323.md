# 如何在没有构造函数的情况下在 React 中绑定它

> 原文：<https://www.freecodecamp.org/news/how-to-bind-this-in-react-without-a-constructor-3a694f5d1b34/>

蒂芙尼·怀特

# 如何在没有构造函数的情况下绑定 React 中的`this`

![1*LyzgUAvHq6Z-q_fvqCA5pg](img/4a0c98d36588d2d11a435b39571be871.png)

Photo by Eva Blue on Unsplash

*这篇文章最初发表在我的[博客](https://tiffanywhite.tech/bind-this-without-constructor/)T3 上。*

`this`在 React 中是对当前组件的引用。通常 React 中的`this`会绑定到它的内置方法。当您想要在窗体上更新状态或使用事件处理程序时，您可以这样做:

其中`this.someInput`将状态传递给正在渲染的 React 组件。

然而不幸的是，React 没有将`this`自动绑定到自定义方法。这意味着，如果我想通过获取一些输入来操作 DOM，而这是用普通的 JavaScript 无法做到的，我将创建一个`ref`来做我想做的任何 DOM 修补。

但是因为 React 不自动绑定`this`，下面的代码在记录时会输出 undefined:

为了避免这种情况，我们可以使用`constructor`函数来呈现组件或获得我们想要的状态:

虽然这是在组件上呈现 ref 的一种不错的方式，但是如果您想在一个组件中绑定几个自定义方法呢？它会变得相当混乱…

你明白了。

相反，我们可以将`this`绑定到自定义的 React 方法，方法是通过将一个方法分配给一个箭头函数来声明它:

这将允许我们将`this`的值绑定到`SomeComponent`组件。

#### 希望这有所帮助！

ES6 给了我们类和构造函数，React 马上就利用了它们。你并不总是需要构造函数，知道什么时候用什么时候不用会有所帮助。

#### 既然你在这里！

我时不时写些不引人注目的信。它们是开发人员的信件，比普通的时事通讯更亲密一些。想报名就报名吧。别担心。