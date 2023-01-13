# 如何使用蓝图来组织你的 Flask 应用

> 原文：<https://www.freecodecamp.org/news/how-to-use-blueprints-to-organize-flask-apps/>

Flask 是一个简单易用的 Python 微框架，可以帮助您构建可伸缩且安全的 web 应用程序。

有时你会发现开发人员将他们所有的逻辑都放在一个名为 app.py 的文件中，你会发现很多教程都遵循相同的模式。但是对于一个大型 app 来说并不是一个好的做法。

这样做，你显然违反了*单一责任原则*，其中你的应用程序的每一部分应该只处理一个责任。

如果您使用过 Django，您可能会发现您的项目被分成了不同的模块。同样，在 Flask 中，您可以使用**蓝图**来组织您的应用程序，这是 Flask 中的一个内置概念，类似于 Python 模块。

## 烧瓶应用程序看起来像什么？

如果您按照 Flask 文档创建一个最小的应用程序，您的项目结构将类似于以下内容:

```
/myapp
├── /templates
├── /static
└── app.py
```

文件夹看起来不是很干净吗？您所拥有的只是一个包含应用程序所有逻辑的`app.py`文件，一个存储 HTML 文件的`templates`文件夹，以及一个存储静态文件的`static`文件夹。

我们来看看 app.py 文件:

```
from flask import Flask

app = Flask(__name__)

# Some Models Here

# Routes related to core functionalities

# Routes related to Profile Page

# Routes related to Products Page

# Routes related to Blog Page

# Routes related to Admin Page

if __name__ == '__main__':
    app.run(debug=True)
```

那看起来是不是很乱(想象你做了一个大规模的 app)？你有你所有的模型和不同的路线在同一个文件里，分散在各处。

## 蓝图如何解决问题？

这就是烧瓶蓝图出现的地方。蓝图通过将逻辑组织到子目录中来帮助您构建应用程序。除此之外，您可以将模板和静态文件以及逻辑存储在同一个子目录中。

因此，有了蓝图，您的相同应用程序将如下所示:

```
/blueprint-tutorial
├── /myapp_with_blueprints
│   ├── __init__.py
│   ├── /admin
│   │   ├── /templates
│   │   ├── /static
│   │   └── routes.py
│   ├── /core
│   │   ├── /templates
│   │   ├── /static
│   │   └── routes.py
│   ├── /products
│   │   ├── /templates
│   │   ├── /static
│   │   └── routes.py
│   └── /profile
|       ├── /templates
|       ├── /static
|       └── routes.py		
├── app.py
├── /static
└── /templates
```

现在您可以看到您已经清楚地分离了关注点。与管理相关的逻辑驻留在`admin`文件夹中，与产品相关的逻辑驻留在`products`文件夹中，依此类推。

此外，我们还将模板和静态文件分离到需要它们的子目录中，这样当进入不需要它们的页面时，不相关的文件就不会加载。

## 如何使用蓝图

现在你已经理解了蓝图解决什么问题，让我们看看如何在我们的应用程序中使用蓝图。

### 如何定义蓝图

让我们为`admin/routes.py`文件中的**管理**功能定义第一个蓝图:

```
from flask import Blueprint

# Defining a blueprint
admin_bp = Blueprint(
    'admin_bp', __name__,
    template_folder='templates',
    static_folder='static'
) 
```

因为 Blueprint 是一个 Flask 内置概念，所以可以从 Flask 库中导入它。在创建`Blueprint`类的对象时，第一个参数是您想要给蓝图起的名字。这个名称稍后将用于内部路由(我们将在下面看到)。

第二个参数是蓝图包的名称，一般是`__name__`。这有助于定位蓝图的`root_path`。

传递的第三和第四个参数是可选的[关键字参数](https://ashutoshkrris.hashnode.dev/what-are-args-and-kwargs-in-python)。通过定义`template_folder`和`static_folder`参数，您可以指定您将使用特定于蓝图的模板和静态文件。

### 如何用蓝图定义路线

现在，您已经为管理员相关功能创建了蓝图，您可以在为管理员创建路线时使用它。

```
from flask import Blueprint

# Defining a blueprint
admin_bp = Blueprint(
    'admin_bp', __name__,
    template_folder='templates',
    static_folder='static'
)

@admin_bp.route('/admin')   # Focus here
def admin_home():
    return "Hello Admin!"
```

在上面的代码片段中，关注定义路线的那一行。我们用了`@admin_bp.route('...')`，而不是通常的`@app.route('...')`。这就是将路线绑定到特定蓝图的方式。

### 如何注册您的蓝图

现在你有了一个蓝图和一条路线。但是，你的应用会自动知道这个蓝图吗？

![Screenshot-2022-08-30-112849](img/3e6f28ea5807456eb978e553b4fc5db5.png)

No. You do it.

所以，我们开始吧。在`__init__.py`文件中，我们将创建一个 Flask 应用程序，并在那里注册我们的蓝图:

```
from flask import Flask

app = Flask(__name__)

from .admin import routes

# Registering blueprints
app.register_blueprint(admin.admin_bp) 
```

为了注册蓝图，我们使用`register_blueprint()`方法并传递蓝图的名称。此外，您可以将其他参数传递给该方法进行更多的自定义。其中一个是您可能需要的`url_prefix`。

```
app.register_blueprint(admin.admin_bp, url_prefix='/admin')
```

同样，如果你有更多的蓝图，你可以注册你的蓝图。

## 带蓝图的模板路由

如果没有蓝图，为了在您的模板中创建链接，您将使用类似于下面的内容:

```
<a href="{{ url_for('admin_home') }}">My Link</a>
```

但是有了蓝图，现在您可以将您的链接定义为:

```
<a href="{{ url_for('admin_bp.admin_home') }}">My Link</a>
```

我们上面使用的`admin_bp`是我们在创建对象时给我们的内部路由蓝图起的名字。

要打印当前页面所属的 Jinja2 模板的蓝图名称，可以使用`{{request.blueprint}}`。

Note: To use Blueprint-specific assets, you can use the [Flask-Assets](https://flask-assets.readthedocs.io/en/latest/) library.

## 包扎

Blueprint 是一个组织和构建 Flask 应用程序的神奇工具。

在本文中，您了解了 blueprints 必须提供什么，以及如何在 Flask 应用程序中使用它们。

感谢阅读！

你可以在[推特](https://twitter.com/ashutoshkrris) [上关注我。](https://ireadblog.com/)