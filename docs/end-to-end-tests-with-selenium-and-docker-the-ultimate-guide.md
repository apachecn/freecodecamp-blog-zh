# 使用 Selenium 和 Docker 进行端到端测试的终极指南

> 原文：<https://www.freecodecamp.org/news/end-to-end-tests-with-selenium-and-docker-the-ultimate-guide/>

自动化的端到端测试是测试你的应用最有效的方法。如果您目前根本没有测试，它也需要最少的努力来获得测试的好处。你不需要大量的基础设施或云服务器来实现。在本指南中，我们将看到如何轻松克服端到端测试的两个主要障碍。

第一个障碍是硒。您用来编写测试的 API 既难看又不直观。但是使用起来并不困难，通过一些方便的函数，编写 Selenium 测试变得轻而易举。这种努力是值得的，因为您可以端到端地自动测试您的最终用户的流程。

第二个障碍是在一个隔离的环境中将组件放在一起。我们希望前端，后端，数据库和你的应用程序使用的一切。我们将使用 Docker compose 把事情放在一起并自动化测试。即使您的组件在不同的 Git 存储库中，这也很容易。

## 用 Selenium 编写端到端测试

即使你是一个只有 API 的企业，你也有一个前端，一个管理或后台前端。因此，端到端测试最终会与前端应用程序对话。

行业标准工具是 Selenium。Selenium 提供了一个 API 来与 web 浏览器对话并与 DOM 交互。您可以检查显示了哪些元素，填写输入内容并点击鼠标。真正的用户会对你的应用程序做的任何事情，你都可以自动化。

