# Python Return 语句解释:它们是什么以及为什么使用它们

> 原文：<https://www.freecodecamp.org/news/python-return-statements-explained-what-they-are-and-why-you-use-them/>

所有函数在被调用时都返回值。

如果 return 语句后跟表达式列表，则计算该表达式列表并返回值:

```
>>> def greater_than_1(n):
...     return n > 1
...
>>> print(greater_than_1(1))
False
>>> print(greater_than_1(2))
True
```

如果没有指定表达式列表，则返回`None`:

```
>>> def no_expression_list():
...     return    # No return expression list.
...
>>> print(no_expression_list())
None
```

如果在函数执行期间到达 return 语句，则当前函数调用将停留在该点:

```
>>> def return_middle():
...     a = 1
...     return a
...     a = 2     # This assignment is never reached.
...
>>> print(return_middle())
1
```

如果没有 return 语句，则函数在到达末尾时不返回任何内容:

```
>>> def no_return():
...     pass     # No return statement.
...
>>> print(no_return())
None 
```

一个函数可以有多个`return`语句。当到达这些`return`语句之一时，函数的执行结束:

```
 >>> def multiple_returns(n):
 ...    if(n):
 ...        return "First Return Statement"
 ...    else:
 ...        return "Second Return Statement"
 ...
 >>> print(multiple_returns(True))
 First Return Statement
 >>> print(multiple_returns(False))
 Second Return Statement 
```

单个函数可以返回各种类型:

```
 >>> def various_return_types(n):
 ...     if(n==1):
 ...         return "Hello World."   # Return a string
 ...     elif(n==2):
 ...         return 42               # Return a value
 ...     else:
 ...         return True             # Return a boolean
 ... 
 >>> print(various_return_types(1))
 Hello World.
 >>> print(various_return_types(2))
 42
 >>> print(various_return_types(3))
 True
```

甚至有可能让一个函数只返回一个值就返回多个值:

```
 >>> def return_two_values():
 ...     a = 40
 ...     b = 2
 ...     return a,b
 ...
 >>> print("First value = %d,  Second value = %d" %(return_two_values()))
 First value = 40,  Second value = 2
```

更多信息见 [Python 文档](https://docs.python.org/3/reference/simple_stmts.html#the-return-statement)。