# 使用 Docker 进行端到端 API 测试的完整指南

> 原文：<https://www.freecodecamp.org/news/end-to-end-api-testing-with-docker/>

总的来说，测试是一种痛苦。有些人看不到这一点。有些人看到了，但认为这是让他们慢下来的额外一步。有时测试是有的，但是运行时间很长或者不稳定。在本文中，您将看到如何使用 Docker 为自己设计测试。

我们希望用最少的努力来编写和维护快速、有意义和可靠的测试。它意味着对你作为一个开发人员来说每天都有用的测试。它们应该能提高你的生产力和软件质量。因为每个人都说“你应该做检查”而做检查，如果这会让你慢下来，那就不好了。

让我们看看如何不费吹灰之力实现这一点。

## 我们要测试的例子

在本文中，我们将测试一个用 Node/express 构建的 API，并使用 chai/mocha 进行测试。我选择了 JS'y 堆栈，因为代码非常短，易于阅读。应用的原则对任何技术栈都有效。即使 Javascript 让你恶心，也要坚持读下去。

该示例将为用户提供一组简单的 CRUD 端点。掌握这个概念并应用于 API 的更复杂的业务逻辑已经足够了。

我们将为 API 使用一个非常标准的环境:

*   Postgres 数据库
*   a 重定向群集
*   我们的 API 将使用其他外部 API 来完成它的工作

您的 API 可能需要不同的环境。本文中应用的原则将保持不变。您将使用不同的 Docker 基础映像来运行您可能需要的任何组件。

## 为什么是 Docker？事实上，Docker 作曲

本节包含了许多支持使用 Docker 进行测试的论点。如果你想马上进入技术部分，可以跳过它。

## 痛苦的选择

要在接近生产的环境中测试您的 API，您有两种选择。您可以在代码级别模拟环境，或者在带有数据库的真实服务器上运行测试。已安装。

在代码级别嘲笑一切会使我们的 API 的代码和配置变得混乱。它通常也不能很好地代表 API 在生产中的表现。在真正的服务器上运行这个东西需要大量的基础设施。这需要大量的设置和维护工作，并且无法扩展。有了共享的数据库，您可以一次只运行一个测试，以确保测试运行不会相互干扰。

Docker Compose 让我们可以两全其美。它创造了我们使用的所有外部零件的“容器化”版本。这是嘲弄，但在我们的代码之外。我们的 API 认为是在真实的物理环境中。Docker compose 还将为给定测试运行的所有容器创建一个隔离网络。这允许您在本地计算机或 CI 主机上并行运行其中的几个。

## 过度杀戮？

您可能会想，用 Docker compose 执行端到端测试是不是有些过头了。不如直接运行单元测试吧？

在过去的 10 年里，大型的整体应用程序被分割成较小的服务(趋向于流行的“微服务”)。给定的 API 组件依赖于更多的外部部分(基础设施或其他 API)。随着服务变得越来越小，与基础设施的集成成为工作中更大的一部分。

您应该在生产环境和开发环境之间保持一个小的差距。否则，在进行生产部署时会出现问题。顾名思义，这些问题出现在最糟糕的时刻。它们将导致仓促的修复、质量下降以及团队的挫败感。没人想这样。

您可能想知道使用 Docker compose 的端到端测试是否比传统的单元测试运行时间更长。不完全是。你将在下面的例子中看到，我们可以很容易地将测试保持在 1 分钟以内，并且有很大的好处:测试反映了现实世界中的应用程序行为。这比知道你的类在应用程序中间的某个地方是否工作正常更有价值。

此外，如果您现在没有任何测试，从头到尾开始会让您事半功倍。您将知道应用程序的所有堆栈在最常见的场景下都能协同工作。那已经很了不起了！从那里，你总是可以提炼出一个策略来对你的应用程序的关键部分进行单元测试。

## 我们的第一个测试

让我们从最简单的部分开始:我们的 API 和 Postgres 数据库。让我们运行一个简单的 CRUD 测试。一旦我们有了框架，我们就可以向组件和测试添加更多特性。

下面是我们使用 GET/POST 创建和列出用户的最小 API:

