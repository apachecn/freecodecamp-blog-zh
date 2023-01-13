# Python 打印变量类型——如何获取变量类型

> 原文：<https://www.freecodecamp.org/news/python-print-type-of-variable-how-to-get-var-type/>

如果您是 Python 初学者，开始熟悉它的各种数据类型可能会感到困惑。毕竟，在语言中有很多这样的例子。

在本文中，我将向您展示如何获得 Python 中各种数据结构的类型，方法是将它们赋给一个变量，然后用`print()`函数将类型打印到控制台。

## 如何在 Python 中打印变量的类型

要获得 Python 中变量的类型，可以使用内置的`type()`函数。

基本语法如下所示:

```
type(variableName) 
```

在 Python 中，一切都是对象。因此，当您使用`type()`函数将存储在变量中的值的类型打印到控制台时，它会返回对象的类类型。

例如，如果类型是一个字符串，并且您对它使用了`type()`，那么您将得到`<class ‘string‘>`作为结果。

为了向您展示`type()`函数的作用，我将声明一些变量，并在 Python 中为它们分配各种数据类型。

```
name = "freeCodeCamp"

score = 99.99

lessons =  ["RWD", "JavaScript", "Databases", "Python"]

person = {
    "firstName": "John",
    "lastName": "Doe",
    "age": 28
}

langs = ("Python", "JavaScript", "Golang")

basics = {"HTML", "CSS", "JavaScript"} 
```

然后，我将通过在一些字符串和`type()`函数周围包装`print()`来将类型打印到控制台。

```
print("The variable, name is of type:", type(name))
print("The variable, score is of type:", type(score))
print("The variable, lessons is of type:", type(lessons))
print("The variable, person is of type:", type(person))
print("The variable, langs is of type:", type(langs))
print("The variable, basics is of type:", type(basics)) 
```

**以下是输出:**

```
# Outputs:
# The variable, name is of type:  <class 'str'>
# The variable, score is of type: <class 'float'>  
# The variable, lessons is of type:  <class 'list'>
# The variable, person is of type:  <class 'dict'> 
# The variable, langs is of type:  <class 'tuple'> 
# The variable, basics is of type:  <class 'set'> 
```

## 最后的想法

`type()`函数是 Python 中一个有价值的内置函数，使用它可以获得变量的数据类型。

如果您是初学者，您应该通过使用`type()`函数将变量的类型打印到控制台来省去填鸭式数据类型的麻烦。这会节省你一些时间。

还可以使用`type()`函数进行调试，因为在 Python 中，变量不是用数据类型声明的。因此，`type()`函数被内置到语言中，供您检查变量的数据类型。

感谢您的阅读。