# Python if _ _ name _ _ = = _ _ main _ _ 用代码示例解释

> 原文：<https://www.freecodecamp.org/news/if-name-main-python-example/>

当 Python 解释器读取 Python 文件时，它首先设置几个特殊变量。然后它执行文件中的代码。

其中一个变量叫做`__name__`。

如果你一步一步地跟随这篇文章并阅读它的代码片段，你将学会如何使用`if __name__ == "__main__"`，以及为什么它如此重要。

## Python 模块解释

Python 文件被称为模块，由文件扩展名`.py`标识。模块可以定义函数、类和变量。

所以当解释器运行一个模块时，如果正在运行的模块是主程序，`__name__`变量将被设置为`__main__`。

但是如果代码从另一个模块导入该模块，那么`__name__`变量将被设置为该模块的名称。

让我们看一个例子。创建一个名为`file_one.py`的 Python 模块，并将顶层代码粘贴到其中:

```
# Python file one module

print("File one __name__ is set to: {}" .format(__name__))
```

file_one.py

通过运行该文件，您将会看到我们正在讨论的内容。该模块的变量`__name__`被设置为`__main__`:

```
File one __name__ is set to: __main__
```

现在添加另一个名为`file_two.py`的文件，并将以下代码粘贴到其中:

```
# Python module to import

print("File two __name__ is set to: {}" .format(__name__)) 
```

file_two.py

此外，像这样修改`file_one.py`中的代码，以便我们导入`file_two`模块:

```
# Python module to execute
import file_two

print("File one __name__ is set to: {}" .format(__name__)) 
```

file_one.py

再次运行我们的`file_one`代码将显示`file_one`中的`__name__`变量没有改变，仍然保持设置为`__main__`。但是现在`file_two`中的变量`__name__`被设置为它的模块名，因此有了`file_two`。

结果应该是这样的:

```
File two __name__ is set to: file_two
File one __name__ is set to: __main__ 
```

但是直接运行`file_two`你会看到它的名字被设置为`__main__`:

```
File two __name__ is set to: __main__ 
```

正在运行的文件/模块的变量`__name__`将始终为`__main__`。但是所有其他正在被导入的模块的`__name__`变量将被设置为它们模块的名称。

## Python 文件命名约定

使用`__name__`和`__main__`的通常方式如下:

```
if __name__ == "__main__":
   Do something here 
```

让我们看看这在现实生活中是如何工作的，以及如何实际使用这些变量。

修改`file_one`和`file_two`如下所示:

`file_one`:

```
# Python module to execute
import file_two

print("File one __name__ is set to: {}" .format(__name__))

if __name__ == "__main__":
   print("File one executed when ran directly")
else:
   print("File one executed when imported") 
```

file_one.py

`file_two`:

```
# Python module to import

print("File two __name__ is set to: {}" .format(__name__))

if __name__ == "__main__":
   print("File two executed when ran directly")
else:
   print("File two executed when imported") 
```

file_two.py

同样，当运行`file_one`时，你会看到程序识别出这两个模块中的哪一个是`__main__`，并根据我们的第一个`if else`语句执行代码。

结果应该是这样的:

```
File two __name__ is set to: file_two
File two executed when imported
File one __name__ is set to: __main__
File one executed when ran directly 
```

现在运行`file_two`，你会看到`__name__`变量被设置为`__main__`:

```
File two __name__ is set to: __main__
File two executed when ran directly 
```

当像这样的模块被导入和运行时，它们的函数将被导入，顶层代码将被执行。

要查看这个过程的运行情况，请修改您的文件，如下所示:

`file_one`:

```
# Python module to execute
import file_two

print("File one __name__ is set to: {}" .format(__name__))

def function_one():
   print("Function one is executed")

def function_two():
   print("Function two is executed")

if __name__ == "__main__":
   print("File one executed when ran directly")
else:
   print("File one executed when imported") 
```

file_one.py

`file_two`:

```
# Python module to import

print("File two __name__ is set to: {}" .format(__name__))

def function_three():
   print("Function three is executed")

if __name__ == "__main__":
   print("File two executed when ran directly")
else:
   print("File two executed when imported") 
```

现在函数已经加载，但还没有运行。

要运行这些函数之一，请修改`file_one`的`if __name__ == "__main__"`部分，如下所示:

```
if __name__ == "__main__":
   print("File one executed when ran directly")
   function_two()
else:
   print("File one executed when imported") 
```

运行`file_one`时你应该看到应该是这样的:

```
File two __name__ is set to: file_two
File two executed when imported
File one __name__ is set to: __main__
File one executed when ran directly
Function two is executed 
```

此外，您可以从导入的文件中运行函数。为此，修改`file_one`的`if __name__ == “__main__”`部分，如下所示:

```
if __name__ == "__main__":
   print("File one executed when ran directly")
   function_two()
   file_two.function_three()
else:
   print("File one executed when imported") 
```

你可以期待这样的结果:

```
File two __name__ is set to: file_two
File two executed when imported
File one __name__ is set to: __main__
File one executed when ran directly
Function two is executed
Function three is executed 
```

现在让我们假设`file_two`模块非常大，有很多函数(在我们的例子中是两个),并且您不想导入所有的函数。修改`file_two`,如下所示:

```
# Python module to import

print("File two __name__ is set to: {}" .format(__name__))

def function_three():
   print("Function three is executed")

def function_four():
   print("Function four is executed")

if __name__ == "__main__":
   print("File two executed when ran directly")
else:
   print("File two executed when imported") 
```

file_two.py

为了从模块中导入特定的函数，使用`file_one`文件中的`from`导入块:

```
# Python module to execute
from file_two import function_three

print("File one __name__ is set to: {}" .format(__name__))

def function_one():
   print("Function one is executed")

def function_two():
   print("Function two is executed")

if __name__ == "__main__":
   print("File one executed when ran directly")
   function_two()
   function_three()
else:
   print("File one executed when imported")
```

file_one.py

## 结论

无论您想要一个可以作为主程序运行或由其他模块导入的文件，`__name__`变量都有一个非常好的用例。我们可以使用一个`if __name__ == "__main__"`块来允许或阻止部分代码在模块导入时运行。

当 Python 解释器读取一个文件时，如果模块正在运行，则`__name__`变量被设置为`__main__`,如果是导入的，则被设置为模块的名称。读取文件会执行所有顶级代码，但不会执行函数和类(因为它们只会被导入)。

Bra gjort！(那是瑞典语“干得好”的意思！)

在我的 [freeCodeCamp 个人资料](https://www.freecodecamp.org/news/author/goran/)、 [Medium 个人资料](https://medium.com/@goranaviani)上查看更多类似的文章，以及我在 [GitHub 页面](https://github.com/GoranAviani)上创建的其他有趣的东西。