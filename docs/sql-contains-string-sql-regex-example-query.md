# SQL 包含字符串–SQL RegEx 示例查询

> 原文：<https://www.freecodecamp.org/news/sql-contains-string-sql-regex-example-query/>

能够进行复杂的查询在 SQL 中非常有用。

在本文中，我们将看看如何使用`Contains String`查询。

# SQL 模式

SQL 模式对于模式匹配很有用，而不是使用文字比较。它们的语法比 RegEx 更有限，但在各种 SQL 版本中更通用。

SQL 模式使用`LIKE`和`NOT LIKE`操作符以及元字符(代表除自身以外的东西的字符)`%`和`_`。

运算符的用法是这样的:`column_name LIKE pattern`。

| 性格；角色；字母 | 意义 |
| --- | --- |
| `%` | 任何字符序列 |
| `_` | 正好一个字符 |

您可以在各种各样的用例中使用这些字符。以下是一些例子:

| 示例模式 | 使用 |
| --- | --- |
| `re%` | 以特定子字符串开头的字符串 |
| `%re` | 以特定子字符串结尾的字符串 |
| `%re%` | 在字符串的任何位置都有特定子字符串的字符串 |
| `%re_` | 在从末尾开始的特定位置具有特定子字符串的字符串 |
| `__re%` | 从开始的特定位置具有特定子串的字符串 |

(在该示例中，确定第二至最后和第三至最后的字符)
(在该示例中，确定第三和第四字符)

## 示例查询

```
SELECT name FROM planets
  WHERE name LIKE "%us";
```

其中`planets`是一个包含太阳系行星数据的表格。

通过这个查询，你会得到下面以“us”结尾的行星名称。

| 名字 |
| --- |
| 维纳斯 |
| 天王星 |

`NOT LIKE`操作符查找所有与模式不匹配的字符串。

让我们也在一个例子中使用它。

```
SELECT name FROM planets
  WHERE name NOT LIKE "%u%";
```

通过这个查询，您可以获得名称中不包含字母`u`的所有行星，如下所示。

| 名字 |
| --- |
| 地球 |
| 火星 |

## SQL 中`LIKE`操作符的替代

根据您使用的 SQL 风格，您也可以使用`SIMILAR TO`操作符。你可以用它来补充或代替`LIKE`。

### SQL `SIMILAR TO`操作符

`SIMILAR TO`操作符的工作方式与`LIKE`操作符非常相似，包括哪些元字符可用。您可以对任意数量的字符使用`%`操作符，对一个字符使用`_`操作符。

让我们以与`LIKE`一起使用的例子为例，让我们也在这里使用它。

```
SELECT name FROM planets
  WHERE name SIMILAR TO "%us";
```

可以用这个前面带`NOT`的运算符，效果正好相反。在使用`SIMILAR TO`之前，您应该这样编写我们使用的示例:

```
SELECT name FROM planets
  WHERE name NOT SIMILAR TO "%u%";
```

# SQL 中的正则表达式

如果需要更复杂的模式匹配呢？为此你需要使用正则表达式。

## 什么是正则表达式？

正则表达式本身是一个强大的工具，允许灵活的模式识别。你可以在很多语言中使用正则表达式，比如 PHP、Python 和 SQL。

RegEx 允许您通过字符类(如所有字母，或仅元音，或所有数字)匹配模式，在替代项和其他真正灵活的选项之间进行匹配。你会在下面看到它们。

## 你能用正则表达式做什么