```
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');

const config = require('./config');

const db = require('knex')({
  client: 'pg',
  connection: {
    host : config.db.host,
    user : config.db.user,
    password : config.db.password,
  },
});

const app = express();

app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());
app.use(cors());

app.route('/api/users').post(async (req, res, next) => {
  try {
    const { email, firstname } = req.body;
    // ... validate inputs here ...
    const userData = { email, firstname };

    const result = await db('users').returning('id').insert(userData);
    const id = result[0];
    res.status(201).send({ id, ...userData });
  } catch (err) {
    console.log(`Error: Unable to create user: ${err.message}. ${err.stack}`);
    return next(err);
  }
});

app.route('/api/users').get((req, res, next) => {
  db('users')
  .select('id', 'email', 'firstname')
  .then(users => res.status(200).send(users))
  .catch(err => {
      console.log(`Unable to fetch users: ${err.message}. ${err.stack}`);
      return next(err);
  });
});

try {
  console.log("Starting web server...");

  const port = process.env.PORT || 8000;
  app.listen(port, () => console.log(`Server started on: ${port}`));
} catch(error) {
  console.error(error.stack);
}
```

下面是我们用柴写的测试。测试创建一个新用户并取回它。您可以看到测试没有以任何方式与我们的 API 代码耦合。`SERVER_URL`变量指定要测试的端点。它可以是本地或远程环境。

```
const chai = require("chai");
const chaiHttp = require("chai-http");
const should = chai.should();

const SERVER_URL = process.env.APP_URL || "http://localhost:8000";

chai.use(chaiHttp);

const TEST_USER = {
  email: "john@doe.com",
  firstname: "John"
};

let createdUserId;

describe("Users", () => {
  it("should create a new user", done => {
    chai
      .request(SERVER_URL)
      .post("/api/users")
      .send(TEST_USER)
      .end((err, res) => {
        if (err) done(err)
        res.should.have.status(201);
        res.should.be.json;
        res.body.should.be.a("object");
        res.body.should.have.property("id");
        done();
      });
  });

  it("should get the created user", done => {
    chai
      .request(SERVER_URL)
      .get("/api/users")
      .end((err, res) => {
        if (err) done(err)
        res.should.have.status(200);
        res.body.should.be.a("array");

        const user = res.body.pop();
        user.id.should.equal(createdUserId);
        user.email.should.equal(TEST_USER.email);
        user.firstname.should.equal(TEST_USER.firstname);
        done();
      });
  });
});
```

很好。现在，为了测试我们的 API，让我们定义一个 Docker 组合环境。一个名为`docker-compose.yml`的文件将描述 Docker 需要运行的容器。

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

  myapp:
    build: .
    image: myapp
    command: yarn start
    environment:
      APP_DB_HOST: db
      APP_DB_USER: john
      APP_DB_PASSWORD: mysecretpassword
    expose:
      - 8000
    depends_on:
      - db

  myapp-tests:
    image: myapp
    command: dockerize
        -wait tcp://db:5432 -wait tcp://myapp:8000 -timeout 10s
        bash -c "node db/init.js && yarn test"
    environment:
      APP_URL: http://myapp:8000
      APP_DB_HOST: db
      APP_DB_USER: john
      APP_DB_PASSWORD: mysecretpassword
    depends_on:
      - db
      - myapp
