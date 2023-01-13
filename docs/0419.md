# 如何在 JavaScript 中使用正则表达式——初学者教程

> 原文：<https://www.freecodecamp.org/news/regular-expressions-for-beginners/>

正则表达式(regex)是一种有用的编程工具。它们是高效文本处理的关键。知道如何使用 regex 解决问题对开发人员来说是有帮助的，并且可以提高您的工作效率。

在本文中，您将了解正则表达式的基础知识、正则表达式模式符号、如何解释简单的正则表达式模式，以及如何编写自己的正则表达式模式。我们开始吧！

## 什么是正则表达式？

正则表达式是允许您描述、匹配或解析文本的模式。使用正则表达式，您可以做诸如查找和替换文本、验证输入数据是否符合所需格式等类似的事情。

这里有一个场景:您希望验证用户在表单上输入的电话号码是否符合某种格式，比如说，###-###-####(其中#代表一个数字)。解决这个问题的一个方法是:

```
function isPattern(userInput) {
  if (typeof userInput !== 'string' || userInput.length !== 12) {
    return false;
  }
  for (let i = 0; i < userInput.length; i++) {
    let c = userInput[i];
    switch (i) {
      case 0:
      case 1:
      case 2:
      case 4:
      case 5:
      case 6:
      case 8:
      case 9:
      case 10:
      case 11:
        if (c < 0 || c > 9) return false;
        break;
      case 3:
      case 7:
        if (c !== '-') return false;
        break;
    }
  }
  return true;
} 
```

或者，我们可以像这样使用正则表达式:

```
function isPattern(userInput) {
  return /^\d{3}-\d{3}-\d{4}$/.test(userInput);
} 
```

注意我们是如何使用 regex 重构代码的。很神奇吧？这就是正则表达式的力量。

## 如何创建正则表达式

在 JavaScript 中，可以用两种方法之一创建正则表达式:

*   方法 1:使用正则表达式。这由一个用正斜杠括起来的模式组成。您可以在有或没有标志的情况下写这个(我们很快会看到标志是什么意思)。语法如下:

```
const regExpLiteral = /pattern/;          // Without flags

const regExpLiteralWithFlags = /pattern/; // With flags 
```

正斜线`/…/`表示我们正在创建一个正则表达式模式，就像您使用引号`“ ”`创建一个字符串一样。

*   方法 2:使用 RegExp 构造函数。语法如下:

```
new RegExp(pattern [, flags]) 
```

这里，模式用引号括起来，与 flag 参数一样，它是可选的。

那么什么时候使用这些模式呢？

当您在编写代码时知道正则表达式模式时，应该使用正则表达式文本。

另一方面，如果要动态创建 regex 模式，请使用 Regex 构造函数。此外，regex 构造函数允许您使用模板文字编写模式，但这对于 regex 文字语法是不可能的。

### 什么是正则表达式标志？

标志或修饰符是启用高级搜索功能的字符，包括不区分大小写和全局搜索。您可以单独或集体使用它们。一些常用的有:

*   `g`用于全局搜索，这意味着搜索在第一次匹配后不会返回。
*   `i`用于不区分大小写的搜索，这意味着无论大小写如何都可以匹配。
*   `m`用于多行搜索。
*   `u`用于 Unicode 搜索。

让我们看一些使用这两种语法的正则表达式模式。

#### 如何使用正则表达式文字:

```
// Syntax: /pattern/flags

const regExpStr = 'Hello world! hello there';

const regExpLiteral = /Hello/gi;

console.log(regExpStr.match(regExpLiteral));

// Output: ['Hello', 'hello'] 
```

请注意，如果我们没有用`i`标记模式，将只返回`Hello`。

模式`/Hello/`是一个简单模式的例子。简单模式由必须在目标文本中逐字出现的字符组成。要进行匹配，目标文本必须遵循与模式相同的序列。

例如，如果您重写上一个示例中的文本并尝试匹配它:

```
const regExpLiteral = /Hello/gi;

const regExpStr = 'oHell world, ohell there!';

console.log(regExpStr.match(regExpLiteral));

// Output: null 
```

我们得到空值,因为字符串中的字符没有按照模式中指定的方式出现。所以像`/hello/`这样的字面模式，意思是 *h* 后面跟着 *e* 后面跟着 *l* 后面跟着 *l* 后面跟着 *o* ，就像这样。

#### 如何使用正则表达式构造函数:

```
// Syntax: RegExp(pattern [, flags])

const regExpConstructor = new RegExp('xyz', 'g'); // With flag -g

const str = 'xyz xyz';

console.log(str.match(regExpConstructor));

// Output: ['xyz', 'xyz'] 
```

