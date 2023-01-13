# Python 中的追加——如何追加到列表或数组中

> 原文：<https://www.freecodecamp.org/news/append-in-python-how-to-append-to-a-list-or-an-array/>

在本文中，您将了解 Python 中的`.append()`方法。您还将看到`.append()`与其他用于向列表添加元素的方法有何不同。

我们开始吧！

## Python 中的列表是什么？初学者的定义

编程中的数组是项目的有序集合，所有项目都需要具有相同的数据类型。

然而，与其他编程语言不同，数组不是 Python 中内置的数据结构。Python 使用列表，而不是传统的数组。

列表本质上是动态数组，是 Python 中最常见和最强大的数据结构之一。

你可以把它们想象成有序的容器。它们一起存储和组织相似类型的相关数据。

存储在列表中的元素可以是任何数据类型。

可以有整数列表(整数)、浮点数列表(浮点数)、字符串列表(文本)和任何其他内置 Python 数据类型的列表。

尽管列表可能只保存相同数据类型的项目，但它们比传统的数组更灵活。这意味着在同一个列表中可以有各种不同的数据类型。

列表有 0 个或更多项目，这意味着也可以有空列表。列表中也可能有重复的值。

值由逗号分隔，并用方括号`[]`括起来。

### 如何在 Python 中创建列表

要创建新列表，请先给列表命名。然后添加赋值操作符(`=`)和一对左右方括号。在括号内添加您希望列表包含的值。

```
#create a new list of names
names = ["Jimmy", "Timmy", "Kenny", "Lenny"]

#print the list to the console
print(names)

#output
#['Jimmy', 'Timmy', 'Kenny', 'Lenny'] 
```

### Python 中列表的索引方式

列表维护每个项目的顺序。

集合中的每一项都有自己的索引号，您可以用它来访问该项本身。

Python(和其他现代编程语言)中的索引从 0 开始，列表中的每一项都会增加。

例如，前面创建的列表有 4 个值:

```
names = ["Jimmy", "Timmy", "Kenny", "Lenny"] 
```

列表中的第一个值“Jimmy”的索引为 0。

列表中的第二个值“Timmy”的索引为 1。

列表中的第三个值“Kenny”的索引为 2。

列表中的第四个值“Lenny”的索引为 3。

要通过索引号访问列表中的元素，首先写列表的名称，然后在方括号中写元素索引的整数。

例如，如果您想访问索引为 2 的元素，您应该这样做:

```
names = ["Jimmy", "Timmy", "Kenny", "Lenny"]

print(names[2])

#output
#Kenny 
```

## Python 中的列表是可变的

在 Python 中，当对象是*可变的*时，这意味着它们的值一旦被创建就可以被改变。

列表是可变的对象，所以你可以在创建后更新和改变它们。

列表也是动态的，这意味着它们可以在程序的整个生命周期中增长和收缩。

可以从现有列表中删除项目，也可以向现有列表中添加新项目。

在列表中添加和删除项目都有内置的方法。

比如给**添加**项，有`.append()`、`.insert()`和`.extend()`方法。

要**删除**项，有`.remove()`、`.pop()`和`.pop(index)`三种方法。

### `.append()`方法是做什么的？

`.append()`方法向一个已经存在的列表的**端**添加一个额外的元素。

一般语法如下所示:

```
list_name.append(item) 
```

让我们来分解一下:

*   `list_name`是你给列表取的名字。
*   `.append()`是在`list_name`末尾增加一个项目的列表方法。
*   `item`是您要添加的指定单个项目。

当使用`.append()`时，原始列表被修改。不会创建新列表。

如果您想在之前创建的列表中添加一个额外的名称，您可以执行以下操作:

```
names = ["Jimmy", "Timmy", "Kenny", "Lenny"]

#add the name Dylan to the end of the list
names.append("Dylan")

print(names)

#output
#['Jimmy', 'Timmy', 'Kenny', 'Lenny', 'Dylan'] 
```

### `.append()`和`.insert()`方法有什么区别？

两种方法的区别在于，`.append()`在列表的**末端**添加一个项目，而`.insert()`在列表的指定位置插入一个项目**。**

正如您在上一节中看到的，`.append()`会将您作为参数传递给函数的项始终添加到列表的末尾。

如果你不想只是将项目添加到列表的末尾，你可以用`.insert()`指定你想要添加它们的位置。

一般语法如下所示:

```
list_name.insert(position,item) 
```

让我们来分解一下:

*   `list_name`是列表的名称。
*   `.insert()`是在列表中插入项目的列表方法。
*   `position`是该方法的第一个参数。它总是一个整数——具体来说，它是希望放置新项目的位置的索引号。
*   `item`是该方法的第二个参数。在这里，您可以指定要添加到列表中的新项目。

例如，假设您有以下编程语言列表:

```
programming_languages = ["JavaScript", "Java", "C++"]

print(programming_languages)

#output
#['JavaScript', 'Java', 'C++'] 
```

如果您想在列表的开头插入“Python ”,作为列表的一个新项，您可以使用`.insert()`方法并将位置指定为`0`。(请记住，列表中的第一个值的索引始终为 0。)

