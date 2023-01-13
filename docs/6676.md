# 创建 JavaScript 数组的技巧

> 原文：<https://www.freecodecamp.org/news/https-medium-com-gladchinda-hacks-for-creating-javascript-arrays-a1b80cb372b/>

由高兴钦达

# 创建 JavaScript 数组的技巧

#### 用 JavaScript 创建和克隆数组的有见地的技巧。

![IH842JnVoctZM6QoKOfXQe8luuYHXHjMuNxf](img/343beac26e5e588b8da9ccebbde6c1ce.png)

Original Photo by [Markus Spiske](https://unsplash.com/photos/FXFz-sW0uwo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

每种编程语言的一个非常重要的方面是语言中可用的数据类型和结构。大多数编程语言都提供了表示和处理复杂数据的数据类型。如果你使用过 Python 或 Ruby 之类的语言，你应该见过类似于**列表**、**集合**、**元组**、**散列**、**字典**等数据类型。

在 JavaScript 中，没有那么多复杂的数据类型——你只需要有**数组**和**对象**。然而，在 ES6 中，语言中增加了一些数据类型和结构，比如**符号**、**集合**和**映射**。

> JavaScript 中的*数组是高级的类似列表的对象，以*的长度属性和*的整数属性作为索引。*

在本文中，我分享了一些创建新的 JavaScript 数组或克隆现有数组的技巧。

### 创建数组:数组构造函数

创建数组最流行的方法是使用**数组文字**语法，这非常简单。但是，当您想要动态创建数组时，数组文字语法可能并不总是最好的方法。另一种方法是使用`Array`构造函数。

下面是一个简单的代码片段，展示了`Array`构造函数的用法。