这里，模式`xyz`作为与标志相同的字符串传入。另外，两次出现的`xyz`都匹配，因为我们传入了-g 标志。没有它，将只返回第一个匹配。

我们还可以使用构造函数将动态创建的模式作为模板文字传入。例如:

```
const pattern = prompt('Enter a pattern');
// Suppose the user enters 'xyz'

const regExpConst = new RegExp(`${pattern}`, 'gi');

const str = 'xyz XYZ';

console.log(str.match(regExpConst)); // Output: ['xyz', 'XYZ'] 
```

## 如何使用正则表达式特殊字符

正则表达式中的**特殊** **字符**是具有保留意义的字符。使用特殊字符，您可以做的不仅仅是查找直接匹配。

例如，如果您想要匹配字符串中可能出现或不出现一次或多次的字符，您可以使用特殊字符来实现。这些角色属于不同的小组，执行相似的功能。

让我们来看看每一个子群和它们所对应的角色。

### 锚和边界:

**锚点**是元字符，匹配他们正在检查的一行文本的开始和结束。你用它们来断言边界应该在哪里。

使用的两个字符是`^`和`$`。

*   匹配一行的开始，并在该行的开始锚定一个文本。例如:

```
const regexPattern1 = /^cat/;

console.log(regexPattern1.test('cat and mouse')); // Output: true

console.log(regexPattern1.test('The cat and mouse')); // Output: false because the line does not start with cat

// Without the ^ in the pattern, the output will return true
// because we did not assert a boundary.

const regexPattern2 = /cat/;

console.log(regexPattern2.test('The cat and mouse')); // Output: true 
```

*   匹配一行的末尾，并在该行的末尾锚定一个文字。例如:

```
const regexPattern = /cat$/;

console.log(regexPattern.test('The mouse and the cat')); // Output: true

console.log(regexPattern.test('The cat and mouse')); // Output: false 
```

注意，锚点字符`^`和`$` **只匹配模式**中字符的位置，而不是实际的字符本身。

**单词边界**是匹配单词开始和结束位置的元字符——一系列字母数字字符。你可以把它们想象成基于单词的`^`和`$`的版本。您使用元字符`b`和`B`来断言单词边界。

*   `\b`匹配单词的开头或结尾。根据元字符的位置匹配单词。这里有一个例子:

```
// Syntax 1: /\b.../ where .... represents a word.

// Search for a word that begins with the pattern ward
const regexPattern1 = /\bward/gi;

const text1 = 'backward Wardrobe Ward';

console.log(text1.match(regexPattern1)); // Output: ['Ward', 'Ward']

// Syntax 2: /...\b/

// Search for a word that ends with the pattern ward
const regexPattern2 = /ward\b/gi;

const text2 = 'backward Wardrobe Ward';

console.log(text2.match(regexPattern2)); // Output: ['ward', 'Ward']

// Syntax 3: /\b....\b/

// Search for a stand-alone word that begins and end with the pattern ward
const regexPattern3 = /\bward\b/gi;

const text3 = 'backward Wardrobe Ward';

console.log(text3.match(regexPattern3)); // Output: ['Ward'] 
```

*   `\B`是`\b`的反义词。它匹配每一个`\b`没有的位置。

### 其他元字符的短代码:

除了我们已经研究过的元字符之外，下面是一些最常用的元字符:

*   `\d`–匹配任何十进制数字，是[0-9]的简写。
*   `\w`–匹配任何字母数字字符，可以是字母、数字或下划线。`\w`是【A-Za-z0-9_】的简称。
*   `\s`–匹配任何空白字符。
*   `\D`–匹配任何非数字，与【^0-9.】相同]
*   `\W`–匹配任何非单词(即非字母数字)字符，是【^A-Za-z0-9_].】的简写
*   `\S`–匹配非空白字符。
*   `.`–匹配任何字符。

### 什么是角色类？

字符类用于匹配特定位置的几个字符中的任何一个。要表示一个字符类，您可以使用方括号`[]`，然后在方括号中列出您想要匹配的字符。

让我们看一个例子:

```
// Find and match a word with two alternative spellings

const regexPattern = /ambi[ea]nce/;

console.log(regexPattern.test('ambiance')); // Output: true

console.log(regexPattern.test('ambiance')); // Output: true

// The regex pattern interprets as:  find a followed by m, then b,
// then i, then either e or a, then n, then c, and then e. 
```

### 什么是否定的字符类？

如果您在字符类中添加一个脱字符号，就像这个`[^...]`，它将匹配任何没有在方括号中列出的字符。例如:

