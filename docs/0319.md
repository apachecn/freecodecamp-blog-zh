# Python 中按值排序字典——如何对字典排序

> 原文：<https://www.freecodecamp.org/news/sort-dictionary-by-value-in-python/>

在 Python 中，默认情况下，字典是一个无序的胖结构。因此，有时，您会希望按关键字或值对字典进行排序，以便于查询。

问题是按值对字典进行排序从来都不是一件简单的事情。这是因为 Python 没有内置的方法来做这件事。

然而，我想出了一种按值对字典进行排序的方法，这就是我将在本文中向您展示的方法。

## 我们将涵盖的内容

*   [如何用`sorted()`方法对数据进行排序](#howtosortdatawiththesortedmethod)
*   [`sorted()`方法的工作原理](#howthesortedmethodworks)
    *   [`sorted()`方法的参数](#parametersofthesortedmethod)
*   [如何用`sorted()`方法对字典排序](#howtosortadictionarywiththesortedmethod)
    *   [如何将结果列表转换成字典](#howtoconverttheresultinglisttoadictionary)
    *   [如何按值升序或降序对字典进行排序](#howtosortthedictionarybyvalueinascendingordescendingorder)
*   [结论](#conclusion)

## 如何用`sorted()`方法对数据进行排序

`sorted()`方法对列表、元组和字典等可迭代数据进行排序。但它只按关键字排序。

方法将排序后的条目放在一个列表中。这是我们必须解决的另一个问题，因为我们希望排序后的字典仍然是字典。

例如，`sorted()`按字母顺序排列了下面的列表:

```
persons = ['Chris', 'Amber', 'David', 'El-dorado', 'Brad', 'Folake']
sortedPersons = sorted(persons)

print(sortedPersons)
# Output: ['Amber', 'Brad', 'Chris', 'David', 'El-dorado', 'Folake'] 
```

而`sorted()`方法对下面元组中的数字进行升序排序:

```
numbers = (14, 3, 1, 4, 2, 9, 8, 10, 13, 12)
sortedNumbers = sorted(numbers)

print(sortedNumbers)
# Output: [1, 2, 3, 4, 8, 9, 10, 12, 13, 14] 
```

如果您对字典使用`sorted()`方法，将只返回键，像往常一样，它将在一个列表中:

```
my_dict = { 'num6': 6, 'num3': 3, 'num2': 2, 'num4': 4, 'num1': 1, 'num5': 5}
sortedDict = sorted(my_dict)

print(sortedDict)
# ['num1', 'num2', 'num3', 'num4', 'num5', 'num6'] 
```

这不是你想要的行为。您希望字典按值排序，并且仍然是一个字典。这就是我接下来要给你们展示的。

## `sorted()`方法的工作原理

为了对字典进行排序，我们仍然要使用 sorted 函数，但是以更复杂的方式。别担心，我会解释你需要知道的一切。

既然我们还是要用`sorted()`方法，那么就该详细解释一下`sorted()`方法了。

### `sorted()`方法的参数

`sorted()`方法最多可以接受 3 个参数:

*   iterable–要迭代的数据。它可以是元组、列表或字典。

*   key–可选值，帮助您执行自定义排序操作的函数。

*   反向–另一个可选值。它帮助你按升序或降序排列已排序的数据

如果您猜对了，关键参数就是我们将传递给`sorted()`方法的内容，以获得按值排序的字典。

现在，是时候按照值对我们的字典进行排序，并确保它仍然是一个字典。

## 如何用`sorted()`方法对字典进行排序

要使用`sorted()`方法按值对字典进行正确排序，您必须执行以下操作:

*   将字典作为第一个值传递给`sorted()`方法
*   在字典上使用`items()`方法来检索它的键和值
*   编写一个 lambda 函数来获取用`item()`方法检索的值

这里有一个例子:

```
footballers_goals = {'Eusebio': 120, 'Cruyff': 104, 'Pele': 150, 'Ronaldo': 132, 'Messi': 125}

sorted_footballers_by_goals = sorted(footballers_goals.items(), key=lambda x:x[1])
print(sorted_footballers_by_goals) 
```

正如我前面所说的，我们必须获得字典中的这些值，这样我们就可以按值对字典进行排序。这就是为什么你能在 lambda 函数中看到 1。

1 表示值的索引。密钥是 0。记住程序员是从 0 开始计数，而不是从 1 开始计数。

使用上面代码，我得到了下面的结果:

```
# [('Cruyff', 104), ('Eusebio', 120), ('Messi', 125), ('Ronaldo', 132), ('Pele', 150)] 
```

下面是完整的代码，这样你就不会感到困惑了:

```
footballers_goals = {'Eusebio': 120, 'Cruyff': 104, 'Pele': 150, 'Ronaldo': 132, 'Messi': 125}

sorted_footballers_by_goals = sorted(footballers_goals.items(), key=lambda x:x[1])
print(sorted_footballers_by_goals)

# [('Cruyff', 104), ('Eusebio', 120), ('Messi', 125), ('Ronaldo', 132), ('Pele', 150)] 
```

您可以看到字典已经按照值的升序进行了排序。也可以降序排序。但是我们稍后再看，因为我们对得到的结果还有一个问题。

问题是字典不再是字典了。各个键和值被放在一个元组中，并进一步压缩成一个列表。记住，无论你从`sorted()`方法中得到什么结果，都放在一个列表中。

我们已经能够按价值对字典中的条目进行排序。剩下的就是把它转换回字典。

### 如何将结果列表转换为词典

要将结果列表转换为字典，您不需要编写另一个复杂的函数或循环。您只需要将保存结果列表的变量传递给`dict()`方法。

```
converted_dict = dict(sorted_footballers_by_goals)
print(converted_dict)
# Output: {'Cruyff': 104, 'Eusebio': 120, 'Messi': 125, 'Ronaldo': 132, 'Pele': 150} 
```

记住，我们将排序后的字典保存在名为`sorted_footballers_by_goals`的变量中，所以它是我们必须传递给`dict()`的变量。

完整的代码如下所示:

```
footballers_goals = {'Eusebio': 120, 'Cruyff': 104, 'Pele': 150, 'Ronaldo': 132, 'Messi': 125}

sorted_footballers_by_goals = sorted(footballers_goals.items(), key=lambda x:x[1])
converted_dict = dict(sorted_footballers_by_goals)

print(converted_dict)
# Output: {'Cruyff': 104, 'Eusebio': 120, 'Messi': 125, 'Ronaldo': 132, 'Pele': 150} 
```

就是这样！我们已经能够对字典中的条目进行排序，并将它们转换回字典。我们刚刚吃了蛋糕，而且还吃掉了！

### 如何按值对字典进行升序或降序排序

记住，`sorted()`方法接受第三个名为`reverse`的值。

值为`True`的`reverse`将按降序排列已排序的字典。

```
footballers_goals = {'Eusebio': 120, 'Cruyff': 104, 'Pele': 150, 'Ronaldo': 132, 'Messi': 125}

sorted_footballers_by_goals = sorted(footballers_goals.items(), key=lambda x:x[1], reverse=True)
converted_dict = dict(sorted_footballers_by_goals)

print(converted_dict)
# Output: {'Pele': 150, 'Ronaldo': 132, 'Messi': 125, 'Eusebio': 120, 'Cruyff': 104} 
```

您可以看到输出是反向的，因为我们将`reverse=True`传递给了`sorted()`方法。

如果您根本没有设置`reverse`或者您将它的值设置为 false，字典将按升序排列。这是默认的。

## 结论

恭喜你。尽管 Python 中没有内置的方法或函数，但现在可以按值对字典进行排序。

然而，当我准备写这篇文章时，有一件事引起了我的好奇心。记住，我们能够在字典上直接使用`sorted()`。结果得到了一个列表，尽管我们只得到了键而没有得到值。

如果我们用`dict()`方法将这个列表转换成一个字典呢？你认为我们能得到想要的结果吗？让我们看看:

```
my_dict = { 'num6': 6, 'num3': 3, 'num2': 2, 'num4': 4, 'num1': 1, 'num5': 5}
sortedDict = sorted(my_dict)

converted_dict = dict(sortedDict)
print(converted_dict)
"""
Output: 
dict_by_value.py
Traceback (most recent call last):
  File "sort_dict_by_value.py", line 17, in <module>
    converted_dict = dict(sortedDict)
ValueError: dictionary update sequence element #0 has length 4; 2 is required
""" 
```

我们出错了！这是因为如果你想从一个列表中创建一个字典，你必须使用字典理解。如果您对这种类型的数据使用字典理解，您必须为所有条目指定一个值。这违背了字典按值排序的目的，所以这不是我们想要的。

如果你想了解更多关于词典理解的知识，你应该[阅读这篇文章](https://www.freecodecamp.org/news/dictionary-comprehension-in-python-explained-with-examples/)。

感谢您的阅读！