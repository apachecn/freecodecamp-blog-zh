# Python 字符串操作手册——学习如何为初学者操作 Python 字符串

> 原文：<https://www.freecodecamp.org/news/python-string-manipulation-handbook/>

字符串操作是我们作为程序员一直在做的编程活动之一。在许多编程语言中，你必须自己完成许多繁重的工作。

另一方面，在 Python 中，标准库中有几个内置函数可以帮助你以多种不同的方式操作字符串。

在这篇文章中，我将向你展示如何使用字符串以及一些好的技巧。

快速信息:[你可以在这里下载这本 Python 字符串操作手册的 PDF 版本](https://renanmf.com/python-string-manipulation-handbook/)。

准备好潜进去了吗？

## 目录

1.  [Python 字符串基础知识](#pythonstringbasics)
2.  [如何在 Python 中拆分字符串](#howtosplitastringinpython)
3.  [如何在 Python 中删除字符串中的所有空格](#howtoremoveallwhitespacesinastringinpython)
4.  [如何在 Python 中处理多行字符串](#howtohandlemultilinestringsinpython)
5.  [lstrip():如何在 Python 中删除字符串开头的空格和字符](#lstriphowtoremovespacesandcharsfromthebeginningofastringinpython)
6.  [rstrip():如何在 Python 中删除字符串末尾的空格和字符](#rstriphowtoremovespacesandcharsfromtheendofastringinpython)
7.  [strip():如何在 Python 中删除字符串开头和结尾的空格和字符](#striphowtoremovespacesandcharsfromthebeginningandendofastringinpython)
8.  [如何在 Python 中使整个字符串小写](#howtomakeawholestringlowercaseinpython)
9.  [如何在 Python 中使整个字符串大写](#howtomakeawholestringuppercaseinpython)
10.  [如何在 Python 中使用标题大小写](#howtousetitlecaseinpython)
11.  [如何在 Python 中使用 Swap Case](#howtouseswapcaseinpython)
12.  [如何在 Python 中检查字符串是否为空](#howtocheckifastringisemptyinpython)
13.  [rjust():如何在 Python 中右对齐一个字符串](#rjusthowtorightjustifyastringinpython)
14.  [ljust():如何在 Python 中左对齐一个字符串](#ljusthowtoleftjustifyastringinpython)
15.  [isalnum():如何在 Python 中只检查字符串中的字母数字字符](#isalnumhowtocheckforalphanumericcharactersonlyinastringinpython)
16.  [isprintable():如何在 Python 中检查字符串中的可打印字符](#isprintablecheckingprintablecharactersinastringinpython)
17.  [isspace():如何在 Python 中只检查字符串中的空格](#isspacehowtocheckforwhitespaceonlyinastringinpython)
18.  [startswith():如何在 Python 中检查一个字符串是否以某个值开头](#startswithhowtocheckifastringbeginswithacertainvalueinpython)
19.  [capital():如何在 Python 中把字符串的第一个字符只设置成大写](#capitalizehowtosetthefirstcharacteronlytouppercaseinastringinpython)
20.  [isupper():如何在 Python 中检查字符串中的大写字母](#isupperhowtocheckforuppercaseonlyinastringinpython)
21.  [join():如何在 Python 中把一个 Iterable 的条目连接成一个字符串](#joinhowtojoinitemsofaniterableintoonestringinpython)
22.  [splitlines():如何在 Python 中的换行符处分割字符串](#splitlineshowtosplitastringatlinebreaksinpython)
23.  [islower():如何在 Python 中检查字符串中的小写字母](#islowerhowtocheckforlowercaseonlyinastringinpython)
24.  [isnumeric():如何在 Python 中只检查字符串中的数字](#isnumerichowtocheckfornumericsonlyinastringinpython)
25.  [isdigit():如何在 Python 中只检查字符串中的数字](#isdigithowtocheckfordigitsonlyinastringinpython)
26.  [isdecimal():如何在 Python 中只检查字符串中的小数](#isdecimalhowtocheckfordecimalsonlyinastringinpython)
27.  [isalpha():如何在 Python 中只检查字符串中的字母](#isalphahowtocheckforlettersonlyinastringinpython)
28.  [istitle():如何在 Python 中检查字符串中每个单词是否以大写字符开头](#istitlehowtocheckifeverywordbeginswithanuppercasecharinastringinpython)
29.  [expandtabs():如何在 Python 中设置字符串中制表符的空格数](#expandtabshowtosetthenumberofspacesforatabinastringinpython)
30.  [center():如何在 Python 中使字符串居中](#centerhowtocenterastringinpython)
31.  [zfill():如何在 Python 中给字符串加零](#zfillhowtoaddzerostoastringinpython)
32.  [find():如何在 Python 中检查一个字符串是否有某个子串](#findcheckhowtocheckifastringhasacertainsubstringinpython)
33.  [如何在 Python 中移除字符串中的前缀或后缀](#howtoremoveaprefixorasuffixinastringinpython)
34.  [lstrip() vs removeprefix()和 rstrip() vs removesuffix()](#lstripvsremoveprefixandrstripvsremovesuffix)
35.  [Python 中切片的工作原理](#howslicingworksinpython)
36.  [如何在 Python 中反转一个字符串](#howtoreverseastringinpython)
37.  [结论](#conclusion)

## Python 字符串基础

`text`类型是最常见的类型之一，通常被称为*字符串*，或者在 Python 中，简称为`str`。

```
my_city = "New York"
print(type(my_city))

#Single quotes have exactly
#the same use as double quotes
my_city = 'New York'
print(type(my_city))

#Setting the variable type explicitly
my_city = str("New York")
print(type(my_city)) 
```

```
<class 'str'>
<class 'str'>
<class 'str'> 
```

### 如何连接字符串

您可以使用`+`操作符来连接字符串。

串联是指当你有两个或更多的字符串，你想把它们连接成一个。

```
word1 = 'New '
word2 = 'York'

print(word1 + word2) 
```

```
New York 
```

### 如何选择字符

要选择一个字符，使用`[]`并指定字符的位置。

位置 0 是指第一个位置。

```
>>> word = "Rio de Janeiro"
>>> char=word[0]
>>> print(char)
R 
```

### 如何获得字符串的大小

`len()`函数返回一个字符串的长度。

```
>>> len('Rio')
3
>>> len('Rio de Janeiro')
14 
```

### 如何替换字符串的一部分

`replace()`方法用另一部分替换字符串的一部分。举个例子，让我们用“Rio”代替“Mar”。

```
>>> 'Rio de Janeiro'.replace('Rio', 'Mar')
'Mar de Janeiro' 
```

Rio 在葡萄牙语中是河的意思，Mar 是海的意思——只是想让你知道，我并不是那么随意地选择了这个替代品。

### 怎么算

指定作为参数的内容。

在这种情况下，我们正在计算“里约热内卢”中有多少个空格，是 2。

```
>>> word = "Rio de Janeiro"
>>> print(word.count(' '))
2 
```

### 如何重复一个字符串

您可以使用`*`符号重复一个字符串。

这里我们将单词“东京”乘以 3。

```
>>> words = "Tokyo" * 3 
>>> print(words)
TokyoTokyoTokyo 
```

## 如何在 Python 中拆分字符串

将字符串拆分成更小的部分是一项非常常见的任务。为此，我们使用 Python 中的`split()`方法。

让我们看一些如何做到这一点的例子。

### 示例 1:使用空格作为分隔符

在这个例子中，我们通过空格分割短语，创建一个名为 **my_words** 的列表，其中五个条目对应于短语中的每个单词。

```
my_phrase = "let's go to the beach"
my_words = my_phrase.split(" ")

for word in my_words:
    print(word)
#output:
#let's
#go
#to
#the
#beach

print(my_words)
#output:
#["let's", 'go', 'to', 'the', 'beach'] 
```

注意，默认情况下，`split()`方法使用任意连续数量的空格作为分隔符。我们可以将上面的代码改为:

```
my_phrase = "let's go to the beach"
my_words = my_phrase.split()

for word in my_words:
    print(word)

#output:
#let's
#go
#to
#the
#beach 
```

输出是相同的，因为每个单词之间只有一个空格。

### 示例 2:传递不同的参数作为分隔符

在处理数据时，读取一些 CSV 文件以从中提取信息是非常常见的。

因此，您可能需要存储某一列中的某些特定数据。

CSV 文件通常包含由分号“；”分隔的字段或者逗号“，”。

在这个例子中，我们将使用`split()`方法传递一个特定的分隔符“；”作为参数这种情况下。

```
my_csv = "mary;32;australia;mary@email.com"
my_data = my_csv.split(";")

for data in my_data:
    print(data)

#output:
#mary
#32
#australia
#mary@email.com

print(my_data[3])
#output:
# mary@email.com 
```

## 如何在 Python 中删除字符串中的所有空格

如果您想真正删除字符串中的任何空格，只留下字符，最好的解决方案是使用正则表达式。

需要导入提供正则表达式操作的`re`模块。

注意，`\s`不仅代表空格`' '`，还代表换页符`\f`、换行符`\n`、回车符`\r`、制表符`\t`和垂直制表符`\v`。

综上，`\s = [ \f\n\r\t\v]`。

`+`符号称为量词，读作‘一个或多个’。这意味着，在这种情况下，它将考虑一个或多个空白，因为它正好位于`\s`之后。

```
import re

phrase = ' Do   or do    not   there    is  no try   '

phrase_no_space = re.sub(r'\s+', '', phrase)

print(phrase)
# Do   or do    not   there    is  no try   

print(phrase_no_space)
#Doordonotthereisnotry 
```

原始变量`phrase`保持不变。您必须将新的清理后的字符串赋给一个新的变量，在本例中为`phrase_no_space`。

## 如何在 Python 中处理多行字符串

### 三重引号

要在 Python 中处理多行字符串，可以使用单引号或双引号。

第一个例子使用了双引号。

```
long_text = """This is a multiline,

a long string with lots of text,

I'm wrapping it in triple quotes to make it work."""

print(long_text)
#output:
#This is a multiline,
#
#a long string with lots of text,
#
#I'm wrapping it in triple quotes to make it work. 
```

现在和以前一样，但是用单引号:

```
long_text = '''This is a multiline,

a long string with lots of text,

I'm wrapping it in triple quotes to make it work.'''

print(long_text)
#output:
#This is a multiline,
#
#a long string with lots of text,
#
#I'm wrapping it in triple quotes to make it work. 
```

请注意，两个输出是相同的。

### 圆括号

让我们看一个带括号的例子。

```
long_text = ("This is a multiline, "
"a long string with lots of text "
"I'm wrapping it in brackets to make it work.")
print(long_text)
#This is a multiline, a long string with lots of text I'm wrapping it in triple quotes to make it work. 
```

如你所见，结果是不一样的。为了获得新的线条，我必须添加`\n`，就像这样:

```
long_text = ("This is a multiline, \n\n"
"a long string with lots of text \n\n"
"I'm wrapping it in brackets to make it work.")
print(long_text)
#This is a multiline, 
#
#a long string with lots of text 
#
#I'm wrapping it in triple quotes to make it work. 
```

### 反斜杠

最后，反斜杠也是一种可能性。

注意在`\`字符后面没有空格，因为它会抛出一个错误。

```
long_text = "This is a multiline, \n\n" \
"a long string with lots of text \n\n" \
"I'm using backlashes to make it work."
print(long_text)
#This is a multiline, 
#
#a long string with lots of text 
#
#I'm wrapping it in triple quotes to make it work. 
```

## lstrip():如何在 Python 中删除字符串开头的空格和字符

使用`lstrip()`方法删除字符串开头的空格。

```
regular_text = "   This is a regular text."

no_space_begin_text = regular_text.lstrip()

print(regular_text)
#'   This is a regular text.'

print(no_space_begin_text)
#'This is a regular text.' 
```

注意，原来的`regular_text`变量保持不变，因此您需要将方法的返回赋给一个新变量，在本例中为`no_space_begin_text`。

### 如何删除字符

`lstrip()`方法也接受要移除的特定字符作为参数。

```
regular_text = "$@G#This is a regular text."

clean_begin_text = regular_text.lstrip("#$@G")

print(regular_text)
#$@G#This is a regular text.

print(clean_begin_text)
#This is a regular text. 
```

## rstrip():如何在 Python 中删除字符串末尾的空格和字符

使用`rstrip()`方法删除字符串末尾的空格。

```
regular_text = "This is a regular text.   "

no_space_end_text = regular_text.rstrip()

print(regular_text)
#'This is a regular text.   '

print(no_space_end_text)
#'This is a regular text.' 
```

注意，原来的`regular_text`变量保持不变，所以您需要将方法的返回赋给一个新变量，在本例中为`no_space_end_text`。

`rstrip()`方法也接受要移除的特定字符作为参数。

```
regular_text = "This is a regular text.$@G#"

clean_end_text = regular_text.rstrip("#$@G")

print(regular_text)
#This is a regular text.$@G#

print(clean_end_text)
#This is a regular text. 
```

## strip():如何在 Python 中删除字符串开头和结尾的空格和字符

使用`strip()`方法删除字符串开头和结尾的空格。

```
regular_text = "  This is a regular text.   "

no_space_text = regular_text.strip()

print(regular_text)
#'  This is a regular text.   '

print(no_space_text)
#'This is a regular text.' 
```

注意，原来的`regular_text`变量保持不变，所以您需要将方法的返回赋给一个新变量，在本例中为`no_space_text`。

`strip()`方法也接受要移除的特定字符作为参数。

```
regular_text = "AbC#This is a regular text.$@G#"

clean_text = regular_text.strip("AbC#$@G")

print(regular_text)
#AbC#This is a regular text.$@G#

print(clean_text)
#This is a regular text. 
```

## 如何在 Python 中使整个字符串小写

使用`lower()`方法将整个字符串转换成小写。

```
regular_text = "This is a Regular TEXT."

lower_case_text = regular_text.lower()

print(regular_text)
#This is a Regular TEXT.

print(lower_case_text)
#this is a regular text. 
```

注意，原来的`regular_text`变量保持不变，因此您需要将方法的返回赋给一个新变量，在本例中为`lower_case_text`。

## 如何在 Python 中使整个字符串大写

使用`upper()`方法将整个字符串转换成大写。

```
regular_text = "This is a regular text."

upper_case_text = regular_text.upper()

print(regular_text)
#This is a regular text.

print(upper_case_text)
#THIS IS A REGULAR TEXT. 
```

注意，原来的`regular_text`变量保持不变，因此您需要将方法的返回赋给一个新变量，在本例中为`upper_case_text`。

## 如何在 Python 中使用标题大小写

使用`title()`方法将每个单词的第一个字母转换成大写，其余字符转换成小写。

```
regular_text = "This is a regular text."

title_case_text = regular_text.title()

print(regular_text)
#This is a regular text.

print(title_case_text)
#This Is A Regular Text. 
```

注意，原来的`regular_text`变量保持不变，所以您需要将方法的返回赋给一个新变量，在本例中为`title_case_text`。

## 如何在 Python 中使用交换大小写

使用`swapcase()`方法将大写字符转换成小写字符，反之亦然。

```
regular_text = "This IS a reguLar text."

swapped_case_text = regular_text.swapcase()

print(regular_text)
#This IS a reguLar text.

print(swapped_case_text)
#tHIS is A REGUlAR TEXT. 
```

注意，原来的`regular_text`变量保持不变，所以您需要将方法的返回赋给一个新变量，在本例中为`swapped_case_text`。

## 如何在 Python 中检查字符串是否为空

检查`string`是否为空的 pythonic 方法是使用`not`操作符。

```
my_string = ''
if not my_string:
  print("My string is empty!!!") 
```

要检查对面的字符串是否为空，做如下操作:

```
my_string = 'amazon, microsoft'
if my_string:
  print("My string is NOT empty!!!") 
```

## rjust():如何在 Python 中右对齐字符串

使用`rjust()`来右对齐一个字符串。

```
word = 'beach'
number_spaces = 32

word_justified = word.rjust(number_spaces)

print(word)
#'beach'

print(word_justified)
#'                           beach' 
```

注意第二个字符串中的空格。单词“beach”有 5 个字符，这给了我们 27 个空格来填充空白。

原始的`word`变量保持不变，因此我们需要将方法的返回赋给一个新变量，在本例中为`word_justified`。

`rjust()`还接受一个特定的字符作为参数来填充剩余的空间。

```
word = 'beach'
number_chars = 32
char = '

与第一种情况类似，当我计算单词“beach”中包含的 5 个字符时，我有 27 个`$`符号，总共是 32 个。

## ljust():如何在 Python 中左对齐字符串

使用`ljust()`来左对齐一个字符串。

```
word = 'beach'
number_spaces = 32

word_justified = word.ljust(number_spaces)

print(word)
#'beach'

print(word_justified)
#'beach                           ' 
```

注意第二个字符串中的空格。单词“beach”有 5 个字符，这给了我们 27 个空格来填充空白。

原始的`word`变量保持不变，因此我们需要将方法的返回赋给一个新的变量，在本例中为`word_justified`。

`ljust()`还接受一个特定的字符作为参数来填充剩余的空间。

```
word = 'beach'
number_chars = 32
char = '

与第一种情况类似，当我计算单词“beach”中包含的 5 个字符时，我有 27 个`$`符号，总共是 32 个。

## isalnum():如何在 Python 中只检查字符串中的字母数字字符

使用`isalnum()`方法检查字符串是否只包含字母数字字符。

```
word = 'beach'
print(word.isalnum())
#output: True

word = '32'
print(word.isalnum())
#output: True

word = 'number32' #notice there is no space
print(word.isalnum())
#output: True

word = 'Favorite number is 32' #notice the space between words
print(word.isalnum())
#output: False

word = '@number32

## isprintable():如何在 Python 中检查字符串中的可打印字符

使用`isprintable()`方法检查字符串中的字符是否可打印。

```
text = '' # notice this is an empty string, there is no white space here
print(text.isprintable())
#output: True

text = 'This is a regular text'
print(text.isprintable())
#output: True

text = ' ' #one space
print(text.isprintable())
#output: True

text = '                        '  #many spaces
print(text.isprintable())
#output: True

text = '\f\n\r\t\v'
print(text.isprintable())
#output: False 
```

请注意，在前 4 个例子中，每个字符都占用了一些空间，即使它是一个空白，正如您在第一个例子中看到的。

最后一个例子返回`False`，显示了 5 种不可打印的字符:换页符`\f`，换行符`\n`，回车符`\r`，制表符`\t`，垂直制表符`\v`。

这些“看不见的”字符中的一些可能会弄乱你的打印，给你一个意想不到的输出，即使一切“看起来”正常。

## isspace():如何在 Python 中只检查字符串中的空白

使用`isspace()`方法检查字符串中的字符是否都是空格。

```
text = ' '
print(text.isspace())
#output: True

text = ' \f\n\r\t\v'
print(text.isspace())
#output: True

text = '                        '
print(text.isspace())
#output: True

text = '' # notice this is an empty string, there is no white space here
print(text.isspace())
#output: False

text = 'This is a regular text'
print(text.isspace())
#output: False 
```

注意第二个例子中的空白不仅是`' '`，还有换页符`\f`、换行符`\n`、回车符`\r`、制表符`\t`和垂直制表符`\v`。

## startswith():如何在 Python 中检查一个字符串是否以某个值开头

使用`startswith()`方法检查字符串是否以某个值开始。

```
phrase = "This is a regular text"

print(phrase.startswith('This is'))
#output: True

print(phrase.startswith('text'))
#output: False 
```

您还可以设置是否要在字符串的特定位置开始匹配，并在另一个特定位置结束匹配。

```
phrase = "This is a regular text"

print(phrase.startswith('regular', 10)) #the word regular starts at position 10 of the phrase
#output: True

print(phrase.startswith('regular', 10, 22)) #look for in 'regular text'
#output: True

print(phrase.startswith('regular', 10, 15)) ##look for in 'regul'
#output: False 
```

最后，您可能希望一次检查多个字符串。除了使用某种循环，您还可以使用一个元组作为参数，其中包含您想要匹配的所有字符串。

```
phrase = "This is a regular text"

print(phrase.startswith(('regular', 'This')))
#output: True

print(phrase.startswith(('regular', 'text')))
#output: False

print(phrase.startswith(('regular', 'text'), 10, 22)) #look for in 'regular text'
#output: True 
```

## capitalize():如何在 Python 中将字符串中的第一个字符仅设置为大写

使用`capitalize()`方法将字符串中的第一个字符转换成大写。

字符串的其余部分被转换成小写。

```
text = 'this is a regular text'
print(text.capitalize())
#This is a regular text

text = 'THIS IS A REGULAR TEXT'
print(text.capitalize())
#This is a regular text

text = 'THIS $ 1S @ A R3GULAR TEXT!'
print(text.capitalize())
#This $ 1s @ a r3gular text!

text = '3THIS $ 1S @ A R3GULAR TEXT!'
print(text.capitalize())
#3this $ 1s @ a r3gular text! 
```

请注意，任何字符都有效，例如数字或特殊字符。因此，在最后一个示例中，`3`是第一个字符，没有发生变化，而字符串的其余部分被转换为小写。

## isupper():如何在 Python 中检查字符串中的大写字母

使用`isupper()`方法检查字符串中的字符是否都是大写的。

```
text = 'This is a regular text'
print(text.isupper())
#output: False

text = 'THIS IS A REGULAR TEXT'
print(text.isupper())
#output: True

text = 'THIS $ 1S @ A R3GULAR TEXT!'
print(text.isupper())
#output: True 
```

如果您注意到最后一个例子，字符串中的数字和特殊字符如`@`和`$`没有任何区别，并且`isupper()`仍然返回`True`，因为该方法只验证字母字符。

## join():如何在 Python 中将 Iterable 的项连接成一个字符串

使用`join()`方法将 iterable 中的所有条目连接成一个字符串。

基本语法是:`string.join(iterable)`

按照上面的语法，需要一个字符串作为分隔符。

该方法返回一个新字符串，这意味着原始迭代器保持不变。

由于`join()`方法只接受字符串，如果 iterable 中的任何元素是不同的类型，就会抛出错误。

让我们看一些例子:字符串、列表、元组、集合和字典

### join():字符串

`join()`方法将`$`符号作为字符串中每个字符的分隔符。

```
my_string = 'beach'

print('

### join():列表

我有一个代表汽车品牌的三个项目的简单列表。

`join()`方法将使用`$`符号作为分隔符。

它将列表中的所有项目连接起来，并在它们之间放置一个`$`符号。

```
my_list = ['bmw', 'ferrari', 'mclaren']

print('

这个例子提醒你`join()`不能处理非字符串项。

当试图连接`int`项时，会出现一个错误。

```
my_list = [1, 2, 3]

print('

### join():元组

元组遵循与前面解释的列表示例相同的基本原理。

同样，我使用`$`符号作为分隔符。

```
my_tuple = ('bmw', 'ferrari', 'mclaren')

print('

### join():集合

因为集合也与元组和列表相同，所以我在这个例子中使用了不同的分隔符。

```
my_set = {'bmw', 'ferrari', 'mclaren'}
print('|'.join(my_set))
#output: ferrari|bmw|mclaren 
```

### join():字典

当您使用`join()`方法时，字典有一个问题:它连接键，而不是值。

此示例显示了键的连接。

```
my_dict = {'bmw': 'BMW I8', 'ferrari': 'Ferrari F8', 'mclaren': 'McLaren 720S'}

print(','.join(my_dict))
#output: bmw,ferrari,mclaren 
```

## splitlines():如何在 Python 中的换行符处分割字符串

使用`splitlines()`方法在换行符处分割字符串。

该方法的返回是一个行列表。

```
my_string = 'world \n cup'

print(my_string.splitlines())
#output: ['world ', ' cup'] 
```

如果你想保持换行，`splitlines()`接受一个可以设置为*真*的参数，默认为*假*。

```
my_string = 'world \n cup'

print(my_string.splitlines(True))
#output: ['world \n', ' cup'] 
```

## islower():如何在 Python 中只检查字符串中的小写

使用`islower()`方法检查字符串中的字符是否都是小写。

```
text = 'This is a regular text'
print(text.islower())
#output: False

text = 'this is a regular text'
print(text.islower())
#output: True

text = 'this $ 1s @ a r3gular text!'
print(text.islower())
#output: True 
```

如果你注意到在最后一个例子中，字符串中的数字和特殊字符如`@`和`$`没有区别，并且`islower()`仍然返回`True`，因为该方法只验证字母字符。

## isnumeric():如何在 Python 中只检查字符串中的数字

使用`isnumeric()`方法检查字符串是否只包含数字字符。

数字包括从 0 到 9 的数字以及它们的组合、罗马数字、上标、下标、分数和其他变体。

```
word = '32'
print(word.isnumeric())
#output: True

print("\u2083".isnumeric()) #unicode for subscript 3
#output: True

print("\u2169".isnumeric()) #unicode for roman numeral X
#output: True

word = 'beach'
print(word.isnumeric())
#output: False

word = 'number32'
print(word.isnumeric())
#output: False

word = '1 2 3' #notice the space between chars
print(word.isnumeric())
#output: False

word = '@32

`isdecimal()`比`isdigit()`严格，T1 又比`isnumeric()`严格。

## isdigit():如何在 Python 中只检查字符串中的数字

使用`isdigit()`方法检查字符串是否只包含数字。

数字包括从 0 到 9 的数字，也包括上标和下标。

```
word = '32'
print(word.isdigit())
#output: True

print("\u2083".isdigit()) #unicode for subscript 3
#output: True

word = 'beach'
print(word.isdigit())
#output: False

word = 'number32'
print(word.isdigit())
#output: False

word = '1 2 3' #notice the space between chars
print(word.isdigit())
#output: False

word = '@32

`isdecimal()`比`isdigit()`严格，T1 又比`isnumeric()`严格。

## isdecimal():如何在 Python 中只检查字符串中的小数

使用`isdecimal()`方法检查字符串是否只包含小数，即只包含从 0 到 9 的数字以及这些数字的组合。

下标、上标、罗马数字和其他变体将作为`False`返回。

```
word = '32'
print(word.isdecimal())
#output: True

word = '954'
print(word.isdecimal())
#output: True

print("\u2083".isdecimal()) #unicode for subscript 3
#output: False

word = 'beach'
print(word.isdecimal())
#output: False

word = 'number32'
print(word.isdecimal())
#output: False

word = '1 2 3' #notice the space between chars
print(word.isdecimal())
#output: False

word = '@32

`isdecimal()`比`isdigit()`更严格，T1 又比`isnumeric()`更严格。

## isalpha():如何在 Python 中只检查字符串中的字母

使用`isalpha()`方法检查字符串是否只包含字母。

```
word = 'beach'
print(word.isalpha())
#output: True

word = '32'
print(word.isalpha())
#output: False

word = 'number32'
print(word.isalpha())
#output: False

word = 'Favorite number is blue' #notice the space between words
print(word.isalpha())
#output: False

word = '@beach

## istitle():如何在 Python 中检查字符串中每个单词是否以大写字符开头

使用`istitle()`方法检查字符串中每个单词的第一个字符是否大写，其他字符是否小写。

```
text = 'This is a regular text'
print(text.istitle())
#output: False

text = 'This Is A Regular Text'
print(text.istitle())
#output: True

text = 'This $ Is @ A Regular 3 Text!'
print(text.istitle())
#output: True 
```

如果你注意到在最后一个例子中，字符串中的数字和特殊字符如`@`和`$`没有区别，并且`istitle()`仍然返回`True`，因为该方法只验证字母字符。

## expandtabs():如何在 Python 中设置字符串中制表符的空格数

使用`expandtabs()`方法设置制表符的空格数。

您可以设置任意数量的空格，但是如果没有给定参数，则默认值为 8。

### 基本用法

```
my_string = 'B\tR'

print(my_string.expandtabs())
#output: B       R 
```

注意字母 B 和 r 之间的 7 个空格。

`\t`位于一个字符后的第二个位置，因此它将被替换为 7 个空格。

让我们看另一个例子。

```
my_string = 'WORL\tD'

print(my_string.expandtabs())
#output: WORL    D 
```

因为`WORL`有四个字符，所以`\t`被替换为 4 个空格，总共 8 个，这是默认的 tabsize。

下面的代码给了我们四个字符“WORL”后的第一个制表符的 *4* 个空格和一个字符“D”后的第二个制表符的 *7* 个空格。

```
my_string = 'WORL\tD\tCUP'

print(my_string.expandtabs())
#output: WORL    D       CUP 
```

### 自定义标签尺寸

可以根据需要设置 tabsize。

在这个例子中，tabsize 是 *4* ，这给了我们在字符“B”之后 3 个空格。

```
my_string = 'B\tR'

print(my_string.expandtabs(4))
#output: B   R 
```

这段代码将 tabsize 设置为 *6* ，这样在字符‘B’后面就有了 5 个空格。

```
my_string = 'B\tR'

print(my_string.expandtabs(6))
#output: B     R 
```

## center():如何在 Python 中使字符串居中

使用`center()`方法将字符串居中。

```
word = 'beach'
number_spaces = 32

word_centered = word.center(number_spaces)

print(word)
#'beach'

print(word_centered)
##output: '              beach              ' 
```

注意第二个字符串中的空格。单词“beach”有 5 个字符，这给了我们 28 个空格来填充空白，14 个空格在单词前面，14 个空格在单词后面居中。

原始的`word`变量保持不变，因此我们需要将方法的返回赋给一个新变量，在本例中为`word_centered`。

`center()`也接受一个特定的字符作为参数来填充剩余的空间。

```
word = 'beach'
number_chars = 33
char = '

与第一种情况类似，当我计算单词“beach”中包含的 5 个字符时，每边有 14 个`$`,总共有 33 个。

## zfill():如何在 Python 中给字符串加零

使用`zfill()`在字符串的开头插入零`0`。

零的数量由作为参数传递的数量减去字符串中的字符数得出。

单词“beach”有 5 个字符，这给了我们 27 个空格来填充零，使总数达到 32 个，如变量`size_string`中所指定的

```
word = 'beach'
size_string = 32

word_zeros = word.zfill(size_string)

print(word)
#beach

print(word_zeros)
#000000000000000000000000000beach 
```

原始的`word`变量保持不变，因此我们需要将方法的返回赋给一个新变量，在本例中为`word_zeros`。

另请注意，如果参数少于字符串中的字符数，则不会发生任何变化。

在下面的例子中，“beach”有 5 个字符，我们想要添加零，直到它到达 4 的`size_string`，这意味着什么也不做。

```
word = 'beach'
size_string = 4

word_zeros = word.zfill(size_string)

print(word)
#beach

print(word_zeros)
#'beach' 
```

## find():如何在 Python 中检查一个字符串是否有某个子串

使用`find()`方法检查一个字符串是否有某个子串。

方法返回给定值的第一个匹配项的索引。

记住索引计数从 0 开始。

```
phrase = "This is a regular text"

print(phrase.find('This'))

print(phrase.find('regular'))

print(phrase.find('text')) 
```

```
0
10
18 
```

如果没有找到该值，它将返回`-1`。

```
phrase = "This is a regular text"

print(phrase.find('train')) 
```

```
-1 
```

您也可以选择从特定位置开始搜索，并在字符串的另一个特定位置结束搜索。

```
phrase = "This is a regular text"

#look for in 'This is', the rest of the phrase is not included
print(phrase.find('This', 0, 7))

#look for in 'This is a regular'
print(phrase.find('regular', 0, 17))

#look for in 'This is a regul'
print(phrase.find('a', 0, 15)) 
```

```
0
10
8 
```

## 如何在 Python 中删除字符串中的前缀或后缀

从 Python 3.9 开始，String 类型将有两个新方法。

您可以使用`removeprefix()`方法专门从字符串中删除前缀:

```
>>> 'Rio de Janeiro'.removeprefix("Rio")
' de Janeiro' 
```

或者使用`removesuffix()`方法删除后缀:

```
>>> 'Rio de Janeiro'.removesuffix("eiro")
'Rio de Jan' 
```

只需将被视为要删除的前缀或后缀的文本作为参数传递，该方法将返回一个新的字符串作为结果。

如果你想知道这些特性是如何添加到语言中的，我推荐你阅读官方文档中的 PEP 616。

这是一个非常简单的改变，对于习惯阅读官方文档的初学者来说非常友好。

## lstrip() vs removeprefix()和 rstrip() vs removesuffix()

这给很多人造成了困惑。

看着`lstrip()`和`removeprefix()`很容易想知道两者的真正区别是什么。

使用`lstrip()`时，参数是一组前导字符，出现多少次就删除多少次:

```
>>> word = 'hubbubbubboo'
>>> word.lstrip('hub')
'oo' 
```

而`removeprefix()`将仅移除精确匹配:

```
>>> word = 'hubbubbubboo'
>>> word.removeprefix('hub')
'bubbubboo' 
```

你可以用同样的原理来区分`rstrip()`和`removesuffix()`。

```
>>> word = 'peekeeneenee'
>>> word.rstrip('nee')
'peek' 
```

```
>>> word = 'peekeeneenee'
>>> word.removesuffix('nee')
'peekeenee' 
```

另外，万一您以前从未使用过正则表达式，您应该庆幸自己有`strip()`来从字符串而不是正则表达式中修剪字符集:

```
>>> import re
>>> word = 'amazonia'
>>> word.strip('ami')
'zon'
>>> re.search('^[ami]*(.*?)[ami]*

## Python 中切片的工作原理

切片是 Python 语言中最有用的工具之一。

因此，很好地掌握它的工作原理是很重要的。

### 基本切片符号

假设我们有一个名为“list”的数组。

```
list[start:stop:step] 
```

*   开始:希望切片开始的位置
*   停止:直到你想要切片的地方，但是记住*停止*的值不包括在内
*   步骤:如果你想跳过一个项目，缺省值是 1，那么你要遍历数组中的所有项目

### 指数

切片时，索引是字符之间的点，而不是字符上的点。

对于单词“电影”:

```
 +---+---+---+---+---+
 | m | o | v | i | e |
 +---+---+---+---+---+
 0   1   2   3   4   5 
-5  -4  -3  -2  -1 
```

如果我从 0 切片到 2，我得到上面例子中的' mo '和*而不是* 'mov '。

由于字符串只是一个字符列表，这同样适用于列表:

```
my_list = [1, 2 , 3, 4, 5] 
```

变成了:

```
 +---+---+---+---+---+
 | 1 | 2 | 3 | 4 | 5 |
 +---+---+---+---+---+
 0   1   2   3   4   5 
-5  -4  -3  -2  -1 
```

### Python 中切片的例子

我们有一个包含字符串“电影”的变量，如下所示:

```
word = 'movie' 
```

下面所有的例子都适用于这个词。

#### 示例 1

要获得前两个字符:

```
sliced = word[:2]
print(sliced)
mo 
```

注意，我们可以用 0 来表示开始，但这不是必须的。

#### 示例 2

最后一项:

```
sliced = word[-1]
print(sliced)
e 
```

#### 示例 3

以 2 为步长跳过字母:

```
sliced = word[::2]
print(sliced)
mve 
```

## 如何在 Python 中反转字符串

要反转字符串，请使用切片语法:

```
my_string = "ferrari"

my_string_reversed = my_string[::-1]

print(my_string)

print(my_string_reversed) 
```

```
ferrari

irarref 
```

slice 语法允许您设置一个步骤，在本例中为`-1`。

默认的步长是`1`，即一次前进字符串的 1 个字符。

如果您将步长设置为`-1`，则相反，一次返回 1 个字符。

因此，从最后一个字符的位置开始，向后移动到第一个字符的位置 0。

# 结论

就是这样！

祝贺你到达终点。

我想感谢你阅读这篇文章。

如果你想了解更多，请查看我的博客[renanmf.com](https://renanmf.com/)。

记得[下载 PDF 版本的 Python 字符串操作手册](https://renanmf.com/python-string-manipulation-handbook/)。

也可以在推特上找我: [@renanmouraf](https://twitter.com/renanmouraf) 。