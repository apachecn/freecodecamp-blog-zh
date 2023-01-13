# 如何在 JavaScript 中使用可选链接

> 原文：<https://www.freecodecamp.org/news/javascript-optional-chaining/>

可选链接是对嵌套对象特性执行访问检查的一种安全而简洁的方式。

可选的链接操作符`?.`获取其左边的引用，并检查它是否未定义或为空。如果引用是这些空值中的任何一个，检查将停止并返回 undefined。否则，访问检查链将继续沿着快乐的路径到达最终值。

```
// An empty person object with missing optional location information
const person = {}

// The following will equate to undefined instead of an error
const currentAddress = person.location?.address 
```

ES2020 中引入了可选链接。根据 [TC39](https://github.com/tc39/proposal-optional-chaining) 的消息，它目前正处于提案流程的第 4 阶段，准备纳入最终的 ECMAScript 标准。这意味着您可以使用它，但请注意，较旧的浏览器可能仍然需要使用多填充。

可选链接是一个有用的特性，可以帮助您编写更简洁的代码。现在让我们来学习如何使用它。

## 可选链接语法

在本文中，我将主要讲述如何访问对象属性。但是您也可以使用可选的链接来检查函数。

以下是可选链接的所有使用案例:

```
obj?.prop       // optional static property access
obj?.[expr]     // optional dynamic property access
func?.(...args) // optional function or method call 
```

来源: [MDN 网络文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)

### 示例:

```
const value = obj?.propOne?.propTwo?.propThree?.lastProp;
```

在上面的代码片段中，我们检查`obj`是否为空或未定义，然后是`propOne`，然后是`propTwo`，依此类推。可选链接名副其实。在对象属性访问链中，我们可以检查每个值是否未定义或为空。

当访问深度嵌套的对象值时，这种检查非常有用。这是一个备受期待的特性，它让你不必做大量的空检查。这也意味着您不需要使用临时变量来存储检查值，例如:

```
const neighborhood = city.nashville && city.nashvile.eastnashville; 
```

在试图访问`eastnashville`的内部邻域属性之前，我们可以在这里检查`nashville`是否是`city`内的属性。我们可以将上述内容转换为使用可选链接，如下所示:

```
const neighborhood = city?.nashville?.eastnashville;
```

可选的链接简化了这个表达式。

## 使用可选链接的错误处理

可选链接在处理 API 数据时特别有用。如果不确定可选属性是否存在，可以使用可选链接。

### 提醒一句

不要一有机会就使用可选链接。这可能会因为在许多地方有未定义的潜在返回而导致错误消失。

同样重要的是要记住，当检查遇到一个空值时，它将停止并“短路”。请考虑链中的后续属性，以及如果无法到达这些属性将会发生什么。

当您知道某些东西可能没有值时，例如可选属性，最好使用这种检查。如果某个必需的值启用了 nullish 检查，则它可能会被静音并返回 undefined，而不是返回一个错误来警告此问题。

## 可选链接+无效合并

可选的链接对以及 nullish 合并`??`来提供回退值。

```
const data = obj?.prop ?? "fallback string";
```

```
const data = obj?.prop?.func() ?? fallbackFunc();
```

如果`??`左边的项目是 nullish，右边的项目将被返回。

我们知道，如果任何`?.`检查等于链中的一个空值，它将返回`undefined`。因此，我们可以使用 nullish 合并来响应未定义的结果，并设置一个显式的回退值。

```
const meal = menu.breakfast?.waffles ?? "No Waffles Found."
```

### 包扎

可选链接是 JavaScript 的一个方便的新特性，允许您在访问属性值时检查 nullish 值。也可以和`?.`操作符一起使用。

我希望这篇文章有助于介绍或阐明可选链接。编码快乐！