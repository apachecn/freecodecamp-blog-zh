# 构建 Heroku 克隆——以编程方式供应基础设施

> 原文：<https://www.freecodecamp.org/news/build-a-heroku-clone-provision-infrastructure-programmatically/>

Heroku 是一个平台即服务，使开发人员能够完全在云中构建、运行和操作应用程序。

Heroku 简化了创建虚拟机来托管应用程序和部署网站的工作。Heroku 提供的一些功能实际上可以用其他工具轻松创建。

在本文中，您将了解如何创建一个非常简单的 web 应用程序，允许用户通过点击一个按钮来配置虚拟机和部署静态网站，所有这些都托管在 Amazon Web Services 上。

您将学习如何为基础设施提供代码，然后能够将所学应用到自己的应用程序中。

本文是我们刚刚在 freeCodeCamp.org YouTube 频道上发布的完整课程的配套，该课程将教您如何使用 Python 以编程方式提供基础设施。

你可以在下面或者 freeCodeCamp.org YouTube 频道上观看课程(1.5 小时观看)。

[https://www.youtube.com/embed/zhJLVFR3pE8?feature=oembed](https://www.youtube.com/embed/zhJLVFR3pE8?feature=oembed)

供应基础设施与平台工程相关。平台工程团队通过规划、设计和管理云平台来为组织提供服务。这通常可以通过编程来实现。

我在本课程中教授的工具不仅可用于调配虚拟机和部署网站。它们可用于平台工程，以简化云平台的管理。

### 自动化 API

本课程重点介绍 Pulumi 的自动化 API。Pulumi 向 freeCodeCamp 提供了一笔赠款，使这一课程成为可能。

Pulumi 的开源基础设施代码 SDK 使您能够使用许多不同的编程语言在任何云上创建、部署和管理基础设施。

他们的自动化 API 使得使用 Pulumi 引擎以编程方式提供基础设施成为可能。基本上，它使编写一个在各种不同的云平台上自动创建虚拟机、数据库、VPC、静态网站等的程序变得简单。

我将向您展示如何在后端使用 Flask 和 Python 创建我们的 Heroku-clone web 应用程序。然而，您不必已经知道如何使用 Flask 和 Python 来跟进。此外，我向您展示的一切也可以用许多其他不同的框架和编程语言来完成，无论您使用什么 web 框架，许多步骤都是相同的。

我们的应用程序将在 AWS 上提供资源，但 Pulumi 使在大多数主要云提供商上提供资源变得简单，并且使用不同的提供商不需要太多代码更新。

感谢 Komal Ali，他创造了我在本课程中的代码。

## 创造赫罗库克隆人

### cli

首先，确保您安装了 Pulumi CLI。安装方式因操作系统而异。

如果你有 MacsOS 和[家酿](https://brew.sh/)，你可以使用命令`brew install pulumi`。

如果你有 Windows 和 [Chocolatey](https://chocolatey.org/) ，你可以使用命令`choco install pulumi`。

[这一页将告诉你安装 Pulumi](https://www.pulumi.com/docs/get-started/install/) 的其他方法。

### AWS CLI

这个项目也使用 AWS，所以你必须确保你有一个 AWS 帐户，并有 CLI 设置和认证。

你可以在这里注册一个免费的 AWS 账户:[https://aws.amazon.com/free/](https://aws.amazon.com/free/)

在此了解如何为您的操作系统安装 AWS CLI:[https://docs . AWS . Amazon . com/CLI/latest/user guide/install-CLI v2 . html](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

对于 MacOS，您可以使用以下命令:

```
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

对于 Windows，有一些额外的步骤，你应该[按照这里的说明](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html)操作。

接下来，您需要从 AWS 获得访问密钥 ID 和秘密访问密钥。[按照亚马逊的指示去拿这些](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-creds)。

现在在命令行中运行以下命令:

`aws configure`

出现提示时，输入您的访问密钥 ID 和秘密访问密钥。您可以将“默认片段名称”和“默认输出格式”保留为无。

### 
设置项目

Pulumi 项目只是一个包含一些文件的目录。你可以手工创建一个新的。然而,`pulumi new`命令自动完成了这个过程:

`pulumi new`

如果这是您第一次使用 Pulumi，将会引导您输入访问代码或登录。要获取访问代码，请前往[https://app.pulumi.com/account/tokens](https://app.pulumi.com/account/tokens)

`pulumi new`命令允许您在许多模板中进行选择。选择“aws-python”模板。您可以选择其他选项的默认值。

这个命令创建了我们需要的所有文件，初始化了一个名为`dev`(我们项目的一个实例)的新堆栈。

每个 Pulumi 程序都被部署到一个*堆栈*中。栈是一个独立的、可独立配置的 Pulumi 程序实例。栈通常用于表示开发的不同阶段。在这种情况下，我们称之为“dev”。

现在我们需要为我们的项目安装另外两个依赖项

`venv/bin/pip install flask requests`

### 创建烧瓶应用程序

我们现在可以开始为 Flask web 应用程序创建文件了。

Pulumi 创建了一个名为“__main__”的文件。py”。将该文件的名称改为“app.py ”,因为这是一个 Flask 应用程序，Flask 将查找具有该名称的文件。

该文件中有一些代码，我们最终将使用类似的代码，但现在让我们通过将“app.py”中的代码替换为以下代码来测试 flask 是否工作:

```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World'

if __name__ == '__main__':

    app.run()
```

这是最基本的 Flask web 应用程序，我们可以通过在终端中运行以下命令来测试它:

`env/bin/flask run`

然后我们就可以在网页浏览器的网址“ [http://127.0.0.1:5000/](http://127.0.0.1:5000/) 访问正在运行的 app 了。

如果你去那个网址，它应该说“你好，世界”。如果是这样，Flask 工作正常，因此我们可以将“app.py”文件更新为以下内容:

```
import os
from flask import Flask, render_template

import pulumi.automation as auto

def ensure_plugins():
    ws = auto.LocalWorkspace()
    ws.install_plugin("aws", "v4.0.0")

def create_app():
    ensure_plugins()
    app = Flask(__name__, instance_relative_config=True)
    app.config.from_mapping(
        SECRET_KEY="secret",
        PROJECT_NAME="herocool",
        PULUMI_ORG=os.environ.get("PULUMI_ORG"),
    )

    @app.route("/", methods=["GET"])
    def index():
        return render_template("index.html")

    from . import sites

    app.register_blueprint(sites.bp)

    from . import virtual_machines

    app.register_blueprint(virtual_machines.bp)

    return app 
```

我们将 Pulumi 的自动化框架作为一个名为`auto`的变量导入。然后在`ensure_plugins`函数中，我们可以访问`LocalWorkspace`。一个`Workspace`是包含单个 Pulumi 项目的执行上下文。工作区用于管理执行环境，提供各种实用程序，如插件安装、环境配置以及堆栈的创建、删除和列表。

因为我们在本教程中部署 AWS 资源，所以我们必须在`Workspace`中安装 AWS provider 插件，以便 Pulumi 程序在执行期间可以使用它。

`create_app`函数是当我们启动 flask 应用程序时 Flask 将运行的函数。我们首先运行`ensure_plugins`函数，然后创建 Flask 应用程序。Flask 使用`app.config.from_mapping()`函数来设置应用程序将使用的一些默认配置。这将不是一个生产就绪的应用程序，但如果你曾经部署一个烧瓶应用程序，请确保更改密钥。其他变量由 pulumi 使用。

文件的其余部分对于 Flask 应用程序是通用的。我们设置默认路径(`@app.route("/", methods=["GET"])`)来呈现模板“index.html”(我们仍然需要创建它。

最后，该文件导入站点和虚拟机文件，并将它们注册为蓝图。

Flask 中的蓝图是一种组织一组相关视图和其他代码的方式。它们不是直接向应用程序注册视图和其他代码，而是向蓝图注册。然后，当蓝图在工厂功能中可用时，它将注册到应用程序中。

基本上，我们使用蓝图，这样我们就可以在其他文件中为我们的 web 应用程序定义额外的路线，我们仍然需要创建这些文件。

### 创建模板

说到创建其他文件，让我们创建一些。创建一个名为“templates”的目录，然后在该目录下创建一个名为“index.html”的文件。

添加以下代码:

```
{% extends "base.html" %}

{% block content %}
  <div class="row row-cols-1 row-cols-md-2 g-4">
    <div class="col">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">Static Websites</h5>
          <p class="card-text">Deploy your own static website in seconds!</p>
          <a href="{{ url_for("sites.list_sites") }}" class="btn btn-primary">Get started</a>
        </div>
      </div>
    </div>
    <div class="col">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">Virtual Machines</h5>
          <p class="card-text">Set up a virtual machine for development and testing.</p>
          <a href="{{ url_for("virtual_machines.list_vms") }}" class="btn btn-primary">Get started</a>
        </div>
      </div>
    </div>
  </div>
{% endblock %}
```

关于这段代码，我就不赘述了。这是一个基本的 HTML，但是花括号中的内容是一个表达式，它将被输出到最终的文档中。Flask 使用 Jinja 模板库来渲染模板。您会注意到它动态地呈现了站点和虚拟机的列表。很快我们将创建 Python 代码，使用 Pulumi 获取这些列表。

但是接下来创建一个名为“base.html”的文件。你会注意到“index.html”扩展了“base.html”。这将基本上是我们所有页面共享的标题。

添加此代码:

```
<!DOCTYPE html>
<head>
  <title>Herocool - {% block title %}{% endblock %}</title>
  <link rel="stylesheet" href="{{ url_for("static", filename="bootstrap.min.css") }}">
  <script src="{{ url_for("static", filename="bootstrap.min.js") }}"></script>
</head>
<body>
  <div class="container p-2">
    <header class="d-flex flex-wrap justify-content-center py-3 mb-4 border-bottom">
      <a href="{{ url_for("index") }}" class="d-flex align-items-center mb-3 mb-md-0 me-md-auto text-dark text-decoration-none">
        <span class="fs-3">Herocool</span>
      </a>
      {% block nav %}{% endblock %}
    </header>
  </div>
  <section class="container-md px-4">
    <div>
      <header class="row gy-4">
        <span class="fs-4">{% block header %}{% endblock %}</span>
        {% for category, message in get_flashed_messages(with_categories=true) %}
        <div class="alert alert-{{ category }}" role="alert">{{ message }}</div>
        {% endfor %}
      </header>
    </div>
    {% block content %}{% endblock %}
  </section>
</body> 
```

现在，我们将快速创建我们的 HTML 模板的其余部分，然后创建 Python 文件，在我们的应用程序中做真正的繁重工作。

在我们的“模板”目录中，再创建两个名为“站点”和“虚拟机”的目录。在每个目录中创建相同的三个文件:“index.html”、“create.html”和“update.html”。

下面是要添加到每个中的代码:

**sites/index.html**

```
{% extends "base.html" %}

{% block nav %}
  <ul class="nav nav-pills">
    <li class="nav-item fs-6"><a href="{{ url_for("sites.create_site") }}" class="nav-link active">Create static site</a></li>
  </ul>
{% endblock %}

{% block header %}
  {% block title %}Site Directory{% endblock %}
{% endblock %}

{% block content %}
  <table class="table">
    <tbody>
      {% if not sites %}
      <div class="container gy-5">
        <div class="row py-4">
          <div class="alert alert-secondary" role="alert">
            <p>No websites are currently deployed. Create one to get started!</p>
            <a href="{{ url_for("sites.create_site") }}" class="btn btn-primary">Create static site</a>
          </div>
        </div>
      </div>
      {%  endif %}
      {% for site in sites %}
        <tr>
          <td class="align-bottom" colspan="4">
            <div class="p-1">
              <a href="{{ site["url"] }}" class="fs-5 align-bottom" target="_blank">{{ site["name"] }}</a>
            </div>
          </td>
          <td>
            <div class="float-end p-1">
              <form action="{{ url_for("sites.delete_site", id=site["name"]) }}" method="post">
                <input class="btn btn-sm btn-danger" type="submit" value="Delete">
              </form>
            </div>
            <div class="float-end p-1">
              <form action="{{ url_for("sites.update_site", id=site["name"]) }}" method="get">
                <input class="btn btn-sm btn-primary" type="submit" value="Edit">
              </form>
            </div>
            <div class="float-end p-1">
              <a href="{{ site["console_url"] }}" class="btn btn-sm btn-outline-primary" target="_blank">View in console</a>
            </div>
          </td>
        </tr>
      {% endfor %}
    </tbody>
  </table>
{% endblock %} 
```

**sites/create.html**

```
{% extends "base.html" %}

{% block nav %}
  <ul class="nav nav-pills">
    <li class="nav-item fs-6"><a href="{{ url_for("sites.list_sites") }}" class="nav-link">Back to site directory</a></li>
  </ul>
{% endblock %}

{% block header %}
  {% block title %}Create new static site{% endblock %}
{% endblock %}

{% block content %}
<section class="p-2">
  <form method="post">
    <div class="mb-3">
      <label for="site-id" class="form-label">Name</label>
      <input type="text" class="form-control" name="site-id" id="site-id" aria-describedby="nameHelp" required>
      <div id="nameHelp" class="form-text">Choose a unique name as a label for your website</div>
    </div>
    <div class="mb-3">
      <label for="file-url" class="form-label">File URL</label>
      <input type="text" class="form-control" id="file-url" name="file-url">
    </div>
    <div class="mb-3">
      <strong>OR</strong>
    </div>
    <div class="mb-3">
      <label for="site-content" class="form-label">Content</label>
      <textarea class="form-control" name="site-content" id="site-content" rows="5"></textarea>
    </div>
    <button type="submit" class="btn btn-primary">Create</button>
  </form>
</section>
{% endblock %} 
```

**sites/update.html**

```
{% extends "base.html" %}

{% block nav %}
  <ul class="nav nav-pills">
    <li class="nav-item fs-6"><a href="{{ url_for("sites.list_sites") }}" class="nav-link">Back to site directory</a></li>
  </ul>
{% endblock %}

{% block header %}
  {% block title %}Update site '{{ name }}'{% endblock %}
{% endblock %}

{% block content %}
  <section class="p-2">
    <form method="post">
      <div class="mb-3">
        <label for="file-url" class="form-label">File URL</label>
        <input type="text" class="form-control" name="file-url" id="file-url">
      </div>
      <div class="mb-3">
        <strong>OR</strong>
      </div>
      <div class="mb-3">
        <label for="site-content" class="form-label">Content</label>
        <textarea class="form-control" name="site-content" id="site-content" rows="5">{{ content }}</textarea>
      </div>
      <button type="submit" class="btn btn-primary">Update</button>
    </form>
  </section>
{% endblock %} 
```

**虚拟机器/索引. html**

```
{% extends "base.html" %}

{% block nav %}
  <ul class="nav nav-pills">
    <li class="nav-item fs-6"><a href="{{ url_for("virtual_machines.create_vm") }}" class="nav-link active">Create VM</a></li>
  </ul>
{% endblock %}

{% block header %}
  {% block title %}Deployed Virtual Machines{% endblock %}
{% endblock %}

{% block content %}
  <table class="table">
    <tbody>
      {% if not vms %}
      <div class="container gy-5">
        <div class="row py-4">
          <div class="alert alert-secondary" role="alert">
            <p>No virtual machines are currently deployed. Create one to get started!</p>
            <a href="{{ url_for("virtual_machines.create_vm") }}" class="btn btn-primary">Create VM</a>
          </div>
        </div>
      </div>
      {%  endif %}
      {% for vm in vms %}
      <tr>
        <td class="align-bottom" colspan="4">
          <div class="p-1">
            <pre> ssh -i ~/.ssh/id_rsa.pem ec2-user@{{ vm["dns_name"] }} </pre>
          </div>
        </td>
        <td>
          <div class="float-end p-1">
            <form action="{{ url_for("virtual_machines.delete_vm", id=vm["name"]) }}" method="post">
              <input class="btn btn-sm btn-danger" type="submit" value="Delete">
            </form>
          </div>
          <div class="float-end p-1">
            <form action="{{ url_for("virtual_machines.update_vm", id=vm["name"]) }}" method="get">
              <input class="btn btn-sm btn-primary" type="submit" value="Edit">
            </form>
          </div>
          <div class="float-end p-1">
            <a href="{{ vm["console_url"] }}" class="btn btn-sm btn-outline-primary" target="_blank">View in console</a>
          </div>
        </td>
      </tr>
      {% endfor %}
    </tbody>
  </table>
{% endblock %} 
```

**virtual _ machines/create . html**

```
{% extends "base.html" %}

{% block nav %}
  <ul class="nav nav-pills">
    <li class="nav-item fs-6"><a href="{{ url_for("virtual_machines.list_vms") }}" class="nav-link">Back to Virtual Machines directory</a></li>
  </ul>
{% endblock %}

{% block header %}
  {% block title %}Create new virtual machine{% endblock %}
{% endblock %}

{% block content %}
<section class="p-2">
  <form method="post">
    <div class="mb-3">
      <label for="vm-id" class="form-label">Name</label>
      <input type="text" class="form-control" name="vm-id" id="vm-id" aria-describedby="nameHelp" required>
      <div id="nameHelp" class="form-text">Choose a unique name as a label for your virtual machine</div>
    </div>
    <div class="mb-3">
      <label for="vm-id" class="form-label">Instance Type</label>
      <select name="instance_type" class="form-control" id="instance_type">
      {% for instance_type in instance_types %}
        <option value="{{ instance_type }}" {% if instance_type == curr_instance_type %} selected {% endif %}>
          {{ instance_type }}
        </option>
      {% endfor %}
      </select>
    </div>
    <div class="mb-3">
      <label for="vm-keypair" class="form-label">Public Key</label>
      <textarea class="form-control" name="vm-keypair" id="vm-keypair-content" rows="5" aria-describedby="keypairHelp"></textarea>
      <div id="keypairHelp" class="form-text">The public key to use to connect to the VM </div>
    </div>
    <button type="submit" class="btn btn-primary">Create</button>
  </form>
</section>
{% endblock %}
```

**虚拟机器/更新. html**

```
{% extends "base.html" %}

{% block nav %}
  <ul class="nav nav-pills">
    <li class="nav-item fs-6"><a href="{{ url_for("virtual_machines.list_vms") }}" class="nav-link">Back to Virtual Machines directory</a></li>
  </ul>
{% endblock %}

{% block header %}
  {% block title %}Update Virtual Machine '{{ name }}'{% endblock %}
{% endblock %}

{% block content %}
  <section class="p-2">
    <form method="post">
      <div class="mb-3">
        <label for="vm-id" class="form-label">Instance Type</label>
        <select name="instance_type" class="form-control" id="instance_type">
          {% for instance_type in instance_types %}
            <option value="{{ instance_type }}" {% if instance_type == curr_instance_type %} selected {% endif %}>
              {{ instance_type }}
            </option>
          {% endfor %}
          </select>
      </div>
      <div class="mb-3">
        <label for="vm-keypair" class="form-label">Public Key</label>
        <textarea class="form-control" name="vm-keypair" id="vm-keypair-content" rows="5">{{ public_key }}</textarea>
      </div>
      <button type="submit" class="btn btn-primary">Update</button>
    </form>
  </section>
{% endblock %} 
```

### 使用 Python 创建功能

现在是时候使用 Python 创建 Heroku 克隆的功能了。“app.py”文件从我们仍然需要创建的文件中导入。在与“app.py”相同的目录下，创建“sites.py”。

将以下内容添加到“sites.py”中:

```
import json
import requests
from flask import (
    current_app,
    Blueprint,
    request,
    flash,
    redirect,
    url_for,
    render_template,
)

import pulumi
import pulumi.automation as auto
from pulumi_aws import s3

bp = Blueprint("sites", __name__, url_prefix="/sites")
```

这些只是我们对 Flask 和 Pulumi 需要的基本导入，包括导入自动化框架。

接下来，将以下代码添加到该文件中。这个函数根据调用者传入的内容来定义我们的 Pulumi s3 静态网站。这使我们能够根据用户定义的帖子正文值动态部署网站。

```
def create_pulumi_program(content: str):
    # Create a bucket and expose a website index document
    site_bucket = s3.Bucket(
        "s3-website-bucket", website=s3.BucketWebsiteArgs(index_document="index.html")
    )
    index_content = content

    # Write our index.html into the site bucket
    s3.BucketObject(
        "index",
        bucket=site_bucket.id,
        content=index_content,
        key="index.html",
        content_type="text/html; charset=utf-8",
    )

    # Set the access policy for the bucket so all objects are readable
    s3.BucketPolicy(
        "bucket-policy",
        bucket=site_bucket.id,
        policy=site_bucket.id.apply(
            lambda id: json.dumps(
                {
                    "Version": "2012-10-17",
                    "Statement": {
                        "Effect": "Allow",
                        "Principal": "*",
                        "Action": ["s3:GetObject"],
                        # Policy refers to bucket explicitly
                        "Resource": [f"arn:aws:s3:::{id}/*"],
                    },
                }
            )
        ),
    )

    # Export the website URL
    pulumi.export("website_url", site_bucket.website_endpoint)
    pulumi.export("website_content", index_content)
```

您在代码末尾看到的`pulumi.export`的目的是导出一个命名的堆栈输出。导出的值被附加到程序的堆栈资源中。稍后，您将看到我们如何访问正在导出的数据。

到目前为止，没有任何东西调用我们刚刚创建的函数。此时，我们将创建可用于调用函数和创建网站 URL 端点。

首先，我们将添加用于创建新站点的/new URL 的代码:

```
@bp.route("/new", methods=["GET", "POST"])
def create_site():
    """creates new sites"""
    if request.method == "POST":
        stack_name = request.form.get("site-id")
        file_url = request.form.get("file-url")
        if file_url:
            site_content = requests.get(file_url).text
        else:
            site_content = request.form.get("site-content")

        def pulumi_program():
            return create_pulumi_program(str(site_content))

        try:
            # create a new stack, generating our pulumi program on the fly from the POST body
            stack = auto.create_stack(
                stack_name=str(stack_name),
                project_name=current_app.config["PROJECT_NAME"],
                program=pulumi_program,
            )
            stack.set_config("aws:region", auto.ConfigValue("us-east-1"))
            # deploy the stack, tailing the logs to stdout
            stack.up(on_output=print)
            flash(
                f"Successfully created site '{stack_name}'", category="success")
        except auto.StackAlreadyExistsError:
            flash(
                f"Error: Site with name '{stack_name}' already exists, pick a unique name",
                category="danger",
            )

        return redirect(url_for("sites.list_sites"))

    return render_template("sites/create.html")
```

对于 GET 请求，该路径呈现“create.html”模板。对于 POST 请求，它创建一个新站点，并重定向到位于`/`路径的站点列表。

您会注意到创建了一个`stack`。提醒一下，每个 Pulumi 程序都部署到一个堆栈中。栈是一个独立的、可独立配置的 Pulumi 程序实例。在这段代码中，堆栈名称基于用户在表单中键入的 id。还有一个项目名和程序，也就是我们之前讨论过的代码块。

一旦创建了堆栈，我们就可以对`Stack`执行命令，包括更新、预览、刷新、销毁、导入和导出。在这段代码中，我们使用`stack.up()`和`up`来确定堆栈的日期。我们将. callback 函数传递给`stack.up()`进行标准输出。

接下来，添加将列出站点的根 URL 的代码:

```
@bp.route("/", methods=["GET"])
def list_sites():
    """lists all sites"""
    sites = []
    org_name = current_app.config["PULUMI_ORG"]
    project_name = current_app.config["PROJECT_NAME"]
    try:
        ws = auto.LocalWorkspace(
            project_settings=auto.ProjectSettings(
                name=project_name, runtime="python")
        )
        all_stacks = ws.list_stacks()
        for stack in all_stacks:
            stack = auto.select_stack(
                stack_name=stack.name,
                project_name=project_name,
                # no-op program, just to get outputs
                program=lambda: None,
            )
            outs = stack.outputs()
            if 'website_url' in outs:
                sites.append(
                    {
                        "name": stack.name,
                        "url": f"http://{outs['website_url'].value}",
                        "console_url": f"https://app.pulumi.com/{org_name}/{project_name}/{stack.name}",
                    }
                )
    except Exception as exn:
        flash(str(exn), category="danger")

    return render_template("sites/index.html", sites=sites)
```

一个`Workspace`是包含一个 Pulumi 项目、一个程序和多个栈的执行上下文。所以我们首先获得访问权`ws = auto.LocalWorkspace()`。

然后我们`list_stacks()`。这只是给了我们访问栈名的权限，所以我们必须使用`auto.select_stack()`来访问栈。我们特别想要`stack.outputs()`。这将是我们导出到堆栈中的内容，包括`website_url`。

然后我们将每个站点的信息添加到`sites`列表中，该列表在前端显示站点列表。

现在，添加/update 路由的代码。

```
@bp.route("/<string:id>/update", methods=["GET", "POST"])
def update_site(id: str):
    stack_name = id

    if request.method == "POST":
        file_url = request.form.get("file-url")
        if file_url:
            site_content = requests.get(file_url).text
        else:
            site_content = str(request.form.get("site-content"))

        try:

            def pulumi_program():
                create_pulumi_program(str(site_content))

            stack = auto.select_stack(
                stack_name=stack_name,
                project_name=current_app.config["PROJECT_NAME"],
                program=pulumi_program,
            )
            stack.set_config("aws:region", auto.ConfigValue("us-east-1"))
            # deploy the stack, tailing the logs to stdout
            stack.up(on_output=print)
            flash(f"Site '{stack_name}' successfully updated!",
                  category="success")
        except auto.ConcurrentUpdateError:
            flash(
                f"Error: site '{stack_name}' already has an update in progress",
                category="danger",
            )
        except Exception as exn:
            flash(str(exn), category="danger")
        return redirect(url_for("sites.list_sites"))

    stack = auto.select_stack(
        stack_name=stack_name,
        project_name=current_app.config["PROJECT_NAME"],
        # noop just to get the outputs
        program=lambda: None,
    )
    outs = stack.outputs()
    content_output = outs.get("website_content")
    content = content_output.value if content_output else None
    return render_template("sites/update.html", name=stack_name, content=content)
```

您会注意到/update 路由与/new 路由非常相似。因为它使用相同的`stack_name`，所以不会创建新站点。

这个函数中的一个新东西是代码获取网站的内容返回到前端模板。

最后，为/delete 路由添加以下代码。

```
@bp.route("/<string:id>/delete", methods=["POST"])
def delete_site(id: str):
    stack_name = id
    try:
        stack = auto.select_stack(
            stack_name=stack_name,
            project_name=current_app.config["PROJECT_NAME"],
            # noop program for destroy
            program=lambda: None,
        )
        stack.destroy(on_output=print)
        stack.workspace.remove_stack(stack_name)
        flash(f"Site '{stack_name}' successfully deleted!", category="success")
    except auto.ConcurrentUpdateError:
        flash(
            f"Error: Site '{stack_name}' already has update in progress",
            category="danger",
        )
    except Exception as exn:
        flash(str(exn), category="danger")

    return redirect(url_for("sites.list_sites")) 
```

在这个删除函数中，我们首先访问堆栈，然后销毁并删除堆栈。只要调用`stack.destroy()`函数就会删除 AWS 上的资源。

现在，在与“app.py”相同的目录下，创建“virtual_machines.py”。

将以下内容添加到“virtual_machines.py”中。这一次，我们将一次性添加所有内容:

```
from flask import (Blueprint, current_app, request, flash,
                   redirect, url_for, render_template)

import pulumi
import pulumi_aws as aws
import pulumi.automation as auto
import os
from pathlib import Path

bp = Blueprint("virtual_machines", __name__, url_prefix="/vms")
instance_types = ['c5.xlarge', 'p2.xlarge', 'p3.2xlarge']

def create_pulumi_program(keydata: str, instance_type=str):
    # Choose the latest minimal amzn2 Linux AMI.
    # TODO: Make this something the user can choose.
    ami = aws.ec2.get_ami(most_recent=True,
                          owners=["amazon"],
                          filters=[aws.GetAmiFilterArgs(name="name", values=["*amzn2-ami-minimal-hvm*"])])

    group = aws.ec2.SecurityGroup('web-secgrp',
                                  description='Enable SSH access',
                                  ingress=[aws.ec2.SecurityGroupIngressArgs(
                                      protocol='tcp',
                                      from_port=22,
                                      to_port=22,
                                      cidr_blocks=['0.0.0.0/0'],
                                  )])

    public_key = keydata
    if public_key is None or public_key == "":
        home = str(Path.home())
        f = open(os.path.join(home, '.ssh/id_rsa.pub'), 'r')
        public_key = f.read()
        f.close()

    public_key = public_key.strip()

    print(f"Public Key: '{public_key}'\n")

    keypair = aws.ec2.KeyPair("dlami-keypair", public_key=public_key)

    server = aws.ec2.Instance('dlami-server',
                              instance_type=instance_type,
                              vpc_security_group_ids=[group.id],
                              key_name=keypair.id,
                              ami=ami.id)

    pulumi.export('instance_type', server.instance_type)
    pulumi.export('public_key', keypair.public_key)
    pulumi.export('public_ip', server.public_ip)
    pulumi.export('public_dns', server.public_dns)

@bp.route("/new", methods=["GET", "POST"])
def create_vm():
    """creates new VM"""
    if request.method == "POST":
        stack_name = request.form.get("vm-id")
        keydata = request.form.get("vm-keypair")
        instance_type = request.form.get("instance_type")

        def pulumi_program():
            return create_pulumi_program(keydata, instance_type)
        try:
            # create a new stack, generating our pulumi program on the fly from the POST body
            stack = auto.create_stack(
                stack_name=str(stack_name),
                project_name=current_app.config["PROJECT_NAME"],
                program=pulumi_program,
            )
            stack.set_config("aws:region", auto.ConfigValue("us-east-1"))
            # deploy the stack, tailing the logs to stdout
            stack.up(on_output=print)
            flash(
                f"Successfully created VM '{stack_name}'", category="success")
        except auto.StackAlreadyExistsError:
            flash(
                f"Error: VM with name '{stack_name}' already exists, pick a unique name",
                category="danger",
            )
        return redirect(url_for("virtual_machines.list_vms"))

    current_app.logger.info(f"Instance types: {instance_types}")
    return render_template("virtual_machines/create.html", instance_types=instance_types, curr_instance_type=None)

@bp.route("/", methods=["GET"])
def list_vms():
    """lists all vms"""
    vms = []
    org_name = current_app.config["PULUMI_ORG"]
    project_name = current_app.config["PROJECT_NAME"]
    try:
        ws = auto.LocalWorkspace(
            project_settings=auto.ProjectSettings(
                name=project_name, runtime="python")
        )
        all_stacks = ws.list_stacks()
        for stack in all_stacks:
            stack = auto.select_stack(
                stack_name=stack.name,
                project_name=project_name,
                # no-op program, just to get outputs
                program=lambda: None,
            )
            outs = stack.outputs()
            if 'public_dns' in outs:
                vms.append(
                    {
                        "name": stack.name,
                        "dns_name": f"{outs['public_dns'].value}",
                        "console_url": f"https://app.pulumi.com/{org_name}/{project_name}/{stack.name}",
                    }
                )
    except Exception as exn:
        flash(str(exn), category="danger")

    current_app.logger.info(f"VMS: {vms}")
    return render_template("virtual_machines/index.html", vms=vms)

@bp.route("/<string:id>/update", methods=["GET", "POST"])
def update_vm(id: str):
    stack_name = id
    if request.method == "POST":
        current_app.logger.info(
            f"Updating VM: {stack_name}, form data: {request.form}")
        keydata = request.form.get("vm-keypair")
        current_app.logger.info(f"updating keydata: {keydata}")
        instance_type = request.form.get("instance_type")

        def pulumi_program():
            return create_pulumi_program(keydata, instance_type)
        try:
            stack = auto.select_stack(
                stack_name=stack_name,
                project_name=current_app.config["PROJECT_NAME"],
                program=pulumi_program,
            )
            stack.set_config("aws:region", auto.ConfigValue("us-east-1"))
            # deploy the stack, tailing the logs to stdout
            stack.up(on_output=print)
            flash(f"VM '{stack_name}' successfully updated!",
                  category="success")
        except auto.ConcurrentUpdateError:
            flash(
                f"Error: VM '{stack_name}' already has an update in progress",
                category="danger",
            )
        except Exception as exn:
            flash(str(exn), category="danger")
        return redirect(url_for("virtual_machines.list_vms"))

    stack = auto.select_stack(
        stack_name=stack_name,
        project_name=current_app.config["PROJECT_NAME"],
        # noop just to get the outputs
        program=lambda: None,
    )
    outs = stack.outputs()
    public_key = outs.get("public_key")
    pk = public_key.value if public_key else None
    instance_type = outs.get("instance_type")
    return render_template("virtual_machines/update.html", name=stack_name, public_key=pk, instance_types=instance_types, curr_instance_type=instance_type.value)

@bp.route("/<string:id>/delete", methods=["POST"])
def delete_vm(id: str):
    stack_name = id
    try:
        stack = auto.select_stack(
            stack_name=stack_name,
            project_name=current_app.config["PROJECT_NAME"],
            # noop program for destroy
            program=lambda: None,
        )
        stack.destroy(on_output=print)
        stack.workspace.remove_stack(stack_name)
        flash(f"VM '{stack_name}' successfully deleted!", category="success")
    except auto.ConcurrentUpdateError:
        flash(
            f"Error: VM '{stack_name}' already has update in progress",
            category="danger",
        )
    except Exception as exn:
        flash(str(exn), category="danger")

    return redirect(url_for("virtual_machines.list_vms"))
```

这个文件和“sites.py”文件有很多相似之处。一台虚拟机有相当多的设置必须设置，Pulumi 能够创建我们想要的确切类型的虚拟机。

我们可以让用户从 web 界面定制一切，但我们只是给用户选择实例类型的能力。

VM 需要的一件事是公钥/私钥对。在 AWS 上创建 VM 时，您必须给出一个公钥。然后，为了访问虚拟机，您必须拥有私钥。

您可以在终端中创建密钥。运行以下命令:

`ssh-keygen -m PEM`

稍后测试应用程序时，您必须打开公钥文件，以便复制密钥并将其粘贴到网站上。

现在用刚刚创建的文件切换目录。无论你使用的是 MacOS 还是 Windows，这个目录应该是一样的。

在您的终端上键入:

`cd /Users/[username]/.ssh`

AWS 需要该文件。pem 格式，这就是为什么我们用上面的“PEM”创建它。现在让我们重命名文件，使其具有正确的扩展名。您需要将名为`id_rsa`的文件的名称改为`id_rsa.pem`。

在 macOS 上，您可以使用以下命令重命名:

`mv id_rsa id_rsa.pem`

在 Windows 上，使用:

`rename id_rsa id_rsa.pem`

运行 Flask 应用程序时，可能需要输入刚刚创建的公钥。您可以在任何文本编辑器中打开`id_rsa.pub`来复制文本。如果您有 vim，可以使用这个命令打开文件:

`vim /Users/beau/.ssh/id_rsa.pub`

### 测试应用程序

现在是时候试用这个应用程序了。在终端上，运行以下命令:

`FLASK_RUN_PORT=1337 FLASK_ENV=development FLASK_APP=__init__ PULUMI_ORG=[your-org-name] venv/bin/flask run`

现在，您可以试用该应用程序并创建网站和虚拟机。确保在创建后删除它们，这样 AWS 就不会继续向您收取资源费用。

## 结论

现在，您应该已经了解了足够的知识，可以开始使用 Pulumi 的自动化 API 在您自己的应用程序中提供基础设施了。如果你想一步一步地了解如何做这篇文章中的事情，请查看视频教程:

[https://www.youtube.com/embed/zhJLVFR3pE8?feature=oembed](https://www.youtube.com/embed/zhJLVFR3pE8?feature=oembed)