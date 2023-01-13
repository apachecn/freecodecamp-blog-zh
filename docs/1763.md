# 如何在 Python 中格式化字符串

> 原文：<https://www.freecodecamp.org/news/how-to-format-a-string-in-python/>

当你用 Python 编码时，格式化一个字符串是你一直要做的事情。对于我们定义的每个变量、元组、字典和逻辑，我们最终必须以某种形式打印或显示出来。

所有这些，无一例外，都要求我们将所需的输出格式化成一个字符串，然后才能最终打印出来，或者使用相关的函数在 Python 中显示该字符串。

您可能已经使用过`str`函数并处理过字符串文字，但是很多时候我们需要更多的东西。记住这一点，让我们看看如何使用不同的 Python3 技术格式化字符串。

# 如何使用串联设置字符串的格式:print('Hello'+name)

这是最简单——也是最容易——的入门技巧。这也是我更喜欢教一个刚开始编码的人的方法。

使用按字面顺序排列的部分和'+'操作符可以让你在没有任何编码知识的情况下轻松掌握。

```
name = 'YoungWonks'
year = 2014

string = 'Hello, ' + name
print(string)

string = name + ' was started in ' + str(year)
print(string)
```

Formatting Strings Using Concatenation

这是结果:

```
Hello, YoungWonks 
YoungWonks was started in 2014
```

上面的例子演示了如何使用加法运算符连接变量来创建格式化字符串。这里要记住的是，我们需要将所有变量转换成字符串。这里，变量名已经是一个字符串，年份是一个整数。

# 如何用老方法格式化字符串:打印' Hello %s' % name

这种方法在 Python2 中更常用，当时这种语言还很年轻，还在不断发展。对于有 C 编程背景的资深程序员来说，这是一种很容易理解的技术。

在这里，正如您在下面的代码中看到的，我们用指定的数据类型和语句末尾的参数来定义占位符。这类似于 C 语言(以及其他语言)。

```
name = 'YoungWonks'
year = 2014

string = 'Hello, %s ' % name
print(string)

string = '%s was started in %d' % (name, year)
print(string)
```

Formatting Strings in the Old Style

这是结果:

```
Hello, YoungWonks 
YoungWonks was started in 2014
```

在上面的例子中，我们定义了想要格式化成字符串的变量。然后我们用变量的占位符创建字符串。

请记住，占位符应该与相应变量的类型相匹配。我们可以在语句末尾使用%符号将实际变量链接到占位符。

# 如何使用格式设置字符串的格式:“”。格式()

这是一种被许多 Python 程序员视为新鲜空气的技术，因为它使事情变得更容易。

它是在 Python3 早期引入的。本质上，新语法移除了“%”符号，并提供了作为字符串方法的`.format()`，它接受在花括号中描述的替换的位置参数。

我们还可以在这里指定关键字参数，尽管有时感觉有点冗长。

使用`str.format`方法还可以做更多的事情，比如指定精度、舍入和补零。出于本文的目的，我们将只关注基本用法。

```
name = 'YoungWonks'
year = 2014

string = '{} was started in {}'.format(name, year)
print(string)

string = '{0} was started in {1}'.format(name, year)
print(string)

yw = {'name': 'YoungWonks', 'year': 2014}
string = "{name} was started in {year}.".format(name=yw['name'], year=yw['year'])
print(string)
```

Formatting Strings with the .format method

结果如下:

```
YoungWonks was started in 2014
YoungWonks was started in 2014
YoungWonks was started in 2014
```

在上面的例子中，我们创建了要格式化成字符串的变量。然后，在最简单的形式中，我们可以使用{}作为要使用的变量的占位符。然后，我们将`.format()`方法应用于字符串，并将变量指定为一组有序的参数。

示例的第二部分使用占位符中有序参数的索引。

该示例的第三部分还向我们展示了如何在字符串的格式中使用字典，同时在。格式方法。

# 如何使用 F Strings 格式化字符串:f'hello {name} '

f 字符串也是格式字符串的缩写，是 Python3 现在支持的最新技术，所以它们被迅速采用。

