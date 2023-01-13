# 如何在手机上构建网络应用——Python & Pydroid Android 应用教程

> 原文：<https://www.freecodecamp.org/news/how-to-code-on-your-phone-python-pydroid-android-app-tutorial/>

嘿，你好吗？我是一名 18 岁的后端开发人员，也是一名有抱负的机器学习工程师。在这篇文章中，我将讲述如何使用 Python 在你的手机上构建一个 web 应用程序😁。让我们深入研究一下。

![TW_PdXBpgeWY4mLcHjFisp8e7Lk7Zsn1aFarXBkmvhEMP0XR5xzTDxhKcCizsrJ25rkPeMeWp7ctlG0Wy7_WFUS0bzT-JVJfpe6X_3OqnuE_df2q5B3KIrhl3EG47w3Dik3nIZE](img/186f55e84a350d5fc89ef81afd58b3c8.png "Placeholder image")

## **要求**

这里首先需要的是一部安卓手机，至少 6.0 及以上版本。但是如果我告诉你这就是我们所需要的呢？似乎好得难以置信。

现在我们需要做的下一件事是在我们的手机上安装一个名为 pydroid3 的移动应用程序。

![fwM9r46B-sTofVF6IybUOhCTYoM8vSAPfumfBIiiL_wWLQpgdQgeR2B_2-N28NtNLaA7HvTtsZxlXdX03anCGvbt4QAlhQ_wyb9_AIfqG9L4ZMCjQOrKLg5OFPeZgKrJdqKEeb8](img/805592c2a6ac6356c674fc948041f55a.png)

正如您所看到的，pydroid3 是一个移动应用程序，可以让您在手机上编写 Python，所以继续安装它吧。

我们需要做的下一件事是安装 Django。如果你不熟悉 Django，请点击这里查看 Django 文档。

要安装 Django，我们需要在 pydroid3 中打开侧面导航，然后选择终端:

![qO3djIyoXMZB8MzcIaFDmNddhB2t9XgLLgCzonR2CDkWJc0pXtap9gyGhqZfpv0uFCCvtYnynL6pAOfgactlDfpwoy03TfgqEoN2W_gAO7nOeoaLbySZEQkOSBuprhs67jc-Ens](img/699bc2798fae2c07546f5bc47657ee2a.png)

然后点击它，我们应该看到这个:

![fTwNrfhCQpxKBFbrsN3B6dt4kFWvDUEJElZ897o-d21XbiYj42gZBLhiLMt7ffvSp44OQBrubC9jK62WvzneTlF-7WxcZZygHEqo4hmQ_9V42Pw4FgvdKB75EA3fv4q5nGZiL7k](img/4c53ecd2c635d716df4d7a6188284b10.png)

完成后，您需要做的就是键入以下命令:

```
pip install django
```

你应该得到下面的。我收到一条“满足要求”的消息，因为我已经安装了它。

![vYhoSBXGgvq2EiX6iXQ1RBLrvUe8zQHM3Aq65ZDIDRKSOoLqOrW5QQWE5yQ-ThbhzYTb6kwKf_jHzVoQ79wTbz2KZNv32oEBX1LjAFeMYaiQb4bebYOWii-h1W3EKQkTWvgA2_Q](img/203ea301ba5da6ee804b28cc0633cc50.png)

它已成功安装，但让我们确认一下。在终端中键入`django-admin`并按回车键。

你应该知道这个:

![jU17O6AVeFcy6rYMJ0mp_DEqnR9q51F-mhLZH1K7Ny8tixSeY7Xl8Jx27hBfxWfHPimt-1xCfO6x2AOlvYKYR92slC3sBwJNRg9uDJsJ6had0Yq1UTXZ5_CQvfCwwKneKCO_Gp4](img/51180c0b978ebe6e00a9f209866e5afa.png)

这意味着它实际上已经安装好了。

## 如何构建我们的项目

所以让我们开始构建我们的项目。打开您的终端，键入以下命令:

`django-admin startproject myapp`

这将在根文件夹中创建一个名为 myapp 的 Django 应用程序。

通过输入`cd myapp`和`python manage.py runserver`来改变目录。那么你应该知道这个:

![fqO-uHACjoAXNQSrm5Pikjr-GQQkY3SbkE3G9Sgel1XZbePIf7hJaePd8yGxdrbYiyRdpeWCFUYBNo6iKMzTJqZg3s8j6CTGIoZYH-YJjT-tjHA0FCKtdGJEGzNy0Y8Qj5uTQrg](img/8a782a91402755c8990ca0e5fc1922ad.png)

