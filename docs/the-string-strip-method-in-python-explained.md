# 用例子解释 Python 字符串方法

> 原文：<https://www.freecodecamp.org/news/the-string-strip-method-in-python-explained/>

## **字符串查找方法**

在 Python 中，有两个选项可以用来查找字符串中的子字符串，`find()`和`rfind()`。

每个都将返回子字符串所在的位置。两者的区别在于`find()`返回最低位置，`rfind()`返回最高位置。

可以提供可选的 start 和 end 参数，以将子字符串的搜索限制在字符串的某些部分内。

示例:

```
>>> string = "Don't you call me a mindless philosopher, you overweight glob of grease!"
>>> string.find('you')
6
>>> string.rfind('you')
42
```

如果找不到子字符串，则返回-1。

```
>>> string = "Don't you call me a mindless philosopher, you overweight glob of grease!"
>>> string.find('you', 43)  # find 'you' in string anywhere from position 43 to the end of the string
-1
```

更多信息:

字符串方法[文档](https://docs.python.org/3/library/stdtypes.html#string-methods)。

## **字符串连接方法**

`str.join(iterable)`方法用于用指定的字符串`str`连接`iterable`中的所有元素。如果 iterable 包含任何非字符串值，它将引发 TypeError 异常。

`iterable`:字符串的所有可重复项。可以是字符串列表，字符串元组，甚至是一个普通的字符串。

#### **例题**

用`":"`连接一串字符串

```
print ":".join(["freeCodeCamp", "is", "fun"])
```

输出

```
freeCodeCamp:is:fun
```

用`" and "`连接一组字符串

```
print " and ".join(["A", "B", "C"])
```

输出

```
A and B and C
```

在字符串中的每个字符后插入一个`" "`

```
print " ".join("freeCodeCamp")
```

输出:

```
f r e e C o d e C a m p
```

用空字符串连接。

```
list1 = ['p','r','o','g','r','a','m']  
print("".join(list1))
```

输出:

```
program
```

用器械包连接。

```
test =  {'2', '1', '3'}
s = ', '
print(s.join(test))
```

输出:

```
2, 3, 1
```

#### **更多信息:**

[关于字符串连接的 Python 文档](https://docs.python.org/2/library/stdtypes.html#str.join)

## **字符串替换方法**

使用`str.replace(old, new, max)`方法将子串`old`替换为字符串`new`，总共替换了`max`次。此方法返回替换字符串的新副本。原来的字符串`str`不变。

#### **例题**

1.  用`"WAS"`替换所有出现的`"is"`

```
string = "This is nice. This is good."
newString = string.replace("is","WAS")
print(newString)
```

输出

```
ThWAS WAS nice. ThWAS WAS good.
```

1.  用`"WAS"`替换前两次出现的`"is"`

```
string = "This is nice. This is good."
newString = string.replace("is","WAS", 2)
print(newString)
```

输出

```
ThWAS WAS nice. This is good.
```

#### **更多信息:**

在 [Python 文档](https://docs.python.org/2/library/string.html#string.replace)中阅读更多关于字符串替换的内容

## **串条法**

Python 中从字符串中剥离字符有三个选项，`lstrip()`、`rstrip()`和`strip()`。

每个函数都将返回从开头、结尾或开头和结尾都删除了字符的字符串副本。如果没有给定参数，缺省情况是去掉空白字符。

示例:

```
>>> string = '   Hello, World!    '
>>> strip_beginning = string.lstrip()
>>> strip_beginning
'Hello, World!    '
>>> strip_end = string.rstrip()
>>> strip_end
'   Hello, World!'
>>> strip_both = string.strip()
>>> strip_both
'Hello, World!'
```

一个可选参数可以作为一个字符串提供，该字符串包含您希望删除的所有字符。

```
>>> url = 'www.example.com/'
>>> url.strip('w./')
'example.com'
```

但是，请注意，只有第一个`.`从字符串中删除。这是因为`strip`函数只去除位于左边或最右边的参数字符。因为 w 出现在第一个`.`之前，所以它们被一起剥离，而‘com’出现在剥离`/`之后的`.`之前的右端。

## 字符串拆分方法

`split()`函数通常用于 Python 中的字符串拆分。

#### **`split()`法**

模板:`string.split(separator, maxsplit)`

`separator`:分隔符字符串。你根据这个字符拆分字符串。例如，它可以是“”、“:”、“”等等

`maxsplit`:根据`separator`拆分字符串的次数。如果未指定或为-1，则根据所有出现的`separator`分割字符串

该方法返回由`separator`分隔的子字符串列表

#### **例题**

在空格上分割字串: " "

```
string = "freeCodeCamp is fun."
print(string.split(" "))
```

输出:

```
['freeCodeCamp', 'is', 'fun.']
```

逗号分隔字符串: "，"

```
string = "freeCodeCamp,is fun, and informative"
print(string.split(","))
```

输出:

```
['freeCodeCamp', 'is fun', ' and informative']
```

未指定`separator`

```
string = "freeCodeCamp is fun and informative"
print(string.split())
```

输出:

```
['freeCodeCamp', 'is', 'fun', 'and', 'informative']
```

注意:如果没有指定`separator`，那么字符串将去掉 ****中所有的**** 空格

```
string = "freeCodeCamp        is     fun and    informative"
print(string.split())
```

输出:

```
['freeCodeCamp', 'is', 'fun', 'and', 'informative']
```

使用`maxsplit`分割字符串。这里我们将“”上的字符串拆分两次:

```
string = "freeCodeCamp is fun and informative"
print(string.split(" ", 2))
```

输出:

```
['freeCodeCamp', 'is', 'fun and informative']
```

#### **更多信息**

查看关于字符串拆分的 [Python 文档](https://docs.python.org/2/library/stdtypes.html#str.split)