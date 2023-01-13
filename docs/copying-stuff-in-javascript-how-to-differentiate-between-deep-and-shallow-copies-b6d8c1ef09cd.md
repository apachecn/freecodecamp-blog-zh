# 如何在 JavaScript 中区分深层和浅层拷贝

> 原文：<https://www.freecodecamp.org/news/copying-stuff-in-javascript-how-to-differentiate-between-deep-and-shallow-copies-b6d8c1ef09cd/>

由 lukas GIS der-Dube 撰写

# 如何在 JavaScript 中区分深层和浅层拷贝

![1*CZaCLmeCWGIhvuhKnV7mVg](img/218e51c51a84464bf70a8c9a462e7aa8.png)

Photo by [Oliver Zenglein](https://unsplash.com/photos/VWYif1k5OZc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/storm-trooper?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

新的总是更好！

您以前肯定处理过 JavaScript 的副本，即使您并不知道。也许你也听说过函数式编程中的范例，即你不应该修改任何现有的数据。为了做到这一点，您必须知道如何在 JavaScript 中安全地复制值。今天，我们将看看如何做到这一点，同时避免陷阱！

首先，什么是文案？

复制品只是看起来像旧的东西，但不是。当您更改副本时，您希望原始内容保持不变，而副本却发生了变化。

在编程中，我们将值存储在变量中。制作副本意味着您用相同的值启动一个新变量。然而，有一个很大的潜在陷阱需要考虑:**深度复制**与**浅层复制**。深度复制意味着新变量的所有值都被复制，并且**与原来的**变量断开。浅层拷贝意味着某些(子)值仍然与原始变量相连。

> 要真正理解复制，您必须了解 JavaScript 如何存储值。

#### 原始数据类型

原始数据类型包括以下几种:

*   数字—例如`1`
*   字符串—例如`'Hello'`
*   布尔型—例如`true`
*   `undefined`
*   `null`

当您创建这些值时，它们与它们被赋给的变量紧密耦合。它们只存在一次。这意味着您真的不必担心在 JavaScript 中复制原始数据类型。当你复制时，它将是一个真正的副本。让我们看一个例子:

```
const a = 5
```

```
let b = a // this is the copy
```

```
b = 6
```

```
console.log(b) // 6
```

```
console.log(a) // 5
```

通过执行`b = a`，您制作了副本。现在，当您给`b`重新分配一个新值时，`b`的值会改变，但`a`的值不会改变。

#### 复合数据类型—对象和数组

从技术上讲，数组也是对象，所以它们的行为是一样的。稍后我将详细讨论这两个问题。

这里变得更有趣了。这些值实际上在实例化时只存储一次，给一个变量赋值只是为值创建了一个指针(引用)。

现在，如果我们复制`b = a`，并改变`b`中的一些嵌套值，它实际上也改变了`a`的嵌套值，因为`a`和`b`实际上指向相同的东西。示例:

```
const a = {
```

```
 en: 'Hello',
```

```
 de: 'Hallo',
```

```
 es: 'Hola',
```

```
 pt: 'Olà'
```

```
}
```

```
let b = a
```

```
b.pt = 'Oi'
```

```
console.log(b.pt) // Oi
```

```
console.log(a.pt) // Oi
```

在上面的例子中，我们实际上做了一个**浅拷贝**。这经常是有问题的，因为我们期望旧变量有原始值，而不是改变的值。当我们访问它时，有时会出错。在发现错误之前，您可能会尝试调试一段时间，因为许多开发人员并没有真正理解这个概念，也不认为这是错误。

![1*niSsr1dxva2RWXrvHT8hxg](img/0e0bc4dd9806f58abd242e79c241784a.png)

Photo by [Thomas Millot](https://unsplash.com/photos/7ocA9xwN3yQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/fungi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

让我们看看如何复制对象和数组。

### 目标

有多种方法可以制作对象的副本，特别是使用新的扩展和改进的 JavaScript 规范。

#### 传播算子

ES2015 推出的这款操作器非常棒，因为它非常简洁。它将所有的值“分散”到一个新的对象中。您可以按如下方式使用它:

```
const a = {
```

```
 en: 'Bye',
```

```
 de: 'Tschüss'
```

```
}
```

```
let b = {...a}
```

```
b.de = 'Ciao'
```

```
console.log(b.de) // Ciao
```

```
console.log(a.de) // Tschüss
```

你也可以用它来合并两个对象，例如`const c = {...a, ...b}`。

#### 对象.分配

这主要是在 spread 操作符出现之前使用的，它基本上做同样的事情。但是您必须小心，因为`Object.assign()`方法中的第一个参数实际上会被修改和返回。因此，请确保至少将要复制的对象作为第二个参数传递。通常，您只需传递一个空对象作为第一个参数，以防止修改任何现有数据。

```
const a = {
```

```
 en: 'Bye',
```

```
 de: 'Tschüss'
```

```
}
```

```
let b = Object.assign({}, a)
```

```
b.de = 'Ciao'
```

```
console.log(b.de) // Ciao
```

```
console.log(a.de) // Tschüss
```

#### 陷阱:嵌套对象

如前所述，在处理复制对象时有一个很大的注意事项，它适用于上面列出的两种方法。当你有一个嵌套对象(或数组)并复制它时，该对象内的嵌套对象将不会被复制，因为它们只是指针/引用。因此，如果您更改嵌套对象，您将为两个实例更改它，这意味着您将再次进行**浅层复制**。示例://错误的示例

```
const a = {
```

```
 foods: {
```

```
 dinner: 'Pasta'
```

```
 }
```

```
}
```

```
let b = {...a}
```

```
b.foods.dinner = 'Soup' // changes for both objects
```

```
console.log(b.foods.dinner) // Soup
```

```
console.log(a.foods.dinner) // Soup
```

要制作嵌套对象的**深度副本，您必须考虑这一点。防止这种情况的一种方法是手动复制所有嵌套对象:**

```
const a = {
```

```
 foods: {
```

```
 dinner: 'Pasta'
```

```
 }
```

```
}
```

```
let b = {foods: {...a.foods}}
```

```
b.foods.dinner = 'Soup'
```

```
console.log(b.foods.dinner) // Soup
```

```
console.log(a.foods.dinner) // Pasta
```

如果您想知道当对象有比仅仅`foods`更多的键时该怎么办，您可以使用 spread 操作符的全部潜力。当在`...spread`之后传递更多属性时，它们会覆盖原始值，例如`const b = {...a, foods: {...a.foods}}`。

#### 不假思索地进行深层复制

如果你不知道嵌套结构有多深呢？手动遍历大对象并手动复制每个嵌套对象是非常乏味的。有一种方法可以不假思索地复制一切。你只需`stringify`你的对象，然后`parse`它:

```
const a = {
```

```
 foods: {
```

```
 dinner: 'Pasta'
```

```
 }
```

```
}
```

```
let b = JSON.parse(JSON.stringify(a))
```

```
b.foods.dinner = 'Soup'
```

```
console.log(b.foods.dinner) // Soup
```

```
console.log(a.foods.dinner) // Pasta
```

在这里，你要考虑到你将无法复制自定义类实例，所以你只能在复制内部带有**原生 JavaScript 值**的对象时使用它。

![1*hepu5hNOaAqnhE60z-oicA](img/8e84589029f0bd1c4323086f13d9d551.png)

Photo by [Robert Zunikoff](https://unsplash.com/photos/j0vIeF69Jgc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/dolls?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### 数组

复制数组和复制对象一样常见。它背后的许多逻辑是相似的，因为数组也只是幕后的对象。

#### 传播算子

与对象一样，您可以使用 spread 运算符来复制数组:

```
const a = [1,2,3]
```

```
let b = [...a]
```

```
b[1] = 4
```

```
console.log(b[1]) // 4
```

```
console.log(a[1]) // 2
```

#### 数组函数—映射、过滤、归约

这些方法将返回一个新数组，其中包含原始数组的所有(或部分)值。在这样做的同时，您还可以修改这些值，这非常方便:

```
const a = [1,2,3]
```

```
let b = a.map(el => el)
```

```
b[1] = 4
```

```
console.log(b[1]) // 4
```

```
console.log(a[1]) // 2
```

或者，您可以在复制时更改所需的元素:

```
const a = [1,2,3]
```

```
const b = a.map((el, index) => index === 1 ? 4 : el)
```

```
console.log(b[1]) // 4
```

```
console.log(a[1]) // 2
```

#### 数组.切片

此方法通常用于返回元素的子集，从原始数组的特定索引处开始，也可以在原始数组的特定索引处结束。当使用`array.slice()`或`array.slice(0)`时，你将得到原始数组的副本。

```
const a = [1,2,3]
```

```
let b = a.slice(0)
```

```
b[1] = 4
```

```
console.log(b[1]) // 4
```

```
console.log(a[1]) // 2
```

#### 嵌套数组

类似于对象，使用上面的方法复制一个数组和另一个数组或对象将会生成一个**浅拷贝**。为了防止这种情况，也可以使用`JSON.parse(JSON.stringify(someArray))`。

#### 奖励:复制定制类的实例

当你已经是一个 JavaScript 专家，并且处理你的自定义构造函数或类时，也许你也想复制它们的实例。

如前所述，您不能只是 stringify +解析它们，因为您将丢失您的类方法。相反，您可能希望添加一个定制的`copy`方法来创建一个包含所有旧值的新实例。让我们看看它是如何工作的:

```
class Counter {
```

```
 constructor() {
```

```
 this.count = 5
```

```
 }
```

```
 copy() {
```

```
 const copy = new Counter()
```

```
 copy.count = this.count
```

```
 return copy
```

```
 }
```

```
}
```

```
const originalCounter = new Counter()
```

```
const copiedCounter = originalCounter.copy()
```

```
console.log(originalCounter.count) // 5
```

```
console.log(copiedCounter.count) // 5
```

```
copiedCounter.count = 7
```

```
console.log(originalCounter.count) // 5
```

```
console.log(copiedCounter.count) // 7
```

为了处理实例内部引用的对象和数组，您必须应用您新学到的关于**深度复制**的技能！我将为自定义构造函数`copy`方法添加一个最终解决方案，使其更加动态:

使用这种复制方法，您可以在构造函数中放入任意多的值，而不必手动复制所有内容！

*关于作者:Lukas Gisder-Dubé作为首席技术官共同创立并领导了一家初创公司 1 年半，建立了技术团队和架构。离开这家初创公司后，他在 T2 的 Ironhack 担任首席讲师，教授编程，现在正在柏林建立一家初创机构咨询公司。查看 [dube.io](https://dube.io) 了解更多信息。*

![1*p-l0Cee1IHvX0RQkVTOceQ](img/65e37b79b649b51075cca3751bdeaf5b.png)