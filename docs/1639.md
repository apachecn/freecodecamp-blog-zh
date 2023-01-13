# HTML 表格–表格教程及示例代码

> 原文：<https://www.freecodecamp.org/news/html-tables-table-tutorial-with-css-example-code/>

当您构建一个需要可视化表示数据的项目时，您需要一种好的方式来显示信息，以便于阅读和理解。

现在，根据数据的类型，您可以使用 HTML 元素在不同的表示方法之间进行选择。

在大多数情况下，表格更便于很好地显示大量结构化数据。这就是为什么，在这篇文章中，我们将学习如何在 HTML 中使用表格，然后如何设置它们的样式。

但是，首先，什么是 HTML 中的表格？

## HTML 中的表格是什么？

表格是按行和列排列的数据的表示。真的，更像是电子表格。在 HTML 中，借助于表格，你可以将图像、文本、链接等数据排列成单元格的行和列。

最近，在 web 中使用表格变得越来越流行，因为惊人的 HTML 表格标签使得创建和设计表格变得更加容易。

要用 HTML 创建一个表格，你需要使用标签。最重要的一个是`<table>`标签，它是表格的主要容器。它显示了表格的开始位置和结束位置。

### 常见的 HTML 表格标签

其他标签包括:

*   `tr>` -代表行
*   `<td>` -用于创建数据单元格
*   `<th>` -用于添加表格标题
*   `<caption>` -用于插入标题
*   `<thead>` -给表格添加一个单独的标题
*   `<tbody>` -显示表格的主体
*   `<tfoot>` -为表格创建一个单独的页脚

## HTML 表格语法:

```
<table>
  <tr>
    <td>Cell 1</td>
    <td>Cell 2</td>
    <td>Cell 3</td>
  </tr>
  <tr>
    <td>Cell 4</td>
    <td>Cell 5</td>
    <td>Cell 6</td>
  </tr>
</table> 
```

| 单元格 1 | 细胞 2 | 3 号牢房 |
| 4 号牢房 | 5 号牢房 | 6 号牢房 |

既然您已经了解了 HTML 表格是什么，以及如何创建它，那么让我们来看看如何利用这些标签来创建具有更多功能的表格。

## 如何在 HTML 中添加表格标题

`<th>`用于给表格添加标题。在基本设计中，表格标题总是占据第一行，这意味着我们将在第一个表格行中声明`<th>`,然后是表格中的实际数据。默认情况下，标题中传递的文本居中并加粗。

**用`<th>`举例**

```
<table>
  <tr>
    <th>First Name</th>
    <th>Last Name</th>
    <th>Email Address</th>
  </tr>
  <tr>
   <td>Hillary</td>
   <td>Nyakundi</td>
   <td>tables@mail.com</td>
  </tr>
  <tr>
    <td>Lary</td>
    <td>Mak</td>
    <td>developer@mail.com</td>
  </tr>
</table> 
```

| 西方人名的第一个字 | 姓 | 电子邮件地址 |
| 希拉里 | Nyakundi | tables@mail.com |
| 拉里 | 马克 | developer@mail.com |

从上面的例子中，我们能够知道哪一列包含哪一信息。这可以通过使用`<th>`标签来实现。

## 如何给表格添加标题

向表中添加标题的主要用途是提供关于表中表示的数据的描述。标题可以放在表格的顶部或底部，默认情况下它总是居中。

要在表格中插入标题，请使用`<caption>`标签。

### 标题语法

```
<table>
  <caption></caption>
  <tr> </tr>
</table> 
```

**用`<caption>`举例**

```
<table>
  <caption>Free Coding Resources</caption>
  <tr>
    <th>Sites</th>
    <th>Youtube Channels</th>
    <th>Mobile Appss</th>
  </tr>
  <tr>
    <td>Freecode Camp</td>
    <td>Freecode Camp</td>
    <td>Enki</td>
  </tr>
  <tr>
    <td>W3Schools</td>
    <td>Academind</td>
    <td>Programming Hero</td>
  </tr>
  <tr>
    <td>Khan Academy</td>
    <td>The Coding Train</td>
    <td>Solo learn</td>
  </tr>
</table> 
```

