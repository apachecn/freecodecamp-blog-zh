# Docker 开发工作流程 Flask 和 Postgres 指南

> 原文：<https://www.freecodecamp.org/news/docker-development-workflow-a-guide-with-flask-and-postgres-db1a1843044a/>

作者 Timothy Ko

# Docker 开发工作流程 Flask 和 Postgres 指南

Docker 是最新的 crazes 之一，它是打包、运输和运行应用程序的强大工具。然而，理解并为您的特定应用程序设置 Docker 可能需要一点时间。由于互联网上充斥着概念指南，所以我不会在概念上对容器进行太深入的探讨。相反，我将解释我写的每一行的含义，以及如何将其应用到您的特定应用程序和配置中。

![Qv18K6q0lHj07pm6jZk8lWgCkfib7Zg6xQMP](img/40d5c97eff0f8912d062cc054a97afcd.png)

Photo by [Christopher Gower](https://unsplash.com/photos/m_HRfLhgABo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### 为什么是 Docker？

我是 UIUC 学生运营的非营利组织 Hack4Impact 的一员，我们为非营利组织开发技术项目，以帮助他们进一步完成使命。每学期，我们都有多个由 5-7 名学生软件开发人员组成的项目团队，他们拥有各种不同的技能水平，包括刚刚完成第一门大学计算机科学课程的学生。

由于许多非营利组织经常要求 web 应用程序，我策划了一个 Flask 样板文件，让团队能够快速启动并运行他们的后端 REST API 服务。常见的实用函数、应用程序结构、数据库包装器和连接都与设置文档、最佳编码实践和 Heroku 部署步骤一起提供。

#### 开发环境和依赖性的问题

然而，由于我们每学期都有新的学生软件开发人员加入，团队需要花费大量时间来配置和解决环境问题。我们经常会有多个成员在不同的操作系统上开发，遇到无数的问题(Windows，我指的是你)。尽管这些问题中有许多是微不足道的，比如用正确的用户/密码启动正确的 PostgreSQL 数据库版本，但它浪费了本可以投入到产品本身的时间。

除此之外，我只为只有 bash 指令的 MacOS 用户编写文档(我有一台 Mac)，基本上让 Windows 和 Linux 用户自生自灭。我本来可以启动一些虚拟机，并为每个操作系统再次记录设置，但如果有 Docker，我为什么要这样做呢？

#### 输入 Docker

使用 Docker，整个应用程序可以被隔离在容器中，这些容器可以在机器之间移植。这允许一致的环境和依赖性。因此，你可以“构建一次，在任何地方运行”，开发者现在可以只安装**一个**东西——Docker——并运行几个命令来运行应用程序。新来者将能够迅速开始发展，而不用担心他们的环境。非营利组织也将能够在未来迅速做出改变。

Docker 还有许多其他好处，比如它的可移植性和资源高效性(与虚拟机相比)，以及如何轻松设置持续集成和快速部署应用程序。

### Docker 核心组件概述

网上有很多资源会比我更好地解释 Docker，所以我不会过多地赘述。这里有一篇关于其概念的[很棒的博客文章](https://medium.freecodecamp.org/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b)，还有[在 Docker 上的另一篇](https://medium.com/@xenonstack/docker-overview-a-complete-guide-43decd218eca)。然而，我将回顾一下 Docker 的一些核心组件，它们是理解这篇博文的其余部分所必需的。

#### Docker 图像

Docker 图像是描述 Docker 容器的只读模板。它们包括在定义应用程序及其依赖项的 Dockerfile 文件中编写的特定指令。可以把它们看作是应用程序在某一时刻的快照。当你`docker build`的时候你会得到图像。

#### 码头集装箱

Docker 容器是 Docker 图像的实例。它们包括操作系统、应用程序代码、运行时、系统工具、系统库等等。您可以将多个 Docker 容器连接在一起，例如将 Node.js 应用程序放在一个连接到 Redis 数据库容器的容器中。您将使用`docker start`运行一个 Docker 容器。

#### 码头登记处

Docker 注册表是您存储和分发 Docker 图像的地方。我们将使用 Docker 图片作为 DockerHub 的基础图片，Docker hub 是一个由 Docker 自己托管的免费注册表。

#### 复合坞站

Docker Compose 是一个工具，允许您一次构建和启动多个 Docker 映像。只要提供了特定的配置，您就可以在一个命令中完成所有操作，而不是每次启动应用程序时都运行相同的多个命令。

### 带有烧瓶和 Postgres 的 Docker 示例

记住所有的 Docker 组件后，让我们使用 Postgres 作为数据存储来设置一个带有 Flask 应用程序的 Docker 开发环境。在这篇博文的剩余部分，我将引用前面提到的 Hack4Impact 的库 [Flask 样板文件](https://github.com/tko22/flask-boilerplate)。

在这个配置中，我们将使用 Docker 构建两个映像:

*   `app` —在端口 5000 中提供的烧瓶应用程序
*   `postgres` —在 5432 端口服务的 Postgres 数据库

当您查看顶层目录时，有三个文件定义了此配置:

*   **Dockerfile** —由设置`app`容器的指令组成的脚本。每个命令都是自动的，并且是连续执行的。该文件将位于您运行应用程序的目录中(例如`python manage.py runserver`或`python app.py`或`npm start`)。在我们的例子中，它在顶层目录中(T4 所在的位置)。Docker 文件接受 [Docker 指令](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#build-cache)。
*   **。dockerignore** —指定哪些文件不包含在容器中。它就像`.gitignore`一样，只是针对码头集装箱。该文件与 Dockerfile 文件成对出现。
*   **Docker-Compose . yml**—Docker Compose 的配置文件。这将允许我们同时构建`app`和`postgres`映像，定义卷并声明`app`依赖于`postgres`，并设置所需的环境变量。

**注意:**两个图像只有一个 Docker 文件，因为我们将从 DockerHub 获取一个官方 Docker Postgres 图像！您可以通过为 Postgres 编写自己的 docker 文件来包含自己的 Postgres 图像，但这没有意义。

#### Dockerfile

再次澄清一下，这个 Dockerfile 是针对`app`容器的。总的来说，这里是整个 over 文件——它基本上获取一个基础映像，复制应用程序，安装依赖项，并设置一个特定的环境变量。

```
FROM python:3.6
```

```
LABEL maintainer "Timothy Ko <tk2@illinois.edu>"
```

```
RUN apt-get update
```

```
RUN mkdir /app
```

```
WORKDIR /app
```

```
COPY . /app
```

```
RUN pip install --no-cache-dir -r requirements.txt
```

```
ENV FLASK_ENV="docker"
```

```
EXPOSE 5000
```

因为这个 Flask 应用程序使用 Python 3.6，所以我们需要一个支持它并已经安装了它的环境。幸运的是， [DockerHub](https://hub.docker.com/) 有一个安装在 Ubuntu 上的官方镜像。在一行中，我们将有一个包含 Python 3.6、virtualenv 和 pip 的基本 Ubuntu 映像。DockerHub 上有大量的图片，但是如果你想从一个新的 Ubuntu 图片开始，并在它的基础上构建，你可以这样做。

```
FROM python:3.6
```

然后我注意到我是维护者。

```
LABEL maintainer "Timothy Ko <tk2@illinois.edu>"
```

现在是时候将 Flask 应用程序添加到图像中了。为了简单起见，我决定将应用程序复制到 Docker 映像的`/app`目录下。

```
RUN mkdir /app
```

```
COPY . /app
```

```
WORKDIR /app
```

`WORKDIR`本质上是 bash 中的一个`cd`,`COPY`将某个目录复制到映像中提供的目录。`ADD`是另一个与`COPY`做同样事情的命令，但是它也允许您从 URL 添加存储库。因此，如果您想要克隆您的 git 存储库，而不是从您的本地存储库复制它(出于登台和生产的目的)，您可以使用它。然而，大多数时候应该使用`COPY`，除非你有一个 URL。每次使用`RUN`、`COPY`、`FROM`或`CMD`时，都会在 docker 图像中创建新层，这会影响 Docker 存储和缓存图像的方式。有关最佳实践和分层的更多信息，请参见 [Dockerfile 最佳实践](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)。

现在我们已经将我们的存储库复制到映像中，我们将安装我们所有的依赖项，这在`requirements.txt`中定义

```
RUN pip install --no-cache-dir -r requirements.txt
```

但是假设您有一个节点应用程序而不是 Flask——您应该编写`RUN npm install`。下一步是告诉 Flask 使用我硬编码到`config.py`中的 Docker 配置。在该配置中，Flask 将连接到我们稍后将设置的正确数据库。因为我有生产和常规开发配置，所以只要`FLASK_ENV`环境变量被设置为`docker`，Flask 就会选择 Docker 配置。因此，我们需要在我们的`app`形象中设置这一点。

```
ENV FLASK_ENV="docker"
```

然后，公开运行 Flask 应用程序的端口(5000 ):

```
EXPOSE 5000
```

就是这样！因此，无论你使用什么操作系统，或者你在遵循文档说明方面有多糟糕，你的 Docker 形象都会因为这个 Docker 文件而和你的团队成员一样。

在您构建映像时，将会运行以下命令。您现在可以用`sudo docker build -t app .`构建这个映像。但是，当您用`sudo docker run app`运行它来启动 Docker 容器时，应用程序将遇到数据库连接错误。这是因为您还没有设置数据库。

#### 坞站-组合. yml

Docker Compose 将允许你这样做，同时建立你的`app`图像。整个文件如下所示:

```
version: '2.1'services:  postgres:    restart: always    image: postgres:10    environment:      - POSTGRES_USER=${POSTGRES_USER}      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}      - POSTGRES_DB=${POSTGRES_DB}    volumes:      - ./postgres-data/postgres:/var/lib/postgresql/data    ports:      - "5432:5432"  app:    restart: always    build: .    ports:      - 5000:5000    volumes:      - .:/app
```

对于这个特定的存储库，我决定使用版本 2.1，因为我对它更熟悉，而且它有更多的指南和教程——是的，这是我不使用版本 3 的唯一理由。对于版本 2，您必须提供想要包含的“服务”或图像。在我们的例子中，它是`app`和`postgres`(这些只是您在使用 docker-compose 命令时可以参考的名称。你称它们为`database`和`api`或任何能让你高兴的东西。

#### Postgres 图像

查看 Postgres 服务，我指定它是一个`postgres:10`图像，这是另一个 DockerHub 图像。这个镜像是一个安装了 Postgres 的 Ubuntu 镜像，它将自动启动 Postgres 服务器。

```
postgres:  restart: always  image: postgres:10  environment:    - POSTGRES_USER=${USER}    - POSTGRES_PASSWORD=${PASSWORD}    - POSTGRES_DB=${DB}  volumes:    - ./postgres-data/postgres:/var/lib/postgresql/data  ports:    - "5432:5432"
```

如果您想要不同的版本，只需将“10”更改为其他版本。要指定 Postgres 中需要的用户、密码和数据库，必须事先定义环境变量——这是在官方 postgres Docker 映像的 Docker 文件中实现的。在这种情况下，`postgres`映像将注入`$USER`、`$PASSWORD`和`$DB`环境变量，并使它们成为 postgres 容器中的`POSTGRES_USER`、`POSTGRES_PASSWORD`和`POSTGRES_DB`环境变量**。注意`$USER`和其他注入的环境变量是在您自己的计算机中指定的环境变量(更具体地说，是您用来运行`docker-compose up`命令的命令行进程)。通过注入您的凭证，这允许您不将凭证提交到公共存储库中。**

如果在与`docker-compose.yml`文件相同的目录中有一个`.env`文件，Docker-compose 也会自动注入环境变量。以下是这种情况下的. env 文件示例:

```
USER=testusrPASSWORD=passwordDB=testdb
```

因此，我们的 PostgreSQL 数据库将被命名为 **testdb** ，用户名为 **testusr** ，密码为 **password。**

我们的 Flask 应用程序将连接到这个特定的数据库，因为我在前面提到的 Docker 配置中记下了它的 URL。

每次停止和移除容器时，数据都会被删除。因此，您必须提供一个持久的数据存储，这样就不会删除任何数据库数据。有两种方法可以做到:

*   Docker 卷
*   本地目录装载

我选择将它本地安装到`./postgres-data/postgres`，但是它可以在任何地方。语法总是`[HOST]:[CONTAINER]`。这意味着来自`/var/lib/postgresql/data`的任何数据实际上都存储在`./postgres-data`中。

```
volumes:- ./postgres-data/postgres:/var/lib/postgresql/data
```

我们将对端口使用相同的语法:

```
ports:- "5432:5432"
```

#### 应用程序图像

然后我们将定义`app`图像。

```
app:  restart: always  build: .  ports:    - 5000:5000  volumes:     - .:/app  depends_on:    - postgres  entrypoint: ["python", "manage.py","runserver"]
```

我们先定义它有`restart: always`。这意味着无论何时失败，它都会重新启动。这在我们构建和启动这些容器时特别有用。`app`通常会在`postgres`之前启动，这意味着`app`会尝试连接到数据库并失败，因为`postgres`还没有启动。没有这个属性，`app`就会停止，这就是它的结束。

然后，我们定义希望这个构建成为当前目录中的 docker 文件:

```
build: .
```

下一步对于 Flask 服务器来说非常重要，只要您在本地存储库中更改了任何代码，就需要重新启动。这非常有帮助，所以你不需要每次都一遍又一遍地重建你的图像来查看你的变化。为了做到这一点，我们做了与对`postgres`相同的事情:我们声明容器中的`/app`目录将是任何内容。(当前目录)。因此，本地回购中的任何更改都将反映在容器内部。

```
volumes:  - .:/app
```

在这之后，我们需要告诉 Docker Compose app 依赖于`postgres` 容器。请注意，如果您将图像的名称更改为类似于`database`的其他名称，您必须用该名称替换那个`postgres`。

```
depends_on:  - postgres
```

最后，我们需要提供启动应用程序所需的命令。在我们的例子中，是`python manage.py runserver`。

```
entrypoint: ["python", "manage.py","runserver"]
```

对 Flask 的一个警告是，您必须明确指出您希望在哪个主机(端口)上运行它，以及当您运行它时是否希望它处于调试模式。所以在`manage.py`中，我用:

```
def runserver():    app.run(debug=True, host=’0.0.0.0', port=5000)
```

最后，使用命令行构建并启动 Flask 应用程序和 Postgres 数据库:

```
docker-compose builddocker-compose up -ddocker-compose exec app python manage.py recreate_db
```

最后一个命令实际上是在 Postgres 中创建由我的 Flask 应用程序定义的数据库模式。

就是这样！您应该能够看到 Flask 应用程序运行在 http://localhost:5000！

#### Docker 命令

一开始，记住并找到 Docker 命令可能会非常令人沮丧，所以[这里有](https://medium.com/statuscode/dockercheatsheet-9730ce03630d)一个列表！如果你想参考的话，我也在我的[烧瓶样板文件](https://github.com/tko22/flask-boilerplate)中写了一些常用的。

### 结论

Docker 凭借其可移植性和跨平台的一致环境，真正允许团队更快地开发。虽然我只经历过使用 Docker 进行开发，但当你使用它进行持续集成/测试和部署时，Docker 表现出色。

我可以再添加几行代码，用 Nginx 和 Gunicorn 完成完整的生产设置。如果我想使用 Redis 进行会话缓存或作为一个队列，我可以很快做到，并且我团队中的每个人在重建 Docker 映像时都可以拥有相同的环境。

不仅如此，如果我愿意，我可以在几秒钟内运行 Flask 应用程序的 20 个实例。感谢阅读！:)

如果你有任何想法和评论，欢迎在下面留下你的评论或者发邮件给我，地址是 tk2@illinois.edu！此外，请随意使用我的代码或与您的同行分享！