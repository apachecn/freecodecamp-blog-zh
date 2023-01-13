# JavaScript Replace–如何使用 String.prototype.replace()方法 JS 示例

> 原文：<https://www.freecodecamp.org/news/javascript-replace-how-to-use-the-string-prototype-replace-method-js-example/>

`String.prototype.replace()`方法搜索第一个出现的字符串，并用指定的字符串替换它。这样做不会改变原始字符串。

这种方法也适用于正则表达式，所以您要搜索的项目可以用正则表达式来表示。

作为替换值返回的值可以表示为字符串或函数。

## String.prototype.replace()方法的基本语法

```
const variable = variable.replace("stringToReplace", "expectedString"); 
```

您可以通过以下方式使用`replace()`方法:

*   将初始字符串赋给变量
*   声明另一个变量
*   对于新变量的值，使用 replace()方法在新变量名称前添加前缀
*   用逗号分隔要替换的字符串和括号中的预期字符串

## String.prototype.replace()方法的示例

一个基本的例子是这样的:

```
const coding = "I learned how to code from TV";
const replacedString = coding.replace("TV", "freeCodeCamp");

console.log(replacedString); // Result: I learned how to code from freeCodeCamp 
```

在上面的例子中:

*   我声明了一个名为 coding 的变量，并给它分配了字符串“`I learned how to code from TV`”
*   我声明了另一个名为`replacedString`的变量
*   对于`replacedString`变量的值，我引入了`replace()`方法，并指定我想用“freeCodeCamp”替换初始变量中的“TV”。

下面的例子展示了初始字符串不会被`replace()`方法改变:

```
const coding = "I learned how to code from TV";
const replacedString = coding.replace("TV", "freeCodeCamp");

console.log(replacedString); // Result: I learned how to code from freeCodeCamp
console.log(coding); // Result: I learned how to code from TV 
```

在下面的示例中，我使用正则表达式来搜索匹配“TV”的文本，并将其替换为“freeCodeCamp”:

```
const coding = "I learned how to code from TV";
const replacedString = coding.replace(/TV/, "freeCodeCamp");

console.log(replacedString); // Result: I learned how to code from freeCodeCamp 
```

由于`replace()`方法只适用于字符串中第一次出现的文本，如果您想用字符串中的另一个单词替换一个单词的所有出现，该怎么做呢？可以用`replaceAll()`的方法。

## 如何使用`replaceAll()`方法

与`replace()`方法稍微相似的一个字符串方法是`replaceAll()`方法。

这个方法替换字符串中某个单词的所有出现。

### `replaceAll()`方法的例子

```
const coding = "I learned how to code from TV. TV remains in my heart for life.";
const replacedString = coding.replaceAll("TV", "freeCodeCamp");

console.log(replacedString); // Result: I learned how to code from freeCodeCamp. freeCodeCamp remains in my heart for life. 
```

由于使用了`replaceAll()`方法，每次出现的“TV”都被替换为“freeCodeCamp”。

使用最初的`replace()`方法，您可以通过使用正则表达式来搜索字符串中某个单词的每一次出现，并用另一个单词替换它，从而实现`replaceAll()`所做的事情。

```
const coding = "I learned how to code from TV. TV remains in my heart for life.";
const replacedString = coding.replace(/TV/g, "freeCodeCamp");

console.log(replacedString); // Result: I learned how to code from freeCodeCamp. freeCodeCamp remains in my heart for life. 
```

我可以用正则表达式的`g`标志搜索匹配“TV”的每个单词，并用“freeCodeCamp”替换它。

## 如何将函数传递给`replace()`方法

我之前说过，你可以把你要返回的值作为替换值表示成一个函数。

在下面的例子中，我用 replace 方法将这篇文章的标题转换为 URL slug:

```
const articleTitle = "JavaScript Replace – How to Use the String.prototype.replace() Method";
const slugifyArticleTitle = articleTitle.toLowerCase().replace(/ /g, function (article) {
    return article.split(" ").join("-");
  });

console.log(slugifyArticleTitle); //Result: javascript-replace-–-how-to-use-the-string.prototype.replace()-method 
```

在上面的脚本中:

*   我声明了一个名为`articleTitle`的变量，并指定了本文的标题
*   我声明了另一个名为`slugifyArticleTitle`的变量，并用`toLowerCase()`方法将文章的标题转换成小写字母
*   我引入了`replace()`方法，用`/ /g`搜索每个空格
*   然后，我向`replace()`方法传递了一个函数，并给它分配了一个参数`article`。这个参数指的是转换成小写字母的字符串(文章的标题)
*   在函数内部，我返回了我要在任何有空白的地方拆分文章标题。这是用`split()`方法完成的。
*   在将文章标题分割成空白后，我使用了`join()`方法用连字符连接字符串中的各个字母。

## 结论

`String.prototype.replace()`方法是一个强大的字符串方法，使用它可以在 JavaScript 中处理字符串的同时完成很多事情。

除了`String.prototype.replace()`方法，我还向您展示了如何使用`String.prototype.replaceAll()`方法——一种`String.prototype.replace()`方法的混合。

你应该小心使用`String.prototype.replaceAll()`方法，因为有些浏览器还不支持它。我没有使用`replaceAll()`，而是向您展示了如何通过使用正则表达式搜索特定字符串中的所有值来实现同样的事情。

如果你觉得这篇文章有帮助，不要犹豫，与你的朋友和家人分享。

感谢您的阅读。