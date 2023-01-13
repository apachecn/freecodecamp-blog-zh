# 如何使用 Pytype 快速找到 Python 代码中的类型问题

> 原文：<https://www.freecodecamp.org/news/how-to-quickly-find-type-issues-in-your-python-code-with-pytype-c022782f61c3/>

**TL；DR —** 如果你正在从事一个大型的 Python 项目，或者只是想保持你的代码库整洁， [Pytype](https://github.com/google/pytype) 就是你的工具。

Python 是一种很好的原型和脚本编程语言。简洁的语法、灵活的类型系统和解释的性质允许我们快速尝试一个想法，调整它，并再次尝试。

当 Python 项目**增长**时，曾经是速度推动者的灵活性变成了开发速度的负担。随着更多的开发人员加入项目，编写了更多的代码，类型信息的缺乏使得阅读和理解代码变得更加困难。没有类型检查系统，错误很容易犯，也很难被发现。

![python](img/fa5cd32ec3fe71648e932debf2de62bf.png)

CC BY 2.5, [https://commons.wikimedia.org/w/index.php?curid=648709](https://commons.wikimedia.org/w/index.php?curid=648709)

[**Pytype**](https://github.com/google/pytype) 来救援了！Pytype 是 Python 中用于类型检查和类型推断的开源工具。它开箱即用，只需安装并运行即可！

Pytype 将…

1.  静态推断类型信息并检查代码中的类型错误。
2.  验证代码中 PEP 484 类型注释的一致性。
3.  将推断出的类型信息合并回代码中，如果需要的话，可以使用。

如果你被说服了，请访问 [Pytype](https://github.com/google/pytype) 获取安装和使用说明。下面我给出一些很酷的用法示例！

### 示例 1:类型推断和检查

这是最常见的情况。你写了一些代码，并想检查你没有犯任何错误。考虑这个函数:

```
import re

def GetUsername(email_address):
  match = re.match(r'([^@]+)@example\.com', email_address)
  return match.group(1)
```

很简单。它使用正则表达式提取@之前的电子邮件地址部分，并返回它。你注意到这个 bug 了吗？

让我们看看当我们使用`pytype`来检查它时会发生什么:

```
% pytype get_username.py
Analyzing 1 sources with 0 dependencies
File "/.../get_username.py", 
line 5, in GetUsername: No attribute 'group' on None [attribute-error]
  In Optional[Match[str]]
```

Pytype 告诉我们`group`不是对`match`的有效函数调用。哦！当没有找到匹配时，`re.match()`返回`None`。的确，在这些情况下`match.group(1)`会抛出一个异常。

让我们修复这个错误，让函数为无效的电子邮件地址返回 None:

```
import re
def GetUsername(email_address):
  match = re.match(r'([^@]+)@example\.com', email_address)

if match is None:
    return None
  return match.group(1)  # <-- Here, match can't be None
```

现在，当我们重新运行`pytype`时，错误消失了。Pytype 推断如果执行了 **if** 之后的代码，match 保证不会是`None`。

### 示例 2:类型注释的验证

在 Python 3 中，你可以对你的代码进行类型注释( [PEP 484](https://www.python.org/dev/peps/pep-0484) )，以帮助类型检查工具**和其他开发者**理解你的意图。当您的类型注释有错误时，Pytype 能够发出警告:

```
import re
from typing import Match

def GetEmailMatch(email) -> Match:
  return re.match(r'([^@]+)@example\.com', email)
```

让我们使用`pytype`来检查这段代码:

```
% pytype example.py
Analyzing 1 sources with 0 dependencies
File "/.../example.py", line 5, in GetEmailMatch: 
bad option in return type [bad-return-type]
  Expected: Match
  Actually returned: None
```

Pytype 告诉我们`GetEmailMatch`可能会返回`None`，但是我们将其返回类型标注为`Match`。为了解决这个问题，我们可以使用类型模块中的`Optional`类型注释:

```
import re
from typing import Match, Optional

def GetEmailMatch(email) -> Optional[Match]:
  return re.match(r'([^@]+)@example\.com', email)
```

`Optional`表示返回值可以是一个`Match`对象或者是`None`。

### 示例#3:合并回推断的类型信息

为了帮助您采用类型注释，Pytype 可以为您将它们添加到代码中。让我们来看看这段代码:

```
import re

def GetEmailMatch(email):
  return re.match(r'([^@]+)@example\.com', email)

def GetUsername(email_address):
  match = GetEmailMatch(email_address)
  if match is None:
    return None
  return match.group(1)
```

为了给这段代码添加类型注释，我们首先在文件上运行`pytype`。`pytype`将推断出的类型信息保存到`.pyi`文件中。然后，我们可以运行`merge-pyi`将类型注释合并回代码中:

```
% pytype email.py
% merge-pyi -i email.py pytype_output/email.pyi
```

瞧！

```
import re
from typing import Match
from typing import Optional

def GetEmailMatch(email) -> Optional[Match[str]]:
  return re.match(r'([^@]+)@example\.com', email)

def GetUsername(email_address) -> Optional[str]:
  match = GetEmailMatch(email_address)
  if match is None:
    return None
  return match.group(1)
```

类型注释，包括`import`语句，现在在源文件中。

更多使用示例和安装说明，请访问 GitHub 上的 [Pytype。](https://github.com/google/pytype)

感谢阅读！