# 用 Python 加载 JSON 文件——如何读取和解析 JSON

> 原文：<https://www.freecodecamp.org/news/loading-a-json-file-in-python-how-to-read-and-parse-json/>

在本文中，您将学习如何用 Python 读取和解析 JSON。

## JSON 是什么？

JSON 是 JavaScript 对象符号的缩写。这是一种以名称-值对存储数据的简单语法。只要有效，值可以是不同的数据类型。JSON 不可接受的类型包括函数、日期和`undefined`。

JSON 文件以有效 JSON 结构的`.json`扩展名存储。

JSON 文件的结构如下所示:

```
{
  "name": "John",
  "age": 50,
  "is_married": false,
  "profession": null,
  "hobbies": ["traveling", "photography"]
} 
```

在 web 应用程序中，您将经常使用 JSON 发送和接收来自服务器的数据。

当收到数据时，程序读取并解析 JSON 以提取特定的数据。不同的语言有自己的方法来做到这一点。我们将在这里看看如何用 Python 来做这些。

## 如何读取 JSON 文件

假设上面代码块中的 JSON 存储在一个`user.json`文件中。使用 Python 中的`open()`内置函数，我们可以读取该文件并将内容赋给一个变量。方法如下:

```
with open('user.json') as user_file:
  file_contents = user_file.read()

print(file_contents)
# {
#   "name": "John",
#   "age": 50,
#   "is_married": false,
#   "profession": null,
#   "hobbies": ["travelling", "photography"]
# } 
```

您将文件路径传递给`open`方法，该方法打开文件并将文件中的流数据分配给`user_file`变量。使用`read`方法，您可以将文件的文本内容传递给`file_contents`变量。

我在表达式的开头使用了`with`,这样在读取文件内容后，Python 可以关闭文件。

现在包含了 JSON 的一个字符串版本。下一步，您现在可以解析 JSON 了。

## 如何解析 JSON

Python 内置了用于各种操作的模块。对于管理 JSON 文件，Python 有`json`模块。

这个模块有很多方法。其中之一是用于解析 JSON 字符串的`loads()`方法。然后，您可以将解析后的数据赋给一个变量，如下所示:

```
import json

with open('user.json') as user_file:
  file_contents = user_file.read()

print(file_contents)

parsed_json = json.loads(file_contents)
# {
#   'name': 'John',
#   'age': 50,
#   'is_married': False,
#   'profession': None,
#   'hobbies': ['travelling', 'photography']
# } 
```

使用`loads()`方法，您可以看到`parsed_json`变量现在有了一个有效的字典。从这个字典中，您可以访问其中的键和值。

还要注意 JSON 中的`null`是如何转换成 python 中的`None`的。这是因为`null`在`Python`中无效。

## 如何使用`json.load()`读取和解析 JSON 文件

`json`模块也有`load`方法，您可以用它来读取文件对象并同时解析它。使用此方法，您可以将前面的代码更新为:

```
import json

with open('user.json') as user_file:
  parsed_json = json.load(user_file)

print(parsed_json)
# {
#   'name': 'John',
#   'age': 50,
#   'is_married': False,
#   'profession': None,
#   'hobbies': ['travelling', 'photography']
# } 
```

不用使用 file 对象的`read`方法和`json`模块的`loads`方法，可以直接使用读取和解析 file 对象的`load`方法。

## 包扎

JSON 数据以其简单的结构而闻名，并且在服务器和客户机之间的信息交换中很流行(大多数情况下是一个标准)。

不同的语言和技术可以用不同的方式读取和解析 JSON 文件。在本文中，我们学习了如何使用 file 对象的`read`方法和`json`模块的`loads`和`load`方法读取 JSON 文件并解析这些文件。