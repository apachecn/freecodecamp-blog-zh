# 用 JavaScript 反转一个字符串——用 JS 反转一个字符串。反向()方法

> 原文：<https://www.freecodecamp.org/news/reversing-a-string-in-javascript-invert-a-string-with-the-js-reverse-method/>

在 JavaScript 中反转字符串是您在 web 开发之旅中经常需要做的事情。在面试、求解算法或处理数据时，你可能需要反转一个字符串。

在本文中，我们将学习如何使用内置的 JavaScript 方法以及 JavaScript `reverse()`方法来反转 JavaScript 中的字符串。

对于那些着急的人，这里有一行代码可以帮助你在 JavaScript 中反转一个字符串:

```
let myReversedString = myString.split("").reverse().join(""); 
```

或者你可以用这个:

```
let myReversedString = [...myString].reverse().join(""); 
```

现在让我们来讨论这些方法以及它们在帮助我们在 JavaScript 中反转字符串时所扮演的角色。

## 如何用 JavaScript 方法反转字符串

使用 JavaScript 方法反转字符串很简单。这是因为我们将只使用三种方法，它们执行不同的功能，并且一起使用来实现这个共同的目标。

通常，我们使用 spread 操作符或`split()`方法将特定的字符串分割成一个数组。然后我们使用`reverse()`方法，它只能用来反转一个数组中的元素。最后，我们使用`join()`方法将这个数组连接成一个字符串。

让我们分别尝试这些方法。

### 如何在 JavaScript 中拆分字符串

JavaScript 中有两种主要的拆分字符串的方法:使用 spread 操作符或`split()`方法。

#### 如何用 Split()方法拆分字符串

`split()`方法是一个非常强大的方法，可以用来根据给定的模式将一个字符串分解成有序的子字符串列表。

例如，如果我们有一串用逗号分隔的月份，我们想把它们分成一个月的数组，我们可以这样做:

```
const months_string = 'Jan,Feb,Mar,Apr,May,Jun,Jul,Aug,Sep,Oct,Nov,Dec';

console.log(months_string.split(',')) 
```

这将输出以下数组:

```
["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"] 
```

在我们的例子中，我们的字符串可能是一个没有任何字符分隔的普通字符串。然后我们要做的就是传递一个没有空格的空字符串，如下所示:

```
let myString = "Hello World";

console.log(myString.split("")); // ["H","e","l","l","o"," ","W","o","r","l","d"]
console.log(myString.split(" ")); // ["Hello","World"] 
```

#### 如何使用扩展运算符拆分字符串

spread 操作符是 ES6 的一个新增功能，可以很容易地将一个字符串拆分成一个数组。它不仅仅是拆分字符串:

```
let myString = "Hello World";

console.log([...myString]); // ["H","e","l","l","o"," ","W","o","r","l","d"] 
```

### 如何用`reverse()`方法反转字符串数组

到目前为止，我们已经学会了如何拆分字符串。而`split()`方法，当然是把字符串分成一个数组。现在您可以对其应用反向数组方法，如下所示:

```
let myString = "Hello World";

let splitString1 = myString.split("");
let splitString2 = myString.split(" ");

console.log(splitString1.reverse()); // ["d","l","r","o","W"," ","o","l","l","e","H"]
console.log(splitString2.reverse()); // ["World","Hello"] 
```

我们也可以这样将它应用于 spread 操作符，但是我们将不再能够定义我们想要如何分割我们的字符串:

```
let myString = "Hello World";

console.log([...myString].reverse()); // ["d","l","r","o","W"," ","o","l","l","e","H"] 
```

### 如何用`join()`方法连接一个字符串数组

这是另一个与`split()`方法相反的强大方法。它通过连接数组中由逗号分隔的所有元素或指定为分隔符的任何其他字符串来创建新字符串。

例如，如果我们有一个字符串数组，我们想把它连接成一个由破折号(-)分隔的字符串，我们可以这样做:

```
let monthArray = ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"];
console.log(monthArray.join("-")); 
```

这将返回以下内容:

```
"Jan-Feb-Mar-Apr-May-Jun-Jul-Aug-Sep-Oct-Nov-Dec" 
```

在我们的例子中，我们已经反转了字符串，我们不想要中间的任何东西。这意味着我们将以这种方式传递一个空字符串:

```
let myString = "Hello World";

let splitString1 = myString.split("");
let splitString2 = myString.split(" ");

let reversedStringArray1 = splitString1.reverse();
let reversedStringArray2 = splitString2.reverse();

console.log(reversedStringArray1.join("")); // "dlroW olleH"
console.log(reversedStringArray2.join("")); // "WorldHello" 
```

最后，我们只需一行代码就可以执行所有这些操作，方法是按照正确的顺序将所有方法放在一起:

```
let myString = "Hello World";

let myReversedString = myString.split("").reverse().join("");

console.log(myReversedString); // "dlroW olleH" 
```

这同样适用于 spread 运算符:

```
let myString = "Hello World";

let myReversedString = [...myString].reverse().join("");

console.log(myReversedString); // "dlroW olleH" 
```

## 结论

在本教程中，我们学习了如何使用`reverse()`方法以及其他 JavaScript 方法来反转字符串。我们还看到了这些方法是如何通过例子工作的。

祝编码愉快！