```
const regexPattern = /[^bc]at/;

console.log(regexPattern.test('bat')); // Output: false

console.log(regexPattern.test('cat')); // Output: false

console.log(regexPattern.test('mat')); // Output: true 
```

### 什么是范围？

当在字符类中使用时，连字符`-`表示范围。假设您想要匹配一组数字，比如说[0123456789]，或者一组字符，比如说[abcdefg]。你可以把它写成这样的范围，分别是[0-9]和[a-g]。

### 什么是另类？

交替是指定一组选项的另一种方式。这里，您使用管道字符`|`来匹配几个子表达式中的任何一个。任何一个子表达式都被称为**替代**。

管道符号的意思是‘或’，所以它匹配一系列选项。它允许您组合子表达式作为选择。

比如`(x|y|z)a`会匹配`xa`或者`ya`，或者`za`。为了限制交替的范围，您可以使用括号将选项组合在一起。

如果没有括号，`x|y|za`将意味着`x`或`y`或`za`。例如:

```
const regexPattern = /(Bob|George)\sClan/;

console.log(regexPattern.test('Bob Clan')); // Output: true

console.log(regexPattern.test('George Clan')); // Output: true 
```

### 什么是量词和贪心？

量词表示一个字符、一个字符类或一个字符组应该在目标文本中出现多少次才能匹配。以下是一些奇特的例子:

*   `+`将匹配它所附加的任何字符，如果该字符至少出现一次。例如:

```
const regexPattern = /hel+o/;

console.log(regexPattern.test('helo'));          // Output:true

console.log(regexPattern.test('hellllllllllo')); // Output: true

console.log(regexPattern.test('heo'));           // Output: false 
```

*   `*`与+字符相似，但略有不同。当您将*附加到一个字符上时，这意味着您想要匹配该字符的任意数量，包括无。这里有一个例子:

```
const regexPattern = /hel*o/;

console.log(regexPattern.test('helo'));    // Output: true

console.log(regexPattern.test('hellllo')); // Output: true

console.log(regexPattern.test('heo'));     // Output: true

// Here the * matches 0 or any number of 'l' 
```

*   `?`暗示“可选”。当你把它附加到一个字符后，就意味着这个字符可能会出现，也可能不会出现。例如:

```
const regexPattern = /colou?r/;

console.log(regexPattern.test('color'));  // Output: true

console.log(regexPattern.test('colour')); // Output: true

// The ? after the character u makes u optional 
```

*   `{N}`，当追加到一个字符或字符类时，指定我们需要多少个字符。例如`/\d{3}/`表示匹配三个连续的数字。
*   `{N,M}`称为区间量词，用于**和**指定最小和最大可能匹配的范围。例如，`/\d{3, 6}/`表示匹配最少 3 个、最多 6 个连续数字。
*   `{N, }`表示开放式范围。例如`/\d{3, }/`表示匹配任意 3 个或更多连续数字。

### 什么是 Regex 中的贪婪？

所有量词默认都是**贪心**。这意味着它们将尝试匹配所有可能的字符。

要删除这个默认状态并使它们不贪婪，您可以像这样给操作符添加一个`?`:`+?`，`*?`，`{N}?`，`{N,M}?`.....诸如此类。

### 什么是分组和反向引用？

我们之前看了如何使用括号来限制交替的范围。

如果您想一次在多个字符上使用像`+`或`*`这样的量词——比如一个字符类或组，该怎么办？您可以在附加量词之前使用括号将它们组合成一个整体，如下例所示:

```
const regExp = /abc+(xyz+)+/i;

console.log(regExp.test('abcxyzzzzXYZ')); // Output: true 
```

模式的意思是:第一个+匹配 abc 的 c，第二个+匹配 xyz 的 z，第三个+匹配子表达式 xyz，如果序列重复，就会匹配。

**反向引用**允许你匹配一个新模式，该模式与正则表达式中先前匹配的模式相同。您还可以使用括号进行反向引用，因为它可以记住它所包含的以前匹配的子表达式(即捕获的组)。

但是，正则表达式中可能有多个捕获组。因此，要反向引用任何捕获的组，需要使用一个数字来标识括号。

假设在一个正则表达式中有 3 个被捕获的组，并且想要反向引用其中的任何一个。你可以用`\1`、`\2`或`\3`来指代第一、第二或第三个括号。要给括号编号，从左边开始数左括号。

让我们看一些例子:

**`(x)`** 匹配 x 并记住匹配。

```
const regExp = /(abc)bar\1/i;

// abc is backreferenced and is anchored at the same position as \1
console.log(regExp.test('abcbarAbc')); // Output: true

console.log(regExp.test('abcbar')); // Output: false 
```