```

那么我们这里有什么？有 3 个容器:

*   db 创建了一个新的 PostgreSQL 实例。我们使用 Docker Hub 的公共 Postgres 图像。我们设置数据库用户名和密码。我们告诉 Docker 公开数据库将监听的端口 5432，以便其他容器可以连接
*   myapp 是运行我们的 API 的容器。`build`命令告诉 Docker 从我们的源实际构建容器映像。其余的就像 db 容器:环境变量和端口
*   **myapp-tests** 是将执行我们的测试的容器。它将使用与 myapp 相同的图像，因为代码已经在那里了，所以没有必要再次构建它。在容器上运行的命令`node db/init.js && yarn test`将初始化数据库(创建表等)。)并运行测试。我们使用 dockerize 来等待所有需要的服务器启动并运行。`depends_on`选项将确保容器以一定的顺序开始。它不能确保 db 容器中的数据库确实准备好接受连接。也不是说我们的 API 服务器已经启动了。

环境的定义就像 20 行非常容易理解的代码。唯一聪明的部分是环境定义。用户名、密码和 URL 必须一致，这样容器才能真正协同工作。

需要注意的一点是，Docker compose 会将它创建的容器的主机设置为容器的名称。所以数据库在`localhost:5432`下不可用，但在`db:5432`下可用。同样，我们的 API 将在`myapp:8000`下提供服务。这里没有任何类型的本地主机。

这意味着当涉及到环境定义时，您的 API 必须支持环境变量。没有硬编码的东西。但那与 Docker 或这篇文章无关。一个可配置的应用是第三点 [12 要素应用宣言](https://12factor.net/)，所以你应该已经在做了。

我们需要告诉 Docker 的最后一件事是如何实际构建容器 **myapp** 。我们使用如下所示的 docker 文件。内容是特定于您的技术栈的，但是想法是将您的 API 捆绑到一个可运行的服务器中。

下面的例子是我们的节点 API installdockerize，安装 API 依赖项，并在容器中复制 API 的代码(服务器是用原始 JS 编写的，所以不需要编译它)。

```
FROM node AS base

# Dockerize is needed to sync containers startup
ENV DOCKERIZE_VERSION v0.6.0
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz

RUN mkdir -p ~/app

WORKDIR ~/app

COPY package.json .
COPY yarn.lock .

FROM base AS dependencies

RUN yarn

FROM dependencies AS runtime

