# Python 列表排序——如何在 Python 中对列表进行排序

> 原文：<https://www.freecodecamp.org/news/python-list-sorting-how-to-order-lists-in-python/>

在 Python 应用程序中对列表进行排序有很多原因。

在本文中，我将向您展示如何根据您需要做的事情对列表进行升序和降序排序。

## Python 中的 a `List`是什么？

列表是 Python 中的一种数据类型，其中可以存储不同数据类型的多个值(包括嵌套列表)。

以下是列表示例:

```
numList = [1, 2, 3, 4, 5]

stringList = ["banana", "orange", "apple"]

mixedList = [1, "banana", "orange", [5, 6]] 
```

您可以使用项目的索引位置来访问列表中的项目。列表中的索引位置从 0 开始:

```
stringList = ["banana", "orange", "apple"]

print(stringList[1])
# "orange" 
```

## 如何在 Python 中对列表进行排序

您可以使用`sort()`方法在 Python 中对列表进行排序。

方法允许你对列表中的条目进行排序。下面是语法:

```
list.sort(reverse=True|False, key=sortFunction) 
```

该方法接受两个可选参数:

*   `reverse`:如果是`True`则按逆序(降序)排列，如果是`False`(默认情况下)则按正序(升序)排列
*   `key`:您提供的描述排序方式的功能

默认情况下，您可以按升序对字符串和数字进行排序，而无需向此方法传递参数:

```
items = ["orange", "cashew", "banana"]

items.sort()
# ['banana', 'cashew', 'orange'] 
```

在上面的例子中，你可以看到排序后的列表首先是 **b** (在香蕉中)，然后是 **c** (在腰果中)，因为它在 b 之后，最后是 **o** (在橙色中)，它在字母顺序中靠后。

请注意，该方法修改了原始数组。

对于降序，可以传递相反的参数:

```
items = [6, 8, 10, 5, 7, 2]

items.sort(reverse=True)
# [10, 8, 7, 6, 5, 2] 
```

通过将`True`传递给`reverse`参数，您会看到`items`列表中的数字是反向排序的，即降序。

### 如何指定排序函数

如果你在字典列表上尝试这个方法会怎么样？让我们看看:

```
items = [{
    'name': 'John',
    'age': 40
}, {
    'name': 'Mike',
    'age': 45
}, {
    'name': 'Jane',
    'age': 33
}, {
    'name': 'Asa',
    'age': 42
}]

items.sort() 
```

你会得到一个错误，因为字典是不可订购的。在这里，您可以使用`key`参数指定排序标准:

```
items = [
  {
  'name': 'John',
  'age': 40
  },
  {   
    'name': 'Mike',
    'age': 45
  },
  {   
    'name': 'Jane',
    'age': 33
  },
  {   
    'name': 'Asa',
    'age': 42
  }
]

def sortFn(dict):
  return dict['age']

items.sort(key=sortFn)
# [
#   {'name': 'Jane', 'age': 33},
#   {'name': 'John', 'age': 40},
#   {'name': 'Asa', 'age': 42},
#   {'name': 'Mike', 'age': 45}
# ] 
```

正如您将在上面的代码块中注意到的，使用排序函数，我们已经指定排序决定应该基于每个字典中的`age`键。

如果这里将`reverse`参数作为`True`传递，那么排序后的字典将按降序排列。

下面是另一个使用排序函数的例子:

```
items = ["cow", "elephant", "duck"]

def sortFn(value):
    return len(value)

items.sort(key=sortFn, reverse=True)
# ['elephant', 'duck', 'cow'] 
```

在这种情况下，sort 函数返回列表中值的长度，作为排序过程的标准。通过传递`True`的`reverse`，可以看到排序后的列表首先是较长的字符串，其次是较短的字符串。

## 包扎

在构建应用程序时，有许多排序列表的场景。它可能是基于一个`last_opened`键对文件列表进行排序。它可以根据一个`price`键对产品进行分类。如您所见，在现实应用中有许多标准可供使用。

在本文中，我们看到了如何使用不同的方法对 Python 中的列表进行升序和降序排序。