# 如何使用 Docker Compose 运行多个容器

> 原文：<https://www.freecodecamp.org/news/run-multiple-containers-with-docker-compose/>

Docker 在过去几年变得越来越受欢迎。这样做的一个原因是，您可以创建快速且易于部署的可移植容器。

正如 Docker 的[网站](https://www.docker.com/resources/what-container/)所描述的那样，**容器**是将你的代码和任何其他依赖项打包在一起的东西，这样它就可以可靠地跨多个平台部署。

这些容器可以在您的 Windows、Mac 和 Linux 上本地运行。像 AWS 或 Azure 这样的主要云系统都支持开箱即用。你也可以在任何可以安装和运行 Docker 的托管空间上使用它。

如果你想学习更多的 Docker 基础知识，并且需要 Docker CLI 的备忘单，我[在这里](https://www.daveops.co.in/post/docker-a-beginner-s-cheat-sheet-2022)写了一篇关于它的介绍性文章。

在本教程中，我们将更深入，这样你就可以理解一些更高级的功能，如如何运行多个容器。

## 为什么坞站会拨号？

使用 Docker compose，您可以用一个 yaml 文件配置和启动多个容器。如果您正在使用包含多种技术的技术堆栈，这将非常有帮助。

举个例子，假设你正在做一个项目，这个项目使用 MySQL 数据库，Python 用于 AI/ML，NodeJS 用于实时处理，还有。NET 为 API 提供服务。为每个团队成员设置这样的环境是很麻烦的。幸运的是，在 compose 的帮助下，Docker 使这变得更容易。

### Docker Compose 是如何工作的？

`docker compose`是一个 yaml 文件，我们可以在其中配置不同类型的服务。然后，通过一个简单的命令，所有的容器都将被构建并启动。

使用 compose 涉及 3 个主要步骤:

*   为每个项目生成一个 docker 文件。
*   在 docker-compose.yml 文件中设置服务。
*   点燃容器。

我们现在要看看如何使用`docker compose`来帮助你为一个使用一堆不同工具的项目建立一个环境，就像我们上面讨论的那样。

### 先决条件

你可能认为你必须安装所有的技术来运行 MySQL、Python、NodeJS 等技术栈。NET 和 PHP。

但实际上，你需要的只是一个运行的 Docker 引擎。Docker 的最新版本安装了 Docker compose。现在你不需要安装其他任何东西。

## 我们将在本教程中做什么

在我们开始之前，这里有一个我们将要做的事情的简要概述。我们将逐一解决每项技术。

对于每项技术，我们将创建一个示例应用程序(MySQL 除外)，并为每项技术创建一个 docker 文件。

然后我们将这些 docker 文件指向我们的`docker compose` yaml 文件。

最后，我们将配置`docker compose`以便每个应用程序做它应该做的事情。

在我们开始之前，创建一个名为`super-app`的文件夹。接下来，创建一个`docker-compose.yml`文件。在这个文件中，我们将配置所有的应用程序。所以让我们开始吧。

对于那些对代码感兴趣的人，可以访问这里的[库。](https://github.com/shenanigan/super-app-docker)

### 如何配置 MySQL

在 docker-compose.yml 文件中添加以下内容:

```
version: '3.4'
services:
  super-app-db:
    image: mysql:8.0.28
    environment:
      MYSQL_DATABASE: 'super-app'
      MYSQL_ROOT_PASSWORD: '$SuperApp1'
    ports:
      - '3306:3306'
    expose:
      - '3306'
```

在`services`部分，我们将列出所有需要配置的应用类型。

首先，我们配置了一个`super-app-db` 服务，该服务从 8.0.28 版本的 MySQL 中提取一个 Docker 映像。

接下来，我们指示容器创建一个名为`super-app`的数据库，默认用户为`root`，密码设置为 *$SuperApp1* 。

最后，由于 MySQL 的默认端口是 3306，我们将其映射到容器的端口 3306，并公开该端口以供访问。

一旦创建了上面的文件，运行下面的命令来创建 Docker 映像，并将其作为容器运行。

```
docker compose up
```

MySQL 镜像将被提取，然后 Docker 将启动一个容器来运行这个镜像。可以通过 MySQL 客户端连接 MySQL 服务器进行验证。

如果没有，不要担心-我们将在下面看到如何通过我们的应用程序连接到它。只要容器没有被删除，表就会被持久化。

让我们配置下一个应用程序节点。

### 如何配置节点

我们将创建一个非常简单的 Express 节点应用程序。为此，在我们的 super-app 文件夹中创建一个名为 node 的文件夹。

在节点文件夹中添加文件 server.js、package.json 和 Dockerfile。

server.js:

```
const server = require("express")();
server.listen(3000, async () => { });
server.get("/super-app", async (_, response) => {
    response.json({ "super": "app" });
});
```

package.json:

```
{
    "name": "super-app-node",
    "dependencies": {
        "express": "^4.17.1"
    }
}
```

Dockerfile:

```
# Download the slim version of node
FROM node:17-slim

# Set the work directory to app folder. 
# We will be copying our code here
WORKDIR /node

#Copy package.json file in the node folder inside container
COPY package.json .

# Install the dependencies in the container
RUN npm install

# Copy the rest of the code in the container
COPY . .

# Run the node server with server.js file
CMD ["node", "server.js"]

# Expose the service over PORT 3000
EXPOSE 3000
```

这里我们创建了一个节点应用程序，当我们在浏览器中点击 localhost:3000/super-app 时，它会返回 JSON。现在，我们不会直接从这个文件夹运行项目。

相反，返回到您的超级应用程序文件夹，将下面几行添加到 docker-compose.yml 文件中:

```
 super-app-node:
    build: ./node
    ports:
      - "3000:3000"
```

我们只是提到创建一个名为 super-app-node 的服务。我们还将容器端口映射到主机端口 3000。

最后，运行下面的命令来运行您的两个容器(MySQL 和 NodeJS):

```
docker compose up
```

现在，如果你点击 localhost:3000/super-app，你会看到一个响应{"super":"app"}。同时你的 MySQL 服务也是。耶！我们已经使用 docker 合成文件成功创建了两个容器。

下一个应用程序。让我们创建一个与数据库交互并返回字符串列表的. NET 应用程序。

### 如何配置。NET 6.0

我们想要。NET 应用程序来连接数据库。它将通过 GET API 从数据库中获取数据，并在浏览器中显示。

为此，在我们的超级应用程序项目中创建一个名为 dotnet 的文件夹。

### 如何设置项目

要开始，安装[。NET 6.0 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/6.0) 如果你还没有安装的话。启动下面的命令来创建一个新的点网应用程序:

```
dotnet new webapi --name dotnet
```

它将创建一个新的。NET 项目以及控制器和一些其他文件。为了支持与 MySQL 的连接，我们需要添加一个 nuget 包。我们还将加入微软。EntityFrameworkCore，它基本上是一个用于连接数据库的 ORM。

为此，在新创建的*中执行以下命令。*网络项目:

```
dotnet add package Pomelo.EntityFrameworkCore.MySql --version 6.0.1
dotnet add package Microsoft.EntityFrameworkCore --version 6.0.4
dotnet add package Microsoft.EntityFrameworkCore.Design --version 6.0.4
```

因为我们不再需要 WeatherForecast.cs 文件，所以您可以删除它。相反，在 Job.cs 和 User.cs 中创建另外两个实体，如下所示:

```
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace dotnet;
public class User
{
    [Key]
    [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int Id { get; set; }

    public string FirstName { get; set; }
} 
```

```
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace dotnet;
public class Job
{
    [Key]
    [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int Id { get; set; }
    public string Name { get; set; }
    public int UserId { get; set; }
    [ForeignKey("UserId")]
    public virtual User User { get; set; }
}
```

我们还需要一个 DbContext 子类来访问这些实体。创建一个文件名 MySQLDBContext.cs，并添加以下内容:

```
using Microsoft.EntityFrameworkCore;

namespace dotnet;
public class MySQLDBContext : DbContext
{
    public DbSet<User> User { get; set; }
    public DbSet<Job> Job { get; set; }
    public MySQLDBContext(DbContextOptions<MySQLDBContext> options) : base(options) { }
}
```

我们想要配置。NET 使用这个 DbContext 类进行 O/RM 映射。导航到您的 Program.cs 文件，并用以下内容替换其中的内容:

```
using dotnet;
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Configuration;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddDbContext<MySQLDBContext>(options =>
    {
    var connectionString = builder.Configuration.GetConnectionString("DefaultConnection");
    options.UseMySql(connectionString, ServerVersion.AutoDetect(connectionString));
    });

builder.Services.AddControllers();
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

// Remove this line
// app.UseHttpsRedirection();

app.UseAuthorization();

app.MapControllers();

app.Run();
```

容器内的应用程序不需要 HTTPS 重定向。因为 HTTPS 应该由服务器处理，所以从 Program.cs 中删除行`app.UseHtttpsRedirecttion();`:

注:自。NET 6.0 中，Startup.cs 文件被删除，而 Program.cs 用于所有配置。

因为我们使用的配置从 ConnectionStrings 中获取 DefaultConnection，所以我们必须将它添加到 appsettings 文件中。

为此，请将 appsettings.development.json 和 appsettings.json 文件的内容设置如下。请注意，我们使用 super-app-db 作为服务器名称，因为它是我们的 MySQL 容器名称。

```
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {  
    "DefaultConnection": "server=super-app-db; port=3306; database=super-app; user=root; password=$SuperApp1; Persist Security Info=False; Connect Timeout=300"  
  } 
}super-app-db
```

接下来，我们将创建一个 GET API，它返回数据库中的作业对象列表。为此，请删除 WeatherForecastController.cs 并添加一个包含以下内容的 UserController.cs 文件。

```
using System.Collections.Generic;
using System.Linq;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;

namespace dotnet.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class JobController : Controller
    {
        private MySQLDBContext _dbContext;  

        public JobController(MySQLDBContext context)  
        {  
            _dbContext = context;  
        }  

        [HttpGet]  
        public IList<Job> Get()  
        {  
            return (this._dbContext.Job.Include(x => x.User).ToList());  
        } 
    }
}
```

我们都设置了代码。但是我们仍然需要建立我们的数据库。为此，我们将在 super-app 数据库中创建一个用户和作业表。

### 。NET EF 工具

。NET 的实体框架核心提供了一个非常方便的实现方式。首先通过执行以下命令安装 dotnet-ef CLI 工具:

```
dotnet tool install --global dotnet-ef
```

一旦安装，我们将使用代码优先的方法，并创建我们的实体迁移，然后将被推到我们的数据库。

```
dotnet ef migrations add InitialCreate
dotnet ef database update
```

上面两条语句一旦执行，将创建数据库和其中的表，并设置两个表之间的关系。

### 如何向 MySQL 添加数据

为了从数据库中获取数据，我们首先需要在表中添加数据。安装任何 MySQL 客户端来连接数据库。我个人最喜欢的是 [DBeaver](https://dbeaver.io/) 。

现在，您可以从 DBeaver 添加数据，首先添加一个连接，详细信息如下:Host=super-app-db，Port=3306，User=root & password=$SuperApp1。

连接后，导航到 super-app 数据库，打开用户表，添加一行并保存数据。同样，导航到职务表，添加一行，然后保存数据。我们的数据库现在都设置好了。

#### 如何配置 Docker

一旦项目建立并运行，就应该使用 Dockerfile 和 docker compose 来配置它在 Docker 中运行。

在 dotnet 文件夹中创建一个 Dockerfile ，内容如下:

```
#Get the SDK image to build and publish the project
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-env
WORKDIR /app

# Copy everything
COPY . ./

# Restore as distinct layers
RUN dotnet restore

# Build and publish a release
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app

#Copy the build file to the app directory
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "dotnet.dll"]

#Expose the port for communication
EXPOSE 80
```

现在回到 docker-compose.yml 文件，添加以下内容:

```
 super-app-dotnet:
    build: ./dotnet
    ports:
    - "8080:80"
```

这里，我们将主机的端口 8080 绑定到容器的端口 80。目前就这些。执行以下命令启动所有容器:

```
docker compose up
```

最后，在浏览器中点击 [localhost:8080/api/job](http://127.0.0.1:8080/api/job) 。GET api 将从数据库中获取作业列表。

### 如何用 Docker 配置 Python

到目前为止，您可能已经猜到我们需要在超级应用程序文件夹中创建一个 python 文件夹😊。

其次，创建我们项目所需的三个文件:ai-ml.py、requirements.txt 和 Dockerfile，内容如下:

ai-ml.py:

```
import matplotlib.pyplot as plt
import pandas as pd
from scipy import signal

if __name__ == "__main__":
    print("All working good")
```

requirements.txt:

```
pandas
scipy
matplotlib
```

Dockerfile:

```
# Get the python image
FROM python:3.7.13-slim

# Switch to app directory
WORKDIR /app

# Copy the requirements in to the app
COPY requirements.txt ./

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy everything else
COPY . .

#Run the python script
CMD [ "python", "./ai-ml.py" ]
```

最后，返回 docker-compose.yml 文件，添加以下内容:

```
 super-app-python:
    build: ./python
```

就这么简单。由于这只是一个简单的脚本，它将运行一次，然后容器将退出。但是容器的日志将显示*所有工作良好的*打印。这就是 Python 的全部内容。

### 如何用 Docker 配置 PHP

用 Docker 设置 PHP 是最简单的部分之一。创建两个文件 index.php 和 Dockerfile 如下:

索引. php:

```
<?php echo "I am running in a container."; ?>
```

Dockerfile:

```
# Get the php apache image
FROM php:8.0-apache

# Switch to app directory
WORKDIR /var/www/html

# Copy everything
COPY . .

EXPOSE 80
```

最后，将下面的内容添加到 docker-compose.yml 中。

```
 super-app-php:
    build: ./php
    ports:
    - "8000:80"
```

最后，用`docker compose up`再次点燃所有容器。当你点击 [http://localhost:8000](http://localhost:8000) 时，一条漂亮的消息说*“我正在一个容器中运行。”*就会出现。

这是最终的[存储库](https://github.com/shenanigan/super-app-docker)，包含所有 other 文件和其他设置信息。

## 结论

Docker 是一个很棒的装箱工具，通过 docker compose 它变得更加强大。它允许您并排运行多个容器，而不会相互干扰。你绝对应该把它放在你的工具库中。

你觉得这篇文章怎么样？你还能想到 Docker 的哪些用例？有任何反馈或意见吗？

如果你从 Docker 开始，你可以在这里找到更多关于它的信息。