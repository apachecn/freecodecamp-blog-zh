# 如何在几分钟内用 Python 编写一个简单的玩具数据库

> 原文：<https://www.freecodecamp.org/news/how-to-write-a-simple-toy-database-in-python-within-minutes-51ff49f47f1/>

MySQL、PostgreSQL、Oracle、Redis，以及更多，你只要说出它的名字——数据库是人类文明进步中非常重要的一项技术。今天，我们可以看到**数据**是多么有价值，因此保持它们的安全和稳定是数据库的用武之地！

因此，我们可以看到数据库是多么重要。很长一段时间以来，我一直在考虑创建自己的玩具数据库，只是为了理解、玩和试验它。正如 [**理查德·费曼**](https://en.wikipedia.org/wiki/Richard_Feynman) 所说:

> ***“我不能创造的，我不理解。”***

因此，不再多说，让我们进入有趣的部分:编码。

### 让我们开始编码…

对于这个玩具数据库，我们将使用 **Python** (我最喜欢的❤️).我把这个数据库命名为 **FooBarDB** (我找不到其他名字了？)，但是你想怎么叫都行！

因此，首先让我们导入一些必要的 Python 库，这些库在 Python 标准库中已经可用:

```
import json
import os
```

是的，我们只需要这两个库！我们需要`json`，因为我们的数据库将基于 JSON，而`os`用于一些路径相关的东西。

现在让我们用一些非常基本的函数来定义主类`FoobarDB`，我将在下面解释。

```
class FoobarDB(object):
    def __init__(self , location):
        self.location = os.path.expanduser(location)
        self.load(self.location)

    def load(self , location):
        if os.path.exists(location):
            self._load()
        else:
            self.db = {}
        return True

    def _load(self):
        self.db = json.load(open(self.location , "r"))

    def dumpdb(self):
        try:
            json.dump(self.db , open(self.location, "w+"))
            return True
        except:
            return False
```

这里我们用一个`__init__`函数定义了我们的主类。每当创建 Foobar 数据库时，我们只需要传递数据库的位置。在第一个`__init__`函数中，我们采用位置参数，并用用户的主目录替换`~`或`~user`，使其按照预期的方式工作。最后，把它放在`self.location`变量中，以便以后在同一个类函数中访问它。最后，我们调用的是将`self.location`作为参数传递的`load`函数。

```
. . . .
    def load(self , location):
        if os.path.exists(location):
            self._load()
        else:
            self.db = {}
        return True
. . . .
```

在下一个`load`函数中，我们将数据库的位置作为参数。然后检查数据库是否存在。如果它存在，我们用`_load()`函数加载它(解释如下)。否则，我们创建一个空的内存 JSON 对象。最后，成功时返回 true。

```
. . . . 

    def _load(self):
        self.db = json.load(open(self.location , "r"))
. . . .
```

在`_load`函数中，我们只是简单地从`self.location`中存储的位置打开数据库文件。然后我们将它转换成一个 JSON 对象，并加载到`self.db`变量中。

```
. . . .
    def dumpdb(self):
        try:
            json.dump(self.db , open(self.location, "w+"))
            return True
        except:
            return False

. . . .
```

最后是`dumpdb`函数:它的名字说明了它的作用。它从`self.db`变量中获取内存数据库(实际上是一个 JSON 对象)并将其保存在数据库文件中！如果保存成功，返回**真**，否则返回**假。**

### 让它更有用一点…？

等一下！？如果数据库不能存储和检索数据，它就没有用，不是吗？我们去把他们也加进去…？

```
. . . .
    def set(self , key , value):
        try:
            self.db[str(key)] = value
            self.dumpdb()
            return True
        except Exception as e:
            print("[X] Error Saving Values to Database : " + str(e))
            return False

    def get(self , key):
        try:
            return self.db[key]
        except KeyError:
            print("No Value Can Be Found for " + str(key))  
            return False

    def delete(self , key):
        if not key in self.db:
            return False
        del self.db[key]
        self.dumpdb()
        return True
. . . .
```

`set`功能是向数据库添加数据。由于我们的数据库是一个简单的基于键值的数据库，我们将只把`key`和`value`作为一个参数。

首先，我们将尝试向数据库添加键和值，然后保存数据库。如果一切顺利，它将返回 True。否则，它将打印一条错误消息并返回 False。(我们不希望每次出错时它都崩溃并擦除我们的数据？).

```
. . . .
    def get(self, key):
        try:
            return self.db[key]
        except KeyError:
            return False
. . . .
```

`get`是一个简单的函数，我们以`key`为参数，尝试从数据库中返回与键相链接的值。否则将返回 False 并显示一条消息。

```
. . . .
    def delete(self , key):
        if not key in self.db:
            return False
        del self.db[key]
        self.dumpdb()
        return True

. . . .
```

`delete`功能是从数据库中删除一个键及其值。首先，我们确保数据库中存在这个密钥。如果不是，我们返回 False。否则，我们用内置的`del`删除这个键，它会自动删除这个键的值。接下来，我们保存数据库，它返回 false。

现在你可能会想，如果我已经创建了一个大型数据库，并想重置它呢？理论上我们可以用`delete` —但是不实用，而且还很费时间！⏳所以我们可以创建一个函数来完成这项任务...

```
. . . . 

    def resetdb(self):
        self.db={}
        self.dumpdb()
        return True
. . . .
```

下面是重置数据库的函数，`resetdb`！这很简单:首先，我们做的是用一个空的 JSON 对象重新分配内存中的数据库，然后保存它！就是这样！我们的数据库现在再次被清理干净。

### 最后…？

就是这样朋友们！我们创建了自己的**玩具数据库**！？？实际上，Fo **obarDB i** 只是一个简单的数据库演示。这就像一个廉价的 DIY 玩具:你可以随意改进它。还可以根据需要增加很多其他功能。

完整的源代码在这里？ [bauripalash/foobardb](https://github.com/bauripalash/foobardb)

我希望你喜欢它！请在下面的评论中告诉我你的建议、想法或我犯的错误！？

在社交网站上关注/ping 我？F [acebook，](https://facebook.com/bauripalash) T [witter，](https://twitter.com/bauripalash)I[insta gram](https://instagram.com/bauripalash)

谢谢大家！回头见！

* * *

如果你喜欢我的工作(我的文章、故事、软件、研究等等),考虑给我买一杯☕咖啡？