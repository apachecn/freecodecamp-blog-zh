# 如何在 Docker 镜像或容器中运行一个命令

> 原文：<https://www.freecodecamp.org/news/docker-exec-how-to-run-a-command-inside-a-docker-image-or-container/>

我要告诉你一个 DevOps 的秘密:所有 DevOpsy 人都喜欢做的事情是构建一个超级花哨复杂的系统，然后找到一种像普通 shell 一样处理它的方法。或者用 SSH 连接到它，然后像对待普通 shell 一样对待它。

Docker 也不例外！您正在其他计算机中运行一台计算机。也许那台计算机是 EC2 实例或笔记本电脑。你甚至可以疯狂地运行一个虚拟机，然后运行 Docker。

当我使用 Docker 的时候，大部分时间我都是用它来打包和分发应用程序。有时候我会用它来做一些更酷的事情，比如分布式计算项目。但是很多时候我在 GitHub repo 中抛出一个 Dockerfile，这样我就不必安装 CLIs，我知道它最终会在我的笔记本电脑上发生冲突。

长话短说，您可以告诉 Docker 运行命令`bash`，这会让您进入一个 shell:

```
docker run -it name-of-image bash
# docker run -it continuumio/miniconda3:latest bash
# docker run -it node:latest bash
```

但是请继续阅读。；-)

## 尝试一下

谷歌你最喜欢的编程语言的 Docker up。对我来说，这是 Python，特别是我喜欢 conda。然后运行几个命令来确保您确实在那个 shell 中。

```
# From Host
echo $(pwd)
# Drop into docker shell
docker run -it continuumio/miniconda3:latest bash
# Now you are in the docker shell!
echo $(pwd)
echo $USER
```

很酷吧。这对于调试绝对应该正常工作的容器来说是完美的。对于我最常见的“我不想把它安装到我的电脑上”的用例来说，这也很棒。

## 使用 Docker 运行调试 Docker 版本

当试图调试 Docker 构建时，将 Docker 映像视为常规 shell 会很方便。

假设您有一个 docker 文件，用于您正在尝试构建的图像。通常情况下，当运行`docker build -t my-image .` (-t 代表 tag)时，Docker 会运行您的每一个运行步骤，并在运行到一个不能正确退出的命令时停止。

在 UNIX shell 中，退出代码 0 意味着命令一切正常。为了说明这一点，我让 docker 文件有一个 RUN 命令，用 1 退出。

```
FROM continuumio/miniconda3:latest

RUN apt-get update -y; \
    apt-get upgrade -y; \
    apt-get install -y \
    vim-tiny vim-athena build-essential

RUN  conda update conda \
    && conda clean --all --yes

RUN exit 1
```

```
docker build -t my-image .
```

这将得到如下所示的输出:

```
(base) ➜  my-image docker build -t my-image .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM continuumio/miniconda3:latest
 ---> 406f2b43ea59
Step 2/4 : RUN apt-get update -y;     apt-get upgrade -y;     apt-get install -y     vim-tiny vim-athena build-essential
 ---> Using cache
 ---> 726af29a48a0
Step 3/4 : RUN  conda update conda     && conda clean --all --yes
 ---> Using cache
 ---> 19478bb3ce67
Step 4/4 : RUN exit 1
 ---> Running in 7c98aab6b52c
The command '/bin/sh -c exit 1' returned a non-zero code: 1
```

您可以通过运行`docker images`并检查`my-image`来确认您的 Docker 映像不是构建的。不会有，因为没有造成功。

现在我们能做的就是注释掉那个麻烦的 Dockerfile RUN 命令。

```
FROM continuumio/miniconda3:latest

RUN apt-get update -y; \
    apt-get upgrade -y; \
    apt-get install -y \
    vim-tiny vim-athena build-essential

RUN  conda update conda \
    && conda clean --all --yes

#RUN exit 1
```

那么你会看到的是:

```
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM continuumio/miniconda3:latest
 ---> 406f2b43ea59
Step 2/3 : RUN apt-get update -y;     apt-get upgrade -y;     apt-get install -y     vim-tiny vim-athena build-essential
 ---> Using cache
 ---> 726af29a48a0
Step 3/3 : RUN  conda update conda     && conda clean --all --yes
 ---> Using cache
 ---> 19478bb3ce67
Successfully built 19478bb3ce67
Successfully tagged my-image:latest 
```

你现在可以进入你的 Docker 镜像并开始交互式运行命令了！

```
docker run -it my-image bash
# you can also run
# docker run -it my-image:latest bash
```

从这里开始，您可以一个接一个地调试您的运行命令，看看哪里出错了。如果您不确定某个命令是否正确退出，运行`$?`:

```
# First run docker run -it my-image bash to get to the shell
# Print the string hello
echo "hello"
# hello
echo $?
# 0

# Run a non existant command hello
$(hello)
# bash: hello: command not found
echo $?
# 127
```

您可以继续运行这些步骤，注释掉 Docker 文件，进入 shell，并找出有问题的命令，直到您的 Docker 映像完美构建。

## 包裹

希望我已经向您展示了使用 Docker 镜像与在您的计算机上使用终端没有什么不同。使用 Docker 镜像是分发应用程序的一种很棒的方式。

尝试使用您最喜欢的 CLI 应用程序或下一个 GitHub 项目，而不是创建安装脚本，用 Docker 打包它。；-)