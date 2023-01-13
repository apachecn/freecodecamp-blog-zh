# Python 中的 For 循环

> 原文：<https://www.freecodecamp.org/news/for-loops-in-python/>

## **For 循环语句**

Python 利用 for 循环来迭代元素列表。不像 C 或 Java，它们使用 for 循环逐步改变一个值，并使用该值访问诸如数组之类的东西。

For 循环遍历基于集合的数据结构，如列表、元组和字典。

基本语法是:

```
for value in list_of_values:
  # use value inside this block
```

一般来说，您可以使用任何东西作为迭代器值，其中 iterable 的条目可以被赋值给。例如，您可以从元组列表中解包元组:

```
list_of_tuples = [(1,2), (3,4)]

for a, b in list_of_tuples:
  print("a:", a, "b:", b)
```

另一方面，你可以循环任何可迭代的东西。您可以调用函数或使用列表文字。

```
for person in load_persons():
  print("The name is:", person.name)
```

```
for character in ["P", "y", "t", "h", "o", "n"]:
  print("Give me a '{}'!".format(character))
```

使用 For 循环的一些方式:

### 迭代 range()函数

```
for i in range(10):
    print(i)
```

range 不是一个函数，它实际上是一个不可变的序列类型。输出将包含从下限(即 0)到上限(即 10)的结果，但不包括 10。默认情况下，下限或起始索引设置为零。输出:

```
>
0
1
2
3
4
5
6
7
8
9
>
```

此外，可以通过添加第二个和第三个参数来指定序列的下限，甚至序列的步长。

```
for i in range(4,10,2): #From 4 to 9 using a step of two
    print(i)
```

输出:

```
>
4
6
8
>
```

### xrange()函数

在很大程度上，xrange 和 range 在功能上是完全相同的。它们都提供了一种生成整数列表的方法，供您随意使用。唯一的区别是 range 返回一个 Python 列表对象，而 xrange 返回一个 xrange 对象。这意味着 xrange 并不像 range 那样在运行时生成静态列表。它用一种叫做让步的特殊技术创造出你需要的价值。这种技术用于一种称为发生器的对象。

再补充一点。在 Python 3.x 中，xrange 函数不再存在。range 函数现在做 Python 2.x 中 xrange 做的事情

### 迭代列表或元组中的值

```
A = ["hello", 1, 65, "thank you", [2, 3]]
for value in A:
    print(value)
```

输出:

```
>
hello
1
65
thank you
[2, 3]
>
```

### 迭代字典中的键(也称为 hashmap)

```
fruits_to_colors = {"apple": "#ff0000",
                    "lemon": "#ffff00",
                    "orange": "#ffa500"}

for key in fruits_to_colors:
    print(key, fruits_to_colors[key])
```

输出:

```
>
apple #ff0000
lemon #ffff00
orange #ffa500
>
```

### 用 zip()函数在一个循环中遍历两个相同大小的列表

```
A = ["a", "b", "c"]
B = ["a", "d", "e"]

for a, b in zip(A, B):
  print a, b, a == b 
```

输出:

```
>
a a True
b d False
c e False
>
```

### 遍历一个列表，用 enumerate()函数获得相应的索引

```
A = ["this", "is", "something", "fun"]

for index,word in enumerate(A):
    print(index, word)
```

输出:

```
>
0 this
1 is
2 something
3 fun
>
```

一个常见的用例是迭代字典:

```
for name, phonenumber in contacts.items():
  print(name, "is reachable under", phonenumber)
```

如果您绝对需要访问您的迭代的当前索引，请使用 ****而不是****`range(len(iterable))`！这是一种极其糟糕的做法，会让你遭到资深 Python 开发者的嘲笑。请改用内置函数`enumerate()`:

```
for index, item in enumerate(shopping_basket):
  print("Item", index, "is a", item)
```

### for/else 语句

Pyhton 允许在 for 循环中使用 else，当循环体中的所有条件都不满足时，就会执行 else。要使用 else，我们必须使用`break`语句，这样我们就可以在满足条件的情况下跳出循环。如果我们不越狱，那么 else 部分将被执行。

```
week_days = ['Monday','Tuesday','Wednesday','Thursday','Friday']
today = 'Saturday'
for day in week_days:
  if day == today:
    print('today is a week day')
    break
else:
  print('today is not a week day')
```

在上面的例子中，输出将是`today is not a week day`,因为循环中的 break 永远不会被执行。

### 使用内联循环函数遍历列表

我们还可以使用 python 进行内联迭代，例如，如果我们需要将列表中的所有单词大写，我们可以简单地执行以下操作:

```
A = ["this", "is", "awesome", "shinning", "star"]

UPPERCASE = [word.upper() for word in A]
print (UPPERCASE)
```

输出:

```
>
['THIS', 'IS', 'AWESOME', 'SHINNING', 'STAR']
>
```

#### **更多信息:**

*   [Python2 for 循环文档](https://docs.python.org/2.7/tutorial/controlflow.html#for-statements)
*   [Python3 for 循环文档](https://docs.python.org/3/tutorial/controlflow.html#for-statements)