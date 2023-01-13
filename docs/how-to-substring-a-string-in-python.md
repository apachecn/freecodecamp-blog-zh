# 如何在 Python 中对字符串进行子串化

> 原文：<https://www.freecodecamp.org/news/how-to-substring-a-string-in-python/>

Python 提供了很多方法来子串一个字符串。这通常被称为“切片”。

下面是语法:

```
string[start:end:step]
```

在哪里，

`start`:子串的起始索引。该索引处的字符包含在子字符串中。如果不包括`start`，则假设等于 0。

`end`:子串的终止索引。该索引处的字符是子字符串中包含的*而不是*。如果没有包含`end`，或者指定的值超过了字符串长度，则默认情况下假定等于字符串的长度。

`step`:当前字符后的每一个“步”字符都要包含在内。默认值为 1。如果不包括`step`，则假设等于 1。

## 基本用法

`string[start:end]`:获取从`start`到`end` - 1 的所有角色

`string[:end]`:获取从字符串开头到`end` - 1 的所有字符

`string[start:]`:获取从`start`到字符串末尾的所有字符

`string[start:end:step]`:获取从`start`到`end` - 1 的所有字符，不包括每一个`step`字符

## 例子

**1。**获取一个字符串的前 5 个字符****

```
string = "freeCodeCamp"
print(string[0:5])
```

输出:

```
> freeC
```

注意:`print(string[:5])`返回与`print(string[0:5])`相同的结果

**2。**得到一个 4 个字符长的子串**，**，**从字符串的第 3 个字符**** 开始**

```
string = "freeCodeCamp"
print(string[2:6])
```

输出:

```
> eeCo
```

**3。**获取字符串的最后一个字符****

```
string = "freeCodeCamp"
print(string[-1])
```

输出:

```
> p
```

请注意，`start`或`end`索引可以是负数。负索引意味着从字符串的结尾而不是开头开始计数(从右到左)。

Index -1 表示字符串的最后一个字符，-2 表示倒数第二个字符，依此类推。

**4。**获取一个字符串的最后 5 个字符****

```
string = "freeCodeCamp"
print(string[-5:])
```

输出:

```
> eCamp
```

**5。**得到一个子串，该子串包含除了最后 4 个字符和第一个字符**T3 之外的所有字符**

```
string = "freeCodeCamp"
print(string[1:-4])
```

输出:

```
> reeCode
```

**6。**从字符串中获取每隔一个字符****

```
string = "freeCodeCamp"
print(string[::2])
```

输出:

```
> feCdCm
```