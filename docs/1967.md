# Pandas 的数据分析——如何从 Pandas 数据框架中删除行列表

> 原文：<https://www.freecodecamp.org/news/drop-list-of-rows-from-pandas-dataframe/>

Pandas dataframe 是一个二维数据结构，它允许你以行和列的形式存储数据。当你在分析数据时，这非常有用。

当您在数据帧中有一个数据记录列表时，您可能需要删除特定的行列表，这取决于您的模型的需要以及您在研究分析时的目标。

在本教程中，您将学习如何从 Pandas 数据帧中删除行列表。

要了解如何删除列，你可以在这里阅读关于[如何在 Pandas](https://www.stackvidhya.com/drop-column-in-pandas/) 中删除列。

## 如何在熊猫数据框中拖放一行或一列

要在数据帧中删除一行或一列，需要使用数据帧中可用的`drop()`方法。你可以在这里的[文档中阅读更多关于`drop()`方法的内容。](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.drop.html)

**数据帧轴**

*   使用`axis=0`表示行
*   使用`axis=1`表示列

**数据帧标签**

*   默认情况下，使用从 0 开始的索引号来标记行。
*   使用名称来标记列。

**Drop()方法参数**

*   `index` -要删除的行的列表
*   `axis=0` -标记数据帧中要删除的行
*   `inplace=True` -在同一数据帧中执行删除操作，而不是在删除操作期间创建新的数据帧对象。

### 样本熊猫数据帧

我们的示例数据框架包含列 *product_name* 、 *Unit_Price* 、 *No_Of_Units* 、 *Available_Quantity* 和 *Available_Since_Date* 列。它还包含带有 NaN 值的行，这些值用于表示缺少的值。

```
import pandas as pd

data = {"product_name":["Keyboard","Mouse", "Monitor", "CPU","CPU", "Speakers",pd.NaT],
        "Unit_Price":[500,200, 5000.235, 10000.550, 10000.550, 250.50,None],
        "No_Of_Units":[5,5, 10, 20, 20, 8,pd.NaT],
        "Available_Quantity":[5,6,10,"Not Available","Not Available", pd.NaT,pd.NaT],
        "Available_Since_Date":['11/5/2021', '4/23/2021', '08/21/2021','09/18/2021','09/18/2021','01/05/2021',pd.NaT]
       }

df = pd.DataFrame(data)

df 
```

数据帧将如下所示:

|  | 产品名称 | 单价 | 单位数量 | 可用 _ 数量 | 可用 _ 自 _ 日期 |
| --- | --- | --- | --- | --- | --- |
| Zero | 键盘 | Five hundred | five | five | 11/5/2021 |
| one | 老鼠 | Two hundred | five | six | 4/23/2021 |
| Two | 班长 | Five thousand point two three five | Ten | Ten | 08/21/2021 |
| three | 中央处理器 | Ten thousand point five five | Twenty | 无法使用 | 09/18/2021 |
| four | 中央处理器 | Ten thousand point five five | Twenty | 无法使用 | 09/18/2021 |
| five | 扬声器 | Two hundred and fifty point five | eight | 精灵 | 01/05/2021 |
| six | 精灵 | 圆盘烤饼 | 精灵 | 精灵 | 精灵 |

就这样，我们创建了样本数据框架。

每次拖放操作后，您将使用`df`打印数据帧，它将以常规的`HTML`表格格式打印数据帧。

你可以在这里阅读如何[漂亮地打印数据帧](https://www.stackvidhya.com/pretty-print-dataframe/)以不同的视觉格式打印数据帧。

接下来，您将学习如何在不同的用例中删除行列表。

## 如何在 Pandas 中通过索引删除行列表

您可以通过将索引列表传递给`drop()`方法来删除 Pandas 中的行列表。

```
df.drop([5,6], axis=0, inplace=True)

df 
```

在这段代码中，

*   `[5,6]`是要删除的行的索引
*   `axis=0`表示应从数据帧中删除行
*   `inplace=True`在同一数据帧中执行删除操作

删除索引为 5 和 6 的行后，数据帧中会有以下数据:

|  | 产品名称 | 单价 | 单位数量 | 可用 _ 数量 | 可用 _ 自 _ 日期 |
| --- | --- | --- | --- | --- | --- |
| Zero | 键盘 | Five hundred | five | five | 11/5/2021 |
| one | 老鼠 | Two hundred | five | six | 4/23/2021 |
| Two | 班长 | Five thousand point two three five | Ten | Ten | 08/21/2021 |
| three | 中央处理器 | Ten thousand point five five | Twenty | 无法使用 | 09/18/2021 |
| four | 中央处理器 | Ten thousand point five five | Twenty | 无法使用 | 09/18/2021 |

这就是删除具有特定索引的行的方法。

接下来，您将学习删除一系列索引。

## 如何在 Pandas 中按索引范围删除行

您还可以删除特定范围内的行列表。

范围是一组具有下限和上限的值。

如果您希望创建一个包含特定数据范围的样本数据集，这可能会很有用。

您可以使用`df.index()`方法在数据帧中创建一系列行。然后您可以将这个范围传递给`drop()`方法来删除这些行，如下所示。

```
df.drop(df.index[2:4], inplace=True)

df 
```

下面是这段代码的作用:

*   `df.index[2:4]`生成 2 到 4 行的范围。范围的下限包括端值，范围的上限包括端值。这意味着第 2 行和第 3 行将被删除，而第 4 行将*而不是*被删除。
*   `inplace=True`在同一数据帧中执行删除操作

删除范围 2-4 内的行后，数据框中会出现以下数据:

|  | 产品名称 | 单价 | 单位数量 | 可用 _ 数量 | 可用 _ 自 _ 日期 |
| --- | --- | --- | --- | --- | --- |
| Zero | 键盘 | Five hundred | five | five | 11/5/2021 |
| one | 老鼠 | Two hundred | five | six | 4/23/2021 |
| four | 中央处理器 | Ten thousand point five five | Twenty | 无法使用 | 09/18/2021 |

这就是如何使用数据帧的范围来删除数据帧中的行列表。

## 如何在 Pandas 中删除索引后的所有行

您可以使用`iloc[]`删除特定索引之后的所有行。

您可以使用`iloc[]`通过其位置索引来选择行。您可以指定由`:`分隔的开始和结束位置。例如，您可以使用`2:3`从 2 到 3 中选择行。如果你想选择所有的行，你可以使用`iloc[]`中的`:`。

这在出于训练和测试目的想要分割数据集的情况下可能很有用。

使用下面的代码段选择从 0 到索引 2 的行。这导致删除索引 2 之后的行。

```
df = df.iloc[:2]

df 
```

在这段代码中，`:2`选择直到索引 2 的行。

这是删除特定索引后的所有行的方法。

删除索引 2 之后的行后，数据帧中会有以下数据:

|  | 产品名称 | 单价 | 单位数量 | 可用 _ 数量 | 可用 _ 自 _ 日期 |
| --- | --- | --- | --- | --- | --- |
| Zero | 键盘 | Five hundred | five | five | 11/5/2021 |
| one | 老鼠 | Two hundred | five | six | 4/23/2021 |

这就是在特定索引后删除行的方法。

接下来，您将学习如何删除有条件的行。

## 如何在 Pandas 中删除具有多个条件的行

您可以根据特定条件删除数据帧中的行。

例如，您可以删除列值大于 *X* 且小于 *Y* 的行。

当您希望创建一个忽略具有特定值的列的数据集时，这可能很有用。

要根据特定条件删除行，选择通过特定条件的行的索引，并将该索引传递给`drop()`方法。

```
df.drop(df[(df['Unit_Price'] >400) & (df['Unit_Price'] < 600)].index, inplace=True)

df 
```

在这段代码中，

*   `(df['Unit_Price'] >400) & (df['Unit_Price'] < 600)`是删除行的条件。
*   `df[].index`选择通过条件的行的索引。
*   `inplace=True`在同一个数据帧中执行删除操作，而不是创建一个新的数据帧。

删除条件为`unit_price`大于 400 且小于 600 的行后，您将在数据帧中看到以下数据:

|  | 产品名称 | 单价 | 单位数量 | 可用 _ 数量 | 可用 _ 自 _ 日期 |
| --- | --- | --- | --- | --- | --- |
| one | 老鼠 | Two hundred | five | six | 4/23/2021 |

这就是使用某些条件在数据帧中删除行的方法。

## 结论

总而言之，在本文中，你已经了解了熊猫数据帧中的`drop()`方法。您还看到了数据帧的行和列是如何标记的。最后，您已经学会了如何使用索引、索引范围以及基于条件来删除行。

如果你喜欢这篇文章，请随意分享。

### 你可能也喜欢

*   [如何在 Pandas 中向数据帧添加列](https://www.stackvidhya.com/add-column-to-dataframe/)
*   [如何重命名熊猫中的一列](https://www.stackvidhya.com/rename-column-in-pandas/)