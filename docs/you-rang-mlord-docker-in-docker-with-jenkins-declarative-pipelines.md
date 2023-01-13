# 你按铃了吗，老爷？具有 Jenkins 声明管道的 Docker 中的 Docker

> 原文：<https://www.freecodecamp.org/news/you-rang-mlord-docker-in-docker-with-jenkins-declarative-pipelines/>

资源。当它们不受限制时，它们就不重要了。但是当它们有限的时候，你就有挑战了！

最近，我的团队自己也面临着这样的挑战:我们意识到我们需要在我们的一个 Jenkins 代理上升级节点版本，以便我们可以构建并正确测试我们的 Angular 7 应用程序。然而，我们发现我们也将失去构建需要 Node 8 的遗留 AngularJS 应用的能力。

我们该怎么办？

除了消除著名的“它在我的机器上工作”的问题，Docker 在处理这样的问题上派上了用场。然而，有一些挑战需要解决，例如 Docker 中的 Docker。

为此，经过长时间的反复试验，我们[构建并发布了](https://hub.docker.com/repository/docker/btapai/pipelines)一个符合我们团队需求的 [docker 文件](https://github.com/TapaiBalazs/build-pipeline-docker-images)。它有助于运行我们的构建，看起来如下所示:

```
1\. Install dependencies
2\. Lint the code
3\. Run unit tests
4\. Run SonarQube analysis
5\. Build the application
6\. Build a docker image which would be deployed
7\. Run the docker container
8\. Run cypress tests
9\. Push docker image to the repository
10\. Run another Jenkins job to deploy it to the environment
11\. Generate unit and functional test reports and publish them
12\. Stop any running containers
13\. Notify chat/email about the build 
```

## 我们需要的码头工人形象

我们的项目是 Angular 7 项目，是用`angular-cli`生成的。我们还有一些依赖项需要 Node 10.x.x。我们用`tslint`来 lint 我们的代码，用`Karma`和`Jasmine`来运行我们的单元测试。对于单元测试，我们需要安装一个 Chrome 浏览器，这样它们就可以和 headless Chrome 一起运行。

这就是我们决定使用`cypress/browsers:node10.16.0-chrome77`图像的原因。在我们安装了依赖项、链接了代码并运行了单元测试之后，我们运行了 [SonarQube](https://www.npmjs.com/package/sonar-scanner) 分析。这要求我们也有`Openjdk 8`。

```
FROM cypress/browsers:node10.16.0-chrome77

# Install OpenJDK-8
RUN apt-get update && \
    apt-get install -y openjdk-8-jdk && \
    apt-get install -y ant && \
    apt-get clean;

# Fix certificate issues
RUN apt-get update && \
    apt-get install ca-certificates-java && \
    apt-get clean && \
    update-ca-certificates -f;

# Setup JAVA_HOME -- useful for docker commandline
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME 
```

一旦声纳扫描准备就绪，我们就构建了应用程序。测试中最重要的原则之一就是你应该测试你的用户将要使用的东西。这就是我们想要在与生产中完全相同的 docker 容器中测试构建代码的原因。

当然，我们可以从一个非常简单的静态服务器提供前端服务。
但这意味着 Apache HTTP 服务器或 NGINX 服务器通常所做的一切都将丢失(例如所有的代理，`gzip`或`brotli`)。

虽然这是一个强有力的原则，但最大的问题是我们已经在 Docker 容器中运行了。这就是为什么我们需要 DIND(码头工人中的码头工人)。

在和我的同事研究了一整天之后，我们找到了一个解决方案，结果非常有效。第一件也是最重要的事情是，我们的构建容器需要 Docker 可执行文件。

```
# Install Docker executable
RUN apt-get update && apt-get install -y \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg2 \
        software-properties-common \
    && curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - \
    && add-apt-repository \
        "deb [arch=amd64] https://download.docker.com/linux/debian \
        $(lsb_release -cs) \
        stable" \
    && apt-get update \
    && apt-get install -y \
        docker-ce

RUN usermod -u 1002 node && groupmod -g 1002 node && gpasswd -a node docker 
```

正如您所看到的，我们安装了 docker 可执行文件和必要的证书，但是我们也为我们的用户添加了权限和组。这第二部分是必要的，因为主机，我们的 Jenkins 代理，用`-u 1002:1002`启动容器。这是我们的 Jenkins 代理的用户 ID，它运行无特权的容器。

当然这不是一切。当容器启动时，必须挂载主机的 docker 守护进程。所以我们需要用一些额外的参数来启动构建容器
。在 Jenkinsfile 中，它看起来像下面这样:

```
pipeline {
  agent {
    docker {
     image 'btapai/pipelines:node-10.16.0-chrome77-openjdk8-CETtime-dind'
     label 'frontend'
     args '-v /var/run/docker.sock:/var/run/docker.sock -v /var/run/dbus/system_bus_socket:/var/run/dbus/system_bus_socket -e HOME=${workspace} --group-add docker'
    }
  }

// ...
} 
```

如您所见，我们安装了两个 Unix 套接字。将 docker 守护进程安装到构建容器中。

是一个套接字，它允许 cypress 在我们的容器中运行。

我们需要`-e HOME=${workspace}`来避免构建期间的访问权限问题。

将主机 docker 组向下传递，这样在容器内部，我们的用户可以使用 docker 守护进程。

有了这些合适的参数，我们就能够构建我们的图像，启动它并对它进行 cypress 测试。

但是让我们深呼吸一下。在詹金斯，我们想使用多分支管道。Jenkins 中的多分支管道将为包含 Jenkinsfile 的每个分支创建一个 Jenkins 作业。这意味着当我们开发多个分支时，他们会有自己的视图。

这有一些问题。第一个问题是，如果我们在所有分支中用相同的名称构建我们的映像，将会有冲突(因为我们的 docker 守护进程技术上不在我们的构建容器中)。

第二个问题出现在 docker run 命令在每个构建中使用同一个端口时(因为您不能在已经被占用的端口上启动第二个容器)。

第三个问题是为正在运行的应用程序获取正确的 URL，因为 Dorothy，您已经不在本地主机中了。

先说命名。用 git 获得一个惟一的名称相当容易，因为提交散列是惟一的。然而，为了获得一个唯一的端口，我们必须在声明环境变量时使用一些小技巧:

```
pipeline {

// ..

  environment {
    BUILD_PORT = sh(
        script: 'shuf -i 2000-65000 -n 1',
        returnStdout: true
    ).trim()
  }

// ...

    stage('Functional Tests') {
      steps {
        sh "docker run -d -p ${BUILD_PORT}:80 --name ${GIT_COMMIT} application"
        // be patient, we are going to get the url as well. :)
      }
    }

// ...

} 
```

在某些 Linux 发行版上使用`shuf -i 2000-65000 -n 1`命令，您可以生成一个随机数。我们的基本映像使用 Debian，所以我们很幸运。
环境变量`GIT_COMMIT`是通过 SCM 插件在 Jenkins 中提供的。

现在困难的部分来了:我们在 docker 容器中，没有本地主机，docker 容器中的网络可以改变。

有趣的是，当我们启动容器时，它运行在主机的 docker 守护进程上。所以从技术上讲，它不在我们的容器中运行。我们必须从内部接近它。

经过几个小时的调查，我的同事找到了一个可能的解决方案:
`docker inspect --format "{{ .NetworkSettings.IPAddress }}"`

但是它没有工作，因为这个 IP 地址不是容器内部的 IP 地址，而是容器外部的 IP 地址。

然后我们尝试了`NetworkSettings.Gateway`属性，它非常有效。
因此，我们的功能测试阶段如下所示:

```
stage('Functional Tests') {
  steps {
    sh "docker run -d -p ${BUILD_PORT}:80 --name ${GIT_COMMIT} application"
    sh 'npm run cypress:run -- --config baseUrl=http://`docker inspect --format "{{ .NetworkSettings.Gateway }}" "${GIT_COMMIT}"`:${BUILD_PORT}'
  }
} 
```

看到我们的 cypress 测试在 docker 容器中运行是一种美妙的感觉。

但后来有些人悲惨地失败了。因为失败的柏树测试预计看到一些日期。

```
cy.get("created-date-cell")
  .should("be.visible")
  .and("contain", "2019.12.24 12:33:17") 
```

但是因为我们的构建容器被设置为不同的时区，所以我们前端显示的日期是不同的。

幸运的是，这很容易解决，我的同事以前见过。我们安装了必要的时区和地区。在我们的例子中，我们将构建容器的时区设置为`Europe/Budapest`，因为我们的测试是在这个时区编写的。

```
# SETUP-LOCALE
RUN apt-get update \
    && apt-get install --assume-yes --no-install-recommends locales \
    && apt-get clean \
    && sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
    && sed -i -e 's/# hu_HU.UTF-8 UTF-8/hu_HU.UTF-8 UTF-8/' /etc/locale.gen \
    && locale-gen

ENV LANG="en_US.UTF-8" \
    LANGUAGE= \
    LC_CTYPE="en_US.UTF-8" \
    LC_NUMERIC="hu_HU.UTF-8" \
    LC_TIME="hu_HU.UTF-8" \
    LC_COLLATE="en_US.UTF-8" \
    LC_MONETARY="hu_HU.UTF-8" \
    LC_MESSAGES="en_US.UTF-8" \
    LC_PAPER="hu_HU.UTF-8" \
    LC_NAME="hu_HU.UTF-8" \
    LC_ADDRESS="hu_HU.UTF-8" \
    LC_TELEPHONE="hu_HU.UTF-8" \
    LC_MEASUREMENT="hu_HU.UTF-8" \
    LC_IDENTIFICATION="hu_HU.UTF-8" \
    LC_ALL=

# SETUP-TIMEZONE
RUN apt-get update \
    && apt-get install --assume-yes --no-install-recommends tzdata \
    && apt-get clean \
    && echo 'Europe/Budapest' > /etc/timezone && rm /etc/localtime \
    && ln -snf /usr/share/zoneinfo/'Europe/Budapest' /etc/localtime \
    && dpkg-reconfigure -f noninteractive tzdata 
```

由于构建的每个关键部分现在都已解决，将构建的映像推送到注册表只是 docker 推送命令。你可以点击查看整个 dockerfile [。](https://github.com/TapaiBalazs/build-pipeline-docker-images/blob/master/pipelines/node-chrome-openjdk-CET-dind/Dockerfile)

还有一件事，就是当 cypress 测试失败时停止运行容器。我们使用`always` post 步骤很容易地做到了这一点。

```
post {
  always {
    script {
      try {
        sh "docker stop ${GIT_COMMIT} && docker rm ${GIT_COMMIT}"
      } catch (Exception e) {
        echo 'No docker containers were running'
      }
    }
  }
} 
```

非常感谢你阅读这篇博文。希望对你有帮助。

原文可以在我的博客上看:

[You Rang, M’Lord? Docker in Docker with Jenkins declarative pipelinesA good CI/CD pipeline is extremely important for quality software development. I have spent some time creating pipelines with docker and Jenkins and I’d like to share my experience...![icon-512x512](img/f754969b55891bc9bfb0847f0683a320.png)Would you like to TALK about it?![butler](img/5c10165234905c7fb438797a817a07d0.png)](https://tapaibalazs.netlify.com/jenkins-and-docker-in-docker/)