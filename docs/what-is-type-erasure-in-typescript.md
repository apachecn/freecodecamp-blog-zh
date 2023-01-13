# 什么是 TypeScript 中的类型擦除？

> 原文：<https://www.freecodecamp.org/news/what-is-type-erasure-in-typescript/>

TypeScript 是一种 **transpiled** 语言，在这个过程中有一个步骤叫做**类型擦除**。

那么到底什么是**传输**，什么是**式擦除**？

# 高级与低级编程语言

在我们进一步解释之前，我们必须了解高级和低级语言。

高级语言比低级语言更抽象。我说的抽象是指它们更容易被人类理解。

例如，你会说机器码(二进制)比 JavaScript 更低级，更接近计算机。高级语言通常比低级语言(例如汇编语言)更容易编写和理解，低级语言需要理解和直接处理内存地址等等。

**编译**和**传输**是非常相似的步骤，但它们并不完全相同。我会解释两者，这样你就知道区别了。

## 什么是编译？

编译是一个包罗万象的术语，指的是将你编写的代码转换成一些低级别的计算机可执行程序(通常是机器码)。

一些编译语言的例子是 Java、C#或 C。有时它是分多个步骤编译的，每个步骤都优化代码，并且每次“通过”都使代码更接近机器码。

通过这个过程，一种更接近人类可读的高级语言最终变得“低级”或更接近二进制。

## 什么是运输？

Transpilers 有时被称为“源代码到源代码的编译器”——所以，这是“源代码到源代码”的简称。翻译意味着将一种高级语言转换成另一种高级语言。

例如，TypeScript 是一种高级语言，但是在被编译之后，它就变成了 JavaScript(另一种高级语言)。例如，Babel 可以将 ES6 JavaScript 代码转换成 ES5 JavaScript 代码。

transpiling 的好处是，你可以编写一种高级语言，但最终仍然可以得到另一种高级语言。

# 打字稿中的类型擦除

这个**传输**过程的一部分被称为**型擦除**。

**类型删除**非常简单，当所有的类型都从类型脚本代码中删除时，它就会被转换成 JavaScript。

当 JavaScript 正在执行时，您在 TypeScript 中使用的类型不能在运行时被检查。**类型**仅在编译/转换步骤中可访问。

看起来像这样的类型脚本代码:

`let name: string = 'Kealan';`

最终被编译/传输成:

`let name = 'Kealan'`

根据您的具体构建步骤，输出可能会有所不同(变量可能会被重命名或内联)，但是**类型擦除**的例子仍然成立。

这不仅仅适用于像`number`或`string`这样的原始类型**——甚至你可以创建自己的定制类型:**

```
`type StringType = string;

const firstName: StringType = "Kealan";`
```

## **实践中的类型擦除**

**不仅仅是从概念上理解什么是**类型擦除**，这个概念解释了 transpiling 过程中的一个重要步骤，在这个步骤中，类型被丢弃，并且不在您生成的源代码中使用。**

**这也意味着在传输过程中，你的代码片段甚至没有在 JavaScript 中“使用”——这些代码完全被删除了。因此，您创建的 100 行接口被删除了，发送给用户的代码变少了。**

**你可以在 [TypeScript playground](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgApQPYHMpwLZ7TIDeAUMhciPhAFzIDOYUoWANOZQDbADWEDekxYh2nCpAQALEF2wBPAKoMIAEyHNWAbQC6pAL6lSCDCCbJ+cLnBD102XASIBeEuKo16AcgDSEKzZeHJTIPPyCyF4AwlICDEHukjJyWEoq6shaXgBScABucADKCCwADmBBkQAq8qUQxWUVbJEASv4ITZEAchiqEF46yAZAA) 中看到这样的例子，其中在 TypeScript 代码中使用的接口在 transpiled JavaScript 中是不存在的。**

# **结论**

**我希望 TypeScript 将您的代码转换成 JavaScript 的一些步骤更加清晰，并且您对编译和传输的区别有一个很好的了解。**

**如果你想了解更多，我在这里发推文。**