# 如何入门 Python 中的 TinyDB

> 原文：<https://www.freecodecamp.org/news/get-started-with-tinydb-in-python/>

在处理个人项目时，您经常需要存储一些数据。您可以在服务器上使用 SQL 或 NoSQL 数据库，但这需要您做一些设置。

在本文中，我们将了解 TinyDB 以及如何使用它以 JSON 格式存储数据。

## TinyDB 是什么？

TinyDB 是一个纯 Python 编写的面向文档的数据库，没有外部依赖性。

通过提供一个简单干净的 API，它被设计得简单有趣。即使对初学者来说，学习和设置也很简单。

### 何时不使用 TinyDB

正如 TinyDB 文档中提到的，它并不总是您项目的正确选择。如果您需要高级功能，如:

*   从多个进程或线程访问，
*   为表创建索引，
*   一个 HTTP 服务器，
*   管理表或类似物之间的关系，
*   [酸担保](https://en.wikipedia.org/wiki/ACID)。

TinyDB 不适合您。在这些情况下，可以考虑使用类似于 [SQLite](https://www.sqlite.org/) 、 [Buzhug](https://buzhug.sourceforge.net/) 、 [CodernityDB](http://labs.codernity.com/codernitydb/) 或 [MongoDB](https://mongodb.org/) 的数据库。

## 如何安装 TinyDB

安装 TinyDB 极其容易。只需在您的终端中运行以下命令:

```
pip install tinydb
```

## 如何使用 TinyDB

考虑一个 Todo 应用程序的例子，我们只需要执行 CRUD 操作。现在我们已经安装了 TinyDB，让我们看看如何使用 TinyDB 存储我们的数据。TinyDB 使用 JSON 做所有的事情。

我们要做的第一件事就是从`tinydb`模块导入所需的类。因此，启动您的 Python shell 来编码吧。

```
$ python
Python 3.9.7 (tags/v3.9.7:1016ef3, Aug 30 2021, 20:19:38) [MSC v.1929 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

现在我们准备编码。

```
>>> from tinydb import TinyDB       
>>> db = TinyDB("todo_db.json")
>>> 
```

我们已经创建了一个 *TinyDB* 类的实例，并将文件名传递给它。这将创建一个 JSON 文件`todo_app.json`,我们的数据将存储在这里。

### 如何在 TinyDB 中插入数据

当我们处理 JSON 时，要添加的数据只是一个 [Python 字典](https://iread.ga/posts/84/everything-you-need-to-know-about-python-dictionaries)。那么，让我们看看如何插入一个项目:

```
>>> new_item = {"name": "Book", "quantity": 5}
>>> db.insert(new_item) 
1
>>> 
```

首先，我们创建了一个名为`new_item`的新字典，将`name`和`quantity`分别设置为`Book`和`5`。然后我们使用 *TinyDB* 类中的`insert()`方法将数据插入数据库。`insert()`方法返回新创建的对象的`id`。

在这个阶段，将创建一个名为`todo_db.json`的新 JSON 文件，并在插入数据之后。它看起来会像这样:

```
{"_default": {"1": {"name": "Book", "quantity": 5}}}
```

仔细一看，`_default`是表的名称(默认设置但可以更改)，`1`是新创建的对象的 id，它的值就是我们刚刚插入的数据。所以，基本上它只是一个嵌套的字典。

我们也可以像这样一次插入多个项目:

```
>>> items = [
...         {"name": "Cake", "quantity": 1},
...         {"name": "Candles", "quantity": 10},
...         {"name": "Balloons", "quantity": 50}
...     ]
>>> db.insert_multiple(items)   
[2, 3, 4]
```

在本例中，我们创建了一个名为`items`的字典列表，并使用`insert_multiple()`方法来插入条目。该方法还返回插入对象的`id`列表。

`todo_db.json`文件现在看起来像这样:

```
{
  "_default": {
    "1": { "name": "Book", "quantity": 5 },
    "2": { "name": "Cake", "quantity": 1 },
    "3": { "name": "Candles", "quantity": 10 },
    "4": { "name": "Balloons", "quantity": 50 }
  }
} 
```

### 如何从 TinyDB 中检索数据

有几种方法可以从数据库中检索数据。但是您需要首先创建一个*查询*类的实例。所以，让我们这样做:

```
>>> from tinydb import Query
>>> Todo = Query()
>>> 
```

您可以使用`db.search()`方法来检索数据。

```
>>> db.search(Todo.name == 'Book')
[{'name': 'Book', 'quantity': 5}]
>>> db.search(Todo.name == 'Copies') 
[]
>>>
```

`search()`方法返回与查询匹配的项目列表。如果没有这样的条目，它将返回一个空列表。

我们还可以使用`get()`方法来获取一个项目。

```
>>> db.get(Todo.name == 'Book')
{'name': 'Book', 'quantity': 5}
>>> db.get(Todo.name == 'Copies') 
>>> 
```

`get()`方法只返回一个匹配文档。如果没有匹配的文档，则返回`None`。

为了检查一个文档是否存在于数据库中，我们使用了`contains()`方法。

```
>>> db.contains(Todo.name == 'Copies') 
False
>>> db.contains(Todo.name == 'Book')   
True
>>>
```

我们还可以使用`count()`方法获得匹配查询的文档数量。

```
>>> db.insert({"name": "Balloons", "quantity": 500}) 
5
>>> db.count(Todo.name == 'Balloons') 
2
>>> 
```

为了获得数据库中存储的文档总数，我们使用了`len()`方法。

```
>>> len(db)
5
>>>
```

要获取数据库中的所有文档，我们可以使用`all()`方法:

```
>>> db.all()
[{'name': 'Book', 'quantity': 5}, {'name': 'Cake', 'quantity': 1}, {'name': 'Candles', 'quantity': 10}, {'name': 'Balloons', 'quantity': 50}, {'name': 'Balloons', 'quantity': 500}]
>>>
```

### 如何更新 TinyDB 中的数据

`update()`方法获取匹配文档将拥有的字段，或者一个更新文档的方法。它也可以带一个条件来选择性地查询一个参数。

要更新与查询匹配的文档，我们可以这样做:

```
>>> db.update({'name': 'Books'}, Todo.name == 'Book')
[1]
>>>
```

在这里，我们将文档的名称更新为`Books`(上面它的名称是`Book`)。

有时候，我们需要更新所有的文档。在这种情况下，我们不编写查询参数。

```
>>> db.update({'quantity': 10}) 
[1, 2, 3, 4, 5]
>>> db.all()
[{'name': 'Books', 'quantity': 10}, {'name': 'Cake', 'quantity': 10}, {'name': 'Candles', 'quantity': 10}, {'name': 'Balloons', 'quantity': 10}, {'name': 'Balloons', 'quantity': 10}]
>>> 
```

我们将所有文件的数量更新为`10`。

### 如何删除 TinyDB 中的数据

要从数据库中删除文档，我们使用`remove()`方法。这个方法接受一个可选的条件和一个可选的文档 id 列表。如果条件匹配，文档将被删除。

```
>>> db.remove(Todo.name == 'Cake')                              
[2]
>>> db.all()                      
[{'name': 'Books', 'quantity': 10}, {'name': 'Candles', 'quantity': 10}, {'name': 'Balloons', 'quantity': 10}, {'name': 'Balloons', 'quantity': 10}]
>>> db.remove(Todo.name == 'Copies')
[]
>>> 
```

要使用文档 id 删除文档，我们可以编写以下代码:

```
>>> db.remove(doc_ids=[4,5]) 
[4, 5]
>>> db.all()
[{'name': 'Books', 'quantity': 10}, {'name': 'Candles', 'quantity': 10}]
>>>
```

要删除数据库中的所有内容，我们可以使用`truncate()`方法:

```
>>> db.truncate()
>>> db.all()
[]
>>>
```

## 包扎

在本文中，我们讨论了 TinyDB 以及如何在数据库上执行 CRUD 操作。

这只是一个基础教程。要了解更多关于 TinyDB 的高级用法，你可以浏览它的[官方文档](https://tinydb.readthedocs.io/en/latest/index.html)。

感谢阅读！

[Subscribe to my newsletter](https://newsletter.ashutoshkrris.tk)