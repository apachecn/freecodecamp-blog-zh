# JSON 注释示例——如何在 JSON 文件中进行注释

> 原文：<https://www.freecodecamp.org/news/json-comment-example-how-to-comment-in-json-files/>

如果您在向 JSON 文件添加注释时遇到困难，有一个很好的原因:JSON 不支持注释。

推广基于文本的数据格式的[道格拉斯·克洛克福特](https://web.archive.org/web/20150105080225if_/https://plus.google.com/+DouglasCrockfordEsq/posts/RK8qyGVaGSr)写道:“我从 JSON 中删除了注释，因为我看到人们在用它们来保存解析指令，这种做法会破坏互操作性。”。

然而，有一个变通办法。这就是本文的内容:如何向 JSON 文件添加注释。

## 将数据添加为注释

绕过注释问题的一个方法是向 JSON 文件中添加数据，作为注释。

让我们看一个例子，从 JSON 文件中的信息开始:

```
{
   "sport": "basketball",
   "coach": "Joe Smith",
   "wins": 15,
   "losses": 5
} 
```

现在让我们添加另一个键值对作为我们的注释，你可以在下面代码的第一行看到:

```
{
   "_comment1": "this is my comment",
   "sport": "basketball",
   "coach": "Joe Smith",
   "wins": 15,
   "losses": 5
} 
```

这是另一个例子。这一次，我们在键的开头和结尾使用了两个下划线:

```
 "__comment2__": "this is another comment", 
```

下划线有助于区分注释和文件中的其他数据。

### 提醒一句

有一个重要的细节要记住。

我们添加到 JSON 文件中的注释包含在 JSON 对象中。换句话说，评论被视为数据。

我们的意思是。

这是我们文件中的代码， **`data.json`** :

```
{
   "_comment1": "this is my comment",
   "sport": "basketball",
   "coach": "Joe Smith",
   "wins": 15,
   "losses": 5
} 
```

现在我们要从文件中读取那个数据， **`read_comments.py`** :

```
import json
with open("data.json", mode="r") as j_object:
   data = json.load(j_object)
print(data) 
```

结果包括我们的评论:

```
{'_comment1': 'this is my comment', 'sport': 'basketball', 'coach': 'Joe Smith', 'wins': 15, 'losses': 5} 
```

我们甚至可以从 JSON 对象中提取注释的值:`this is my comment`:

```
import json

with open("data.json", mode="r") as j_object:
   data = json.load(j_object)
   print(data["_comment1"]) 
```

请记住，在开发人员眼中，注释只是注释，而不是计算机。

### 不同类型的评论

这种 JSON 注释实践不同于 Python 等编程语言中的注释，后者在程序运行时通常会被忽略。

```
# Here's my comment
word = "house"
for letter in word:
   print(letter) 
```

当我们运行上面的 Python 程序时，我们得到单词“house”中的字母。但是我们没有看到评论。它被忽略了。

## 评论选项

JSMin 是另一个可以考虑的选项。

这是一个从 JavaScript 文件中删除多余空格和注释的工具。但是它也适用于 JSON 文件。JSMin 会在解析 JSON 文件之前删除其中的注释。

因此，在 JSON 文件中进行注释时有一些选项。尽管它们不是完美的解决方案，但至少有办法在需要时包含您需要的文档。

**我写的是学习编程以及学习编程的最佳方法(**【amymhaddad.com】)。