COPY . .
```

通常，从第`WORKDIR ~/app`行开始，您将运行构建您的应用程序的命令。

下面是我们用来运行测试的命令:

```
docker-compose up --build --abort-on-container-exit
```

这个命令将告诉 Docker compose 启动我们的`docker-compose.yml`文件中定义的组件。通过执行上面的`Dockerfile`的内容，`--build`标志将触发 myapp 容器的构建。一旦一个容器退出，`--abort-on-container-exit`将告诉 Docker compose 关闭环境。

这样做很好，因为在测试执行后，唯一要退出的组件是测试容器 **myapp-tests** 。蛋糕上的樱桃，`docker-compose`命令将以与触发退出的容器相同的退出代码退出。这意味着我们可以从命令行检查测试是否成功。这对于 CI 环境中的自动化构建非常有用。

这难道不是完美的测试设置吗？

完整的例子是 GitHub 上的[。您可以克隆存储库并运行 docker compose 命令:](https://github.com/fire-ci/tuto-api-e2e-testing)

```
docker-compose up --build --abort-on-container-exit
```

当然你需要安装 Docker。Docker 有一个麻烦的倾向，就是强迫你注册一个账户来下载这个东西。但实际上你不必这么做。转到发行说明(Windows 的[链接和 Mac](https://docs.docker.com/docker-for-windows/release-notes/) 的[链接)，下载之前的版本，而不是最新的版本。这是一个直接下载链接。](https://docs.docker.com/docker-for-mac/release-notes/)

测试的第一轮将会比平时长。这是因为 Docker 将不得不为您的容器下载基本映像并缓存一些东西。接下来的运行会快得多。

运行日志将如下所示。您可以看到 Docker 非常酷，可以将所有组件的日志放在同一个时间线上。这在查找错误时非常方便。

```
Creating tuto-api-e2e-testing_db_1    ... done
Creating tuto-api-e2e-testing_redis_1 ... done
Creating tuto-api-e2e-testing_myapp_1 ... done
Creating tuto-api-e2e-testing_myapp-tests_1 ... done
Attaching to tuto-api-e2e-testing_redis_1, tuto-api-e2e-testing_db_1, tuto-api-e2e-testing_myapp_1, tuto-api-e2e-testing_myapp-tests_1
db_1           | The files belonging to this database system will be owned by user "postgres".
redis_1        | 1:M 09 Nov 2019 21:57:22.161 * Running mode=standalone, port=6379.
myapp_1        | yarn run v1.19.0
redis_1        | 1:M 09 Nov 2019 21:57:22.162 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
redis_1        | 1:M 09 Nov 2019 21:57:22.162 # Server initialized
db_1           | This user must also own the server process.
db_1           | 
db_1           | The database cluster will be initialized with locale "en_US.utf8".
db_1           | The default database encoding has accordingly been set to "UTF8".
db_1           | The default text search configuration will be set to "english".
db_1           | 
db_1           | Data page checksums are disabled.
db_1           | 
db_1           | fixing permissions on existing directory /var/lib/postgresql/data ... ok
db_1           | creating subdirectories ... ok
db_1           | selecting dynamic shared memory implementation ... posix
myapp-tests_1  | 2019/11/09 21:57:25 Waiting for: tcp://db:5432
myapp-tests_1  | 2019/11/09 21:57:25 Waiting for: tcp://redis:6379
myapp-tests_1  | 2019/11/09 21:57:25 Waiting for: tcp://myapp:8000
myapp_1        | $ node server.js
redis_1        | 1:M 09 Nov 2019 21:57:22.163 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
db_1           | selecting default max_connections ... 100
myapp_1        | Starting web server...
myapp-tests_1  | 2019/11/09 21:57:25 Connected to tcp://myapp:8000
myapp-tests_1  | 2019/11/09 21:57:25 Connected to tcp://db:5432
redis_1        | 1:M 09 Nov 2019 21:57:22.164 * Ready to accept connections
myapp-tests_1  | 2019/11/09 21:57:25 Connected to tcp://redis:6379
myapp_1        | Server started on: 8000
db_1           | selecting default shared_buffers ... 128MB
db_1           | selecting default time zone ... Etc/UTC
db_1           | creating configuration files ... ok
db_1           | running bootstrap script ... ok
db_1           | performing post-bootstrap initialization ... ok
db_1           | syncing data to disk ... ok
db_1           | 
db_1           | 
db_1           | Success. You can now start the database server using:
db_1           | 
db_1           |     pg_ctl -D /var/lib/postgresql/data -l logfile start
db_1           | 
db_1           | initdb: warning: enabling "trust" authentication for local connections
db_1           | You can change this by editing pg_hba.conf or using the option -A, or
db_1           | --auth-local and --auth-host, the next time you run initdb.
db_1           | waiting for server to start....2019-11-09 21:57:24.328 UTC [41] LOG:  starting PostgreSQL 12.0 (Debian 12.0-2.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit
db_1           | 2019-11-09 21:57:24.346 UTC [41] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db_1           | 2019-11-09 21:57:24.373 UTC [42] LOG:  database system was shut down at 2019-11-09 21:57:23 UTC
db_1           | 2019-11-09 21:57:24.383 UTC [41] LOG:  database system is ready to accept connections
db_1           |  done
db_1           | server started
db_1           | CREATE DATABASE
db_1           | 
db_1           | 
db_1           | /usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*
db_1           | 
db_1           | waiting for server to shut down....2019-11-09 21:57:24.907 UTC [41] LOG:  received fast shutdown request
db_1           | 2019-11-09 21:57:24.909 UTC [41] LOG:  aborting any active transactions
db_1           | 2019-11-09 21:57:24.914 UTC [41] LOG:  background worker "logical replication launcher" (PID 48) exited with exit code 1
db_1           | 2019-11-09 21:57:24.914 UTC [43] LOG:  shutting down
db_1           | 2019-11-09 21:57:24.930 UTC [41] LOG:  database system is shut down
db_1           |  done
db_1           | server stopped
db_1           | 
db_1           | PostgreSQL init process complete; ready for start up.
db_1           | 
db_1           | 2019-11-09 21:57:25.038 UTC [1] LOG:  starting PostgreSQL 12.0 (Debian 12.0-2.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit
db_1           | 2019-11-09 21:57:25.039 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
db_1           | 2019-11-09 21:57:25.039 UTC [1] LOG:  listening on IPv6 address "::", port 5432
db_1           | 2019-11-09 21:57:25.052 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db_1           | 2019-11-09 21:57:25.071 UTC [59] LOG:  database system was shut down at 2019-11-09 21:57:24 UTC
db_1           | 2019-11-09 21:57:25.077 UTC [1] LOG:  database system is ready to accept connections
myapp-tests_1  | Creating tables ...
myapp-tests_1  | Creating table 'users'
myapp-tests_1  | Tables created succesfully
myapp-tests_1  | yarn run v1.19.0
myapp-tests_1  | $ mocha --timeout 10000 --bail
myapp-tests_1  | 
myapp-tests_1  | 
myapp-tests_1  |   Users
myapp-tests_1  | Mock server started on port: 8002
myapp-tests_1  |     ✓ should create a new user (151ms)
myapp-tests_1  |     ✓ should get the created user
myapp-tests_1  |     ✓ should not create user if mail is spammy
myapp-tests_1  |     ✓ should not create user if spammy mail API is down
myapp-tests_1  | 
myapp-tests_1  | 
myapp-tests_1  |   4 passing (234ms)
myapp-tests_1  | 
myapp-tests_1  | Done in 0.88s.
myapp-tests_1  | 2019/11/09 21:57:26 Command finished successfully.
tuto-api-e2e-testing_myapp-tests_1 exited with code 0
```

我们可以看到 **db** 是初始化时间最长的容器。有道理。一旦完成，测试开始。我的笔记本电脑的总运行时间是 16 秒。与实际执行测试所用的 880 毫秒相比，这已经很多了。在实践中，不到 1 分钟的测试是黄金，因为它几乎是即时反馈。大约 15 秒的开销是一个购买时间，随着您添加更多的测试，这个时间将保持不变。您可以添加数百个测试，并且仍然将执行时间保持在 1 分钟以内。

瞧啊。我们已经建立并运行了测试框架。在现实世界的项目中，下一步将是通过更多的测试来增强 API 的功能覆盖率。让我们考虑一下 CRUD 操作。是时候向我们的测试环境添加更多的元素了。

## 添加 Redis 集群

让我们向我们的 API 环境添加另一个元素，以了解它需要什么。剧透提示:不多。

假设我们的 API 将用户会话保存在 Redis 集群中。如果你想知道我们为什么要这样做，想象一下你的 API 在生产中有 100 个实例。基于循环负载平衡，用户访问一个或另一个服务器。每个请求都需要被认证。

这需要用户配置文件数据来检查特权和其他应用程序特定的业务逻辑。一种方法是在每次需要时往返数据库以获取数据，但这不是很有效。使用内存数据库集群可以使数据在所有服务器上可用，只需读取一个本地变量。

这就是如何用一个额外的服务来增强 Docker compose 测试环境。让我们从官方 Docker 映像添加一个 Redis 集群(我只保留了文件的新部分):

```
services:
  db:
    ...

  redis:
    image: "redis:alpine"
    expose:
      - 6379

  myapp:
    environment:
      APP_REDIS_HOST: redis
      APP_REDIS_PORT: 6379
    ...
  myapp-tests:
    command: dockerize ... -wait tcp://redis:6379 ...
    environment:
      APP_REDIS_HOST: redis
      APP_REDIS_PORT: 6379
      ...
    ...
```

你可以看到它并不多。我们添加了一个名为 **redis** 的新容器。它使用名为`redis:alpine`的官方最小 redis 图像。我们在 API 容器中添加了 Redis 主机和端口配置。在执行测试之前，我们让测试等待它和其他容器。

让我们修改我们的应用程序，以实际使用 Redis 集群:

```
const redis = require('redis').createClient({
  host: config.redis.host,
  port: config.redis.port,
})

...

app.route('/api/users').post(async (req, res, next) => {
  try {
    const { email, firstname } = req.body;
    // ... validate inputs here ...
    const userData = { email, firstname };
    const result = await db('users').returning('id').insert(userData);
    const id = result[0];

    // Once the user is created store the data in the Redis cluster
    await redis.set(id, JSON.stringify(userData));

    res.status(201).send({ id, ...userData });
  } catch (err) {
    console.log(`Error: Unable to create user: ${err.message}. ${err.stack}`);
    return next(err);
  }
});
```

现在让我们更改测试，检查 Redis 集群是否填充了正确的数据。这就是为什么 **myapp-tests** 容器也在`docker-compose.yml`中获得 Redis 主机和端口配置。

```
it("should create a new user", done => {
  chai
    .request(SERVER_URL)
    .post("/api/users")
    .send(TEST_USER)
    .end((err, res) => {
      if (err) throw err;
      res.should.have.status(201);
      res.should.be.json;
      res.body.should.be.a("object");
      res.body.should.have.property("id");
      res.body.should.have.property("email");
      res.body.should.have.property("firstname");
      res.body.id.should.not.be.null;
      res.body.email.should.equal(TEST_USER.email);
      res.body.firstname.should.equal(TEST_USER.firstname);
      createdUserId = res.body.id;

      redis.get(createdUserId, (err, cacheData) => {
        if (err) throw err;
        cacheData = JSON.parse(cacheData);
        cacheData.should.have.property("email");
        cacheData.should.have.property("firstname");
        cacheData.email.should.equal(TEST_USER.email);
        cacheData.firstname.should.equal(TEST_USER.firstname);
        done();
      });
    });
});
```

看看这有多简单。您可以像组装乐高积木一样为您的测试构建一个复杂的环境。

我们可以看到这种容器化全环境测试的另一个好处。这些测试实际上可以查看环境的组件。我们的测试不仅可以检查我们的 API 是否返回了正确的响应代码和数据。我们还可以检查 Redis 集群中的数据是否具有正确的值。我们还可以检查数据库内容。

## 添加 API 模拟

API 组件的一个常见元素是调用其他 API 组件。

假设我们的 API 在创建用户时需要检查垃圾邮件。检查由第三方服务完成:

```
const validateUserEmail = async (email) => {
  const res = await fetch(`${config.app.externalUrl}/validate?email=${email}`);
  if(res.status !== 200) return false;
  const json = await res.json();
  return json.result === 'valid';
}

app.route('/api/users').post(async (req, res, next) => {
  try {
    const { email, firstname } = req.body;
    // ... validate inputs here ...
    const userData = { email, firstname };

    // We don't just create any user. Spammy emails should be rejected
    const isValidUser = await validateUserEmail(email);
    if(!isValidUser) {
      return res.sendStatus(403);
    }

    const result = await db('users').returning('id').insert(userData);
    const id = result[0];
    await redis.set(id, JSON.stringify(userData));
    res.status(201).send({ id, ...userData });
  } catch (err) {
    console.log(`Error: Unable to create user: ${err.message}. ${err.stack}`);
    return next(err);
  }
});
```

现在我们有一个测试任何东西的问题。如果检测垃圾邮件的 API 不可用，我们无法创建任何用户。在测试模式下修改我们的 API 来绕过这一步是一种危险的代码混乱。

即使我们可以使用真正的第三方服务，我们也不想这么做。一般来说，我们的测试不应该依赖于外部基础设施。首先，因为你可能会在你的 CI 过程中运行很多测试。为此使用另一个生产 API 并不酷。其次，API 可能会暂时关闭，因为错误的原因导致测试失败。

正确的解决方案是在我们的测试中模拟外部 API。

不需要任何花哨的框架。我们将用大约 20 行代码用普通的 JS 构建一个通用的 mock。这将使我们有机会控制 API 返回给组件的内容。它允许测试错误场景。

现在让我们增强我们的测试。

```
const express = require("express");

...

const MOCK_SERVER_PORT = process.env.MOCK_SERVER_PORT || 8002;

// Some object to encapsulate attributes of our mock server
// The mock stores all requests it receives in the `requests` property.
const mock = {
  app: express(),
  server: null,
  requests: [],
  status: 404,
  responseBody: {}
};

// Define which response code and content the mock will be sending
const setupMock = (status, body) => {
  mock.status = status;
  mock.responseBody = body;
};

// Start the mock server
const initMock = async () => {
  mock.app.use(bodyParser.urlencoded({ extended: false }));
  mock.app.use(bodyParser.json());
  mock.app.use(cors());
  mock.app.get("*", (req, res) => {
    mock.requests.push(req);
    res.status(mock.status).send(mock.responseBody);
  });

  mock.server = await mock.app.listen(MOCK_SERVER_PORT);
  console.log(`Mock server started on port: ${MOCK_SERVER_PORT}`);
};

// Destroy the mock server
const teardownMock = () => {
  if (mock.server) {
    mock.server.close();
    delete mock.server;
  }
};

describe("Users", () => {
  // Our mock is started before any test starts ...
  before(async () => await initMock());

  // ... killed after all the tests are executed ...
  after(() => {
    redis.quit();
    teardownMock();
  });

  // ... and we reset the recorded requests between each test
  beforeEach(() => (mock.requests = []));

  it("should create a new user", done => {
    // The mock will tell us the email is valid in this test
    setupMock(200, { result: "valid" });

    chai
      .request(SERVER_URL)
      .post("/api/users")
      .send(TEST_USER)
      .end((err, res) => {
        // ... check response and redis as before
        createdUserId = res.body.id;

        // Verify that the API called the mocked service with the right parameters
        mock.requests.length.should.equal(1);
        mock.requests[0].path.should.equal("/api/validate");
        mock.requests[0].query.should.have.property("email");
        mock.requests[0].query.email.should.equal(TEST_USER.email);
        done();
      });
  });
});
```

测试现在检查在调用我们的 API 期间，外部 API 是否被正确的数据命中。

我们还可以添加其他测试，根据外部 API 响应代码检查我们的 API 的行为:

```
describe("Users", () => {
  it("should not create user if mail is spammy", done => {
    // The mock will tell us the email is NOT valid in this test ...
    setupMock(200, { result: "invalid" });

    chai
      .request(SERVER_URL)
      .post("/api/users")
      .send(TEST_USER)
      .end((err, res) => {
        // ... so the API should fail to create the user
        // We could test that the DB and Redis are empty here
        res.should.have.status(403);
        done();
      });
  });

  it("should not create user if spammy mail API is down", done => {
    // The mock will tell us the email checking service
    //  is down for this test ...
    setupMock(500, {});

    chai
      .request(SERVER_URL)
      .post("/api/users")
      .send(TEST_USER)
      .end((err, res) => {
        // ... in that case also a user should not be created
        res.should.have.status(403);
        done();
      });
  });
});
```

如何在应用程序中处理来自第三方 API 的错误当然取决于您。但是你明白了。

为了运行这些测试，我们需要告诉容器 **myapp** 第三方服务的基本 URL 是什么:

```
 myapp:
    environment:
      APP_EXTERNAL_URL: http://myapp-tests:8002/api
    ...

  myapp-tests:
    environment:
      MOCK_SERVER_PORT: 8002
    ...
```

## 结论和其他一些想法

希望这篇文章让您领略了 Docker compose 在 API 测试方面的作用。完整的例子是 GitHub 上的[。](https://github.com/fire-ci/tuto-api-e2e-testing)

使用 Docker compose 使测试在接近生产的环境中快速运行。它不需要修改组件代码。唯一的要求是支持环境变量驱动的配置。

这个例子中的组件逻辑非常简单，但是原理适用于任何 API。你的测试只会更长或更复杂。它们也适用于任何可以放在一个容器中的技术堆栈(这就是它们的全部)。一旦你到了那里，如果需要的话，你就离把你的容器部署到生产环境只差一步了。

如果你现在没有测试，我建议你这样开始:用 Docker compose 进行端到端测试。它非常简单，你可以在几个小时内运行你的第一个测试。如果您有任何问题或需要建议，请随时联系我。我很乐意帮忙。

我希望你喜欢这篇文章，并开始用 Docker Compose 测试你的 API。一旦您准备好测试，您就可以在我们的持续集成平台 [Fire CI](https://fire.ci) 上运行它们。

## 自动化测试成功的最后一个想法。

当谈到维护大型测试套件时，最重要的特性是测试易于阅读和理解。这是激励你的团队保持测试更新的关键。从长远来看，复杂的测试框架不太可能被恰当地使用。

不管 API 的栈是什么，您都可以考虑使用 chai/mocha 来编写测试。运行时代码和测试代码有不同的堆栈可能看起来不寻常，但是如果它完成了工作...从本文中的例子可以看出，用 chai/mocha 测试 REST API 非常简单。学习曲线接近于零。

因此，如果你根本没有测试，并且有一个用 Java、Python、RoR 编写的 REST API 要测试。NET 或者其他栈，你可以考虑试试 chai/mocha。

如果你想知道如何开始持续集成，我已经写了一个更广泛的指南。就是这里:[如何开始持续集成](https://fire.ci/blog/how-to-get-started-with-continuous-integration/)

原载于[火词博客](https://fire.ci/blog/)。