# 用 Array.includes()检查一个项目是否在 JavaScript-JS Contains 的数组中

> 原文：<https://www.freecodecamp.org/news/check-if-an-item-is-in-an-array-in-javascript-js-contains-with-array-includes/>

您可以在 JavaScript 中使用`includes()`方法来检查数组中是否存在某个项目。还可以用它来检查字符串中是否存在子字符串。

如果在数组/字符串中找到该项，则返回`true`,如果该项不存在，则返回`false`。

在本文中，您将看到如何在 JavaScript 中使用`includes()`方法来检查一个项目是否在一个数组中，以及一个子串是否存在于一个字符串中。

## 如何使用`Array.includes()`在 JavaScript 中检查一个项目是否在一个数组中

下面是使用`includes()`方法检查一个项目是否在数组中的语法:

```
array.includes(item, fromIndex)
```

让我们分解上面的语法:

`array`表示将被搜索以检查某项是否存在的数组的名称。

`includes()`方法接受两个参数——`item`和`fromIndex`。

*   `item`是您正在搜索的特定项目。
*   可选参数`fromIndex`，指定从哪个索引开始搜索。如果不包含此参数，默认索引将被设置为 0(第一个索引)。

下面是一些例子，展示如何使用`includes()`方法来检查数组中是否存在某个项目:

```
const nums = [ 1, 3, 5, 7];
console.log(nums.includes(3));
// true
```

在上面的例子中，我们创建了一个名为`nums`的数组，有四个数字——1，3，5，7。

使用点符号，我们将`includes()`方法附加到`nums`数组。

在`includes()`方法的参数中，我们传入了 3。这是我们要搜索的项目。

我们得到了返回的`true`,因为 3 存在于`nums`数组中。

让我们尝试搜索数组中不存在的数字。

```
const nums = [ 1, 3, 5, 7];
console.log(nums.includes(8));
// false 
```

正如所料，我们在上面的例子中得到返回的`false`,因为 8 不是`nums`数组中的一项。

## 如何在 JavaScript 中使用`Array.includes()`从指定的索引开始检查一个项目是否在一个数组中

在上一节中，我们看到了如何在不使用`includes()`方法中的第二个参数的情况下检查数组中是否存在某个项目。

提醒一下，第二个参数用于指定在数组中搜索项目时的起始索引。

数组的索引从 0 开始。所以第一项是 0，第二项是 1，第三项是 2，依此类推。

下面的例子展示了我们如何使用`includes()`方法的第二个参数:

```
const nums = [ 1, 3, 5, 7];
console.log(nums.includes(3,2));
// false
```

上面的例子返回了`false`，即使我们在数组中有 3 个元素。原因如下:

使用第二个参数，我们告诉`includes()`方法从索引 2 开始搜索数字 3:`nums.includes(3,2)`。

这是数组:[ 1，3，5，7]

索引 0 = 1。

指数 1 = 3。

指数 2 = 5。

指数 3 = 7。

所以从第二个索引 5 开始，我们只需要搜索 5 和 7 ([5，7])。这就是为什么从索引 2 搜索 3 会返回`false`。

如果您将索引改为从 1 开始搜索，那么您将得到返回的`true`,因为在该范围内可以找到 3。那就是:

```
const nums = [ 1, 3, 5, 7];
console.log(nums.includes(3,1));
// true
```

## 如何使用`includes()`方法在 JavaScript 中检查子串是否在字符串中

与前面的例子类似，您必须使用点符号将`includes()`方法附加到要搜索的字符串的名称上。

下面是语法的样子:

```
string.includes(substring, fromIndex)
```

这里有一个例子:

```
const bio = "I am a web developer";
console.log(bio.includes("web"));
// true
```

在上面的例子中，`bio`变量的值为“我是一名 web 开发人员”。

使用`includes()`方法，我们搜索子字符串“web”。

我们返回了`true`,因为“web”在`bio`字符串中。

您还可以使用第二个参数来指定搜索的开始位置，但是请注意，字符串中的每个字符都代表一个索引，每个子字符串之间的空格也代表一个索引。

这里有一个例子来说明:

```
let bio = "I am a web developer";
console.log(bio.includes("web",9));
// false
```

我们得到`false`是因为索引 9 是“web”中的 e。

从索引 9 开始，字符串将如下所示:“eb developer”。字符串中不存在子字符串“web ”,因此返回了`false`。

## 摘要

在本文中，我们讨论了 JavaScript 中的`includes()`方法。您可以用它来检查数组中是否存在某个项。您还可以使用它来检查是否可以在字符串中找到子字符串。

我们看到了一些例子，解释了如何使用它从第一个索引开始检查数组中的项，然后从指定的索引开始检查另一个例子。

最后，我们看到了如何使用`includes()`方法检查第一个索引和指定索引的字符串中是否存在子字符串。

编码快乐！