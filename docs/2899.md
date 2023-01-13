# Python 唯一列表——如何获得列表或数组中的所有唯一值

> 原文：<https://www.freecodecamp.org/news/python-unique-list-how-to-get-all-the-unique-values-in-a-list-or-array/>

假设您有一个包含重复数字的列表:

```
numbers = [1, 1, 2, 3, 3, 4] 
```

但是你想要一个唯一的数字列表。

```
unique_numbers = [1, 2, 3, 4] 
```

在 Python 中有几种方法可以获得唯一值的列表。这篇文章将告诉你如何做。

# 选项 1–使用集合获取唯一元素

用一种 **`set`** 的方式去做这件事。集合是有用的，因为它包含独特的元素。

您可以使用集合来获取唯一的元素。然后，把集合变成列表。

让我们看看使用集合和列表的两种方法。第一种方法很冗长，但它有助于了解每一步发生了什么。

```
numbers = [1, 2, 2, 3, 3, 4, 5]

def get_unique_numbers(numbers):

    list_of_unique_numbers = []

    unique_numbers = set(numbers)

    for number in unique_numbers:
        list_of_unique_numbers.append(number)

    return list_of_unique_numbers

print(get_unique_numbers(numbers))
# result: [1, 2, 3, 4, 5] 
```

让我们仔细看看发生了什么。我给了一串数字， **`numbers`** 。我把这个列表传入函数， **`get_unique_numbers`** 。

在函数内部，我创建了一个空列表，它将最终保存所有唯一的数字。然后，我用一个 **`set`** 从 **`numbers`** 列表中得到唯一的数字。

```
unique_numbers = set(numbers) 
```

我有我需要的:唯一的号码。现在我需要把这些值放到一个列表中。为此，我使用 for 循环来遍历集合中的每个数字。

```
for number in unique_numbers:
       list_of_unique_numbers.append(number) 
```

在每次迭代中，我将当前的数字添加到列表中，`list_of_unique_numbers`。最后，我在程序结束时返回这个列表。

在 Python 中，有一种使用集合和列表获取唯一值的更简单的方法。这是我们接下来要解决的问题。

### 使用 Set 的较短方法

在 Python 内置函数的帮助下，上面例子中编写的所有代码都可以压缩成一行。

```
numbers = [1, 2, 2, 3, 3, 4, 5]
unique_numbers = list(set(numbers))
print(unique_numbers)
# Result: [1, 2, 3, 4, 5] 
```

虽然这段代码看起来与第一个例子非常不同，但想法是一样的。使用集合来获得唯一的数字。然后，把集合变成列表。

```
unique_numbers = list(set(numbers)) 
```

阅读上面的代码时，思考“从里到外”是有帮助的。最里面的代码首先被求值: **`set(numbers)`** 。然后，对最外层的代码进行求值: **`list(set(numbers))`** 。

# 选项 2——使用迭代来识别唯一值

迭代是另一种可以考虑的方法。

主要思想是创建一个空列表来保存唯一的数字。然后，使用 for 循环迭代给定列表中的每个数字。如果该数字已经在唯一列表中，那么继续下一次迭代。否则，在上面加上数字。

让我们看看使用迭代来获取列表中唯一值的两种方法，从更详细的一种开始。

```
numbers = [20, 20, 30, 30, 40]

def get_unique_numbers(numbers):
    unique = []

    for number in numbers:
        if number in unique:
            continue
        else:
            unique.append(number)
    return unique

print(get_unique_numbers(numbers))
# Result: [20, 30, 40] 
```

这是每一步发生的事情。首先，给我一串数字， **`numbers`** 。我把这个列表传入我的函数， **`get_unique_numbers`** 。

在函数里面，我创建了一个空列表， **`unique`** 。最终，这个列表将包含所有唯一的数字。

我使用一个 for 循环来遍历 **`numbers`** 列表中的每个数字。

```
 for number in numbers:
       if number in unique:
           continue
       else:
           unique.append(number) 
```

循环内部的条件检查当前迭代的编号是否在 **`unique`** 列表中。如果是，循环继续到下一次迭代。否则，该号码将被添加到列表中。

重要的一点是:只添加唯一的数字。一旦循环完成，我就返回包含所有唯一数字的 **`unique`** 。

## 迭代的较短方法

还有一种方法可以用更少的代码编写函数。

```
numbers = [20, 20, 30, 30, 40]

def get_unique_numbers(numbers):
    unique = []
    for number in numbers:
        if number not in unique:
            unique.append(number)
    return unique
#Result: [20, 30, 40] 
```

区别在于条件句。这次是这样设置的:如果数字不在 **`unique`** 里，那就加上去。

```
if number not in unique:
    unique.append(number) 
```

否则，循环将移到列表中的下一个数字， **`numbers`** 。

结果是一样的。然而，当布尔值被否定时，有时更难思考和阅读代码。

还有其他方法可以在 Python 列表中找到唯一值。但是您可能会发现自己正在尝试本文中介绍的一种方法。

我在[amymhaddad.com](http://amymhaddad.com/)上写了关于学习编程的文章，以及学习编程的最佳方法。在推特上关注我: [@amymhaddad](https://twitter.com/amymhaddad) 。