```
programming_languages = ["JavaScript", "Java", "C++"]

programming_languages.insert(0, "Python")

print(programming_languages)

#output
#['Python', 'JavaScript', 'Java', 'C++'] 
```

如果您想让“JavaScript”成为列表中的第一项，然后*和*添加“Python”作为新项，您可以将位置指定为`1`:

```
programming_languages = ["JavaScript", "Java", "C++"]

programming_languages.insert(1,"Python")

print(programming_languages)

#output
#['JavaScript', 'Python', 'Java', 'C++'] 
```

与只在列表末尾添加一个新项目的`.append()`方法相比，`.insert()`方法提供了更多的灵活性。

### `.append()`和`.extend()`方法有什么区别？

如果您想一次向列表中添加多个项目，而不是一次添加一个项目，该怎么办？

您可以使用`.append()`方法将多个项目添加到列表的末尾。

假设您有一个只包含两种编程语言的列表:

```
programming_languages = ["JavaScript", "Java"]

print(programming_languages)

#output
#['JavaScript', 'Java'] 
```

然后你想在它的末尾再添加两种语言。

在这种情况下，您传递一个包含两个想要添加的新值的列表，作为参数给`.append()`:

```
programming_languages = ["JavaScript", "Java"]

#add two new items to the end of the list
programming_languages.append(["Python","C++"])

print(programming_languages)

#output
#['JavaScript', 'Java', ['Python', 'C++']] 
```

如果您仔细看看上面的输出，`['JavaScript', 'Java', ['Python', 'C++']]`，您会看到一个新的列表被添加到已经存在的列表的末尾。

因此，`.append()` **在列表**中添加一个列表。

列表是对象，当您使用`.append()`将另一个列表添加到列表中时，新项目将作为单个对象(项目)添加。

假设您已经有两个列表，如下所示:

```
names = ["Jimmy", "Timmy"]
more_names = ["Kenny", "Lenny"] 
```

如果想要通过将`more_names`的内容添加到`names`来将两个列表的内容合并成一个列表，该怎么办？

当`.append()`方法用于此目的时，在`names`中创建另一个列表:

```
names = ["Jimmy", "Timmy"]
more_names = ["Kenny", "Lenny"]

#add contents of more_names to names
names.append(more_names)

print(names)

#output
#['Jimmy', 'Timmy', ['Kenny', 'Lenny']] 
```

因此，`.append()`通过将对象追加到末尾来添加新元素作为另一个列表。

要实际将列表连接(添加)在一起，并且**将一个列表中的所有项目组合到另一个**中，您需要使用`.extend()`方法。

一般语法如下所示:

```
list_name.extend(iterable/other_list_name) 
```

让我们来分解一下:

*   `list_name`是其中一个列表的名称。
*   `.extend()`是将一个列表的所有内容添加到另一个列表的方法。
*   `iterable`可以是任何可迭代的，例如另一个列表，例如`another_list_name`。在这种情况下，`another_list_name`是一个列表，它将与`list_name`连接在一起，其内容将被逐个添加到`list_name`的末尾，作为单独的项目。

因此，以前面的例子为例，当`.append()`被替换为`.extend()`时，输出将如下所示:

```
names = ["Jimmy", "Timmy"]
more_names = ["Kenny", "Lenny"]

names.extend(more_names)

print(names)

#output
#['Jimmy', 'Timmy', 'Kenny', 'Lenny'] 
```

当我们使用`.extend()`时，`names`列表得到了扩展，其长度增加了 2。

`.extend()`的工作方式是，它将一个列表(或其他 iterable)作为参数，遍历每个元素，然后 iterable 中的每个元素都被添加到列表中。

`.append()`和`.extend()`还有一个区别。

如前所述，当您想要添加一个字符串时，`.append()`会将整个单个项目添加到列表的末尾:

```
names = ["Jimmy", "Timmy", "Kenny", "Lenny"]

#add the name Dylan to the end of the list
names.append("Dylan")

print(names)

#output
#['Jimmy', 'Timmy', 'Kenny', 'Lenny', 'Dylan'] 
```

如果您使用`.extend()`将一个字符串添加到列表的末尾，字符串中的每个字符将作为一个单独的项目添加到列表中。

这是因为字符串是可迭代的，并且`.extend()`迭代传递给它的可迭代参数。

所以，上面的例子应该是这样的:

```
names = ["Jimmy", "Timmy", "Kenny", "Lenny"]

#pass a string(iterable) to .extend()
names.extend("Dylan")

print(names)

#output
#['Jimmy', 'Timmy', 'Kenny', 'Lenny', 'D', 'y', 'l', 'a', 'n'] 
```

## 结论

总之，`.append()`方法用于将一个项目添加到现有列表的末尾，而不创建新列表。

当它用于将一个列表添加到另一个列表时，它在一个列表中创建一个列表。

如果你想了解更多关于 Python 的知识，可以查看 freeCodeCamp 的 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。你将开始以互动和初学者友好的方式学习。最后，您还将构建五个项目来实践您所学到的内容。

感谢阅读和快乐编码！