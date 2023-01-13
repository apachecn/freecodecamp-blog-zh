# JavaScript–JS 字符串到 Int 示例中的 parseInt()

> 原文：<https://www.freecodecamp.org/news/parseint-in-javascript-js-string-to-int-example/>

在本教程中，我们将讨论 JavaScript 中的`parseInt`函数。这个函数解析(分解)一个字符串并返回一个整数或`NaN`(不是一个数字)。

## `parseInt`功能的工作原理

使用`parseInt`函数的主要目的是从字符串中提取一个数字。这将返回值转换为实际数字。

下面是语法:

```
parseInt(string)
```

考虑下面的例子:

```
const myNumber = '3';
console.log(2 + myNumber);
// returns 23
```

在上面的例子中，3 是一个字符串，而不是一个实际的数字。当我们把 2 加到字符串中时，我们得到 23，因为我们只是把 2 加到一个数字形式的字符串中。

使用`parseInt`函数，我们可以从字符串中提取 3，并将其转换为实际数字。这里有一个例子:

```
const myNumber = '3';
console.log(2 + parseInt(myNumber));
// returns 5 
```

现在，该函数已经从字符串中删除了 3，并将其转换为实际数字。

从' 3 '到 3。

回想一下，我们说过`parseInt`函数既可以返回一个整数，也可以返回`NaN`。那么我们什么时候会得到一个`NaN`值呢？

当字符串中的数字前有一些文本时，就会发生这种情况。类似于“年龄是 50”将返回一个`NaN`值，因为`parseInt`函数只查看开始字符串的第一个值。如果值不是数字，则返回`NaN`。这里:

```
const age = 'age is 50';
console.log(parseInt(age));
// returns NaN
```

让我们重新排列绳子，看看会发生什么。

```
const age = '50 is the age';
console.log(parseInt(age));
// returns 50
```

现在字符串中的第一个值是一个数字，这个数字被返回给我们。

注意，`parseInt`函数忽略浮点值。如果上面的年龄是 50.05，那么它仍然会返回 50 并忽略 0.05。

同样，如果我们有一个这样的字符串:“50 100 150 200”，那么我们只能得到 50。这是因为`parseInt`函数只试图提取字符串的第一个值。

如果字符串的值像这样写在一起:“50istheage”，仍然会返回 50。

## `redix`参数

`parseInt`函数接受名为`redix`的第二个参数。此参数指定要使用的数字系统。如果省略 redix，则默认值为 10。

下面是语法:

```
parseInt(string, radix)
```

这通常是一个介于 2 和 36 之间的整数。如果 redix 的值小于 2 或大于 36，则返回`NaN`。

如果我们指定 redix 为 12，那么这意味着字符串中的数字应该从数字的十二进制值解析为其十进制值。

这里有一个简单的例子:

```
console.log(parseInt("50", 12));

// returns 60
```

我们返回 12，因为以 10 为基数的 50 的十二进制值是 60。

## 结论

在本教程中，我们学习了如何使用`parseInt`函数从字符串中提取数字。

我们还看到了如何使用`redix`参数来指定在转换返回的整数时使用什么数字系统。

感谢您的阅读！