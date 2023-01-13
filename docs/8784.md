# 用 Django 最简单的方法搜索 elastics

> 原文：<https://www.freecodecamp.org/news/elasticsearch-with-django-the-easy-way-909375bc16cb/>

作者:亚当·瓦蒂斯

# 用 Django 最简单的方法搜索 elastics

![85hcpFM9YT27mXdg41vAeH4YC9LvJwuIhWhF](img/419f77fc73387087f5f92e4eea7cf0e3.png)

不久前，我在做一个 Django 项目，想实现快速自由文本搜索。我决定使用 NoSQL 的数据库，而不是使用普通的数据库进行搜索，比如 MySQL 或 PostgreSQL。这时我发现了 [ElasticSearch](https://www.elastic.co/) 。

ElasticSearch 为您的数据索引文档，而不是像常规关系数据库那样使用数据表。这加快了搜索速度，并提供了许多常规数据库所没有的其他好处。我还保留了一个常规的关系数据库，用于存储用户详细信息、登录信息和 ElasticSearch 不需要索引的其他数据。

关于如何用 Django 正确实现 ElasticSearch，我搜索了很久，并没有真正找到满意的答案。一些[指南或教程](https://qbox.io/blog/how-to-elasticsearch-python-django-part1)令人费解，似乎采取了不必要的步骤，以便将数据编入 ElasticSearch。有很多关于如何执行搜索的信息，但没有太多关于如何进行索引的信息。我觉得肯定有更简单的解决方案，所以我决定自己尝试一下。

我想让它尽可能简单，因为在我看来，简单的解决方案往往是最好的。接吻(保持简单愚蠢)，少即是多，所有这些东西都让我产生了很多共鸣，尤其是当其他解决方案都很复杂的时候。我决定在这个视频中使用 Honza Král 的例子来作为我的代码的基础。我推荐看，虽然在这一点上有点过时。

因为我使用的是用 Python 编写的 Django，所以很容易与 ElasticSearch 交互。有两个客户端库可以使用 Python 与 ElasticSearch 进行交互。还有 [elasticsearch-py](https://elasticsearch-py.readthedocs.io/en/master/index.html) ，是官方的底层客户端。还有 [elasticsearch-dsl](http://elasticsearch-dsl.readthedocs.io/en/latest/index.html) ，它建立在前者的基础上，但提供了更高层次的抽象，功能更少。

我们将很快进入一些例子，但是首先我需要澄清我们想要完成什么:

*   在我们的本地机器上设置 ElasticSearch 并确保它正常工作
*   建立一个新的 Django 项目
*   对数据库中已经存在的数据进行大容量索引
*   对用户保存到数据库的每个新实例进行索引
*   一个基本搜索示例

好吧，这看起来很简单。让我们从在我们的机器上安装 ElasticSearch 开始。此外，所有的[代码都可以在我的 GitHub](https://github.com/adamwattis/elasticsearch-example) 上找到，这样你就可以很容易地理解这些例子。

#### 安装弹性搜索

因为 ElasticSearch 在 Java 上运行，所以你必须确保你有一个更新的 JVM 版本。在终端中使用`java -version`检查您的版本。然后运行以下命令创建一个新目录，下载、解压并启动 ElasticSearch:

```
mkdir elasticsearch-example
```

```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.1.1.tar.gz
```

```
tar -xzf elasticsearch-5.1.1.tar.gz
```

```
./elasticsearch-5.1.1/bin/elasticsearch
```

当 ElasticSearch 启动时，应该会有大量输出打印到终端窗口。为了检查它的启动和运行是否正确，打开一个新的终端窗口并运行这个`curl`命令:

```
curl -XGET http://localhost:9200
```

回应应该是这样的:

```
{  "name" : "6xIrzqq",  "cluster_name" : "elasticsearch",  "cluster_uuid" : "eUH9REKyQOy4RKPzkuRI1g",  "version" : {    "number" : "5.1.1",    "build_hash" : "5395e21",    "build_date" : "2016-12-06T12:36:15.409Z",    "build_snapshot" : false,    "lucene_version" : "6.3.0"  },  "tagline" : "You Know, for Search"
```

太好了，您现在已经在本地机器上运行 ElasticSearch 了！是时候建立你的 Django 项目了。

#### 建立 Django 项目

首先你用`virtualenv venv`创建一个虚拟环境，然后用`source venv/bin/activate`进入这个虚拟环境，以便包含所有的东西。然后你安装一些软件包:

```
pip install djangopip install elasticsearch-dsl
```

要启动您运行的新 Django 项目:

```
django-admin startproject elasticsearchprojectcd elasticsearchprojectpython manage.py startapp elasticsearchapp
```

在创建了新的 Django 项目之后，您需要创建一个将要使用的模型。对于本指南，我选择了一个很好的老式博客帖子示例。在`models.py`中，放置以下代码:

```
from django.db import modelsfrom django.utils import timezonefrom django.contrib.auth.models import User# Create your models here.# Blogpost to be indexed into ElasticSearchclass BlogPost(models.Model):   author = models.ForeignKey(User, on_delete=models.CASCADE, related_name='blogpost')   posted_date = models.DateField(default=timezone.now)   title = models.CharField(max_length=200)   text = models.TextField(max_length=1000)
```

到目前为止，很简单。不要忘记在`settings.py`中添加`elasticsearchapp`到`INSTALLED_APPS`，并在`admin.py`中注册你的新博客模型，如下所示:

```
from django.contrib import adminfrom .models import BlogPost# Register your models here.# Need to register my BlogPost so it shows up in the adminadmin.site.register(BlogPost)
```

您还必须使用`python manage.py makemigrations`、`python manage.py migrate` 和`python manage.py createsuperuser`来创建数据库和管理员帐户。现在，`python manage.py runserver`，去`[http://localhost:8000/admin/](http://localhost:8000/admin/)`登陆。现在，您应该能够在那里看到您的博客帖子模型了。继续在管理中创建你的第一篇博客文章。

恭喜你，你现在有了一个正常工作的 Django 项目！终于到了进入有趣内容的时候了——连接 ElasticSearch。

#### 将 ElasticSearch 与 Django 连接起来

首先在我们的`elasticsearchapp`目录中创建一个名为`search.py`的新文件。这是 ElasticSearch 代码将存在的地方。这里需要做的第一件事是创建一个从 Django 应用程序到 ElasticSearch 的连接。你在你的`search.py`文件中这样做:

```
from elasticsearch_dsl.connections import connectionsconnections.create_connection()
```

现在，您已经与 ElasticSearch 建立了全球连接，您需要定义您想要索引到其中的内容。编写以下代码:

```
from elasticsearch_dsl.connections import connectionsfrom elasticsearch_dsl import DocType, Text, Dateconnections.create_connection()class BlogPostIndex(DocType):    author = Text()    posted_date = Date()    title = Text()    text = Text()    class Meta:        index = 'blogpost-index'
```

它看起来和你的模型很相似，对吗？`DocType`是一个包装器，让你可以像写模型一样写一个索引，而`Text`和`Date`是字段，这样当它们被索引时就可以得到正确的格式。

在 Meta 中，你告诉 ElasticSearch 你希望这个索引被命名为什么。这将是 ElasticSearch 的一个参考点，这样当在数据库中初始化它并保存创建的每个新对象实例时，它就知道它在处理什么索引。

现在，您需要在 ElasticSearch 中实际创建新创建的`BlogPostIndex`的映射。您可以这样做，同时还可以创建一种批量索引的方法——多方便啊！

#### 数据的批量索引

命令位于安装`elasticsearch_dsl`时包含的`elasticsearch.helpers`中，因为它是建立在库之上的。在`search.py`中执行以下操作:

```
...from elasticsearch.helpers import bulkfrom elasticsearch import Elasticsearchfrom . import models...
```

```
...def bulk_indexing():    BlogPostIndex.init()    es = Elasticsearch()    bulk(client=es, actions=(b.indexing() for b in models.BlogPost.objects.all().iterator()))
```

“这是怎么回事？”你可能会想。实际上，没那么复杂。

因为你只要在我们的模型中改变一些东西就想做批量索引，所以你`init()`将它映射到 ElasticSearch 的模型。然后，使用`bulk`并传递给它一个`Elasticsearch()`的实例，这将创建一个到 ElasticSearch 的连接。然后将一个生成器传递给`actions=`，遍历常规数据库中的所有`BlogPost`对象，并对每个对象调用`.indexing()`方法。为什么是发电机？因为如果你有很多对象要迭代，生成器就不必先把它们加载到内存中。

上面的代码只有一个问题。您的模型上还没有一个`.indexing()`方法。让我们来解决这个问题:

```
...from .search import BlogPostIndex...
```

```
...# Add indexing method to BlogPostdef indexing(self):   obj = BlogPostIndex(      meta={'id': self.id},      author=self.author.username,      posted_date=self.posted_date,      title=self.title,      text=self.text   )   obj.save()   return obj.to_dict(include_meta=True)
```

您将索引方法添加到`BlogPost`模型中。它返回一个`BlogPostIndex`并保存到 ElasticSearch。

让我们现在试试这个，看看你是否能批量索引你之前创建的博客文章。通过运行`python manage.py shell`,你进入 Django shell，用`from elasticsearchapp.search import *`导入你的`search.py`,然后运行`bulk_indexing()`来索引你数据库中的所有博客文章。要查看它是否工作，您可以运行以下 curl 命令:

```
curl -XGET 'localhost:9200/blogpost-index/blog_post_index/1?pretty'
```

你应该在终端里找回你的第一篇博文。

#### 新保存实例的索引

接下来，您需要添加一个信号，该信号在每次用户保存新的博客文章时都会保存的每个新实例上触发`.indexing()`。在`elasticsearchapp`中创建一个名为`signals.py`的新文件，并添加以下代码:

```
from .models import BlogPostfrom django.db.models.signals import post_savefrom django.dispatch import receiver@receiver(post_save, sender=BlogPost)def index_post(sender, instance, **kwargs):    instance.indexing()
```

`post_save`信号将确保保存的实例在保存后会用`.indexing()`方法进行索引。

为了让这个工作，我们还需要注册 Django，我们正在使用信号。我们打开`apps.py`并添加以下代码:

```
from django.apps import AppConfigclass ElasticsearchappConfig(AppConfig):    name = 'elasticsearchapp'    def ready(self):        import elasticsearchapp.signals
```

为了完成这个，我们还需要告诉 Django 我们正在使用这个新的配置。我们在我们的`elasticsearchapp`目录中的`__init__.py`中添加:

```
default_app_config = 'elasticsearchapp.apps.ElasticsearchappConfig'
```

现在，Django 注册了`post_save`信号，并准备好在保存新的 blogpost 时监听。

再次进入 Django admin，保存一篇新的博客文章，试试看。然后用一个`curl`命令检查它是否被成功索引到 ElasticSearch 中。

#### 简单搜索

现在，让我们在`search.py`中创建一个简单的搜索功能来查找由作者过滤的所有帖子:

```
...from elasticsearch_dsl import DocType, Text, Date, Search...
```

```
...def search(author):    s = Search().filter('term', author=author)    response = s.execute()    return response
```

让我们试试搜索。在 shell 中:`from elasticsearchapp.search import *`并运行`print(search(author="<author name&`gt；")) :

```
>>> print(search(author="home"))<Response: [<Result(blogpost-index/blog_post_index/1): {'text': 'Hello world, this is my first blog post', 'title':...}>]>
```

你有它！现在，您已经成功地将所有实例索引到 ElasticSearch 中，创建了一个`post_save`信号来索引每个新保存的实例，并创建了一个函数来搜索我们的 ElasticSearch 数据库中的数据。

#### 结论

这是一篇相当长的文章，但我希望它写得足够简单，即使是初学者也能理解。

我解释了如何将 Django 模型连接到 ElasticSearch 进行索引和搜索，但是 ElasticSearch 还可以做更多的事情。我建议在他们的网站上阅读，并探索其他存在的可能性，如空间操作和智能高亮的全文搜索。这是一个伟大的工具，我一定会在未来的项目中使用它！

如果你喜欢这篇文章，或者有什么意见或建议，请在下面留言。敬请关注更多有趣的内容！