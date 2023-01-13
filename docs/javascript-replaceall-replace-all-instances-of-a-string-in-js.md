# JavaScript Replace All()–替换 JS 中字符串的所有实例

> 原文：<https://www.freecodecamp.org/news/javascript-replaceall-replace-all-instances-of-a-string-in-js/>

使用 JavaScript 程序时，您可能需要用另一个字符或单词替换它。

具体来说，您可能需要将该字符或单词的所有出现处替换为其他内容，而不仅仅是一处。

使用 JavaScript 有几种方法可以实现这一点。

其中一种方法是使用内置的`replaceAll()`方法，您将在本文中学习使用该方法。

以下是我们将要介绍的内容:

1.  [JavaScript 中的`replaceAll()`是什么？](#intro)
    1.  [`replaceAll()`语法](#syntax)
2.  [`replaceAll()`以一个字符串作为第一个参数](#string-param)
3.  [`replaceAll()`以正则表达式为第一个参数](#regEx)
4.  [`replaceAll()` VS `replace()`](#differences)

## JavaScript 中的`replaceAll()`是什么？

`replaceAll()`方法是 JavaScript 标准库的一部分。当您使用它时，您将替换一个字符串的所有实例。

有不同的方法可以替换一个字符串的所有实例。也就是说，使用`replaceAll()`是最简单快捷的方法。

需要注意的是，该功能是 ES2021 中引入的。

尽管`replaceAll()`方法与所有主流浏览器兼容，但在为旧版本浏览器开发时，它并不是最佳解决方案，因为这些旧版本可能无法理解和支持它。

### `replaceAll()`方法——语法分解

`replaceAll()`方法的一般语法如下所示:

```
string.replaceAll(pattern, replacement) 
```

让我们来分解一下:

*   `string`是您正在处理的原始字符串，也是您将对其调用`replaceAll()`方法的字符串。
*   `replaceAll()`方法有两个参数:
*   `pattern`是第一个参数，可以是一个子串，也可以是一个正则表达式——这是指你要改变的项，用别的东西替换。
    *   如果`pattern`是一个**正则表达式**，你需要包含`g`标志(其中`g`代表`g`全局)或者`replaceAll()`将抛出一个异常——具体来说，错误将是一个`TypeError`。
*   `replacement`是第二个参数，可以是替代`pattern`的另一个字符串或函数。

这里需要注意的是,`replaceAll()`方法不会改变原来的字符串。相反，它返回一个新的副本。

指定的`pattern`的所有实例将被替换为`replacement`。

## 如何使用带字符串的`replaceAll()`作为第一个参数示例

前面，您看到了`replaceAll()`方法接受两个参数——`pattern`作为第一个参数，`replacement`作为第二个参数。

您还看到了`pattern`可以是字符串或正则表达式。

现在，让我们看看当`replaceAll()`将一个**字符串**作为第一个参数时，它是如何工作的。

因此，假设您有以下示例:

```
const my_string = "I like dogs because dogs are adorable!";

let pattern = "dogs";
let replacement = "cats";

let my_new_string = my_string.replaceAll(pattern,replacement); 
```

我将文本`I like dogs because dogs are adorable!`存储在一个名为`my_string`的变量中。

这是我正在处理的原始字符串，我想修改它的一些内容。

具体来说，我想改变子串`dogs`，它在原串中出现了两次*——这将是我的`pattern`。*

 *我将这个想要替换的子串存储在一个名为`pattern`的变量中。

然后，我将字符串`cats`存储在一个名为`replacement`的变量中——这是将替换`dogs`的字符串。

然后，我在原始字符串上调用`replaceAll()`方法，将两个子字符串作为参数传递，并将结果存储在名为`my_new_string`的变量中。

```
console.log(my_new_string);

// I like cats because cats are adorable! 
```

`replaceAll()`方法将用`cats`替换`I like dogs because dogs are adorable!`中的子串`dogs`的所有实例*。*

原始字符串将保持不变。

这里需要注意的是，使用字符串作为第一个参数时的替换是区分大小写的。这意味着只替换与`pattern`匹配的大小写相同的字符串。

```
const my_string = "I like Dogs because dogs are adorable!";

let pattern = "dogs";
let replacement = "cats";

let my_new_string = my_string.replaceAll(pattern,replacement);

console.log(my_new_string); 
```

在上面的例子中，有两个`dogs`的实例，但是第一个有一个大写的`D`。

正如您在输出中看到的，替换是区分大小写的:

```
I like Dogs because cats are adorable! 
```

## 如何使用带有正则表达式的`replaceAll()`作为第一个参数示例

正如您在前面看到的，您可以将一个正则表达式(也称为 regex)作为第一个参数传递。

正则表达式是创建搜索模式的字符序列。

执行此操作的一般语法如下所示:

```
string.replaceAll(/pattern/g, replacement) 
```

让我们以上一节中的示例为例，使用正则表达式代替字符串作为第一个参数:

```
const my_string = "I like dogs because dogs are adorable!";

let pattern = /dogs/g;
let replacement = "cats";

let my_new_string = my_string.replaceAll(pattern,replacement);

console.log(my_new_string);

//output

// I like cats because cats are adorable! 
```

当使用正则表达式作为第一个参数时，确保使用`g`标志。

如果不这样做，代码中就会出现一个错误:

```
const my_string = "I like dogs because dogs are adorable!";

let pattern = /dogs/;
let replacement = "cats";

let my_new_string = my_string.replaceAll(pattern,replacement);

console.log(my_new_string);

// output

// test.js:6 Uncaught TypeError: String.prototype.replaceAll called with a // non-global RegExp argument
//    at String.replaceAll (<anonymous>)
//   at test.js:6:31 
```

让我们稍微调整一下原来的字符串。

```
const my_string = "I like Dogs because dogs are adorable!";

let pattern = /dogs/g;
let replacement = "cats";

let my_new_string = my_string.replaceAll(pattern,replacement);

console.log(my_new_string); 
```

我现在有两个`dogs`的实例，但是其中一个有一个大写的`D`。

我最终得到了以下输出:

```
I like Dogs because cats are adorable! 
```

从输出中，您可以看出这是一个区分大小写的替换。

为了使其不区分大小写，请确保在`g`标志后添加`i`标志，如下所示:

```
const my_string = "I like Dogs because dogs are adorable!";

let pattern = /dogs/gi;
let replacement = "cats";

let my_new_string = my_string.replaceAll(pattern,replacement);

console.log(my_new_string);

// output

// I like cats because cats are adorable! 
```

正则表达式`/dogs/gi`将匹配包含该子字符串的所有实例，并使替换不区分大小写。

## `replaceAll()`与`replace()`方法有什么区别？

`replaceAll()`和`replace()`方法的区别在于`replaceAll()`直接执行全局替换。

`replaceAll()`方法将替换*您指定的字符串或正则表达式模式的所有*实例，而`replace()`方法将只替换第一个出现的*。*

这就是`replace()`将字符串作为第一个参数的工作方式:

```
const my_string = "I like dogs because dogs are adorable!";

let pattern = "dogs";
let replacement = "cats";

let my_new_string = my_string.replace(pattern,replacement);

console.log(my_new_string);

// output
// I like cats because dogs are adorable! 
```

这就是`replace()`使用正则表达式作为第一个参数的工作方式:

```
const my_string = "I like dogs because dogs are adorable!";

let pattern = /dogs/;
let replacement = "cats";

let my_new_string = my_string.replace(pattern,replacement);

console.log(my_new_string);

// output
// I like cats because dogs are adorable! 
```

使用`replace()`方法执行全局替换的唯一方法是使用带有`g`标志的正则表达式:

```
const my_string = "I like dogs because dogs are adorable!";

let pattern = /dogs/g;
let replacement = "cats";

let my_new_string = my_string.replace(pattern,replacement);

console.log(my_new_string);

// output

// I like cats because cats are adorable! 
```

## 结论

现在你知道了！现在你知道了`replaceAll()`方法是如何工作的，以及使用它的一些方法。

要了解更多关于 JavaScript 的知识，请访问 freeCodeCamp 的 [JavaScript 算法和数据结构认证](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)。

这是一个免费的、经过深思熟虑的、结构化的课程，你可以在其中进行互动学习。最后，您还将构建 5 个项目来获得您的认证并巩固您的知识。

感谢阅读！*