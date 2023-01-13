# 如何维护 Python 代码的可伸缩性

> 原文：<https://www.freecodecamp.org/news/how-to-build-scalable-apps-in-python/>

任何处理数据的应用程序都可能开始运行缓慢，甚至开始损坏或中断。如果开发人员能够快速编程并为编码增加更多价值，那就更好了。

作为开发人员，我们应该拥有快速原型化的工具。这就是为什么我们应该努力开发一款可扩展的应用。概括地说，使用 Python 编程语言可以构建一个大型的、可伸缩的应用程序。

Python 是一种高级编程语言，也是面向对象的。凭借其内置的数据结构、动态绑定和动态类型等特性，我们可以使用它尽可能快速地开发应用程序。

Python 也可以作为粘合脚本语言，集成现有组件，帮助我们构建可扩展的应用程序。

Python 是编程语言的先驱之一，开发人员可以用它来完成所有的缩放工作。

这里有一些用 Python 开发可伸缩应用程序的技巧，你可以参考一下。

## 学会巧妙使用“收藏”

Python 支持丰富而强大的“集合”数据结构/容器，如[字典](https://docs.python.org/3.6/library/stdtypes.html#dict)、[列表](https://docs.python.org/3.6/library/stdtypes.html#list)、[集合](https://docs.python.org/3.6/library/stdtypes.html#set)和[元组](https://docs.python.org/3.6/library/stdtypes.html#tuple)。它们对于构建可扩展的应用程序非常有价值。然而，过度使用它们会影响代码的可伸缩性。当收藏被过度使用时，很容易发现。

```
# notebooks.csv holds meta information on a collection of notebooks:
# heading, writer, year of pub, etc.

# load_from_file returns a list of dicts.

notebooks = load_from_file('notebooks.csv')

notebook_summaries = dict()
for notebook in notebooks:

   notebook_summaries[notebook["heading"]] = notebook["summary"]

for heading, summary in notebook_summaries.items():

   # Do something interesting with the summary.

   print(heading, summary) 
```

从上面的代码中，您可以清楚地看到它在从 CSV 文件中读取笔记本数据后创建了表映射标题。如果从内存使用的角度来看，notebooks.csv 有数百个标题并没有什么问题。

但是如果和几十款的整个笔记本专卖店的库存有关，就不对了。您的编码可能会有一两个问题，这也取决于您使用的版本，Python 2 还是 Python 3。

这造成了代码存储器可伸缩性的瓶颈问题。创建一个名为 notebook_summaries 的数据结构在这里是不必要的，但是它提高了可读性。“for”行帮助您立即知道在摘要中有一个循环正在运行。

新的数据结构包含每个笔记本的完整摘要，这些笔记本可能比所有其他字段消耗更多的内存。假设一台笔记本消耗 N 字节内存，那么完整的块至少要消耗 1.5 * N 字节。

**这将在 Python 3 中得到更好的扩展**

```
notebooks = load_from_file('notebooks.csv')

for notebook in notebooks.items():

   print(notebook["title"], notebook["summary"]) 
```

我建议您创建命名良好的变量，因为这有助于提高 Python 代码的可维护性。

## Python 代码的智能迭代

在使用 Python 开发大型应用程序时，可伸缩性不是您应该考虑的唯一问题。你可以面对其他几个问题。例如，迭代问题是最常见的问题。

有时，代码中的 for 行迭代 notebook_summaries.items()并创建另一个笔记本副本。这种代码迭代可能是导致代码性能低下的原因，在这种情况下，Python 代码在启动 for 循环之前就开始挂起。发生这种情况是因为 notebook_summaries.items()形成了一个非常大的列表，消耗了更多的内存。这也是因为 Python 代码在 forloop 之后执行字节码。

它将开始为这个列表分配更多的内存。迭代问题再次影响了 Python 2 和 Python 3 的 items()，并产生了 notebooks_summaries 内容的额外副本。开发人员可以使用 iteritems 来代替 Python 2:

**在 Python 2 中，用“iteritems”代替“items”**

```
notebooks = load_from_file('notebooks.csv')

    for notebook in notebooks.iteritems():

      print(notebook["title"], notebook["summary"])
```

所以，这里的要点是注意在所有 Python 版本中使用迭代器和创建列表之间的区别。开发人员有责任根据编码环境来证明正确的模式。

## 在 Python 代码中使用“生成器”实现可伸缩性

生成器函数允许您以更简单的方式创建迭代器。想象一下，你正在构建一个语法软件程序，它接收文本，分析句子，并执行某种语法分析。句子的每一行都将由一个句点后跟一个或多个字符来分隔。

**参见编码**

```
import re
text = '''Full body of text. It has many sentences.

Some have grammatical errors and some are correct.'''

sentences = re.analyzed(r'\.\s+', text)

for sentence in sentences:

   print(sentence)
```

**运行清单**

```
This is a body of text
It has many sentences

Some have grammatical errors and some are correct.
```

```
import random
def weathermaker(volatility, days):
    '''
    Yield a series of messages giving the day's weather and occasional commentary
    volatility ‑ a float between 0 and 1; the greater this number the greater
  the likelihood that the weather will change on each given day
    days ‑ number of days for which to generate weather
    '''
    #Always start as if yesterday were sunny
    current_weather = 'sunny'
    #First item is the probability that the weather will stay the same
    #Second item is the probability that the weather will change
    #The higher the volatility the greater the likelihood of change
    weights = 1.0‑volatility, volatility    #For fun track how many sunny days in a row there have been
    sunny_run = 1
    #How many rainy days in a row there have been
    rainy_run = 0
    for day in range(days):
        #Figure out the opposite of the current weather
        other_weather = 'rainy' if current_weather == 'sunny' else 'sunny'
        #Set up to choose the next day's weather. First set up the choices
        choose_from = current_weather, other_weather        #random.choices returns a list of random choices based on the weights
        #By default a list of 1 item, so we grab that first and only item with 0 current_weather = random.choices(choose_from, weights)0        yield 'today it is ' + current_weather
        if current_weather == 'sunny':
            #Check for runs of three or more sunny days
            sunny_run += 1
            rainy_run = 0
            if sunny_run >= 3:
                yield "Uh oh! We're getting thirsty!"
        else:
            #Check for runs of three or more rainy days
            rainy_run += 1
            sunny_run = 0
            if rainy_run >= 3:
                yield "Rain, rain go away!"
    return

#Create a generator object and print its series of messages
for msg in weathermaker(0.2, 10):
    print(msg) 
```

**输出**

```
$ python weathermaker.py
today it is sunny
today it is sunny
Uh oh! We're getting thirsty!
today it is sunny
Uh oh! We're getting thirsty!
today it is sunny
Uh oh! We're getting thirsty!
today it is rainy
today it is sunny
today it is rainy
today it is rainy
today it is rainy
Rain, rain go away!
today it is rainy
Rain, rain go away!
```

从上面的代码可以清楚地看出 [Python 生成器](https://wiki.python.org/moin/Generators)是快速创建迭代器的好方法。它们有很多好处，它们为每个句子一次分配一个内存。它们也让开发人员更容易修改代码而不会出错。

生成器提供的另一个好处是[封装](https://pythonspot.com/encapsulation/)，它提供了新的有用的方法来打包和隔离内部代码依赖。这就是为什么您可以在 for 循环中使用生成器。

**您可以在一个生成器中添加多个收益报表**

```
def nums3():
   n = 0

   while n < 6:

  yield n
       n += 1
   yield 63 # Second yield
for num in nums3():
   print(num
```

**输出**

```
0
1
2
3
63
```

**上述代码的解释**

这里，在 whileloop 退出后，第二个产出完成。当函数最后到达隐式返回时，迭代停止。

## 最后的话

因此，如果您还没有在 python 代码中使用生成器，请学习使用它。我知道你会很高兴你这么做了。它们是 Python 编码的核心部分，对您在 Python 上的下一个应用程序开发非常有用。

毫无疑问，Python 是一种非常有用的、多样的、维护良好的语言，并且没有任何绑定到特性上。然而，我分享了我在日常编码过程中使用的想法，以使事情变得简单。

**ValueCoders 是一家经验丰富的[软件开发公司](https://www.valuecoders.com/)。如果您需要 Python 开发服务，请随时[联系](https://www.valuecoders.com/contact) ****。******