使用正则表达式模式可以做很多不同的事情。为了看到各种各样的东西，让我们使用一些在 [RegEx freeCodeCamp 课程](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/#regular-expressions)中提供的例子。

请记住，freeCodeCamp 课程提供的是 JavaScript 的 RegEx，所以没有完美的匹配，我们需要转换语法。尽管如此，它还是给了你一个基本 RegEx 特性的很好的概述，所以让我们跟随这个课程，这样你就能很好地了解 RegEx 能做什么。

### [匹配文字字符串](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-literal-strings)

使用正则表达式最简单的方法是用它来匹配一个精确的字符序列。

例如，正则表达式`"Kevin"`将匹配包含那些字母的所有字符串，如“**凯文**”、“**凯文**很棒”、“这是我的朋友**凯文**”等等。

### [用不同的可能性匹配一个字符串](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-a-literal-string-with-different-possibilities)

使用字符`|`，正则表达式可用于匹配不同的可能性。例如，`"yes|no|maybe"`将匹配包含三个字符序列之一的任何字符串，如"**也许**我会做"，" T4]也许凯琳"，"莫**否**洛格"，" 是，我会做"，" 否，我不喜欢"，等等。

### [匹配任何带有通配符句点的内容](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-anything-with-wildcard-period)

通配符句点`.`匹配任何字符，例如`"hu."`将匹配包含一个`h`后跟一个`u`再后跟任何字符的任何内容，例如"**拥抱**"、"**哼声**"、"**中枢**"、"**哼声**"，而且还有"**胡斯**乐队"、" c **hur** ros "、" t **哼声** b "、" s **小屋**

### **[用多种可能性匹配单个字符](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-single-character-with-multiple-possibilities)**

**您可以使用一个*字符类*(或*字符集*)来匹配一组字符，例如`"b[aiu]g"`将匹配任何包含一个`b`的字符串，然后是一个在`a`、`i`和`u`之间的字母，然后是一个`g`，例如" **bug** 、" **big** 、" **bag** "，还有" cab **bag** e "、" am**

### **[匹配字母表中的字母](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-letters-of-the-alphabet)**

**你已经在上面看到了如何用字符类匹配一组字符，但是如果你想匹配一长串的字母，那就需要大量的输入。**

**为了避免所有的输入，您可以定义一个范围。例如，您可以用`"[a-e]"`匹配`a`和`e`之间的所有字母。**

**像`"[a-e]at"`这样的正则表达式将匹配所有在`a`和`e`之间依次有一个字母、一个`a`和一个`t`的字符串，例如"**猫**"、"**蝙蝠**"和"**吃**"，还有"鸟**蝙蝠** h "、" bu **猫** ini "、" **dat** e "，等等。**

### **[匹配数字和字母表中的字母](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-numbers-and-letters-of-the-alphabet)**

**您也可以使用连字符来匹配数字。例如，`"[0-5]"`将匹配`0`和`5`之间的任何数字，包括`0`和`5`。**

**您也可以在单个字符集中组合不同的范围。例如，`"[a-z0-9]"`将匹配从`a`到`z`的所有字母和从`0`到`5`的所有数字。**

### **[匹配未指定的单个字符](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-single-characters-not-specified)**

**你也可以使用一个字符集从匹配中排除一些字符，这些集被称为*无效字符集*。**

**您可以通过在字符类的左括号后放置一个脱字符(`^`)来创建一个求反的字符集。**

**例如`"[^aeiou]"`匹配所有不是元音的字符。它将匹配像“rythm”这样没有元音字符的字符串，或者“87 + 14”这样的字符串。**

### **[匹配出现一次或多次的字符](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-characters-that-occur-one-or-more-times)**

**如果需要匹配一个特定的字符或一组可以出现一次或多次的字符，可以在这个字符后面使用字符`+`。**

**例如，`"as+i"`将匹配包含一个`a`后跟一个或多个`s`后跟一个`i`的字符串，例如“occ **asi** onal”、“ **assi** duous”等等。**

### **[匹配出现零次或多次的字符](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-characters-that-occur-zero-or-more-times)**

**如果您可以使用`+`来匹配一个字符一次或多次，也有`*`来匹配一个字符零次或多次。**

**诸如`"as*i"`的正则表达式将匹配，除了“occ **asi** onal”和“ **assi** duous”之外，还匹配诸如“ **ai** de”的字符串。**

### **[匹配开始字符串模式](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-beginning-string-patterns)**

**到目前为止，您已经看到了在字符串中的任何位置进行匹配的方法，而没有选择指出匹配必须在哪里。**

**我们使用字符`^`来匹配字符串的开头，例如像`"^Ricky"`这样的正则表达式将匹配“**里基**是我的朋友”，但不是“这是里基”。**

### **[匹配结束字符串模式](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-ending-string-patterns)**

**正如有一种方法可以匹配字符串的开头一样，也有一种方法可以匹配字符串的结尾。**

**您可以使用字符`$`来匹配字符串的结尾，例如`"story$"`将匹配任何以“story”结尾的字符串，例如“这是一个永不结束的**故事**”，而不是诸如“有时一个故事将不得不结束”的字符串。**

### **匹配整个字符串**

**您可以组合两个字符`^`和`$`来匹配整个字符串。**

**因此，以前面的一个例子为例，编写`"b[aiu]g"`可以匹配“big”和“bigger”，但如果相反，您只想匹配“big”、“bag”和“bug”，则添加两个开始和结束字符串字符可以确保字符串中不能有其他字符:`"^b[aiu]g$"`。这种模式只匹配“大”、“包”和“bug”，不匹配“更大”或“不明确”。**

### **[匹配所有字母和数字](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-all-letters-and-numbers)**

**您已经了解了如何用字符类匹配字符。**

**您可以使用一些预定义的类，称为 POSIX 类。所以如果你想匹配所有的字母和数字，比如用`"[0-9a-zA-Z]"`，你可以写`"[[:alphanum:]]"`。**

### **[匹配除字母和数字之外的所有内容](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-everything-but-letters-and-numbers)**

**相反，如果您想匹配不是数字字母的任何内容，您可以使用`alphanum` POSIX 类和一个取反的字符集:`"[^[:alphanum:]]`。**

### **[匹配所有号码](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-all-numbers)**

**也可以使用 POSIX 类来匹配所有数字，而不是使用`"[0-9]"`，比如:`"[[:digit:]]"`。**

### **[匹配所有非数字](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-all-non-numbers)**

**您可以使用带有取反字符集的`digit` POSIX 类来匹配任何不是数字的内容，比如:`"[^[:digit:]]"`。**

### **[匹配空格](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-whitespace)**

**您可以用 POSIX 类`"[[:blank:]]"`或`"[[:space:]]"`匹配空格。这两个类的区别在于，`blank`类只匹配空格和制表符，而`space`匹配所有空白字符，包括回车、换行符、换页符和垂直制表符。**

### **[匹配非空白字符](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/match-non-whitespace-characters)**

**您可以用`"[^[:blank:]]"`匹配任何不是空格或制表符的内容。**

**您可以用`"[^[:space:]]"`匹配除空白、回车、制表符、换页符、空格或垂直制表符之外的任何内容。**

### **[指定匹配的上限和下限数量](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/specify-upper-and-lower-number-of-matches)**

**您之前已经了解了如何匹配一个或多个、零个或多个字符。但是有时候你想匹配一定范围的模式。**

**为此，您可以使用*数量说明符*。**

**数量说明符用花括号写(`{`和`}`)。你把两个数字用逗号分开放在大括号里。第一个是较低数量的模式，第二个是较高数量的模式。**

**例如，如果您的模式是`"Oh{2,4} yes"`，那么它将匹配类似“Ohh yes”或“Ohhhh yes”的字符串，而不是“Oh yes”或“Ohhhhh yes”。**

### **[指定准确的匹配数](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/specify-exact-number-of-matches)**

**您还可以使用范围以外的数量说明符来指定准确的匹配数。你可以通过在大括号内写一个数字来实现。**

**所以，如果你的模式是`"Oh{3} yes"`，那么它将只匹配“Ohhh yes”。**

### **[检查字符的混合分组](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/check-for-mixed-grouping-of-characters)**

**如果您想使用正则表达式检查字符组，您可以使用括号来完成。**

**例如，您可能想同时匹配“企鹅”和“南瓜”，您可以使用这样的正则表达式:`"P(engu|umpk)in"`。**

## **正则表达式模式摘要**

**您已经在这里看到了许多正则表达式选项。所以现在让我们把所有这些，连同其他几个，放入容易查阅的表格中。**

### **正则表达式模式**

| 模式 | 描述 |
| --- | --- |
| `^` | 字符串开头 |
| `$` | 字符串结尾 |
| `.` | 任何字符 |
| `( )` | 分组字符 |
| `[abc]` | 方括号内的任何字符 |
| `[^abc]` | 方括号内的任何字符*不是* |
| `a&#124;b&#124;c` | a 或 b 或 c |
| `*` | 零个或多个前面的元素 |
| `+` | 一个或多个前述元素 |
| `{n}` | 前一个元素的 n 倍 |
| `{n,m}` | 在前元素的 n 到 m 倍之间 |

 **### Posix 类

在下面的表格中，你可以看到我们在上面看到的 posix 类，以及其他一些你可以用来创建模式的类。

| Posix 类 | 与…类似 | 描述 |
| --- | --- | --- |
| `[:alnum:]` | `[a-zA-Z0-9]` | 文字数字式字符 |
| `[:alpha:]` | `[a-zA-Z]` | 字母字符 |
| `[:blank:]` |  | 空格或制表符 |
| `[:cntrl:]` | `[^[:print:]]` | 控制字符 |
| `[:digit:]` | `[0-9]` | 数字字符 |
| `[:graph:]` | `[^ [:ctrl:]]` | 所有具有图形表示的字符 |
| `[:lower:]` | `[a-z]` | 小写字母字符 |
| `[:print:]` | `[[:graph:][:space:]]` | 图形或空格字符 |
| `[:punct:]` |  | 除字母和数字以外的所有图形字符 |
| `[:space:]` |  | 空格、换行符、制表符、回车 |
| `[:upper:]` | `[A-Z]` | 大写字母字符 |
| `[:xdigit:]` | `[0-9a-fA-F]` | 十六进制数字 |

请记住，在使用 POSIX 类时，您总是需要将它放在字符类的方括号内(因此您将有两对方括号)。比如`"a[[:digit:]]b"`匹配`a0b`、`a1b`等等。

## 如何使用正则表达式模式

这里你会看到两种操作符，`REGEXP`操作符和 POSIX 操作符。请注意，您可以使用哪些操作符取决于您使用的 SQL 的风格。

### 正则表达式运算符

RegEx 操作符通常不区分大小写，这意味着它们不区分大写和小写字母。所以对他们来说，`a`相当于`A`。但是你可以改变这种默认行为，所以不要想当然。

| 操作员 | 描述 |
| --- | --- |
| `REGEXP` | 如果匹配给定的模式，则返回 true |
| `NOT REGEXP` | 如果字符串不包含给定的模式，则返回 true |

### Posix 运算符

另一种可用的操作符是 POSIX 操作符。这些不是关键字，而是用标点符号表示，可以区分大小写，也可以不区分大小写。

| 操作员 | 描述 |
| --- | --- |
| `~` | 区分大小写，如果模式包含在字符串中，则为 true |
| `!~` | 区分大小写，如果模式是字符串中包含的**而不是**，则为 true |
| `~*` | 不区分大小写，如果模式包含在字符串中，则为 true |
| `!~*` | 不区分大小写，如果模式是字符串中包含的**而不是**，则为真 |

### RegEx 和 Posix 示例

让我们看看如何在查询中使用这些操作符和正则表达式模式。

#### 示例查询 1

对于第一个示例，您希望匹配第一个字符是“s”或“p”而第二个字符是元音的字符串。

为此，可以使用字符类`[sp]`来匹配第一个字母，可以使用字符类`[aeiou]`来匹配字符串中的第二个字母。

您还需要使用字符来匹配字符串的开头，`^`，所以您将一起编写`"^[sp][aeiou]"`。

您编写下面的查询来获取名称与模式匹配的用户列表。

```
SELECT name FROM users
  WHERE name REGEXP '^[sp][aeiou]';
```

如果改变了默认的不区分大小写的行为，您将需要编写一个允许大写和小写字母的模式，如`"^[spSP][aeiouAEIOU]"`，并在查询中使用它，如下所示:

```
SELECT name FROM users
  WHERE name REGEXP '^[spSP][aeiouAEIOU]';
```

或者使用 POSIX 操作符，在这种情况下，您可以使用不区分大小写的操作符，`~*`,并且您不需要在一个字符类中既写大写字母又写小写字母。您可以编写如下查询。

```
SELECT name FROM users
  WHERE name ~* '^[sp][aeiou]';
```

由于运算符根据定义是不区分大小写的，所以您不需要担心在 character 类中同时指定大写和小写字母。

这些查询将返回一个结果如下的表:

| 名字 |
| --- |
| 【男性名字】塞尔吉奥 |
| 保罗 |
| 萨曼塔 |
| 塞拉菲娜 |

#### 示例查询 2

作为第二个例子，假设你想找到一个十六进制的颜色[。您可以使用 POSIX 类`[:xdigit:]`来实现这一点——它的作用与字符类`[0-9a-fA-F]`相同。](https://www.freecodecamp.org/news/css-background-color-how-to-change-the-background-color-in-html/#hexadecimal-colors)

书写`#[[:xdigit:]]{3}`或`#[[:xdigit:]]{6}`将匹配一个简写或手写形式的十六进制颜色:第一个匹配颜色如`#398`，第二个匹配颜色如`#00F5C4`。

您可以使用字符分组和`|`将它们组合起来，得到一个匹配两者的正则表达式模式，并在查询中使用它，如下所示:

```
SELECR color FROM styles
  WHERE color REGEXP '#([[:xdigit:]]{3}|[[:xdigit:]]{6})'; 
```

```
SELECR color FROM styles
  WHERE color ~ '#([[:xdigit:]]{3}|[[:xdigit:]]{6})';
```

这将返回如下内容:

| 颜色 |
| --- |
| `#341` |
| `#00fa67` |
| `#FF00AB` |

POSIX 类`[:xdigit:]`已经包含了大写和小写字母，所以您不需要担心操作符是否区分大小写。

### 关于资源使用的说明

根据表的大小，Contains 字符串查询可能会占用大量资源。在生产数据库中使用它们时要小心，因为你不想让你的应用程序停止工作。

# 结论

Contains 字符串查询非常有用。您已经在本文中学习了如何使用它们，并且已经看到了一些例子。

希望你已经增加了一个新的工具到你的武器库中，并且你喜欢使用它！小心不要让你的应用崩溃。**