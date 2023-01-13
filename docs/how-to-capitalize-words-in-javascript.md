# 如何将 JavaScript 中每个单词的首字母大写——JS 大写教程

> 原文：<https://www.freecodecamp.org/news/how-to-capitalize-words-in-javascript/>

在本文中，您将学习如何将 JavaScript 中任何单词的首字母大写。之后，你要将一个句子中所有单词的第一个字母大写。

编程的美妙之处在于没有一个万能的解决方案来解决问题。因此，在本文中，您将看到解决同一个问题的多种方法。

# 将单词的第一个字母大写

首先，让我们从一个单词的首字母大写开始。在你学会如何做到这一点之后，我们将进入下一个阶段——对一个句子中的每个单词都这样做。这里有一个例子:

```
const publication = "freeCodeCamp"; 
```

在 JavaScript 中，我们从 0 开始计数。例如，如果我们有一个数组，第一个位置是 0，而不是 1。

此外，我们可以像访问数组中的元素一样访问字符串中的每个字母。例如，单词" *freeCodeCamp* "的第一个字母在位置 0。

这意味着我们可以通过做`publication[0]`从 *freeCodeCamp* 得到字母 **f** 。

以同样的方式，你可以从单词中获取其他字母。只要不超过单词长度，您可以用任何数字替换“0”。超过单词长度，我的意思是尝试做类似`publication[25`的事情，它会抛出一个错误，因为单词“freeCodeCamp”只有十二个字母。

### 如何将首字母大写

既然我们知道了如何从单词中获取一个字母，让我们把它大写。

在 JavaScript 中，我们有一个叫做`toUpperCase()`的方法，我们可以在字符串或单词上调用它。正如我们可以从名字中暗示的那样，你在一个字符串/单词上调用它，它将返回相同的东西，但是是大写的。

例如:

```
const publication = "freeCodeCamp";
publication[0].toUpperCase(); 
```

运行上面的代码，你将得到一个大写的 F 而不是 F。要得到整个单词，我们可以这样做:

```
const publication = "freeCodeCamp";
publication[0].toUpperCase() + publication.substring(1); 
```

现在它把“F”和“reeCodeCamp”串联起来，这意味着我们又得到了单词“FreeCodeCamp”。仅此而已！

### 让我们回顾一下

为了确保事情清楚，让我们回顾一下目前为止我们所学的内容:

*   在 JavaScript 中，从 0 开始计数。
*   我们可以从字符串中访问一个字母，就像我们从数组中访问一个元素一样——例如`string[index]`。
*   不要使用超过字符串长度的索引(使用 length 方法- `string.length` -找到可以使用的范围)。
*   对您想要转换为大写字母的字母使用内置方法`toUpperCase()`。

# 将字符串中每个单词的第一个字母大写

下一步是拿一个句子，把句子中的每个单词都大写。就拿下面这句话来说吧:

```
const mySentence = "freeCodeCamp is an awesome resource"; 
```

### 把它拆分成单词

我们必须将句子`freeCodeCamp is an awesome resource`中每个单词的第一个字母大写。

我们采取的第一步是把句子分成一组单词。**为什么？因此我们可以单独操作每个单词。我们可以这样做:**

```
const mySentence = "freeCodeCamp is an awesome resource";
const words = mySentence.split(" "); 
```

### 迭代每个单词

在我们运行上面的代码之后，变量`words`被赋予一个数组，其中包含句子中的每个单词。数组如下`["freeCodeCamp", "is", "an", "awesome", "resource"]`。

```
const mySentence = "freeCodeCamp is an awesome resource";
const words = mySentence.split(" ");

for (let i = 0; i < words.length; i++) {
    words[i] = words[i][0].toUpperCase() + words[i].substr(1);
} 
```

现在下一步是循环单词数组，并大写每个单词的第一个字母。

在上面的代码中，每个单词都是分开取的。然后，它将第一个字母大写，最后，它将大写的第一个字母与字符串的其余部分连接起来。

### 加入单词

上面的代码在做什么？它遍历每个单词，并用第一个字母的大写+字符串的其余部分替换它。

如果以“freeCodeCamp”为例，看起来是这样的`freeCodeCamp = F + reeCodeCamp`。

在遍历完所有单词之后，`words`数组就是`["FreeCodeCamp", "Is", "An", "Awesome", "Resource"]`。然而，我们有一个数组，而不是字符串，这不是我们想要的。

最后一步是把所有的单词连接起来组成一个句子。那么，我们该怎么做呢？

在 JavaScript 中，我们有一个叫做`join`的方法，我们可以用它来返回一个字符串形式的数组。该方法将分隔符作为参数。也就是说，我们指定在单词之间添加什么，例如空格。

```
const mySentence = "freeCodeCamp is an awesome resource";
const words = mySentence.split(" ");

for (let i = 0; i < words.length; i++) {
    words[i] = words[i][0].toUpperCase() + words[i].substr(1);
}

words.join(" "); 
```

在上面的代码片段中，我们可以看到运行中的 join 方法。我们在`words`数组上调用它，并指定分隔符，在我们的例子中是一个空格。

于是，`["FreeCodeCamp", "Is", "An", "Awesome", "Resource"]`就变成了`FreeCodeCamp Is An Awesome Resource`。

# 其他方法

在编程中，通常有多种方法来解决同一个问题。所以让我们看看另一种方法。

```
const mySentence = "freeCodeCamp is an awesome resource";
const words = mySentence.split(" ");

words.map((word) => { 
    return word[0].toUpperCase() + word.substring(1); 
}).join(" "); 
```

**上面的解决方案和最初的解决方案有什么区别？**这两个解决方案非常相似，不同之处在于，在第二个解决方案中，我们使用了`map`函数，而在第一个解决方案中，我们使用了`for loop`。

让我们更进一步，尝试做一个**一行程序**。要注意！单行解决方案可能看起来很酷，但在现实世界中很少使用，因为理解它们很有挑战性。代码可读性永远是第一位的。

```
const mySentence = "freeCodeCamp is an awesome resource";

const finalSentence = mySentence.replace(/(^\w{1})|(\s+\w{1})/g, letter => letter.toUpperCase()); 
```

上面的代码使用 **RegEx** 来转换字母。正则表达式可能看起来令人困惑，所以让我解释一下发生了什么:

*   `^`匹配字符串的开头。
*   `\w`匹配任何单词字符。
*   `{1}`只取第一个字符。
*   因此，`^\w{1}`匹配单词的第一个字母。
*   `|`的工作原理类似于布尔运算`OR`。它与`|`前后的表达相匹配。
*   `\s+`匹配单词之间任意数量的空格(例如空格、制表符或换行符)。

因此，用一行代码，我们完成了在上面的解决方案中完成的同样的事情。如果你想使用正则表达式并了解更多，你可以使用这个网站。

# 结论

恭喜你，你今天学到了新东西！概括地说，在本文中，您学习了如何:

*   从字符串中访问字符
*   将单词的第一个字母大写
*   将字符串拆分成单词数组
*   将数组中的单词连接起来形成一个字符串
*   使用正则表达式完成同样的任务

感谢阅读！如果你想保持联系，我们就在 Twitter[@ catalinpit](https://twitter.com/intent/follow?screen_name=catalinmpit)上联系吧。如果你想阅读我的更多内容，我也会定期在我的博客 [catalins.tech](https://catalins.tech) 上发表文章。