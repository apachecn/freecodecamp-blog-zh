# Python 中的动态类定义

> 原文：<https://www.freecodecamp.org/news/dynamic-class-definition-in-python-3e6f7d20a381/>

这里有一个很好的 Python 技巧，也许有一天你会发现它很有用。让我们看看如何动态定义类，并根据需要创建它们的实例。

这个技巧利用了 Python 的面向对象编程(OOP)能力，所以我们先来回顾一下。

### 类别和对象

Python 是一种面向对象的语言，这意味着它允许你用面向对象的范例来编写代码。

这种编程范式中的关键概念是类。在 Python 中，这些用于创建可以拥有属性的对象。

对象是一个类的特定实例。一个类本质上是一个对象是什么以及它应该如何表现的蓝图。

类由两种类型的属性定义:

*   [数据属性](https://docs.python.org/3/tutorial/classes.html#instance-objects) —该类的给定实例可用的变量
*   [方法](https://docs.python.org/3/tutorial/classes.html#method-objects) —可用于该类实例的函数

经典的 OOP 例子通常涉及不同类型的动物或食物。在这里，我用一个简单的数据可视化主题进行了更实际的操作。

首先，定义类`BarChart`。

```
class BarChart:
	def __init__(self, title, data):
    	self.title = title
    	self.data = data
   	def plot(self):
    	print("\n"+self.title)
        for k in self.data.keys():
        	print("-"*self.data[k]+" "+k)
```

`__init__`方法允许您在实例化时设置属性。也就是说，当您创建一个新的`BarChart`实例时，您可以传递提供图表标题和数据的参数。

这个类也有一个`plot()`方法。当控制台被调用时，它会将一个非常基本的条形图打印到控制台。在实际应用中，它可以做更多有趣的事情。

接下来，实例化一个`BarChart`的实例:

```
data = {"a":4, "b":7, "c":8}bar = BarChart("A Simple Chart", data)
```

现在您可以在剩下的代码中使用`bar`对象:

```
bar.data['d'] = bar.plot()
```

```
A Simple Chart
---- a
------- b
-------- c
----- d
```

这很棒，因为它允许您定义一个类并动态地创建实例。您可以在一行代码中旋转其他条形图的实例。

```
new_data = {"x":1, "y":2, "z":3}
bar2 = BarChart("Another Chart", new_data)
bar2.plot()
```

```
Another Chart
- x
-- y
--- z
```

假设您想要定义几类图表。[继承](https://docs.python.org/3.7/tutorial/classes.html#inheritance)让你定义从基类“继承”属性的类。

例如，您可以定义一个基类`Chart`。然后可以定义从基类继承的派生类。

```
class Chart:
	def __init__(self, title, data):
    	self.title = title
        self.data = data
    def plot(self):
    	pass
```

```
class BarChart(Chart):
	def plot(self):
    	print("\n"+self.title)
        for k in self.data.keys():
        	print("-"*self.data[k]+" "+k)
```

```
class Scatter(Chart):
	def plot(self):
    	points = zip(data['x'],data['y'])
        y = max(self.data['y'])+1
        x = max(self.data['x'])+1
        print("\n"+self.title)
        for i in range(y,-1,-1):
        	line = str(i)+"|"
            for j in range(x):
            	if (j,i) in points:
                	line += "X"
                else:
                	line += " "
            print(line)
```

在这里，`Chart`类是一个基类。`BarChart`和`Scatter`类从`Chart.`继承了`__init__()`方法，但是它们有自己的`plot()`方法，覆盖了`Chart` *中定义的方法。*

现在您也可以创建散点图对象。

```
data = {'x':[1,2,4,5], 'y':[1,2,3,4]}
scatter = Scatter('Scatter Chart', data)
scatter.plot()
```

```
Scatter Chart
4|     X
3|	  X 
2|  X
1| X
0|
```

这种方法允许您编写更抽象的代码，为您的应用程序提供更大的灵活性。拥有蓝图来创建同一通用对象的无数变体将节省您不必要的重复代码行。它还可以使您的应用程序代码更容易理解。

如果您想在以后重用它们，您也可以将类导入到将来的项目中。

### 工厂方法

有时候，在运行之前你不会知道你想要实现的具体类。例如，您创建的对象可能依赖于用户输入，或者具有可变结果的另一个过程的结果。

[工厂方法](https://en.wikipedia.org/wiki/Factory_method_pattern)提供解决方案。这些方法采用参数的动态列表并返回一个对象。提供的参数确定返回的对象的类。

下面是一个简单的例子。该工厂可以返回条形图或散点图对象，这取决于它接收的`style`参数。一个更聪明的工厂方法甚至可以通过查看`data`参数的结构来猜测最好的类。

```
def chart_factory(title, data, style):
	if style == "bar":
    	return BarChart(title, data)
    if style == "scatter":
    	return Scatter(title, data)
    else:
    	raise Exception("Unrecognized chart style.") 
```

```
chart = chart_factory("New Chart", data, "bar")
chart.plot()
```

当您预先知道要返回哪些类，以及返回这些类的条件时，工厂方法非常有用。

但是如果你事先连这个都不知道呢？

### 动态定义

Python 允许动态定义类，并根据需要用它们实例化对象。

你为什么想这么做？简短的回答是更加抽象。

诚然，需要在这个抽象层次上编写代码的情况通常很少发生。一如往常，在编程时，您应该考虑是否有更简单的解决方案。

然而，有时候动态定义类确实很有用。我们将在下面讨论一个可能的用例。

你可能熟悉 Python 的`type()`函数。对于一个参数，它只是返回参数对象的“类型”。

```
type(1) # <type 'int'>
type('hello') # <type 'str'>
type(True) # <type 'bool'>
```

但是，通过三个参数，`type()`返回一个全新的[类型对象](https://docs.python.org/3/library/stdtypes.html#bltin-type-objects)。这[相当于定义了一个新的类](https://docs.python.org/3/library/functions.html#type)。

```
NewClass = type('NewClass', (object,), {})
```

*   第一个参数是为新类命名的字符串
*   下一个是 tuple，它包含新类应该继承的所有基类
*   最后一个参数是特定于该类的属性字典

什么时候你可能需要使用像这样抽象的东西？考虑下面的例子。

[Flask Table](https://flask-table.readthedocs.io/en/stable/#flask-table) 是一个为 HTML 表格生成语法的 Python 库。它可以通过 pip 软件包管理器安装。

您可以使用 Flask Table 为您想要生成的每个表定义类。您定义了一个继承自基类`Table`的类。它的属性是列对象，列对象是`Col`类的实例。

```
from flask_table import Table, Col
class MonthlyDownloads(Table):
	month = Col('Month')
    downloads = Col('Downloads')

data = [{'month':'Jun', 'downloads':700},
		{'month':'Jul', 'downloads':900},
        {'month':'Aug', 'downloads':1600},
        {'month':'Sep', 'downloads':1900},
        {'month':'Oct', 'downloads':2200}]

table = MonthlyDownloads(data)print(table.__html__())
```

然后，创建该类的一个实例，传入要显示的数据。`__html__()`方法生成所需的 HTML。

现在，假设您正在开发一个工具，它使用 Flask Table 根据用户提供的配置文件生成 HTML 表。您事先不知道用户想要定义多少列——可能是一列，也可能是一百列！您的代码如何为工作定义正确的类？

动态类定义在这里很有用。对于您希望定义的每个类，您可以动态构建`attributes`字典。

假设您的用户配置是一个 CSV 文件，其结构如下:

```
Table1, column1, column2, column3
Table2, column1
Table3, column1, column2
```

您可以使用每行的第一个元素作为每个表类的名称，逐行读取 CSV 文件。该行中的其余元素将用于定义该表类的`Col`对象。这些被添加到一个迭代建立的`attributes`字典中。

```
for row in csv_file:
	attributes = {}
    for column in row[1:]:
    	attributes[column] = Col(column)
        globals()[row[0]] = type(row[0], (Table,), attributes)
```

上面的代码为 CSV 配置文件中的每个表定义了类。每个类都被添加到`globals`字典中。

当然，这是一个比较琐碎的例子。FlaskTable 能够生成更加复杂的表格。现实生活中的用例会更好地利用这一点！但是，希望您已经看到了动态类定义在某些情况下是多么有用。

### 所以现在你知道了…

如果您是 Python 的新手，那么尽早熟悉类和对象是值得的。尝试在你的下一个学习项目中实现它们。或者，[浏览 Github 上的开源项目](https://github.com/trending/python)看看其他开发者是怎么利用的。

对于那些经验稍多一点的人来说，了解事情在“幕后”是如何运作的会非常有益。浏览官方文件会很有启发性！

你在 Python 中找到过动态类定义的用例吗？如果是这样的话，在下面的回复中分享它会很棒。