Free Coding Resources

| 位置 | Youtube 频道 | 移动应用 |
| 自由代码营地 | 自由代码营地 | 恩奇 |
| w3 学校 | 学院 | 编程英雄 |
| 可汗学院 | 编码列车 | 单独学习 |

## 如何在 HTML 表格中使用 Scope 属性

scope 属性用于定义特定的标题是用于列、行还是两者的组合。我知道这个定义可能很难理解，但是坚持住——在一个例子的帮助下，你会更好地理解它。

scope 元素的主要目的是显示目标数据，以便用户不必依赖假设。该属性在标题单元格`<th>`中声明，取值为`col`、`row`、`colgroup`和`rowgroup`。

值`col`和`row`表示标题单元格分别为行或列提供信息。

### 范围语法

```
<table>
 <tr>
   <th scope="value">
 </tr>
</table 
```

**用`<scope>`举例**

```
<table>
  <tr>
    <th></th>
    <th scope="col">Semester</th>
    <th scope="col">Grade</th>
  </tr>

  <tr>
    <td>1</td>
    <td>Jan - April</td>
    <td>Credit</td>
  </tr>

  <tr>
    <td>2</td>
    <td>May - August</td>
    <td>Pass</td>
  </tr>

  <tr>
    <td>2</td>
    <td>September - December</td>
    <td>Distinction</td>
  </tr>
</table> 
```

|  | 学期 | 级别 |
| one | 简-爱普莉尔 | 信用 |
| Two | 五月至八月 | 及格 |
| Two | 9 月至 12 月 | 区别 |

属性所做的是，它显示一个标题单元格是属于列、行，还是属于两者的组合。

在这种情况下，标题属于列，因为它是我们在代码中设置的。您可以通过更改属性来查看不同的行为。

## 如何在 HTML 表格中使用单元格跨越

也许您遇到过跨越表格中两列或多行的单元格。这叫做细胞跨越。

如果你在 MS office 或 Excel 之类的程序中工作，你可能已经通过高亮显示单元格并点击命令来使用该功能，瞧！你拿到了。

通过使用两个单元格属性，可以在 HTML 表格中应用相同的特性，`colspan`用于水平跨度，`rowspan`用于垂直跨度。这两个属性被赋予大于零的数字，这表示您希望跨越的单元格的数量。

**用`span`举例**

```
<table>
  <tr>
    <th>Name</th>
    <th>Subject</th>
    <th>Marks</th>
  </tr>
  <tr>
    <td rowspan = "2">Hillary</td>
    <td>Advanced Web</td>
    <td>75</td>
  </tr>
  <tr>
    <td>Operating Syatem</td>
    <td>60</td>
  </tr>
      <tr>
    <td rowspan = "2">Lary</td>
    <td>Advanced Web</td>
    <td>80</td>
  </tr>
  <tr>
    <td>Operating Syatem</td>
    <td>75</td>
  </tr>
  <tr>
     <td></td>
    <td colspan="3">Total Average: 72.5</td>
  </tr>
</table> 
```

| 名字 | 科目 | 马克斯（英格兰人姓氏） |
| 希拉里 | 高级网站 | Seventy-five |
| 操作系统 | Sixty |
| 拉里 | 高级网站 | Eighty |
| 操作系统 | Seventy-five |
|  | 总平均分:72.5 |

在上面的示例中，我们有一个跨越 2 个行单元格和 3 个列单元格的单元格，如图所示。我们设法在垂直和水平两个方向上应用跨度。

> *使用属性`colspan`和`rowspan`时，确保声明正确赋值，以避免单元格重叠。*

## 如何在 HTML 表格中添加表头、表体和表尾

就像网站或任何其他文档有三个主要部分——页眉、正文和页脚——表格也是如此。