**`(?:x)`** 匹配 x 但不回忆匹配。此外，\n(其中 n 是一个数字)不会记住以前捕获的组，将作为文字进行匹配。使用一个示例:

```
const regExp = /(?:abc)bar\1/i;

console.log(regExp.test('abcbarabc')); // Output: false

console.log(regExp.test('abcbar\1')); // Output: true 
```

### 逃脱法则

如果希望元字符在正则表达式中显示为文字，则必须用反斜杠对其进行转义。对 regex 中的元字符进行转义，元字符就失去了它的特殊意义。

## 正则表达式方法

### `test()`法

在本文中，我们多次使用了这种方法。`test()`方法将目标文本与 regex 模式进行比较，并相应地返回一个布尔值。如果匹配，则返回 true，否则返回 false。

```
const regExp = /abc/i;

console.log(regExp.test('abcdef')); // Output: true

console.log(regExp.test('bcadef')); // Output: false 
```

### `exec()`法

`exec()`方法将目标文本与正则表达式模式进行比较。如果匹配，它返回一个匹配的数组，否则返回 null。例如:

```
const regExp = /abc/i;

console.log(regExp.exec('abcdef'));
// Output: ['abc', index: 0, input: 'abcdef', groups: undefined]

console.log(regExp.exec('bcadef'));
// Output: null 
```

还有一些字符串方法接受正则表达式作为参数，比如`[match()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match)`、`[replace()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)`、`[replaceAll()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll)`、`[matchAll()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)`、`[search()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/search)`和`[split()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)`。

## 正则表达式示例

这里有一些例子来加强我们在这篇文章中学到的一些概念。

第一个例子:如何使用正则表达式模式匹配电子邮件地址:

```
const regexPattern = /^[(\w\d\W)+]+@[\w+]+\.[\w+]+$/i;

console.log(regexPattern.test('abcdef123@gmailcom'));
// Output: false, missing dot

console.log(regexPattern.test('abcdef123gmail.'));
// Output: false, missing end literal 'com'

console.log(regexPattern.test('abcdef123@gmail.com'));
// Output: true, the input matches the pattern correctly 
```

我们来解读一下模式。事情是这样的:

*   **`/`** 代表正则表达式模式的开始。
*   **`^`** 用字符类中的字符检查一行的开头。
*   `**[(\w\d\W)+ ]+**`匹配字符类中的任何单词、数字和非单词字符至少一次。请注意，在添加量词之前，括号是如何用于对字符进行分组的。这个和这个`[\w+\d+\W+]+`一样。
*   `**@**`匹配电子邮件格式中的文字@。
*   `**[\w+]+**`匹配该字符类中的任意单词字符至少一次。
*   对点进行转义，使其显示为文字字符。
*   `**[\w+]+$**`匹配该类中的任何单词字符。此外，这个字符类被锚定在该行的末尾。
*   `**/**` -结束模式

好了，下一个例子:如何匹配格式为 http://example.com 或 https://www.example.com 的 URL:

```
const pattern = /^[https?]+:\/\/((w{3}.)?[\w+]+)\.[\w+]+$/i;

console.log(pattern.test('https://www.example.com'));
// Output: true

console.log(pattern.test('http://example.com'));
// Output: true

console.log(pattern.test('https://example'));
// Output: false 
```

我们也来解读一下这个模式。事情是这样的:

*   `/...../`表示正则表达式模式的开始和结束
*   `^`断言为行的开始
*   `[https?]+`至少匹配一次列出的字符，但是`?`使‘s’可选。
*   `:`匹配一个文字分号。
*   `\/\/`转义两个正斜杠。
*   `(w{3}.)?`匹配字符 w 3 次以及紧随其后的点。但是，该组是可选的。
*   `[\w+]+`至少匹配一次该类中的字符。
*   `\.`逃出生天的小圆点
*   `[\w+]+$` 匹配该类中的任意单词字符。此外，这个字符类被锚定在该行的末尾。

## 结论

在本文中，我们研究了正则表达式的基础。我们还解释了一些正则表达式模式，并用几个例子进行了实践。

正则表达式的内容超出了本文的范围。为了帮助您了解更多关于正则表达式的知识，您可以阅读以下资源:

*   [正则表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
*   [学习 Regex 速成班](https://www.freecodecamp.org/news/regular-expressions-crash-course/)
*   [正则表达式教程](https://www.regular-expressions.info/tutorial.html)
*   [正则表达式备忘单](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet)

这就是本教程的全部内容。快乐编码:)