# 罗马数字–代表 4、6、9 和其他数字的罗马数字

> 原文：<https://www.freecodecamp.org/news/roman-numerals-the-roman-numeral-for-4-6-9-and-others/>

罗马数字是一种起源于古罗马的数字系统。它们用于表示十进制系统中的数字，但不用于数学运算。

在这个系统中，用符号来代表不同的数字，I 代表 1，V 代表 5，X 代表 10，L 代表 50，C 代表 100，D 代表 500，M 代表 1000。

以下是罗马数字系统中使用的符号表:

| one | five | Ten | Fifty | One hundred | Five hundred | One thousand |
| --- | --- | --- | --- | --- | --- | --- |
| 我 | V | X | L | C | D | M |

一个数字的值取决于它相对于其他符号的位置。当相等或较小值的符号放在另一个符号之后时，它们的值相加。但是当某些值较小的符号放在另一个符号之前时，它们的值就会被减去。

例如，数字 VI，或 6，读作“五加一”(5 + 1)，XI，或 11，是“十加一”(10 + 1)。

但是表示 4 和 9 的方法是特殊的。罗马数字 IV，或 4，读作“比 5 小一”(5 - 1)。此外，数字 IX 或 9 可以读作“比 10 小一”(10 - 1)。

下面是一个数字及其罗马数字等价物的表格，后面是关于如何执行转换的更深入的解释。只需滚动表格或使用 Ctrl/Cmd + f 找到您要查找的值:

| 数字 | 罗马数字 |
| --- | --- |
| one | 我 |
| Two | 二 |
| three | 罗马数字 3 |
| four | 注入静脉的 |
| five | V |
| six | 我们吗 |
| seven | 罗马数字 7 |
| eight | 八 |
| nine | 离子交换 |
| Ten | X |
| Eleven | 希腊字母的第 14 个字母 |
| Twelve | 罗马数字 12 |
| Thirteen | 罗马数字 13 |
| Fourteen | 罗马数字 14 |
| Fifteen | 十五人组成的球队 |
| Sixteen | 十六 |
| Seventeen | 罗马数字 17 |
| Eighteen | 罗马数字 18 |
| Nineteen | 罗马数字 19 |
| Twenty | xx |
| Twenty-one | 罗马数字 21 |
| Twenty-two | 二十二 |
| Twenty-three | 二十三 |
| Twenty-four | 二十四 |
| Twenty-five | 罗马数字 25 |
| Thirty | XXX |
| Forty | 特大号 |
| Fifty | L |
| Sixty | LX |
| Seventy | 《圣经》的希腊文译本 |
| Eighty | LXXX |
| Ninety | XC |
| One hundred | C |
| One hundred and one | 海峡群岛 |
| One hundred and two | 卡麦基兴趣调查表 |
| One hundred and three | CIII |
| One hundred and four | 奇洛彩虹色病毒 |
| One hundred and five | 履历 |
| Two hundred | 抄送 |
| Three hundred | 控制台控制电路(Console Control Circuits) |
| four hundred | 激光唱片 |
| Five hundred | D |
| Six hundred | 直流电 |
| Seven hundred | 数码磁带 |
| eight hundred | DCCC |
| Nine hundred | 厘米 |
| One thousand | M |
| One thousand and one | 大调音阶的第三音 |
| One thousand and two | 广播级录象机的型号之一 |
| One thousand and three | MIII |
| One thousand and four | MIV |
| One thousand and five | 市场价值(Market Value) |
| One thousand nine hundred | 美国大学生数学建模竞赛 |
| Two thousand | abbr. 毫米（millimeter） |
| Three thousand | 嗯 |
| Three thousand nine hundred and ninety-nine | MMMCMXCIX |

## 如何将数字转换成罗马数字

因为罗马数字通常是从最大到最小排序的，所以将您要转换的数字分成千位、百位、十位和一位的组，并对每组执行转换。

例如，如果您想将数字 2，freeCodeCamp 成立的年份)转换成罗马数字，请将数字分解如下:

```
2,014 = 2,000 + 10 + 4
```

然后对每组进行转换，并将它们组合起来，得到罗马数字等价物:

```
* 2,000 = MM
* 10 = X
* 4 = IV

2,014 = 2,000 + 10 + 4 = MMXIV 
```

## 如何用罗马数字表示大数

你可能已经注意到，上面的图表只是从 1 到 3，999。

这是由于上面提到的表示 4 和 9 的特殊方法。如果你查看上面的表格，你会发现每当出现 4 或 9(包括 40，90，400，900)时，罗马数字都以特定的方式排序，因此较小的符号会从后面较大的符号中减去。

由于罗马数字从来没有完全标准化，你可能会看到数字 4000 代表 MMMM。

这是可行的，但是许多人认为这是无效的，因为 4(和 9)在较低的数字中有特殊的表示。

相反，表示较大罗马数字的一种最常见的方式是用一个，或者一个或多个符号上方的一条水平直线。

如果你看到一个罗马数字符号上面有一条水平线，那只是意味着将这个符号乘以 1000。

以下是应用了纽结的罗马数字符号:

| One thousand | Five thousand | Ten thousand | Fifty thousand | One hundred thousand | Five hundred thousand | One million |
| --- | --- | --- | --- | --- | --- | --- |
| m 或 I | V | X | L | C | D | M |

使用这一组扩展的罗马数字符号，4000 将表示如下:

IV

这里有一个较大数字及其罗马数字表示的表格，可以帮助您开始:

| 数字 | 罗马数字 |
| --- | --- |
| Four thousand | IV |
| Four thousand and one | 四 I |
| Four thousand and two | 四二 |
| Four thousand and three | IV III |
| Five thousand | V |
| Six thousand | 我们 |
| Seven thousand | VII |
| Eight thousand | VIII |
| Nine thousand | IX |
| Ten thousand | X |
| Forty thousand | XL |
| Ninety thousand | XC |
| Four hundred thousand | CD |
| Nine hundred thousand | 厘米 |
| One million | M |

## 如何用 HTML 和 CSS 在罗马数字上添加横线

对于开发人员来说，在线添加罗马数字的最简单方法是将符号包装在一个元素中，并使用一点 CSS。

例如，要在 IVIII 中的符号 IV 上添加一条水平线，可以将它们包装在一个`span`元素中，并将其`text-decoration`属性设置为`overline`:

```
<p><span style="text-decoration: overline;">IV</span>III</p>
```

这将呈现以下内容:

IV III

## **感谢阅读**

如果你发现这个罗马数字的分类很有帮助，请与你的朋友分享，这样更多的人可以从中受益。

此外，请随时在 [Twitter](https://twitter.com/kriskoishigawa) 上联系我，告诉我你的想法。