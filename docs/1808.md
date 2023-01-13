# 如何用 4 种不同的方式在 Python 中展平字典

> 原文：<https://www.freecodecamp.org/news/how-to-flatten-a-dictionary-in-python-in-4-different-ways/>

在这篇文章中，我们将看看 Python 中 4 种不同的扁平化字典的方法。对于每种方法，我将指出其优缺点，并给出一个快速的性能分析。对于本教程，我在 Python 3.7 上运行了所有示例。

## 为什么要知道如何在 Python 中展平 Dict？

你需要一本扁平的字典有很多原因。一个是它使得比较两个字典变得更简单。另一个原因是它更容易导航和操作，因为扁平结构只有一层深。

Python 是一种通用的语言，这意味着您可以通过几种方式实现相同的目标。为问题选择最佳解决方案需要权衡一个解决方案相对于另一个解决方案的优势。

这篇文章的目的是为你提供这个问题的许多选择，并给你尽可能多的数据，以便你能做出明智的决定。所以，我们走吧。

PS:如果你没有 Python 3.7 你可以用`pyenv`安装，甚至可以同时有[多个版本](https://miguendes.me/how-i-set-up-my-python-workspace)而不冲突。

## 目录

## 如何使用自己的递归函数在 Python 中展平 Dict

快速浏览一下谷歌，我们就会看到 stackoverflow。[的第一个答案](https://stackoverflow.com/a/6027615)显示了一个递归函数，它遍历字典并返回一个扁平化的实例。我将从该函数中汲取灵感，并展示一个略有改进的版本。

我们可以从类型提示开始，以提高可读性并使其类型安全。

```
from collections.abc import MutableMapping

def flatten_dict(d: MutableMapping, parent_key: str = '', sep: str ='.') -> MutableMapping:
    items = []
    for k, v in d.items():
        new_key = parent_key + sep + k if parent_key else k
        if isinstance(v, MutableMapping):
            items.extend(flatten_dict(v, new_key, sep=sep).items())
        else:
            items.append((new_key, v))
    return dict(items)

>>> flatten_dict({'a': 1, 'c': {'a': 2, 'b': {'x': 3, 'y': 4, 'z': 5}}, 'd': [6, 7, 8]})
{'a': 1, 'c.a': 2, 'c.b.x': 3, 'c.b.y': 4, 'c.b.z': 5, 'd': [6, 7, 8]} 
```

### 性能基准

我们已经快速验证了该函数返回一个 flat dict，但是它的性能如何呢？适合生产使用吗？让我们运行一个快速基准测试，看看它有多快。

对于这个以及本文中的所有基准，我将使用`IPython`的`timeit`魔法函数和来自 [`memory_profiler`](https://pypi.org/project/memory-profiler/) 库中的`memit`。

PS:要让`%memit`工作，需要先运行`%load_ext memory_profiler`。

```
In [4]: %timeit flatten_dict({'a': 1, 'c': {'a': 2, 'b': {'x': 3, 'y': 4, 'z': 5}}, 'd': [6, 7, 8]})
7.28 µs ± 54.6 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

In [5]: %load_ext memory_profiler

In [6]: %memit flatten_dict({'a': 1, 'c': {'a': 2, 'b': {'x': 3, 'y': 4, 'z': 5}}, 'd': [6, 7, 8]})
peak memory: 84.94 MiB, increment: 0.29 MiB 
```

**优点:**简单易懂，而且很管用。

**缺点:**它将条目存储在内存中的一个列表中，然后传递给`dict`构造函数。这不仅浪费了内存，也浪费了速度。

尽管在 Python 中向列表添加元素很快，但实际上并不需要重复这样做。在下一节中，我们将看到如何使用生成器来改进这一点。

## 如何使用你自己的递归函数+生成器来展平 Python 中的 Dict

第一个版本是可行的，而且速度有点快。然而，它有一个问题。

为了创建一个带有扁平键的新字典，它在内存中维护一个 Python `list`。这是低效的，因为我们必须将整个数据结构保存在内存中，仅仅作为临时存储。

一个更好的解决方案是使用 [Python 的生成器](https://docs.python.org/3/glossary.html#term-generator)，它是一个可以暂停执行并记住状态的对象，稍后可以恢复。通过使用生成器，我们可以在不改变行为的情况下摆脱临时列表。

```
from collections.abc import MutableMapping

def _flatten_dict_gen(d, parent_key, sep):
    for k, v in d.items():
        new_key = parent_key + sep + k if parent_key else k
        if isinstance(v, MutableMapping):
            yield from flatten_dict(v, new_key, sep=sep).items()
        else:
            yield new_key, v

def flatten_dict(d: MutableMapping, parent_key: str = '', sep: str = '.'):
    return dict(_flatten_dict_gen(d, parent_key, sep))

>>> flatten_dict({'a': 1, 'c': {'a': 2, 'b': {'x': 3, 'y': 4, 'z': 5}}, 'd': [6, 7, 8]})
{'a': 1, 'c.a': 2, 'c.b.x': 3, 'c.b.y': 4, 'c.b.z': 5, 'd': [6, 7, 8]} 
```

### 性能基准

```
In [9]: %timeit flatten_dict({'a': 1, 'c': {'a': 2, 'b': {'x': 3, 'y': 4, 'z': 5}}, 'd': [6, 7, 8]})
7.39 µs ± 78.7 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

In [7]: %memit flatten_dict({'a': 1, 'c': {'a': 2, 'b': {'x': 3, 'y': 4, 'z': 5}}, 'd': [6, 7, 8]})
peak memory: 45.27 MiB, increment: 0.25 MiB 
```

**优点:**简单易懂，工作原理和之前版本一样，内存高效。这个版本比使用列表的版本少消耗大约 50%的内存。

**缺点:**它可能不能很好地处理边缘情况。例如，如果我们传递一个类似字典的对象，它不是`MutableMapping`的实例，那么这个例子将失败。但是这也是以前版本的一个缺点。

## 如何使用熊猫来展平 Python 中的字典`json_normalize`

正如我们所看到的，前面的解决方案工作得很好，但是为这样一个常见的问题编写我们自己的解决方案是在重新发明轮子。作为替代，我们可以使用流行的数据操作库，如 [`pandas`](https://pandas.pydata.org) 。

`pandas`带有一个[通用函数来规范化 JSON 对象](https://pandas.pydata.org/docs/user_guide/io.html?highlight=json_normalize#normalization)，这些对象在 Python 中被表示为一个字典。这是一个绝好的机会，让我们不用重新创建现有的解决方案，而是使用一个更强大的解决方案。

此外，最终结果看起来很棒，只需一行代码，我们甚至可以将它隐藏在一个薄薄的界面后面。

```
from collections.abc import MutableMapping
import pandas as pd

def flatten_dict(d: MutableMapping, sep: str= '.') -> MutableMapping:
    [flat_dict] = pd.json_normalize(d, sep=sep).to_dict(orient='records')
    return flat_dict

>>> flatten_dict({'a': 1, 'c': {'a': 2, 'b': {'x': 3, 'y': 4, 'z': 5}}, 'd': [6, 7, 8]})
{'a': 1, 'd': [6, 7, 8], 'c.a': 2, 'c.b.x': 3, 'c.b.y': 4, 'c.b.z': 5} 
```

### 性能基准

```
In [5]: %timeit flatten_dict({'a': 1, 'c': {'a': 2, 'b': {'x': 3, 'y': 4, 'z': 5}}, 'd': [6, 7, 8]})
779 µs ± 10.7 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)

In [6]: %memit flatten_dict({'a': 1, 'c': {'a': 2, 'b': {'x': 3, 'y': 4, 'z': 5}}, 'd': [6, 7, 8]})
peak memory: 86.30 MiB, increment: 0.90 MiB 
```

**优点:**容易理解，我们重用了一个完善的库。

**反对:**仅仅使用`pandas`来展平一个字典似乎有些矫枉过正。如果你的项目不需要，那么我们可以用一个更轻量级的库比如`FlatDict`。此外，根据`timeit`的说法，这个版本比使用我们自己的解决方案慢了**100 倍**，这并不好。

## 如何使用`flatdict`库在 Python 中展平 Dict

[`flatdict`](https://flatdict.readthedocs.io/en/stable/index.html) 是一个 Python 库，从嵌套的字典创建一个单级字典，从 Python 3.5 开始可用。

到目前为止，我们已经看到，编写我们的定制解决方案可能并不理想，仅仅为了这个目的而使用像`pandas`这样成熟的库也不好。

作为一种替代，我们可以使用`flatdict`，它更加轻便，并且经过了实战考验。

该库非常通用，也使我们能够使用自定义分隔符。然而，它提供的最好特性之一是能够像以前一样访问新创建的字典——也就是说，您可以使用新的键或旧的键来访问值。

让我们看一个例子。

```
>>> import flatdict
>>> d =  flatdict.FlatDict(data, delimiter='.')

# d is a FlatDict instance
>>> d
<FlatDict id=140665244199904 {'a': 1, 'c.a': 2, 'c.b.x': 3, 'c.b.y': 4, 'c.b.z': 5, 'd': [6, 7, 8]}>"

# and it allows accessing flat keys
>>> d['c.b.y']
4

# but also nested ones
>>> d['c']['b']['y']
4

# and can be converted to a flatten dict
>>> dict(d)
{'a': 1, 'c.a': 2, 'c.b.x': 3, 'c.b.y': 4, 'c.b.z': 5, 'd': [6, 7, 8]} 
```

如您所见，`flatdict`提供了极大的灵活性和便利性。

### 性能基准

```
In [3]: %timeit flatdict.FlatDict({'a': 1, 'c': {'a': 2, 'b': {'x': 3, 'y': 4, 'z': 5}}, 'd': [6, 7, 8]}, delimiter='.')
8.97 µs ± 21.6 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

In [4]: %memit flatdict.FlatDict({'a': 1, 'c': {'a': 2, 'b': {'x': 3, 'y': 4, 'z': 5}}, 'd': [6, 7, 8]}, delimiter='.')
peak memory: 45.21 MiB, increment: 0.14 MiB 
```

**优点:**很容易理解，而且很好用，是一个轻量级的库。允许以两种不同的方式访问嵌套元素。就像使用生成器的解决方案一样快速和高效。

缺点:它仍然是一个外部库，像许多开源工具一样，如果有一个 bug，你需要等待作者来修复它。有时作者会放弃他们的项目，这会给你的项目带来风险。不管怎样，我仍然认为在这件事上利大于弊。

## 结论

在这篇文章中，我们看到了 Python 中 4 种不同的字典展平方式。每个解决方案都有优点和缺点，选择最好的一个取决于个人品味和项目约束。

我希望你喜欢它，下次再见！