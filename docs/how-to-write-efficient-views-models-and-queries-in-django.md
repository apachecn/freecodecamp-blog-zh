# 如何在 Django 中编写高效的视图、模型和查询

> 原文：<https://www.freecodecamp.org/news/how-to-write-efficient-views-models-and-queries-in-django/>

我喜欢姜戈。这是一个经过深思熟虑的直观框架，我可以大声说出它的名字。你可以用它来快速启动一个周末规模的项目，也可以用它来大规模运行 T2 成熟的生产应用程序。

这两件事我都做过，这些年来我发现了如何使用 Django 的一些特性来获得最大效率。这些是:

*   基于类的视图与基于函数的视图
*   Django 模型
*   使用查询检索对象

让我们看看这些工具如何让您创建一个易于构建和维护的高性能 Django 应用程序。

## 基于类的视图与基于函数的视图

请记住，Django 完全是 Python。谈到视图，您有两种选择:[视图函数](https://docs.djangoproject.com/en/3.0/topics/http/views/)(有时称为“基于函数的视图”)，或者[基于类的视图](https://docs.djangoproject.com/en/3.0/topics/class-based-views/)。

几年前，当我第一次构建 [ApplyByAPI](https://applybyapi.com) 时，它最初完全由基于函数的视图组成。这些提供了粒度控制，有利于实现复杂的逻辑。就像在 Python 函数中一样，您可以完全控制视图的行为(无论是好是坏)。

但是伴随着强大的控制能力而来的是巨大的责任，基于功能的视图使用起来可能会有点乏味。您负责编写视图工作所需的所有方法——这允许您完全定制您的应用程序。

在 ApplyByAPI 的例子中，只有很少几个地方真正需要这种定制功能。在其他地方，基于函数的视图开始让我的生活变得更加艰难。为一般的操作(如在列表页面上显示数据)编写本质上是自定义的视图变得乏味、重复且容易出错。

对于基于函数的视图，您需要弄清楚要实现哪些 Django 方法，以便处理请求并将数据传递给视图。编写单元测试可能需要一些工作。简而言之，基于功能的视图提供的粒度控制也需要一些粒度上的繁琐才能正确实现。

当我将大多数视图重构为基于类的视图时，我最终抑制了 ApplyByAPI。这不是一个小工作量和重构，但当它完成时，我有一堆微小的看法，使一个巨大的差异。我是说，看看这个:

```
class ApplicationsList(ListView):
    model = Application
    template_name = "applications.html" 
```

是三条线。我的开发人员人机工程学和我的生活变得简单多了。

你可能认为基于类的视图是模板，涵盖了任何应用程序需要的大部分功能。有用于显示事物列表的视图，用于详细查看事物，还有用于执行 CRUD(创建、读取、更新、删除)操作的[编辑视图](https://docs.djangoproject.com/en/3.0/ref/class-based-views/generic-editing/)。

因为实现这些通用视图只需要几行代码，所以我的应用程序逻辑变得非常简洁。这给了我更少的重复代码，更少出错的地方，以及更易于管理的应用程序。

基于类的视图实现和使用都很快。内置的基于类的通用视图可能需要较少的测试工作，因为您不需要为 Django 提供的基本视图编写测试。(Django 自己做了测试；你的应用程序不需要仔细检查。)

为了根据您的需要调整一个通用视图，您可以[子类化一个通用视图](https://docs.djangoproject.com/en/3.0/topics/class-based-views/#subclassing-generic-views)并覆盖属性或方法。在我的例子中，由于我只需要为我添加的任何定制编写测试，我的测试文件变得非常短，运行它们所花费的时间和资源也是如此。

当您在基于函数的视图和基于类的视图之间权衡取舍时，请考虑视图需要的定制量，以及测试和维护它所必需的未来工作。

如果逻辑是通用的，那么您也许可以用一个通用的基于类的视图立即投入运行。如果您需要足够的粒度，以至于重写基本视图的方法会使它过于复杂，那么考虑基于函数的视图。

## Django 模型

模型组织你的 Django 应用程序的中心概念，帮助它们变得灵活、健壮和易于使用。如果使用得当，模型是将你的数据整理成确定的事实来源的有力方法。

和视图一样，Django 提供了一些内置的模型类型，方便实现基本的认证，包括[用户](https://docs.djangoproject.com/en/3.0/ref/contrib/auth/)和[权限](https://docs.djangoproject.com/en/3.0/ref/contrib/auth/)模型。对于其他的事情，你可以通过[从父模型类](https://docs.djangoproject.com/en/3.0/topics/db/models/#model-inheritance)继承来创建一个反映你的概念的模型。

```
class StaffMember(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    company = models.OneToOneField(Company, on_delete=models.CASCADE)

    def __str__(self):
        return self.company.name + " - " + self.user.email 
```

当您在 Django 中创建一个定制模型时，您继承了 Django 的模型类并利用了它的所有功能。您创建的每个模型通常都映射到一个数据库表。每个属性都是一个数据库字段。这使你能够创建人类可以更好理解的工作对象。

通过定义模型的字段，可以使模型对您有用。方便的提供了很多[内置字段类型](https://docs.djangoproject.com/en/3.0/ref/models/fields/#model-field-types)。这些帮助 Django 弄清楚数据类型、呈现表单时使用的 HTML [小部件](https://docs.djangoproject.com/en/3.0/ref/forms/widgets/)，甚至[表单验证](https://docs.djangoproject.com/en/3.0/ref/forms/validation/)需求。如果需要，你可以[编写定制模型字段](https://docs.djangoproject.com/en/3.0/howto/custom-model-fields/)。

数据库[关系](https://docs.djangoproject.com/en/3.0/topics/db/models/#relationships)可以使用[外键](https://docs.djangoproject.com/en/3.0/ref/models/fields/#django.db.models.ForeignKey)字段(多对一)或[多对多字段](https://docs.djangoproject.com/en/3.0/ref/models/fields/#django.db.models.ManyToManyField)来定义(让你猜三次)。如果这些还不够，还有一个[一对一字段](https://docs.djangoproject.com/en/3.0/ref/models/fields/#django.db.models.OneToOneField)。

总之，这些允许你定义你的模型之间的关系，其复杂程度只受你的想象力的限制。(取决于你的想象力，这可能是也可能不是优势。)

## 使用查询检索对象

使用模型的管理器(默认为`objects`)来[构建一个查询集](https://docs.djangoproject.com/en/3.0/topics/db/queries/#retrieving-objects)。这是数据库中对象的表示形式，您可以使用方法对其进行优化，以检索特定的子集。所有可用的方法都在 [QuerySet API](https://docs.djangoproject.com/en/3.0/ref/models/querysets/#django.db.models.query.QuerySet) 中，并且可以链接在一起以获得更多乐趣。

```
Post.objects.filter(
    type="new"
).exclude(
    title__startswith="Blockchain"
) 
```

有些方法返回新的 QuerySets，比如 [`filter()`](https://docs.djangoproject.com/en/3.0/ref/models/querysets/#filter) ，或者 [`exclude()`](https://docs.djangoproject.com/en/3.0/ref/models/querysets/#exclude) 。链接这些可以在不影响性能的情况下为您提供强大的查询，因为查询集在被评估之前不会从数据库[中获取。评估查询集的方法包括`get()`、](https://docs.djangoproject.com/en/3.0/ref/models/querysets/#when-querysets-are-evaluated)[、`count()`、`len()`、`list()`或`bool()`。](https://docs.djangoproject.com/en/3.0/ref/models/querysets/#count)

对 QuerySet 进行迭代也会对其进行计算，因此尽可能避免这样做，以提高查询性能。例如，如果您只想知道一个对象是否存在，您可以使用`exists()`来避免遍历数据库对象。

在您想要检索特定对象的情况下，使用 [`get()`](https://docs.djangoproject.com/en/3.0/ref/models/querysets/#django.db.models.query.QuerySet.get) 。这个方法引发了 [`MultipleObjectsReturned`](https://docs.djangoproject.com/en/3.0/ref/exceptions/#django.core.exceptions.MultipleObjectsReturned) 如果发生了意想不到的事情，以及`DoesNotExist`异常，如果，采取猜测。

如果您想获得一个在用户请求的上下文中可能不存在的对象，请使用方便的 [`get_object_or_404()`](https://docs.djangoproject.com/en/3.0/topics/http/shortcuts/#get-object-or-404) 或 [`get_list_or_404()`](https://docs.djangoproject.com/en/3.0/topics/http/shortcuts/#get-list-or-404) ，这将引出 [`Http404`](https://docs.djangoproject.com/en/3.0/topics/http/views/#django.http.Http404) ，而不是 [`DoesNotExist`](https://docs.djangoproject.com/en/3.0/ref/models/instances/#django.db.models.Model.DoesNotExist) 。这些有用的快捷方式[正好适合这个目的。要创建一个不存在的对象，还有方便的](https://docs.djangoproject.com/en/3.0/topics/http/shortcuts/) [`get_or_create()`](https://docs.djangoproject.com/en/3.0/ref/models/querysets/#get-or-create) 。

## 高效要素

现在您已经掌握了构建高效 Django 应用程序的这三个基本工具——祝贺您！

Django 可以为您做的还有很多，请继续关注未来的文章。

如果你打算在 GitHub 上构建，你可能想设置我的[django-安全检查 GitHub 动作](https://github.com/victoriadrake/django-security-check)。与此同时，您正在构建一个漂亮的软件项目。