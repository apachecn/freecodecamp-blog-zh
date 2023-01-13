# python JSON——如何将字符串转换成 JSON

> 原文：<https://www.freecodecamp.org/news/python-json-how-to-convert-a-string-to-json/>

在本教程中，您将学习 JSON 的基础知识——它是什么，它最常用在哪里，以及它的语法。

您还将看到如何在 Python 中将字符串转换成 JSON。

我们开始吧！

## JSON 是什么？

JSON 代表 JavaScript 对象符号。

这是一种用于为 web 应用程序存储和传输信息的数据格式。

JSON 的灵感来自 JavaScript 编程语言，但它并不局限于一种语言。

大多数现代编程语言都有用于解析和生成 JSON 数据的库。

### JSON 用在哪里？

JSON 主要用于在服务器和客户端之间发送和接收数据，其中客户端是网页或 web 应用程序。

在 web 应用程序通过网络连接时使用的请求-响应周期中，这是一种更可靠的格式。这与复杂且不太紧凑的 XML 形成对比，后者是多年前的首选格式。

### 基本 JSON 语法

在 JSON 中，数据以键值对的形式写入，如下所示:

```
"first_name": "Katie" 
```

数据用双引号括起来，键值对用冒号分隔。

可以有多个键-值对，每个键-值对用逗号分隔:

```
"first_name": "Katie", "last_name": "Rodgers" 
```

上面的例子显示了一个*对象*，它是多个键值对的集合。

对象在花括号内:

```
{
    "first_name": "Katie",  
    "last_name": "Rodgers"
} 
```

还可以用 JSON 创建数组，即有序的值列表。在这种情况下，数组包含在方括号中:

```
[
  { 

    "first_name": "Katie",  
    "last_name": "Rodgers"
  },

  { 

    "first_name": "Naomi",  
    "last_name": "Green"
  },
]

// or:

{
 "employee": [
     { 
    "first_name": "Katie",  
    "last_name": "Rodgers"
  },

  { 
    "first_name": "Naomi",  
    "last_name": "Green"
  },
 ]
}

//this created an 'employee' object that has 2 records.
// It defines the first name and last name of an employee 
```

## 如何在 Python 中使用 JSON 数据

### 包含 Python 的 JSON 模块

要在 Python 中使用 JSON，首先需要在 Python 文件的顶部包含 JSON 模块。这是 Python 内置的，是标准库的一部分。

所以，假设你有一个名为`demo.py`的文件。在顶部，您应该添加以下行:

```
import json 
```

### 使用`json.loads()`功能

如果程序中有 JSON 字符串数据，如下所示:

```
#include json library
import json

#json string data
employee_string = '{"first_name": "Michael", "last_name": "Rodgers", "department": "Marketing"}'

#check data type with type() method
print(type(employee_string))

#output
#<class 'str'> 
```

你可以用 Python 中的`json.loads()`函数把它变成 JSON。

`json.loads()`函数接受有效的字符串作为输入，并将其转换成 Python 字典。

这个过程被称为*反序列化*——将字符串转换为对象的行为。

```
#include json library
import json

#json string data
employee_string = '{"first_name": "Michael", "last_name": "Rodgers", "department": "Marketing"}'

#check data type with type() method
print(type(employee_string))

#convert string to  object
json_object = json.loads(employee_string)

#check new data type
print(type(json_object))

#output
#<class 'dict'> 
```

然后，您可以像使用 Python 字典一样访问每个单独的项目:

```
#include json library
import json

#json string data
employee_string = '{"first_name": "Michael", "last_name": "Rodgers", "department": "Marketing"}'

#check data type with type() method
print(type(employee_string))

#convert string to  object
json_object = json.loads(employee_string)

#check new data type
print(type(json_object))

#output
#<class 'dict'>

#access first_name in dictionary
print(json_object["first_name"])

#output
#Michael 
```

我们再举一个例子:

1.  取一些 JSON 字符串数据:

```
import json

#json string
employees_string = '''
{
    "employees": [
       {
           "first_name": "Michael", 
           "last_name": "Rodgers", 
           "department": "Marketing"
        },
       {
           "first_name": "Michelle", 
           "last_name": "Williams", 
           "department": "Engineering"
        }
    ]
}
'''

#check data type using the type() method
print(type(employees_string))

#output
#<class 'str'> 
```

2.  使用`json.loads()`函数将字符串转换为对象:

```
import json

emoloyees_string = '''
{
    "employees" : [
       {
           "first_name": "Michael", 
           "last_name": "Rodgers", 
           "department": "Marketing"
        },
       {
           "first_name": "Michelle", 
           "last_name": "Williams", 
           "department": "Engineering"
        }
    ]
}
'''

data = json.loads(employees_string)

print(type(data))
#output
#<class 'dict'> 
```

3.  访问数据:

```
import json

employees_string = '''
{
    "employees" : [
       {
           "first_name": "Michael", 
           "last_name": "Rodgers", 
           "department": "Marketing"

        },
       {
           "first_name": "Michelle", 
           "last_name": "Williams", 
           "department": "Engineering"
        }
    ]
}
'''

data = json.loads(employees_string)

print(type(data))
#output
#<class 'dict'>

#access first_name
for employee in data["employees"]: 
    print(employee["first_name"])

#output
#Michael
#Michelle 
```

## 结论

现在，您已经知道了在 Python 中使用 JSON 的基础。

如果你想了解更多关于 Python 的知识，freeCodeCamp 有一个 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)，它会带你从变量、循环、函数等基础知识到数据结构等更高级的概念。最后，您还将构建 5 个项目。

感谢阅读，快乐学习！