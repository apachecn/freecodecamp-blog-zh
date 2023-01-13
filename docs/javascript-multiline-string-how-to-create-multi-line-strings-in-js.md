# JavaScript 多行字符串——如何在 JS 中创建多行字符串

> 原文：<https://www.freecodecamp.org/news/javascript-multiline-string-how-to-create-multi-line-strings-in-js/>

在本文中，您将学习用 JavaScript 创建多行字符串的三种不同方法。

我将首先解释 JavaScript 中字符串的基础知识，并回顾如何使用模板文字。然后，您将学习如何在代码示例的帮助下创建跨多行的字符串。

以下是我们将要介绍的内容:

1.  [JavaScript 中的字符串是什么？](#definition)
    1.  什么是模板文字？为什么以及如何使用模板文字
2.  [如何创建多行字符串](#multi-line)
    1.  [如何用模板文字创建多行字符串](#multi-line-template)
    2.  [如何使用`+`操作符](#operator-1)创建多行字符串
    3.  [如何使用 or 运算符`\`创建多行字符串](#operator-2)

## JavaScript 中的字符串是什么？关于如何在 JS 中创建字符串的介绍

字符串是通过文本进行交流的有效方式。

字符串是字符值的有序序列。具体来说，字符串是一个或多个字符的序列，这些字符可以是字母、数字或符号(如标点符号)。

在 JavaScript 中有三种方法可以创建字符串:

*   使用单引号。
*   通过使用双引号。
*   通过使用反勾号。

下面是如何使用**单引号**创建字符串:

```
// string created using single quotes ('')
let favePhrase = 'Hello World!'; 
```

下面是如何使用**双引号**创建字符串:

```
// string created using double quotes ("")
let favePhrase = "Hello World!"; 
```

下面是如何使用**反斜线**创建一个字符串:

```
// string created using backticks (``)
let favePhrase = `Hello World!`; 
```

在 JavaScript 中创建字符串的最后一种方式被称为**模板文字**。

我创建了一个名为`favePhrase`的变量。

在变量内部，我存储了字符串`Hello World!`，它是我用三种不同的方式创建的。

要在浏览器的控制台中查看字符串的输出，请将变量名传递给`console.log();`。

例如，如果我想查看用双引号创建的字符串的输出，我会执行以下操作:

```
// string created using double quotes ("")
let favePhrase = "Hello World!";

// print string to the console
console.log(favePhrase);

// output

// Hello World! 
```

使用单引号或双引号创建字符串的工作原理是一样的，所以两者没有区别。

您可以选择在整个文件中使用其中一个或两个。也就是说，在文件中保持一致是个好主意。

创建字符串时，确保两边使用的引号类型相同。

```
// Don't do this
let favePhrase = 'Hello World!";

console.log(favePhrase);

// output

// Uncaught SyntaxError: Invalid or unexpected token (at test.js:2:18) 
```

另一件要注意的事情是，你可以在一种引用中使用另一种。

例如，您可以在单引号中使用双引号，如下所示:

```
let favePhrase = 'My fave phrase is "Hello World"!'; 
```

确保内部引号与周围的引号不匹配，因为这样做会导致错误:

```
// Don't do this
let favePhrase = 'My fave phrase is 'Hello World'! ';

console.log(favePhrase)

// output

//Uncaught SyntaxError: Unexpected identifier (at test.js:2:38) 
```

当您试图在单引号中使用撇号时，也会发生同样的情况:

```
// Don't do this
let favePhrase = 'My fave phrase is "Hello world"! Isn't it awesome?';

console.log(favePhrase);

// output

// Uncaught SyntaxError: Unexpected identifier (at test.js:3:56) 
```

我在双引号中使用了单引号，这很有效。然而，当我引入撇号时，代码中断了。

实现这一点的方法是使用`\`转义字符来转义单引号:

```
let favePhrase = 'My fave phrase is \'Hello World\'! ';

console.log(favePhrase);

// output

// My fave phrase is 'Hello World'! 
```

要使撇号起作用，您必须执行以下操作:

```
let favePhrase = 'My fave phrase is "Hello world"! Isn\'t it awesome?';

console.log(favePhrase);

// output

// My fave phrase is "Hello world"! Isn't it awesome? 
```

### JavaScript 中的模板文字是什么？为什么以及如何在 JavaScript 中使用模板文字

前面，您已经看到，要创建一个模板文字，您必须使用反斜线。

ES6 引入了模板文字，它们允许您使用字符串执行更复杂的操作。

其中之一是在字符串中内嵌变量的能力，如下所示:

```
let firstName = 'John';
let lastName = 'Doe';

console.log(`Hi! My first name is ${firstName} and my last name is ${lastName}!`);

// output

//Hi! My first name is John and my last name is Doe! 
```

在上面的例子中，我创建了两个变量，`firstName`和`lastName`，分别存储一个人的名字和姓氏。

然后，使用`console.log()`，我打印了一个用反斜线创建的字符串，也称为模板文字。

在这个字符串中，我嵌入了这两个变量。

为此，我将变量名包装在`${}`中——这也被称为**字符串插值**，它允许您引入任何变量，而不必像这样连接它们:

```
let firstName = 'John';
let lastName = 'Doe';

console.log("Hi! My first name is " + firstName + " and my last name is " + lastName + "!");

// output

// Hi! My first name is John and my last name is Doe! 
```

模板文字允许您在其中使用单引号、双引号和撇号，而无需对它们进行转义:

```
let favePhrase = `My fave phrase is "Hello World" ! Isn't it awesome?`

console.log(favePhrase);

// output

// My fave phrase is "Hello World" ! Isn't it awesome? 
```

字符串文字还允许您创建多行字符串，您将在下一节学习如何创建多行字符串。

## 如何在 JavaScript 中创建多行字符串

有三种方法可以创建跨多行的字符串:

*   通过使用模板文本。
*   通过使用`+`操作符 JavaScript 连接操作符。
*   通过使用`\`操作符 JavaScript 反斜杠操作符和转义符。

如果您选择使用单引号或双引号而不是模板文字来创建跨多行的字符串，您将不得不使用`+`操作符或`\`操作符。

### 如何在 JavaScript 中用模板文字创建多行字符串

模板文本允许您创建跨多行的字符串:

```
let learnCoding = `How to start learning web development?
- Learn HTML
- Learn CSS
- Learn JavaScript
Use freeCodeCamp to learn all the above and much, much more !
`

console.log(learnCoding);

// output

// How to start learning web development?
// - Learn HTML
// - Learn CSS
// - Learn JavaScript
// Use freeCodeCamp to learn all the above and much, much more ! 
```

使用模板文字是创建多行字符串最直接的方式。

### 如何在 JavaScript 中使用`+`操作符创建多行字符串

以上一节中的同一个例子为例，下面是如何使用`+`操作符重写它:

```
let learnCoding = 'How to start learning web development?' +
' - Learn HTML' +
' - Learn CSS' +
' - Learn JavaScript' +
' Use freeCodeCamp to learn all the above and much, much more!'

console.log(learnCoding);

// output

// How to start learning web development?  - Learn HTML - Learn CSS - Learn JavaScript Use freeCodeCamp to learn all the above and much, much more! 
```

您还需要包含`\n`换行符来使句子出现在新的一行:

```
let learnCoding = 'How to start learning web development?\n' +
' - Learn HTML\n' +
' - Learn CSS\n' +
' - Learn JavaScript\n' +
' Use freeCodeCamp to learn all the above and much, much more!'

console.log(learnCoding);

// output

//How to start learning web development?
// - Learn HTML
// - Learn CSS
// - Learn JavaScript
// Use freeCodeCamp to learn all the above and much, much more! 
```

### 如何在 JavaScript 中使用`\`操作符创建多行字符串

如果您想使用`\`操作符，那么您可以重写上一节中的例子:

```
let learnCoding = 'How to start learning web development? \n \
 - Learn HTML \n \
 - Learn CSS\n  \
 - Learn JavaScript \n \
Use freeCodeCamp to learn all the above and much, much more!'

console.log(learnCoding);

// output

// let learnCoding = 'How to start learning web development? \n \
// - Learn HTML \n \
// - Learn CSS\n  \
// - Learn JavaScript \n \
//Use freeCodeCamp to learn all the above and much, much more!'

console.log(learnCoding); 
```

在这个例子中，我使用单引号创建了一个多行字符串。

我首先必须使用`\n`换行符，后跟`\`操作符，使字符串跨越多行。

确保将`\`操作符放在`\n`换行符之后。

## 结论

现在你知道了！您现在知道了如何在 JavaScript 中创建多行字符串。

要了解更多关于 JavaScript 的知识，请访问 freeCodeCamp 的 [JavaScript 算法和数据结构认证](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)。

这是一个免费的、经过深思熟虑的、结构化的课程，你可以在其中进行互动学习。最后，您还将构建 5 个项目来获得您的认证并巩固您的知识。

感谢阅读！