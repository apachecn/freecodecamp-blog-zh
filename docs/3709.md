# Python 列表解释:Len、Pop、Index 和列表理解

> 原文：<https://www.freecodecamp.org/news/python-lists-explained-len-pop-index-and-list-comprehension/>

Python 中的列表类似于 JavaScript 中的数组。它们是 Python 中用于存储数据集合的内置数据类型之一。

## 基本用法

### 如何创建列表

使用一对方括号创建一个空的`list`:

```
>>> empty_list = []
>>> type(empty_list)
<class 'list'>
>>> len(empty_list)
0
```

通过用方括号括起逗号分隔的元素列表，可以用元素创建一个`list`。列表允许元素具有不同的类型(异构)，但最常见的是单一类型(同构):

```
>>> homogeneous_list = [1, 1, 2, 3, 5, 8]
>>> type(homogeneous_list)
<class 'list'>
>>> print(homogeneous_list)
[1, 1, 2, 3, 5, 8]
>>> len(homogeneous_list)
6
>>> heterogeneous_list = [1, "Hello Campers!"]
>>> print(heterogeneous_list)
[1, "Hello Campers!"]
>>> len(heterogeneous_list)
2
```

`list`构造函数也可以用来创建一个`list`:

```
>>> empty_list = list()                            # Creates an empty list
>>> print(empty_list)
[]
>>> list_from_iterable = list("Hello campers!")    # Creates a list from an iterable.
>>> print(list_from_iterable)
['H', 'e', 'l', 'l', 'o', ' ', 'c', 'a', 'm', 'p', 'e', 'r', 's', '!']
```

您还可以使用 List Comprehension 来创建列表，我们将在本文的后面进行介绍。

### 访问列表中的元素

```
>>> my_list = [1, 2, 9, 16, 25]
>>> print(my_list)
[1, 2, 9, 16, 25]
```

*零索引*

```
>>> my_list[0]
1
>>> my_list[1]
2
>>> my_list[2]
9
```

*环绕分度*

```
>>> my_list[-1]
25
>>> my_list[-2]
16
```

*python-3 的解包列表*

```
>>> print(*my_list)
1 2 9 16 25
```

### 列表是可变的

`lists`是可变容器。可变容器是允许改变容器所包含的对象的容器。

来自一个`list`的元素可以被提取并使用另一个`list`作为索引重新排列。

```
>>> my_list = [1, 2, 9, 16, 25, 34, 53, 21]
>>> my_index = [5, 2, 0]
>>> my_new_list = [my_list[i] for i in my_index]
>>> print(my_new_list)
[34, 9, 1]
```

## 列出方法

### `len()`

`len()`方法返回一个对象的长度，无论它是列表、字符串、元组还是字典。

`len()`接受一个参数，它可以是一个序列(如字符串、字节、元组、列表或范围)或集合(如字典、集合或冻结集)。

```
list1 = [123, 'xyz', 'zara'] # list
print(len(list1)) # prints 3 as there are 3 elements in the list1

str1 = 'basketball' # string
print(len(str1)) # prints 10 as the str1 is made of 10 characters

tuple1 = (2, 3, 4, 5) # tuple 
print(len(tuple1)) # prints 4 as there are 4 elements in the tuple1

dict1 = {'name': 'John', 'age': 4, 'score': 45} # dictionary
print(len(dict1)) # prints 3 as there are 3 key and value pairs in the dict1
```

### `index()`

`index()`返回作为函数参数给出的列表中元素的第一次出现/索引。

```
numbers = [1, 2, 2, 3, 9, 5, 6, 10]
words = ["I", "love", "Python", "I", "love"]

print(numbers.index(9)) # 4
print(numbers.index(2)) # 1
print(words.index("I")) # 0
print(words.index("am")) # returns a ValueError as 'am' is not in the `words` list
```

这里，第一个输出非常明显，但是第二个和第三个输出乍一看可能会令人困惑。但是记住`index()`返回元素的第一次出现，因此在这种情况下`1`和`0`是索引，其中`2`和`"I"`分别在列表中第一次出现。

同样，如果在列表中没有找到一个元素，就像在`words`列表中索引`"am"`的情况一样，返回一个`ValueError`。

**可选参数**

您还可以使用可选参数将搜索限制在列表的特定子序列:

