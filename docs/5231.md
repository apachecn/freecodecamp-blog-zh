# 如何用 JavaScript 给句子加标题

> 原文：<https://www.freecodecamp.org/news/title-case-a-sentence-in-javascript-929012c88574/>

还记得在小学时，老师教你如何正确地写论文吗？你首先入手的是一个好标题，每个好标题都要适当的大写。

在这个算法脚本挑战中，我们将学习如何用 JavaScript 给句子加标题。最终，我们将让我们的算法接受一个句子，并将每个单词的第一个字母大写，就像它是一篇论文的标题一样。

#### 算法指令

> 返回提供的字符串，每个单词的第一个字母大写。确保单词的其余部分是小写的。

> 为了这个练习的目的，你也应该大写像“the”和“of”这样的连接词。

#### 提供的测试案例

*   `titleCase("I'm a little tea pot")`应该返回一个字符串。
*   `titleCase("I'm a little tea pot")`应该返回`I'm A Little Tea Pot`。
*   `titleCase("sHoRt AnD sToUt")`应该返回`Short And Stout`。
*   `titleCase("HERE IS MY HANDLE HERE IS MY SPOUT")`应该返回`Here Is My Handle Here Is My Spout`。

### 解决方案#1:。map()和。切片( )

#### 佩达克

**理解问题**:我们有一个输入，一个字符串。我们的输出也是一个字符串。最终，我们希望返回输入字符串，其中每个单词的第一个字母都要大写，而且只能是第一个字母。

**示例/测试用例**:我们提供的测试用例显示，我们应该只在每个单词的开头有一个大写字母。我们需要小写其余的。所提供的测试案例还表明，在用符号而不是空格分隔的奇怪复合词方面，我们没有遇到任何难题。这对我们来说是个好消息！

**数据结构**:我们将不得不把输入字符串转换成一个数组，以便分别操作每个单词。

关于我们将使用的方法有几点注意事项:

再说说`[.map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)`:

用对数组中每个元素调用函数的结果创建一个新数组。

换句话说，`.map()`允许我们用一个函数操作数组中的每个元素，然后返回一个包含操作结果的新数组。该函数可以同时针对当前值和当前值的索引，如下所示:

```
array.map((currentValue, Index) => {  // manipulate the currentValue in some way})
```

我们不必总是使用索引。但是，有时我们需要通过索引来定位数组的元素，所以记住这一点很方便。

现在让我们来看一个`.map()`的例子。我们有一个充满数字的数组，我们想把每个数字乘以 2。

```
let arrayOfNumbers = [3, 6, 10, 42, 98]arrayOfNumbers.map(number => number * 2)// returns [6, 12, 20, 84, 196]
```

现在我们来调查一下`[.slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice)`:

提取字符串的一部分，并作为新字符串返回。如果你在一个字符串上调用`.slice()`而没有传递任何附加信息，它将返回整个字符串。

```
"Bastian".slice()// returns "Bastian"
```

我们可以选择传递一个 beginIndex 和 endIndex，就像这样

```
.slice(beginIndex, endIndex)
```

这告诉`.slice()`在哪里开始切片，在哪里结束切片。记住字符串是零索引的！因此，如果我们想从 2 个索引的字母“Bastian”*返回，但不包括*的 5 个索引的字母“Bastian”，我们可以这样做:

```
"Bastian".slice(2, 5)// returns "sti"
```

考虑到这一点，我们可以去掉单词的开头，只传入一个 beginIndex 来返回其余的单词，如下所示:

```
"Bastian".slice(3)// returns "tian"
```

**算法**:

1.  将`str`中的所有字母转换成小写字母。
2.  将小写的`str`拆分成一个数组，每个单词都是数组中的一个独立元素。
3.  将数组中每个元素的第一个字母大写。
4.  将数组的每个元素连接成一个字符串，用空格分隔每个单词。
5.  返回标题大小写字符串。

**代码**:见下图！

我在上面的代码中创建了许多不必要的局部变量，以显示每个方法对输入的影响。下面我删除了局部变量，将所有的方法链接在一起，并删除了注释。

### 解决方案 2:正则表达式

**警告！对于初学者来说，Regex 不是最好的解决方案。正则表达式本身就很难，对于许多有经验的开发人员来说，它们的复杂性是一个普遍的抱怨。**但是，嘿，当我写这篇文章的时候，我感觉很有冒险精神，我喜欢挑战自己，尽可能地进一步理解 regex。这个算法脚本的挑战实际上很适合正则表达式，所以让我们看一看它，看看我们是否能进一步理解正则表达式！

#### 佩达克

**理解问题**:我们有一个输入，一个字符串。我们的输出也是一个字符串。最终，我们希望返回输入字符串，其中每个单词的第一个字母都要大写，而且只能是第一个字母。

**示例/测试用例**:我们提供的测试用例显示，我们应该只在每个单词的开头有一个大写字母。我们需要小写其余的。所提供的测试案例还表明，在用符号而不是空格分隔的奇怪复合词方面，我们没有遇到任何难题。这对我们来说是个好消息！

**数据结构**:在使用正则表达式时，我们不会将字符串转换成数组。JavaScript 有一个漂亮的方法`[.replace()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)`,允许我们在一个字符串中定位几乎任何我们想要的东西，并用其他东西替换它。我们使用正则表达式来定位我们想要替换的内容。

正则表达式中使用了如此多的符号，我不能指望在这里给出它们的概述。不过，我可以给你指出这个[备忘单](https://www.rexegg.com/regex-quickstart.html)，每当我不得不使用 regex 时，我都会用到它。

我能做的就是告诉你，JavaScript 中带`.replace()`的 regex 遵循一个基本模式。`.replace()`接受两个参数:一个模式(通常是正则表达式)和一个替换(可以是字符串或函数)。

```
string.replace(regex, function)
```

在我们的解决方案中，我们将替换每个单词开头的字母。我们如何让 regex 为我们做这件事？我们告诉`.replace()`匹配空格后面的任何字符或者匹配整个字符串的第一个字符(因为字符串的第一个单词前面没有空格)。

让我们分解解决方案中的正则表达式部分。要做到这一点，让我们看看`.replace()`函数的第一个参数。这是正则表达式代码，它决定了我们要匹配和替换的模式。

```
// full solution:
```

```
function titleCase(str) {  return str.toLowerCase().replace(/(^|\s)\S/g,  (firstLetter) => firstLetter.toUpperCase());}
```

我们最终想要找到所有非空白字符，这由`\S`表示。

然后我们想要指定我们想要匹配字符串开头的那些非空白字符`^`或者任何空白字符`\s`后面的`|`。

我们添加了全局修饰符`g`来搜索和替换整个字符串中的所有这样的模式。

**算法**:

1.  将`str`中的所有字母转换成小写字母。
2.  用大写字母替换字符串中每个单词的第一个字母。
3.  返回标题大小写字符串。

**代码**:见下图！

如果你有其他的解决方案和/或建议，请在评论中分享！

#### 本文是系列 [freeCodeCamp 算法脚本](https://medium.com/@DylanAttal/freecodecamp-algorithm-scripting-b96227b7f837)的一部分。

#### 本文参考 [freeCodeCamp 基础算法脚本:标题案例一句话](https://learn.freecodecamp.org/javascript-algorithms-and-data-structures/basic-algorithm-scripting/title-case-a-sentence)

可以在 [Medium](https://medium.com/@DylanAttal) 、 [LinkedIn](https://www.linkedin.com/in/dylanattal/) 、 [GitHub](https://github.com/DylanAttal) 上关注我！