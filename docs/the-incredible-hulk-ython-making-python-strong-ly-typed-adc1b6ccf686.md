# 为什么应该在 Python 中试验类型检查

> 原文：<https://www.freecodecamp.org/news/the-incredible-hulk-ython-making-python-strong-ly-typed-adc1b6ccf686/>

Python 是一种很棒的动态类型语言。但是相当多的人认为这是它最大的缺点。

### 但是为什么呢？

动态类型语言消除了编写“普通”类型声明的麻烦。这使得写作更愉快，速度也更快。语言的运行时环境处理动态类型语言。

这意味着一些本来可以在引入后立即消除的错误，现在将保持沉默，直到代码被调用。你知道什么时候会发生，对吗？

![UaDW4hJ1lDelQm2C0Qish4GrS1fhZPaZ2LWd](img/21526a63b0d74e79ea13e3b94f838237.png)

### 处理类型

从 3.5 版本开始，Python 通过[类型化](https://docs.python.org/3/library/typing.html)模块添加了**可选的**类型提示支持。

看起来他们愿意为双方妥协。一方面，喜欢动态类型自由的人可以继续忽略类型提示。另一方面，喜欢静态类型安全性的人可以从利用新功能中受益。

### 如何使用它

使用它的方法，或者至少是开始使用它的方法，非常简单明了。如果你熟悉的话，它看起来很像静态类型的 TypeScript [方式](https://www.typescriptlang.org/docs/handbook/basic-types.html)。这里有一个例子:

```
# Typing is the core module that supports type checking.
# In here we import List, which provided equivalent functionality to
# the list() function or the [] equivalent shorthand
from typing import List
# We define a function, as usually but we add the expected
# type to the args and we add a return type too
def find_files_of_type(type: str, files_types: List[str]) -> bool:
return (type in files_types)
files_types: List[str] = [‘ppt’, ‘vcf’, ‘png’]
type_to_search: str = ‘ppt’
print(‘Found files of type {} in list? {}’.format(type_to_search,
find_files_of_type(type_to_search, files_types)))
```

有点尴尬，但还是清楚的，对吧？:)

### 潜在的陷阱

你可能已经注意到我在前面几行提到了“可选”这个词。因此，在撰写本文时，还没有强制类型检查。

你可以把任何不相关的类型添加到你的变量中。对他们做最无效、最不相关、最“变态”的操作，Python 却连眼睛都不会眨一下。

如果你想**执行**类型检查，你应该使用像伟大的 [mypy](http://mypy-lang.org/examples.html) 这样的类型检查器。

当然，大多数 ide 都有一些类型检查的功能。这里的是 Pycharm 的相关文档。

### 认为我将来想看到的

*   在语言的核心中集成类型检查机制
*   由于上述原因，更无缝的类型提示。例如，如果类型检查是打开的，那么我就不应该使用类`List`或`Tuple`来做这件事。这些`[]`和`()`人手应该够了

### 结论

感谢您阅读这篇文章。这绝不是 Python 强大功能的扩展指南。相反，我希望这是一个引导更多研究的引子。

如果您在 Python 3.5+中开始一个新项目，我建议您尝试一下类型检查。我很乐意看到你对这个功能的建议和想法，所以请随意发表评论。

最初发表于[我的博客](http://perigk.github.io)。