```
words = ["I", "am", "a", "I", "am", "Pythonista"]

print(words.index("am", 2, 5)) # 4
```

尽管在索引 2(含)和 5(不含)之间搜索元素，但返回的索引是相对于完整列表的开头而不是 start 参数计算的。

### `pop()`

方法从列表中移除并返回最后一个元素。

有一个可选参数，它是要从列表中删除的元素的索引。如果没有指定索引，`pop()`删除并返回列表中的最后一项。

如果传递给`pop()`方法的索引不在范围内，它抛出`IndexError: pop index out of range`异常。

```
cities = ['New York', 'Dallas', 'San Antonio', 'Houston', 'San Francisco'];

print "City popped is: ", cities.pop() # City popped is: San Francisco
print "City at index 2 is  : ", cities.pop(2) # City at index 2 is: San Antonio
```

**基本堆栈功能**

`pop()`方法通常与`append()`结合使用，以在 Python 应用程序中实现基本的堆栈功能:

```
stack = []

for i in range(5):
    stack.append(i)

while len(stack):
    print(stack.pop())
```

### 列表理解

列表理解是一种循环遍历列表以基于某些条件产生新列表的方法。一开始可能会有些混乱，但是一旦你习惯了这种语法，它就会变得非常强大和快速。

学习如何使用列表理解的第一步是查看循环列表的传统方式。下面是一个简单的示例，它返回一个新的偶数列表。

```
# Example list for demonstration
some_list = [1, 2, 5, 7, 8, 10]

# Empty list that will be populate with a loop
even_list = []

for number in some_list:
  if number % 2 == 0:
    even_list.append(number)

# even_list now equals [2, 8, 10]
```

首先创建一个包含一些数字的列表。然后创建一个空列表来保存循环的结果。在循环中，你检查每个数字是否能被 2 整除，如果能，你就把它加到`even_list`上。这用了 5 行代码，不包括注释和空白，在这个例子中并不多。

现在来看列表理解的例子。

```
# Example list for demonstration
some_list = [1, 2, 5, 7, 8, 10]

# List Comprehension
even_list = [number for number in some_list if number % 2 == 0]

# even_list now equals [2, 8, 10]
```

另一个例子，有同样的两个步骤:下面将创建一个数字列表，对应于`my_starting_list`中的数字乘以 7。

```
my_starting_list = [1, 2, 3, 4, 5, 6, 7, 8]
my_new_list = []

for item in my_starting_list:
my_new_list.append(item * 7)
```

运行这段代码时，`my_new_list`的最终值是:`[7, 14, 21, 28, 35, 42, 49, 56]`

使用 list comprehension 的开发人员可以使用下面的 list comprehension 获得相同的结果，这导致相同的`my_new_list`。

```
my_starting_list = [1, 2, 3, 4, 5, 6, 7, 8]

my_new_list = [item * 7 for item in my_starting_list]
```

用列表理解方式写的一个简单公式是:

`my_list = [{operation with input n} for n in {python iterable}]`

将`{operation with input n}`替换为您希望更改的 iterable 返回的项。上面的例子使用了`n * 7`，但是操作可以根据需要简单或复杂。

用任何 iterable 替换`{python iterable}`。序列类型将是最常见的。上面的示例中使用了列表，但是元组和范围也很常见。

如果满足某些条件，列表理解会将现有列表中的元素添加到新列表中。它更整洁，但在大多数情况下也更快。在某些情况下，列表理解可能会妨碍可读性，所以开发人员在选择使用列表理解时必须权衡他们的选项。

**用条件句理解列表的例子**

列表理解中的控制流可以使用条件来控制。例如:

```
only_even_list = [i for i in range(13) if i%2==0]
```

这相当于以下循环:

```
only_even_list = list()
for i in range(13):
  if i%2 == 0:
    only_even_list.append(i)
```

列表理解也可以包含嵌套的 if 条件。考虑以下循环:

```
divisible = list()
for i in range(50):
  if i%2 == 0:
    if i%3 == 0:
      divisible.append(i)
```

使用列表理解，这可以写成:

```
divisible = [i for i in range(50) if i%2==0 if i%3==0]
```

If-Else 语句也可以与列表理解一起使用。

```
list_1 = [i if i%2==0 else i*-1 for i in range(10)]
```

#### **更多信息:**

*   [最好的 Python 代码示例](https://www.freecodecamp.org/news/python-example/)