F strings 解决了`.format()`技术的冗长性，并提供了一种简单的方法来完成同样的工作，即在字符串前添加字母 f 作为前缀。这种方法似乎具有两种技术中最好的一种:

1.  连接的文字顺序
2.  `.format()`方法的模块化

由于这些优点，越来越多的开发人员开始使用它。我认为它对学生来说也更容易学习，如果你还是编程新手，也更容易采用。

```
name = 'YoungWonks'
year = 2014
string = f'{name} was started in {year}'
print(string)

string = f'{name} was started in {2013 + 1}.'
print(string)

yw = {'name': 'YoungWonks', 'year': 2014}
string = f"{yw['name']} was started in {yw['year']}."
print(string)
```

Formatting strings with f' strings

```
YoungWonks was started in 2014 
YoungWonks was started in 2014.
YoungWonks was started in 2014.
```

您可能已经注意到，上面的例子类似于`.format()`方法。这里我们使用{}作为字符串中的占位符，但也直接在占位符中指定变量。

当我们在字符串的开头使用小写字母`f`时，这个方法就生效了。您还可以对其他数据类型(如字典)使用方法。

# 我们需要知道并使用所有这些字符串格式化方法吗？

所有这些都是学习 Python 时的相关知识。即使你不会经常使用字符串格式的旧样式，但有一些旧的论坛、堆栈溢出帖子和许多旧的机器学习库仍在使用这些旧样式。

因此，了解它们是很好的，这样您就可以认识到发生了什么，并可能调试一个问题。

在我们这里讨论的其他三种方法中，您可以使用其中任何一种来格式化 Python 中的字符串。只要根据上下文甚至当天的心情来挑选哪一个效果最好就行了！

`+`在你处理快速简单的语句时很有用。在开发一个简单的项目时，您还可以使用它来创建调试状态。

例如，如果我们要做一个简单的数字猜谜游戏，那么我们可以使用如下所示的`+`操作符:

```
import random
random_number = random.randint(1,100)
user_guess = None
while True:
	print('Please guess the number between 1 and 100 : ')
    user_guess = int(input())
    if user_guess == random_number:
    	print('Correct!')
    else:
    	print("Try Again!")

    #prints for debugging
    print("Random Number: "+ str(random_number))
    print("User Guess: "+ str(user_guess))
```

上述代码中使用的调试语句不会保留在您的最终代码中。(你不会想让别人猜到你的号码吧？)所以在开发代码时，您可以在打印中使用串联方法。

当您添加日志语句时，您可能更喜欢使用`.format()`或`f string`方法，因为它们会使您的语句更加模块化。为数据类型本身指定精度/格式也更容易。

# Python 中还有其他的字符串格式化技术吗？

是啊！模板字符串就是这样一种技术，它作为 Python 标准库的字符串模块中的一个类提供。它作为一个模块被导入，`.substitute()`方法用于替换占位符。

也就是说，到目前为止，我从未在我的编码经验中使用过它，只是在为这个博客做研究时才了解到它。

# 结论

Python 字符串格式化是 Python 程序员需要了解的一个重要的日常工具。每种编码语言似乎都以相似的方式格式化字符串。这也是我们为什么使用脚本语言的一个非常重要的部分。

在这些不同的字符串格式化方式中，`f string`方法，以及`.format()`和连接似乎会一直存在。随着大多数遗留库开始支持 Python3，我们可能会看到 Python2 语法逐渐消失。

这里有一个[有用的备忘单](https://www.youngwonks.com/resources/python-cheatsheet)，它包含更多基本的 Python 语法和一些字符串方法供你参考。

# 其他语言的字符串格式

其他编程语言在精度和表示方面似乎遵循类似的语法，并依赖于变量替换。但是它们在如何向控制台输出格式化字符串方面有所不同。

C++是标准库中带有`cout`方法的可见异常值。Java 本质上使用了`System.out.println()`，而 JavaScript 使用了`console.log()`。你会发现每种语言都有类似的方法。

*苏钦·拉维是一名教育家&技术专家，在青年学院教授计算机科学。他是 Wonksknow LLC 的首席软件开发人员。*