现在服务器已经启动了。接下来，要在浏览器中测试它，请访问 [127.0.0.1:8000](http://127.0.0.1:8000) 。

![oqMFGPasUPLxuZoRqWHQ9mEhpitsg2XK8XPzLz_U-TvnFGzjkIaHVKUHXxwYkMDskLp_36F75BIAb-qv37bHccUESSZ9Jqa6XV7FGoWYk_IS8SfPYZfMTSNmo2ei-SMVa9cp_C8](img/993cdfe70e1ee8b409d641e928ca65ef.png)

然后嘣！您应该看到 Django 已经成功设置。

接下来我们需要做的是创建我们的 Django 应用程序。在 Django 中，项目文件夹作为根，而 app 作为应用程序本身。

要创建 Django 应用程序，请确保您仍在目录中，然后键入`python manage.py startapp todo`。这在我们的 myapp 项目中创建了一个待办事项应用程序，如下所示:

![ycIZAg7VGJO4Auwc7z_bsx5CU19Ks-rfubo_3amBKgvO-HeHb2I7mQu_loWg6leR22dvlMGh0FPgO1_-anmVpEHO4O4dlQik-MfiqF7Dx9BmxuI6YBjqPMcv8S3czgVCyftBu80](img/96eff49022fd8ccacfb414ed912f7495.png)

然后在 todo 文件夹中，我们应该会看到类似这样的内容:

![Fc60wk6pMuEQ8JvIwfOK2E1zezR9n_N-8o_X__F-yr1D1yD0BEuV62G9zoqG5GQnyA0shbI79JvNs3Z-YHunEoUyZw7LAt2eumkyKjj9M39sDbmDgzZ_axvjRyVeyLZC5ohVQmY](img/88f28f470ecbcd8ef8891a3cf5833615.png)

当我们开始处理这些文件时，我们将进一步查看它们。

## 如何配置我们的应用程序

现在让我们让 Django 项目为应用程序提供服务成为可能。首先，打开 myapp 文件夹中的`settings.py`文件，将`'todo'`添加到已安装的应用程序中，如下所示:

![mxTcaRk-ON73sPH6XL31kvZmUJjfwn1knbhMgTJALeyx6l8A1umvtXjLazS34oTjbPZeivGGTe6w6zsEQ1QzhTjaYDJ5tHsbhpeyxAfrvABzGHrNsElcv7RR9kQZi_Tttt4PjIc](img/6906f337c3cb5bfe8eb6748fb9c331a1.png)

接下来，我们需要打开我们的`urls.py`,将以下内容添加到您的代码中:

```
from django.urls import path, include

path('', include('todo.urls'))
```

![VEWQQt84a9DSeqmuT-LrE9EMmYnDnDfwJQQtJhI21WDTJf4EDaE212wj7BLoBX85Vjm90gFb6KsB6yGJ6PDyfgdTT9BL5hcmDZzNfIdHlceR40qJNVubaNKduXjA2viT7yqLJ14](img/c9f19c7b58fcc7ccc7173232ce38fe9c.png)

实际发生的情况是，我将`include`添加到了 from`django.urls`导入路径。在路径(`admin`)下面，我们创建了一个空路径，指向或包含 todo 应用程序目录中的`urls.py`文件。我希望这很清楚。

接下来，我们需要在 todo 文件目录中创建一个名为`urls.py`的新文件，并在其中添加以下代码:

```
from django.urls import path
from . import views

urlpatterns = [
	path('', views.index, name='home')
 ]
```

![cmxgwJ5PeIXW_yGgo9AKaVK10pDjGFl26gML6VicCQVLtsiCiorL5tBahCMOxHG-1HlrocwbaVod5SN6DFJFIZ5n1gidGOfJdaGW_p8holylN4aCUb-2ankvfIQygHz6cjT2tgc](img/9fe6bec38138367d8960a726b1d1dd33.png)

我们从`Django.urls`导入了`path`，也从根目录导入了`views`。然后我们用第一部分作为根链接创建了我们的`urlpatterns`。正如你所看到的，views.index 意味着我们将这个视图指向`views.py`文件中的索引函数。你马上就会看到它是如何工作的。

让我们继续前进到我们的`views.py`文件并添加一些代码。

在顶部，像这样导入`HttpResponse`:

`from django.http import HttpResponse`

在它下面加上这个:

```
def index(request):
	return HttpResponse('Hello')
```

![QUpf-9cT8Z-dKXTkO1FTm2-IkjD3_NIfYSQCy_XlALUTnIg_XrrxKurZLAJ19DQCk1W5mqBx4Mo5IL9ycL5gGS_w4LyI4zXxSo8y23mNaZ2OodFg-qLEi3Dh2FN_m7ueYjPYrb4](img/bbf4f88dc84501e099c5309b4ce101f6.png)

如您所见，我们创建了在`urls.py`中调用的索引函数，并向其传递了一个请求参数。然后我们回了一个`HttpResponse`。

但是在`HttpResponse`可以工作之前，我们必须从`django.http import HttpResponse`导入它——就像 ABC 一样简单。让我们试试这个:打开你的终端和 cd 进入 myapp，输入`python manage.py runserver`来测试它。

![Tqb7c-adOuVHbyi-7XBQsv0HHJvxjUhcAZ3N4d5nkOEVNVwfSXxkENlD0l0UI3Jd4qLhO3k8ELDW6yG8yRiP0MmjkO0Q4TvGTYunQIBNgSMNrXxfI7ygMHeN2FtjoJc37mVIVr0](img/a12a136cebf8b72728a04550d490eff8.png)

如您所见，它返回了响应。所以接下来我们将加载我们的模板 HTML 文件。

要加载我们的 HTML 文件，我们需要在 todo 目录中按以下顺序创建一个这样的文件夹:

`todo/templates/todo`

在 todo 目录中，创建一个名为 templates 的文件夹。在该文件夹中，创建一个名为 todo 的文件夹，就这么简单。

然后创建一个名为 index.html 的简单 HTML 文件，并在其中写入以下内容:

`<h1>Hello world</h1>`

要加载它，让您的`views.py`代码看起来像这样:

```
def index(request):
	return render(request, 'todo/index.html')
```

![mhirciumIf_FcO764txwH5MOMl40vkZ6f41c0oXFreX1UA2IiqQG9E42TfbUBCZto4xG6-vl0t5sQj3ID1FBk_gL074Rzm4pn5a8RmMsP7DuMZKYVi1KQg-Bk35yr1gJGiE2ukg](img/a7ee40b25572ffc0311f60705821cb45.png)

现在，我们没有返回响应，而是返回了一个渲染视图，允许我们现在渲染我们的 HTML 模板，将这个打开您的终端 cd 保存到 myapp 中并运行它。我们应该有这个

![NzW4_E80BNOtRq-E4qUg1GdvqHUUQQAxMAdUSGhxROCDkSUnzddSyX4E7Wz5_zPY29twa7D2PVmS85LYmCnzEAvgE-oU2MEk1mDeNhFW5FBuD2eAjDxpPkJfXiJAMEyk1uKZVkw](img/1bb7851eea3d5d459489dfc88bdf4343.png)

正如你所看到的，它工作得很好——进入下一步。

## 如何设置静态文件

现在要设置静态文件，在您的 todo 目录中创建一个新文件夹，并将其命名为 static。在该文件夹中，创建一个文件夹并将其命名为 todo。

所以应该是这样的:`/static/todo/`。

在 todo 目录中，创建一个文件并将其命名为`main.css`。然后让我们在里面写一点造型:

```
body {
background-color: red;
}
```

并保存它。

现在让我们通过编写以下代码来重新编辑我们的`index.html`文件:

```
{% load static %}
<!Doctype html>
<html>
<head>
<title>My page</title>
<link rel="stylesheet" href="{% static 'todo/main.css' %}" >
</head>
<body>
Hello
</body>
</html>
```

![Luboze-gNbfQkpTZVwOChtQKrQpC2eWnsTAE41f9mDdWaqaKtk2yYAV0uP3ufKE_EDrpfCoRvOFlHmLCJKucPNB_kQmZoaAZB5reCcW2wrddbsDbRPoIe2iacGLpFfLEcGYZEnA](img/45db7d0651ac5fd3166145c268888554.png)

现在让我们运行它:

![ARWYir-7j8-yF9yCzc3bNuW1ZyLKOG30iljprX4AJsnyIdYLtK_0Of7Uu4WJLuufoyRkVL5LnG8J-bepoBcRzm1e57AuaLmbA5iIyO_RY_KsKRVrsc0OfGmDbLOkT-FIZECwIyY](img/f2ad7b51235a82917eb8b41afa3a4a4f.png)

如果你跟随我，那么你应该有以上。

## 如何加载模型和管理面板

现在加载我们的管理面板，我们需要创建一个超级用户。这很容易做到——只需打开你的终端，将 cd 放入 myapp 文件夹，然后输入`python manage.py createsuperuser`并按回车键。您应该看到这个:

![PBTNq4SLyU4xMFsxh8wXuP0fUCnNKqL0zPiAqclNSPc4J7j4izPVgikXXQpaPqcPeSfFhrlQgf2xwyuhWz-s4RJWn1ftc5icsi9bt2QwmjKxjp3reecfmCxQ3GdVvE04HUAc8po](img/ab830a1e3bbe859d8eec3ef07832c696.png)

我们得到一个错误，因为我们还没有运行`python manage.py migrate`。因此，键入并按回车键，您应该会看到类似这样的内容:

![_oEnoQWnv1VRtZf8W60ZyfFVGQV-nFzYKX4oj45SLCLUlPNNyZOefRkIj8ROdoNdkgECWr4OKmxRVUsRZy2c27XwsM7wQ4_7xeJWnlzPrBFZ79t7J8zZXFJLtfDqJf1vrvtShjc](img/38c1a2f9e043c0c50d22572ede75ad21.png)

现在输入`python manage.py createsuperuser`并按回车键:

![t8Z8qo8Z3xNi9C86RjkiujHiS6en5b16eYPA5uMTfXAQYNpFjjuWaY_WEL0TrxLUlpaJJHzF143Vk0UuTQIzuD4GICQF4X1K2CF0vyb1ws33JN2W_FeyVu3xMOsn1posUZW0eFs](img/934b382d571909f3f49f5cd146f09394.png)

只需填写凭证。我们需要做的下一件事是运行我们的服务器并指向 127.0.0.1:8000/admin。

![Uoen79EV8PaEDuhnt2eBaCnnJAEzHhLydikTi8BOxUSZ9DrGp9GbtUk-Um7TmMDW64Zd0RbAkXja8RjyqiX58hlWdFyrzHTUVN0NCx93e9BOx36Va4ysCX7JyJRlEmdUBnbltuA](img/e2c1b5bd3dc1fd4740f8732f9ae4cee1.png)

登录后，您将被定向到控制面板:

![C8A8OermBdrvdB_6NEHg2mFgkkuVBsePdfdmlNhulSw2m7Jkdhea_jdDFNQnbvVgqxJcXj-ftbcNmdR6nYImJC2AV9edqcPB5pkhUm0zvImzzzAokHZ4bDwYe4BPPvnXsK18Ng0](img/4a480297ed95e0978d99826ffe6e3bd5.png)

现在，我们已经完成了管理面板，让我们与模型(数据库)的工作。我们将创建一个收集内容的模型。因此，打开您的`models.py`文件，键入以下代码:

```
class Post(models.Model):
	content = models.CharField(max_length=255, null=False)

    def __str__(self):
    	return self.content
```

![pyZXf_3jSGzz-sciBxAvb-ry4_TbZMnuHWWWAOl17LQ5hCi55DoKxzq0iYu6wuv8UsQhn3-w27GOzlt2N_9mpdKoHcZza9mWoBgselVQXC6bPD-ev-uTjlW1RbN1c2OussUgEpg](img/8a0fd8cfd2ea4d31877399ea7f424b59.png)

我们创建一个具有参数`models.Model`的类，并给出一个包含`CharField()`的变量`content`，更像是一个文本字段。最后，我们创建了一个神奇的`str`，它返回模型的名称，而不是一个对象。

因此，接下来我们需要运行迁移。打开你的终端，cd 进入 myapp，输入`python manage.py makemigrations`。您应该看到这个:

![UBbVNNg1d8jhPTusB-HRRoUsqFfxaZdJLzSIzNIt3P4kby8Tor4G8Bme1e-yq8mOLFgfrUh3nb6MC3BSaOUQDr68_tEmIRtQBS7N7Y66wTbXdMMg-0EJ0svM3tw3j9GLgquC_IU](img/7643540ccbc1fb68821ac8c15f9cba5b.png)

这意味着它已经在我们的数据库中创建了 Post 表。然后运行`python manage.py migrate`，这将导致以下结果:

![VyQYel1QFdxc2D5oSOdDD6QPth2jVC5_CTj8SVyDo8pAusvl6qjH7XQUhmhbfXNLjdUiAc566pYTj0O2c-AsRHwVLeDo2xeOv1HWsldCwH1oxu3sM5WJTNOj9-fpZEOfVHMYZ6k](img/039270f6806811a6777045ce47fcfc2f.png)

这意味着一切都清楚了。现在将它添加到管理页面，打开`admin.py`并输入以下代码:

```
from .models import *

admin.site.register(Post)
```

![jzqRK8kVE6raStmHC8jJoqr8oYOXhygDpe8hoN_JdSRiF3Mpes3_Evw83U0nMczqgAobIY8zp_Z6ve-xb3jv6x7uChFzvdTyDqDZysD2j0pKxiGu2-V9pkvH02HKAzBA2HZr6WQ](img/92f23984fcd8bd9be8a5e6b1295e54eb.png)

我们从模型中导入了所有的模型类，并在管理面板中注册了 post 模型。现在，如果我们打开管理面板，我们应该看到文章，并保存一些数据。

![E9gkvNmpFiCJg24zYj7GpLzsM8AsoGUkoZHcS1Z3bxMva_Z3Jov5Yy7UzbgU251laLwGGRWqaFK1iIrILblSyktYK42Q-fzgS6ihGf0LYxR0Zl9qvkmG7sneHM2KFRoSPDy2k3o](img/654c546a5dc729f3264efbb59d3294d1.png)

请注意，它现在位于待办事项应用列表中:

![BSyVagLKFGvtINW-jnuhrXRoFdB87S5lGksH37z5uewqVCn_WBHP-eI8gF6BUoG56Dz-SnKUtRonFhNX--c23V07WfXhOxHmCmJ460cXAr__NjTAkvXB4JnxIXlbsQcRtDO0uNU](img/7aedd97a18a1aa0e987168235206394a.png)

点击之后，您应该会看到:

![4zxSgdVcDnDrpr6aIquG854x59GQb0ZMJ3D-YnAs-9EDR0EYwHl_HBAbrpPGGr7YLfWn0PjJA19aukrUcBbUMURpn4ofEGCwWF4541ee_-OKZQj_cWuv_yxWvUGYGOZfdzu6C90](img/0cc15edc8389f040f295490b5cac14c8.png)

如果你愿意，你可以创建一个帖子。

## 如何将数据从数据库渲染到视图

最后，我们将从数据库中获取数据。为此，我们需要如下更新我们的`views.py`:

```
from .models import *

def index(request):
	content = Post.objects.all()
    context = {'content': content}
    return render(request, 'todo/index.html', context)
```

![NHpq8LEAtu06ntzUodCuBZT86FS_u_TPphhlfZ-CiP5rFglQcjtRB0zUdK0jkz_udZeXRh8JNqdZOhRSfV9A69I63b8P5DtBGtQo44zmwufnGTaybAaWAL0yOn9T544_mdXaLN4](img/6d1c0a10c42326e44a720593177e1037.png)

就这么简单:我们从`models.py`导入所有数据，创建一个名为`content`的变量，并从表 Post 中检索所有数据。然后我们把它作为字典传递给我们的视图。因此，在我们的 index.html，要让它工作，只需添加以下内容:

```
{% for contents in content %}
{{content.content}}
{% endfor %}
```

![4zgGmOcVBVa906mn0AVk0Vh9MbaFeYS0VUVoOC00Jw6wtR54S55BMPjz5t0_z2LTgbs9Ldpt3VOKcEjgxMhSE63xGu8XKSx2tWbKFYp2ndxHc31pcAMFdSturJqEy07ca_IYC1c](img/b2d0a175c7035748ecc300f6dae6d63d.png)

这里，我们使用 templates 标记编写了一个循环，并获取了所有数据内容。现在打开您的终端，cd 进入 myapp，运行服务器来看看神奇的事情发生了:

![gKJf7AGR-0ZxOCeD_QKGffg4d-wpK0Lk8Z0Fkdj39Rj1V6dpWGf_KA1iBDJ2xE-Lq_zsJQHq6eIywPujAVmEk_R7e-Ug7ox94Rk5x212Bk7cBm0fHaMnGtqQM9zscDELygE1LvI](img/31bf39e88a47c574be96417a006a81bf.png)

这是可行的，但是让我们来确认一下:

![IVjbVn-_3Exnnoq2s0pvHTeL2paWcqogzg1mp_Aj15GtXKqUPerrFDGZ-SjYKqpUX8Es1KGo0fSWoAOACLgri_LcT5oV7tkG6dtL2OestlnQC25OzFdYEhcyb0KPH3b12BBdJTU](img/a6dee5ae96520b648ebfd8d1291368c7.png)

结果应该是这样的:

![jlYy4UCV3MJd-JytvGUBLgC20k3-cduvDQ2O3FIb9kAF7VgRyGxyqb_G1Mjiqis261HQS68uIJUk5I9ccFJBFL6Ht3LiePvprBcsqkSS9lZZzJ_cc2noxJm32GPp9ytsiYl7t2o](img/f8c00e9062fd9490626d170ddeffb4c7.png)

violà——工作正常。最后，你可以添加一个换行符，这样你可以读得更清楚。我们完事了。

感谢您的阅读。如果你想深入学习 Django 教程，请访问我的 YouTube 频道 [Devstack](https://youtube.com/channel/UCLcHGKxbEO1XGVETXqzYXLA) 并订阅。