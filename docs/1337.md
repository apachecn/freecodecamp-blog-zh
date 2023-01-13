# TypeScript 文本和集合类型

> 原文：<https://www.freecodecamp.org/news/typescript-literal-and-collective-types/>

TypeScript 通过添加静态类型为我们的代码提供了一定程度的“安全性”。

我们可以通过使某些属性或函数符合类型来保证它们存在于我们的代码中。

这可以极大地减少网站中可能出现的客户端错误，因为它减少了人为错误，比如在错误的对象上调用函数。

TypeScript 通过利用**集合类型**和**文字类型**来做到这一点。

那么，有什么区别呢？

# TypeScript 中的集合类型

集合类型是大多数使用 TypeScript 的开发人员都熟悉的一个概念。例如:

`const addOne = (numb: number) => num + 1;`

这段代码使用了**集合类型**。

**集合类型**为`number`、`string`、`boolean`或`number[]`等类型。

这些类型包含了大量的变量——例如,`number`类型可以包含:1，2，3，4，5...诸如此类。

但是 TypeScript 也为我们提供了这些**集合类型**上更严格的子类型。

# TypeScript 中的文字类型

还可以使用*值*作为类型，所以`let eleven: 11 = 11`是完全有效的 TypeScript 代码。

当我第一次看到这个的时候，我觉得它看起来有点奇怪。

但是它被大量使用，并且确实可以使你的代码更具可读性。

您可以开始构造类似枚举的类型，并且严格地只允许指定某些值，例如:

`type Door = 'open' | 'closed' | 'ajar'`

`Door`类型现在可以在你的代码中使用——比`string`类型允许的值集更严格。

如果上面代码中的`|`不清楚的话，它是一个[活接头类型](https://www.typescriptlang.org/docs/handbook/unions-and-intersections.html)，本质上是指`OR`。任何符合`Door`的类型只能是`open`或者`closed`或者`ajar`。

# 结论

**文字类型**是**集合类型**的子类型。

我们可以说所有的文字类型都是集合类型——但并不是所有的集合类型都是文字类型。为了更清楚，我们可以说**文字类型** `11`是一个`number`，但不是所有的`number`类型都是`11`。

我希望这两种类型之间的区别现在更清楚了，如果你需要严格限制类型，你可以使用**文字类型**。

如果你想了解更多，我在这里发推文。