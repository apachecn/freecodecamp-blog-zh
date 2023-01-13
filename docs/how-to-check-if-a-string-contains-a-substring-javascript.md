# 如何在 JavaScript 中检查字符串是否包含子串

> 原文：<https://www.freecodecamp.org/news/how-to-check-if-a-string-contains-a-substring-javascript/>

当您使用 JavaScript 程序时，您可能需要检查字符串是否包含子字符串。子字符串是另一个字符串中的一个字符串。

具体来说，您可能需要检查一个单词是否包含特定的字符或特定的字符集。

幸运的是，使用 JavaScript 有一些快速的方法可以实现这一点。

在本文中，您将了解使用 JavaScript 方法检查字符串是否包含子串的两种不同方法。

具体来说，您将学习:

*   如何使用内置的`includes()` JavaScript 方法？
*   如何使用内置的`indexOf()` JavaScript 方法？

以下是我们将详细介绍的内容:

1.  [JavaScript](#includes-syntax)中`includes()`方法的语法分解
    1.  [如何使用`includes()`方法](#includes-example)检查一个字符串是否包含特定的子串
2.  [JavaScript](#index-syntax)中`indexOf()`方法的语法分解
    1.  [如何使用`indexOf()`方法](#index-example)检查一个字符串是否包含特定的子串
3.  [如何用`includes()`和`indexOf()`方法](#insensitive)进行不区分大小写的检查

## JavaScript 中的`includes()`方法是什么？`includes()`方法语法分解

ES6 引入了 JavaScript `includes()`方法，这是检查字符串是否包含特定字符或一系列字符的最常见和最现代的方法。

`includes()`方法的一般语法如下所示:

```
string.includes(substring, index); 
```

让我们来分解一下:

*   `string`是您要搜索的单词。
*   `includes()`是您对想要搜索的单词调用的方法，在本例中是`string`。
*   `includes()`方法接受两个参数——一个是必需的，一个是可选的。
*   `includes()`方法接受的第一个参数是`substring`，它是*必需的*。`substring`是您正在检查是否存在于`string`中的字符或一系列字符。
*   `includes()`方法接受的第二个参数是`index`，它是*可选的*。`index`是指开始搜索`substring`的位置——默认值是`0`,因为编程语言中的索引从`0`开始。

返回值是一个布尔值。布尔值可以是`true`或`false`，这取决于子字符串是否存在于字符串中。

需要记住的是，`includes()`方法是**区分大小写的**。

### 如何使用`includes()`方法在 JavaScript 中检查一个字符串是否包含特定的子串

让我们看一个`includes()`方法如何工作的例子。

首先，我创建了一个保存字符串`Hello, World`的变量——这是我想要搜索的字符串:

```
let string= "Hello, World"; 
```

接下来，我用子串`Hello`创建了一个变量——这是我想在原始字符串中搜索的子串:

```
let string= "Hello, World";
let substring = "Hello"; 
```

接下来，我将使用`includes()`方法检查`substring`是否在`string`中，并将结果打印到控制台:

```
let string= "Hello, World";
let substring = "Hello";

console.log(string.includes(substring));

// output
// true 
```

返回值是`true`，意味着`Hello`存在于变量`string`中。

如前所述，`includes()`方法是区分大小写的。

看看当我把`substring`的值从`Hello`改成`hello`时会发生什么:

```
let string= "Hello, World";
let substring = "hello";

console.log(string.includes(substring));

// output
// false 
```

在这种情况下，返回值是`false`，因为没有小写`h`的子串`hello`。所以，在使用`includes()`方法时要记住这一点——它区分大写字母和小写字母。

现在，让我们看看如何使用带有第二个参数`index`的`includes()`方法。

提醒一下，第二个参数指定了开始搜索子字符串的位置。

让我们从前面的例子中取出同一个`string`变量:

```
let string= "Hello, World"; 
```

我将把`substring`变量的值改为`H`:

```
let string= "Hello, World";
let substring = "H"; 
```

我将指定从位置`0`开始搜索子字符串:

```
let string= "Hello, World";
let substring = "H";

console.log(string.includes(substring,0));

// output
// true 
```

返回值为`true`，因为子字符串`H`位于字符串`Hello, World`内的索引位置`0`。

记住，字符串中第一个字母的位置是`0`，第二个字母的位置是`1`，以此类推。

## JavaScript 中的`indexOf()`方法是什么？`indexOf()`方法语法分解

类似于`includes()`方法，JavaScript `indexOf()`方法检查字符串是否包含子字符串。

`indexOf()`方法的一般语法如下所示:

```
string.indexOf(substring, index); 
```

让我们来分解一下:

*   `string`是您要搜索的单词。
*   `index0f()`是您对想要搜索的单词调用的方法，在本例中是`string`。
*   `includes()`方法有两个参数——一个是必需的，一个是可选的。
*   `indexOf()`方法的第一个参数是`substring`，它是*必需的*。`substring`是您要搜索的字符或字符序列。
*   `indexOf()`方法的第二个参数是`index`，它是*可选的*。`index`是指开始搜索`substring`的位置。默认值是`0`，因为编程语言中的索引从`0`开始。

这两种方法的区别在于它们的返回值。

`includes()`方法返回一个布尔值(一个或者是`true`或者是`false`的值)，而`indexOf()`方法返回一个数字。

该数字将是在字符串中找到要查找的子字符串的起始索引位置。如果在字符串中没有找到子串，返回值将是`-1`。

就像`includes()`方法一样，`indexOf()`方法是**区分大小写的**。

### 如何使用`indexOf()`方法在 JavaScript 中检查一个字符串是否包含特定的子串

让我们使用前面的同一个例子来看看`indexOf()`方法是如何工作的。

```
let string= "Hello, World";
let substring = "H"; 
```

有一个包含原始字符串的变量`string`，还有一个包含要搜索的子字符串的变量`substring`。

```
let string= "Hello, World";
let substring = "H";

console.log(string.indexOf(substring));

// output
// 0 
```

输出是`0`，这是您正在搜索的子串的起始位置。

在这种情况下，您要搜索的值是单个字符。

让我们将`substring`的值从`H`改为`Hello`:

```
let string= "Hello, World";
let substring = "Hello";

console.log(string.indexOf(substring));

// output
// 0 
```

返回值仍然是`0`，因为`index0f()`返回您正在查找的子字符串的*开始*位置。由于子串的第一个字符位于`0`位置，`indexOf()`返回`0`。

现在，让我们用小写的`h`将`substring`的值从`Hello`更改为`hello`:

```
let string= "Hello, World";
let substring = "hello";

console.log(string.indexOf(substring));

// output
// -1 
```

返回值为`-1`。如前所述，`index0f()`是区分大小写的，因此它无法找到小写`h`的子串`hello`。当`indexOf()`找不到给定的子串时，它返回`-1`。

最后，您可以通过传递`indexOf()`接受的第二个参数来指定开始搜索的索引值。

```
let string= "Hello, World";
let substring = "hello";

console.log(string.indexOf(substring,1));

// output
// -1 
```

假设您想从位置`1`开始搜索。返回值是`-1`，因为您正在搜索的子字符串的起始位置是`0`。在位置`1`没有找到精确匹配，因此`indexOf()`返回`-1`。

## 如何用`includes()`和`indexOf()`方法进行不区分大小写的检查

到目前为止，您已经看到了`includes()`和`indexOf()`方法不区分大小写。

但是当您想要执行不区分大小写的检查时会发生什么呢？

要进行不区分大小写的检查并查看字符串中是否存在子字符串，您需要在调用这两个方法之一之前，使用`toLowerCase()` JavaScript 方法将原始字符串和子字符串都转换为小写。

下面是您如何使用`includes()`方法来实现这一点:

```
let string= "Hello, World";
let substring = "hello";

console.log(string.toLowerCase().includes(substring.toLowerCase()));

// output
// true 
```

默认情况下，返回值应该是`false`，因为原始字符串包含大写的`H`，而子字符串包含小写的`h`。将两个字符串都转换成小写后，就不必担心原始字符串和要搜索的子字符串的大小写了。

下面是如何使用`indexOf()`方法做同样的事情:

```
let string= "Hello, World";
let substring = "hello";

console.log(string.toLowerCase().indexOf(substring.toLowerCase()));

// output
// 0 
```

默认情况下，返回值应该是`-1`，因为原始的
字符串和您正在搜索的子字符串的大小写不同。

使用`toLowerCase()`方法后，`indexOf()`方法返回子串的起始位置。

## 结论

现在你知道了！现在您知道了如何在 JavaScript 中检查字符串是否包含子串。

要了解更多关于 JavaScript 的知识，请访问 freeCodeCamp 的 [JavaScript 算法和数据结构认证](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)。

这是一个免费的、经过深思熟虑的、结构化的课程，你可以在其中进行互动学习。最后，您还将构建 5 个项目来获得您的认证并巩固您的知识。

感谢阅读！