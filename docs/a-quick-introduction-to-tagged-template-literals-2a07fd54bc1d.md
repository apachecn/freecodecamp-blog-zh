# 标记模板文字的快速介绍

> 原文：<https://www.freecodecamp.org/news/a-quick-introduction-to-tagged-template-literals-2a07fd54bc1d/>

迈克尔·奥祖梅纳

在这篇文章中，我将谈论什么是“带标签的模板文字”以及我看到它们被使用的一些地方。

#### 什么是“标记模板文字”？

带标签的模板文字是由 ES6 中引入的新技术支持的，称为“模板文字”。这只是一种语法，使得 JavaScript 中的字符串插值成为可能。在`template literals`诞生之前，JavaScript 开发者需要做难看的字符串拼接。

为您提供了以任何方式解析模板文字的机会。它的工作原理是用`template literals`组合功能。一辆`tagged template literal`有两部分，第一部分是`tag function`，第二部分是`template literal`。

```
const coolVariable = "Cool Value";
```

```
const anotherCoolVariable = "Another Cool Value";
```

```
randomTagFunctionName`${coolVariable} in a tagged template literal with ${anotherCoolVariable} just sitting there`
```

现在，`tag function`中的第一个参数是一个变量，包含模板文本中的字符串数组:

```
function randomTagFunctionName(firstParameter) {
```

```
console.log(firstParameter);     // ["", " in a tagged template literal with ", " just sitting there"]
```

```
}
```

所有其他后续参数将包含传递给模板文字的值:

```
function randomTagFunctionName(firstParameter, secondParameter, thirdParameter) {
```

```
console.log(secondParameter);   // "Cool Value"
```

```
console.log(thirdParameter);   // "Another Cool Value"
```

```
}
```

利用 ES6 Rest 操作符，我们可以这样重新定义我们的函数:

```
function randomTagFunctionName(firstParameter, ...otherParameters) {
```

```
console.log(otherParameters);   // ["Cool Value", "Another Cool Value"]
```

```
}
```

#### 样式化组件中的标记模板文字。

现在您已经理解了什么是标记模板文字，让我们来讨论它在现实世界中的应用。

Styled-Components 是一个 JavaScript 库，允许您创建 CSS 样式并将其附加到 React 和 React 本地组件。看起来是这样的:

```
const Button = styled.button`  color: red;`
```

在上面的例子中，Button 不仅仅是一个变量，它还是一个组件。这意味着您可以将它与其他组件混合使用，并且它的输出是一个 HTML 按钮元素。

对于带标签的模板文字来说，这是一个非常令人兴奋的用例，因为它让您能够以一种仍然保持组件文件整洁的方式来限定 CSS 的范围，并让您感觉像是在用常规样式表编写 CSS。如果没有带标签的模板文字，我们就必须将该样式表示为一个 JavaScript 对象，如下所示:

```
const Button = styled.button({  color: 'red'})
```

另一个用例是在 **Apollo GraphQL** 中使用的 [graphql-tag](https://github.com/apollographql/graphql-tag) 库。我不认为有可能在处理 Apollo GraphQL 时不使用`graphql-tag`库(如果有，请告诉我！).

#### 总之。

我认为学习新技术很棒，更好的是看看别人用它来解决问题的方法。如果您对标记模板文字有其他真实的使用案例，请告诉我。

PS:“真实世界”也意味着你的附带项目或`codesandbox`代码样本。