在表中，它们通过使用属性来划分，即:

*   `<thead>` -为表格提供单独的标题
*   `<tbody>` -包含表格的主要内容
*   `<tfoot>` -为表格创建一个单独的页脚

**用`<thead>`、`<tbody>`、&、`<tfoot>`、**举例说明

```
<table>
  <thead>
    <tr>
      <th colspan="2">October</th>
      <th colspan="2">November</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>Sales</td>
      <td>Profit</td>
      <td>Sales</td>
      <td>Profit</td>
    </tr>
    <tr>
      <td>$200,00</td>
      <td>$50,00</td>
      <td>$300,000</td>
      <td>$70,000</td>
    </tr>
  </tbody>

  <tfoot>
    <tr>
      <th colspan= "4">November was more produstive</th>
    </tr>
  </tfoot>
</table> 
```

| 十月 | 十一月 |
| --- | --- |
| 销售 | 利润 | 销售 | 利润 |
| $200,00 | $50,00 | $300,000 | $70,000 |
| 11 月更有成效 |

在上面的例子中，标题由月份的名称表示，包含销售和利润数字的部分表示表体，最后包含语句的部分表示表的页脚。

另一个需要注意的重要事情是，一个表可以有多个正文部分。在这样的场景中，每个主体将相关的行组合在一起。

## 如何使用 CSS 设计 HTML 表格的样式

即使表格在今天被广泛使用，也很难找到没有样式的表格。他们大多使用不同形式的风格，无论是颜色，字体，突出重要的价值等等。

造型有助于使桌子看起来更专业，更吸引人。毕竟，你不希望你的读者盯着仅仅被一条线分开的数据。

与其他语言或工具的样式不同，在 HTML 中，你需要创建一个带有`.css`扩展名的额外文件，在这里你可以添加你的样式并链接到你的 HTML 文件。

下面，附上一个代码操场与一个样式表的例子。你可以随意摆弄它，看看不同的风格会对展示产生怎样的影响。

[https://codepen.io/larymak/embed/preview/BaZQGYj?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=BaZQGYj](https://codepen.io/larymak/embed/preview/BaZQGYj?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=BaZQGYj)

在上面的代码操场中，我们创建了一个表格，并使用本文中介绍的一些属性对其进行了样式化。

我们使用一个 CSS 文件来设计它的样式，我们在表格中添加了颜色和边框，使其更具可读性和美观性。该表还有一个固定的标题，因此您可以滚动浏览大量数据，但仍然可以看到特定列的标题。

所以，我们已经看到了什么是表格，我们已经创建了一些表格，甚至更进一步，应用了样式。

但是拥有知识却不知道如何应用它是没有任何帮助的。也就是说，在您的设计中，何时何地需要使用表格？

## 何时使用桌子

在许多情况下，开发项目时表格可能会派上用场:

*   当您想要比较和对比具有共同特征的数据时，例如 A 和 B 之间的差异或 X 队与 y 队的得分，您可以使用表格。
*   如果你想给出一个数字数据的概述，你也可以用一个。一个很好的例子就是当你试图表示学生的分数，甚至是团队的分数，比如 EPL 表。
*   表格可以帮助读者快速找到明确列出的具体信息。例如，如果你要浏览一个很长的名字列表，可以用一个表格来细分这个列表，这样对读者来说很容易。

## 包裹

表格是表示表格数据的一种很好的方式，您可以使用基本的 HTML 元素来创建它们，比如`<table>`、`<tr>`、`<td>`。

您还可以添加一些样式，使它们看起来更好，并在 CSS 文件的帮助下清晰地呈现数据。

在我们结束之前，让我们再做一项任务:

使用我们所学的知识创建一个表格，总结我们今天在文章中所涉及的内容。之后，将你的设计与我下面的代码进行比较:

[https://codepen.io/larymak/embed/preview/PojbMGW?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=PojbMGW](https://codepen.io/larymak/embed/preview/PojbMGW?default-tabs=html%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=PojbMGW)