Selenium 使用一种叫做 WebDriver API 的东西。乍一看用起来不是很顺手。但是学习曲线并不陡峭。创建一些方便的功能会让你立刻变得高效。这里就不赘述 WebDriver API 的细节了。可以看看[这篇优秀的文章](https://marmelab.com/blog/2016/04/19/e2e-testing-with-node-and-es6.html)深入挖掘。

还有图书馆让你的生活更轻松。夜巡最受欢迎。

如果你有一个角度应用，[量角器](https://www.protractortest.org/)是你最好的朋友。它集成了角度事件循环，并允许您根据您的模型使用选择器。那是金子。

为你最重要的用户特性或者你的应用编写一个测试应该只需要几个小时，所以开始吧。它会自动运行。让我们看看怎么做。

## 在 Docker 中运行测试

我们需要在一个隔离的环境中运行我们的测试，这样结果是可预测的。因此，我们可以轻松实现[持续集成](https://fire.ci/blog/how-to-get-started-with-continuous-integration/)。我们将使用 Docker compose。

Selenium 提供了开箱即用的 Docker 图像，可以用一个或几个浏览器进行测试。这些图像产生了 Selenium 服务器和底层浏览器。它可以在不同的浏览器上运行。

现在让我们从一个浏览器开始:headless-chrome。可以看到下面的 *docker-compose.yml* 文件(命令来自 Angular 示例)。

注意:如果你从未使用过 Docker，你可以很容易地在你的电脑上安装它。Docker 有一个麻烦的倾向，就是强迫你注册一个账户来下载这个东西。但实际上你不必这么做。转到发行说明(Windows 的[链接和 Mac](https://docs.docker.com/docker-for-windows/release-notes/) 的[链接)，下载之前的版本，而不是最新的版本。这是一个直接下载链接。](https://docs.docker.com/docker-for-mac/release-notes/)

```
version: '3.1'

services:
 app-serve:
   build: .
   image: myapp
   command: npm run serve:production
   expose:
    - 4200

 app-e2e-tests:
   image: myapp
   command: dockerize -wait tcp://app-serve:4200 
             -wait tcp://selenium-chrome-standalone:4444 
             -timeout 10s -wait-retry-interval 1s bash -c "npm run e2e"
   depends_on:
     - app-serve
     - selenium-chrome-standalone

 selenium-chrome-standalone:
   image: selenium/standalone-chrome
   expose:
    - 44444 
```

上面的文件告诉 Docker 构建一个包含 3 个容器的环境:

*   我们要测试的应用程序:容器使用我们将在下面构建的 myapp 映像
*   运行测试的容器:容器使用相同的 myapp 映像。它使用 [dockerize](https://github.com/jwilder/dockerize) 在运行测试之前等待服务器启动
*   Selenium 服务器:容器使用官方的 Selenium 映像。这里无事可做。我们可以从与应用程序相同的容器中运行测试。把它分开会让事情更清楚。它还允许您在结果日志中分离两个容器的输出。

这些容器将生活在一个私有的虚拟网络中，并以 http://the-container-name 的身份互相查看(更多信息请点击 Docker 中的)。

我们需要一个 *Dockerfile* 来构建用于容器的 *myapp* 图像。它应该把你的前端代码变成一个尽可能接近产品的包。在那个阶段运行单元测试和林挺是一个好主意。毕竟，如果基础工作不工作，就没有必要运行端到端的测试。

在下面的 *Dockerfile* 中，我们使用一个节点映像作为基础，安装 dockerize，并捆绑应用程序。为生产而生产很重要。您不想测试预先优化的开发版本。在那里很多事情都会出错。

```
FROM node:12.13.0 AS base

ENV DOCKERIZE_VERSION v0.6.0

RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
   && tar -C /usr/local/bin -xzvf dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
   && rm dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz

RUN mkdir -p ~/app
WORKDIR ~/app

COPY package.json .
COPY package-lock.json .

FROM base AS dependencies

RUN npm install

FROM dependencies AS runtime

COPY . .
RUN npm run lint
RUN npm run test:ci
RUN npm run build --production 
```

现在我们已经把所有的部分都准备好了，让我们使用这个命令来运行测试:

```
docker-compose up --build --abort-on-container-exit 
```

它比较长，所以应该在你的项目中编写脚本。它将基于提供的 *Dockerfile* 构建 myapp 映像，然后启动所有容器。在执行测试之前，Dockerize 会确保您的应用程序和 Selenium 已经启动。

当一个容器存在时， *-在容器退出时中止*选项将终止环境。因为只有我们的测试容器意味着退出(其他的是服务器)，这就是我们想要的。

docker-compose 命令将具有与退出容器相同的退出代码。这意味着您可以很容易地从命令行检测测试是否成功。

您现在可以在本地和任何支持 Docker 的服务器上运行端到端测试了。那挺好的！

不过，目前测试只在一个浏览器上运行。再补充一下吧。

## 在不同浏览器上测试

独立的 Selenium 映像启动一个带有您想要的浏览器的 Selenium 服务器。要在不同的浏览器上运行测试，您需要更新测试的配置，并使用 selenium/hub Docker 映像。

该映像在您的应用程序和独立的 Selenium 映像之间创建了一个中枢。替换 docker-compose.yml 中的 selenium 容器部分，如下所示:

```
 selenium-hub:
    image: selenium/hub
    container_name: selenium-hub
    expose:
      - 4444
  chrome:
    image: selenium/node-chrome
    volumes:
      - /dev/shm:/dev/shm
    depends_on:
      - selenium-hub
    environment:
      - HUB_HOST=selenium-hub
      - HUB_PORT=4444
  firefox:
    image: selenium/node-firefox
    volumes:
      - /dev/shm:/dev/shm
    depends_on:
      - selenium-hub
    environment:
      - HUB_HOST=selenium-hub
      - HUB_PORT=4444 
```

我们现在有 3 个容器:Chrome、Firefox 和 Selenium hub。

Selenium 提供的所有 Docker 图片都在这个库里[。](https://github.com/SeleniumHQ/docker-selenium)

小心点。有一个微妙的时间效应需要考虑。我们使用 dockerize 让测试容器等待 Selenium hub 启动。这还不够，因为我们需要等待独立的映像准备好——实际上是将它们自己注册到中心。

我们可以通过等待独立映像公开端口来做到这一点，但这并不是一种保证。在开始测试之前等待几秒钟更容易。更新您的测试脚本，在测试开始之前等待 5 秒钟(您可以在 dockerize 调用之后添加一个 sleep 命令)。

现在，您可以确信，在所有浏览器都准备好之前，您的测试不会开始。等待并不理想，但几秒钟是值得的。没有什么比因为不稳定的自动化而导致测试失败更令人恼火的了。

很好。我们现在已经讨论了前端部分。让我们添加后端。

## 将后端添加为容器或 git 模块

如果只测试应用程序的前端部分，上面的内容可能会显得有些多余。但是我们的目标远不止这些。我们想测试整个系统。

让我们向 Docker 合成环境添加一个数据库和一个后端。

如果你是前端开发人员，你可能会想“我们是前端团队——我们不关心测试后端。”你确定吗？

> 前端总是最后一个集成所有其他部分的部分。这意味着关键时刻。如果您能够连续测试前端和后端，并更快地发现错误，这种紧张的时间将不复存在。

我在这里描述的技术非常容易应用，即使您的后端在不同的存储库中。

这就是 *docker-compose.yml* 的样子:

```
version: '3.1'

services:
 db:
   image: postgres
   environment:
     POSTGRES_USER: john
     POSTGRES_PASSWORD: mysecretpassword
   expose:
     - 5432
 backend:
   context: ./backend
   dockerfile: ./backend/Dockerfile
   image:mybackend
   command: dockerize
       -wait tcp://db:5432 -timeout 10s
       bash -c "./seed_db.sh && ./start_server.sh"
   environment:
     APP_DB_HOST: db
     APP_DB_USER: john
     APP_DB_PASSWORD: mysecretpassword
   expose:
     - 8000
   depends_on:
     - db
 app-serve:
   build: .
   image: myapp
   command: npm run serve:sw
   expose:
    - 4200
 app-e2e-tests:
   image: myapp
   command: dockerize -wait tcp://app-serve:4200 
        -wait tcp://backend:8000 
        -wait tcp://selenium-chrome-standalone:4444 -timeout 10s 
        -wait-retry-interval 1s bash -c "npm run e2e:docker"
   depends_on:
     - app-serve
     - selenium-chrome-standalone
 selenium-chrome-standalone:
   image: selenium/standalone-chrome
   expose:
    - 44444 
```

在这个例子中，我们添加了一个 postgres 数据库和一个容器供后端运行。Dockerize 同步容器的命令。

如果您的系统有多个后端组件，请根据需要添加任意数量的容器。您需要正确连接容器依赖关系。这意味着将正确的主机名作为组件上的环境变量。和启动顺序(如果一些组件依赖于其他组件的话)。

您编写的 Selenium 测试应该不需要任何修改。您可能需要将测试数据放入数据库中。为了将这一步保留在测试区域，我们在后端启动脚本之前添加了播种脚本。这样我们就可以确定事情是按照正确的顺序发生的:

*   数据库启动并准备好接受连接
*   一个脚本播种数据库数据
*   后端和前端开始，这样测试就可以开始了

### 单一知识库

如果你看看后端容器，你可以看到有一个陷阱。它使用一个名为 *mybackend* 的映像，这个映像是从位于 *backend/Dockerfile* 的文件构建的。这意味着您的后端在同一个 git 存储库中，位于一个名为*后端*的文件夹中。这个名字当然只是一个例子。

如果您的后端和前端在同一个存储库中，那么您就是优秀的。定义 docker 文件来构建您的后端，并根据您的需要调整启动命令。

这很好，但是通常后端不在同一个存储库中。或者您可以在不同的存储库中拥有许多后端组件。那你会怎么做？

### 多个存储库

超级干净的解决方案是在每个后端组件存储库上有一个 CI 过程。

如果你没有，你可以通过 Docker 指南查看 [API 端到端测试来开始。它使用了与本文相同的技术，所以您在整个项目中有一个一致的设置。](https://fire.ci/blog/api-end-to-end-testing-with-docker/)

每个组件的 CI 流程运行自动化测试。成功后，它将带有组件的 docker 映像推送到私有 Docker 注册表。我们上面的 *docker-compose.yml* 文件中的后端容器将使用这个图像。

这个解决方案需要一个私人的 Docker 注册表来存储您的图像。你可以使用 [Docker Hub](https://hub.docker.com/) ，但之后它就变成了公共的。如果你还没有，也不打算这么做，那就不值得你去努力。

另一个解决方案是使用 git 中的子模块特性。您的后端存储库成为您的前端存储库的虚拟子库。你只需要添加文件*。像这样的 gitmodules* 到您的前端存储库:

```
[submodule "backend"]
  path = backend
  url = git@your:backend/repository.git
  branch = develop 
```

运行命令*git submodule update-remote*，该命令会将后端存储库的指定分支拉入一个名为“back end”的文件夹中。如果有多个后端组件，可以根据需要添加任意数量的子模块。

就是这样。让您的配置项运行 submodule 命令，从文件系统的角度来看，就好像您在 monorepository 中一样。

如果在开发前端的时候，你不想要本地的后端代码，就不要运行这个命令。您将拥有一个空的后端文件夹。

### 版本控制和后端/前端不兼容

上面的两种技术用你的后端的最新“CI 测试通过”版本测试前端。如果您的组件有时不兼容，这可能会导致构建失败。

如果它们经常是兼容的，坚持“总是用最新版本测试”的方法。您可以即时修复偶尔出现的不兼容问题。

但是，如果不兼容的问题依然存在，那就行不通了。在这种情况下，您需要手动控制版本更新。这很容易做到。

您可以在 *docker-compose.yml* 文件或*中锁定组件的版本。gitmodules* 文件。当推送到 Docker 注册表时，您可以用相应代码的提交号来标记组件映像。相关的 docker-compose.yml 文件部分变成:

```
backend:
  image: backendapp:34028fc 
```

同样地*。gitmodules* 文件的目标不是分支头，而是给定的提交:

```
[submodule "backend"]
  path = backend
  url = git@your:backend/repository.git
  branch = 34028fc 
```

额外收获:版本更新是根据您的代码进行版本控制的。您可以跟踪每个构建使用了哪个版本。这在修复失败的构建或试图重现旧的 bug 时非常有用。

我们可以把这个方法推进到下一个层次。您可以拥有一个专用的存储库，将所有组件作为 git 模块连接起来。碰撞版本可能是交付和移交给测试/QA 团队的一种形式。

理论上，最好是让最新版本的组件经常一起工作。并且不再需要手动版本控制。如果不是这样，那也没关系。忽略那些会告诉你没有遵循最佳实践等等的纯粹主义者。

> 如果你刚刚开始，不要一开始就把目标定在星星上。选择最适合你的方式来享受自动化测试的好处。然后继续改进你的过程。

## 编写可维护的 Selenium 测试的好处

回到 Selenium 和三个重要的建议来帮助你编写好的 UI 测试。

首先，尽可能避免 CSS 选择器。Selenium 在 DOM 上工作，可以通过 id、CSS 或 XPath 识别元素。尽可能多地使用 id，即使你不得不将它们添加到你的应用程序代码中。CSS 和 XPath 选择器不稳定。一旦应用程序结构发生变化，它们就会被破坏。

第二，使用页面对象方法。它是关于封装你的应用程序，这样选择器就不会直接在测试中使用。如果您的页面 HTML/CSS 发生变化，您的测试将不得不重写以使用新的选择器。页面对象抽象出选择器，并把它们变成用户动作。这里有一篇关于如何正确使用页面对象的文章。

第三，不要在测试中构建漫长的用户旅程。如果您的测试在第 50 次操作时失败，将很难重现和修复。创建测试套件，扮演从登录页面开始的部分场景。通过这种方式，您总是只需点击几下鼠标就能发现测试中的 bug。

也不要冒险编写依赖于先前动作状态的测试。测试套件耦合是您想要避免的。

让我们举一个实际的例子。假设您正在测试一个 SaaS 学校应用程序。用例可能是:

*   创建一个类
*   注册孩子和家长的数据
*   为班级制定周计划
*   检查缺勤
*   输入等级

在这个过程中，您将有登录过程和一些导航检查。

您可以编写一个测试来遍历整个链，如上所述。这很方便，因为要声明孩子，你需要一个类存在。为了检查缺席情况，你需要一个周计划。你需要孩子们输入分数。首先构建一个测试套件来做所有这些事情，这是一个快速的胜利。

如果你目前什么都没有，并且想快速达到良好的测试覆盖率:去做吧！如果 Done 现在允许您捕捉应用程序中的错误，那么它就比 perfect 更好。

更干净的解决方案是使用基线场景来启动更小的测试套件。在上面的例子中，基线场景应该是创建一个类并注册孩子的数据。

创建一个测试套件来完成这个任务:创建一个类，并注册孩子和父母的数据。总是先运行它。如果这不起作用，那么你不需要继续前进。这个版本的代码永远不会到达最终用户手中。

然后创建一个封装基线场景的函数。在某种程度上，这将是与之前的测试套件重复的代码。但是它将允许您使用一行函数作为所有其他测试套件的设置挂钩。这是两全其美的:以最少的努力从应用程序的新状态开始测试场景。

## 结论

我希望这篇文章能让你对如何快速建立复杂系统的端到端测试有所了解。多个存储库中的多个组件不应该成为障碍。Docker compose 使得把东西放在一起变得很容易。

端到端测试是避免危机的最好方法。在复杂的系统中，一些组件的延迟交付会给其他团队带来负担。整合很快就完成了。代码质量下降。这是一个恶性循环。解决方案是经常测试并尽早发现交叉组件错误。

硒含量测试可以快速进行。这完全没问题。自动化事物。然后改进。请记住:

> 一年中的任何一天，完成都比完美好。

感谢阅读！

如果你想要更多我这样的文章，你可以在[火词博客](https://fire.ci/blog)上找到。