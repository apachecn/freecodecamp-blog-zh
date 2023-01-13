# 使用 Docker 构建 Python 数据科学容器

> 原文：<https://www.freecodecamp.org/news/building-python-data-science-container-using-docker/>

![1*oYWC2Wnc4Nf_mH0WL3ep_w](img/cc67572e2cee384fca2614197e43a3e3.png)

Photo by [Bryan Goff](https://unsplash.com/@bryangoffphoto) on [Unsplash](https://unsplash.com/photos/-eDpBjt6UL0)

### TL；速度三角形定位法(dead reckoning)

人工智能(AI)和机器学习(ML)这几天简直如火如荼。为广泛的使用案例提供动力，从自动驾驶汽车到药物发现，再到天知道是什么。人工智能和人工智能有一个光明和繁荣的未来。

另一方面，Docker 通过引入短暂的轻量级容器彻底改变了计算世界。容器基本上将在映像(一堆只读层)中运行所需的所有软件打包成一个 COW(写时复制)层来保存数据。

说得够多了，让我们开始构建一个 Python 数据科学容器。

* * *

### Python 数据科学包

我们的 python 数据科学容器利用了以下超酷的 Python 包:

1.  ****NumPy**** : NumPy 或 Numeric Python 支持大型、多维数组和矩阵。它为数学和数值例程提供了快速的预编译函数。此外，NumPy 使用强大的数据结构优化 Python 编程，以便高效计算多维数组和矩阵。
2.  ****SciPy**** : SciPy 为回归、最小化、傅立叶变换等等提供了有用的函数。基于 NumPy，SciPy 扩展了它的功能。SciPy 的主要数据结构也是一个多维数组，由 Numpy 实现。该软件包包含的工具有助于解决线性代数，概率论，积分学，以及更多的任务。
3.  ****Pandas**** : Pandas 为操纵数据结构和执行广泛的数据分析提供了通用而强大的工具。它可以很好地处理不完整、非结构化和无序的真实世界数据，并附带了用于形成、聚合、分析和可视化数据集的工具。
4.  ****sci kit-Learn****:sci kit-Learn 是一个 Python 模块，集成了各种最先进的机器学习算法，用于中等规模的监督和非监督问题。它是 python 最著名的机器学习库之一。Scikit-learn 包专注于使用通用高级语言将机器学习带给非专业人士。主要重点是易用性、性能、文档和 API 一致性。SciKit-Learn 在简化的 BSD 许可下具有最小的依赖性和简单的分发，广泛用于学术和商业环境。Scikit-learn 为常见的机器学习算法公开了一个简洁而一致的接口，使得将 ML 引入生产系统变得简单。
5.  ****Matplotlib****:Matplotlib 是一个 Python 2D 绘图库，能够以多种硬拷贝格式和跨平台的交互环境生成出版物质量数字。Matplotlib 可用于 Python 脚本、Python 和 IPython shell、Jupyter 笔记本、web 应用服务器和四个图形用户界面工具包。
6.  ****NLTK**** : NLTK 是构建 Python 程序来处理人类语言数据的领先平台。它提供了 50 多个语料库和词汇资源(如 WordNet)的易用接口，以及一套用于分类、标记化、词干化、标记、解析和语义推理的文本处理库。

* * *

### 构建数据科学容器

Python 正迅速成为数据科学家的首选语言，因此我们将使用 Python 作为构建数据科学容器的首选语言。

#### 基本 Alpine Linux 映像

Alpine Linux 是一个小型的 Linux 发行版，是为重视安全性、简单性和资源效率的高级用户设计的。

正如[阿尔卑斯](https://alpinelinux.org/)所宣称的:

> **小。简单。安全。Alpine Linux 是基于 musl libc 和 busybox 的面向安全的轻量级 Linux 发行版。**

令人惊讶的是，Alpine 图像非常小，对于容器来说不超过 8MB。安装最少的包来减少底层容器的攻击面。这使得 Alpine 成为我们的数据科学容器的首选映像。

下载和运行 Alpine Linux 容器非常简单:

```
$ docker container run --rm alpine:latest cat /etc/os-release
```

在我们的 docker 文件中，我们可以简单地使用 Alpine base 图像:

```
FROM alpine:latest
```

* * *

#### 空谈是廉价的，让我们建立档案吧

现在让我们来研究一下 docker 文件。

```
FROM alpine:latest

LABEL MAINTAINER="Faizan Bashir <faizan.ibn.bashir@gmail.com>"

# Linking of locale.h as xlocale.h
# This is done to ensure successfull install of python numpy package
# see https://forum.alpinelinux.org/comment/690#comment-690 for more information.

WORKDIR /var/www/

# SOFTWARE PACKAGES
#   * musl: standard C library
#   * lib6-compat: compatibility libraries for glibc
#   * linux-headers: commonly needed, and an unusual package name from Alpine.
#   * build-base: used so we include the basic development packages (gcc)
#   * bash: so we can access /bin/bash
#   * git: to ease up clones of repos
#   * ca-certificates: for SSL verification during Pip and easy_install
#   * freetype: library used to render text onto bitmaps, and provides support font-related operations
#   * libgfortran: contains a Fortran shared library, needed to run Fortran
#   * libgcc: contains shared code that would be inefficient to duplicate every time as well as auxiliary helper routines and runtime support
#   * libstdc++: The GNU Standard C++ Library. This package contains an additional runtime library for C++ programs built with the GNU compiler
#   * openblas: open source implementation of the BLAS(Basic Linear Algebra Subprograms) API with many hand-crafted optimizations for specific processor types
#   * tcl: scripting language
#   * tk: GUI toolkit for the Tcl scripting language
#   * libssl1.0: SSL shared libraries
ENV PACKAGES="\
    dumb-init \
    musl \
    libc6-compat \
    linux-headers \
    build-base \
    bash \
    git \
    ca-certificates \
    freetype \
    libgfortran \
    libgcc \
    libstdc++ \
    openblas \
    tcl \
    tk \
    libssl1.0 \
"

# PYTHON DATA SCIENCE PACKAGES
#   * numpy: support for large, multi-dimensional arrays and matrices
#   * matplotlib: plotting library for Python and its numerical mathematics extension NumPy.
#   * scipy: library used for scientific computing and technical computing
#   * scikit-learn: machine learning library integrates with NumPy and SciPy
#   * pandas: library providing high-performance, easy-to-use data structures and data analysis tools
#   * nltk: suite of libraries and programs for symbolic and statistical natural language processing for English
ENV PYTHON_PACKAGES="\
    numpy \
    matplotlib \
    scipy \
    scikit-learn \
    pandas \
    nltk \
" 

RUN apk add --no-cache --virtual build-dependencies python --update py-pip \
    && apk add --virtual build-runtime \
    build-base python-dev openblas-dev freetype-dev pkgconfig gfortran \
    && ln -s /usr/include/locale.h /usr/include/xlocale.h \
    && pip install --upgrade pip \
    && pip install --no-cache-dir $PYTHON_PACKAGES \
    && apk del build-runtime \
    && apk add --no-cache --virtual build-dependencies $PACKAGES \
    && rm -rf /var/cache/apk/*

CMD ["python"]
```

[datascience-python2.7.Dockerfile](https://gist.github.com/faizanbashir/c491a4efd3d3b6be8a4b84a439b6ef7d#file-datascience-python2-7-dockerfile)

`FROM`指令用于将`alpine:latest`设置为基础图像。使用`WORKDIR`指令，我们将`/var/www`设置为容器的工作目录。`ENV PACKAGES`列出了我们的容器需要的软件包，如`git`、`blas`和`libgfortran`。我们的数据科学容器的 python 包在`ENV PACKAGES`中定义。

我们将所有命令组合在一个 Dockerfile `RUN`指令下，以减少层数，这反过来有助于减少最终的图像大小。

* * *

#### 构建和标记图像

现在我们已经定义了 Dockerfile，使用终端导航到包含 Dockerfile 的文件夹，并使用以下命令构建映像:

```
$ docker build -t faizanbashir/python-datascience:2.7 -f Dockerfile .
```

`-t`标志用于以“名称:标签”格式命名标签。`-f`标签用于定义 Dockerfile 的名称(默认为‘PATH/docker file’)。

* * *

#### 运行容器

我们已经成功构建并标记了 docker 映像，现在我们可以使用以下命令运行容器:

```
$ docker container run --rm -it faizanbashir/python-datascience:2.7 python
```

瞧，迎接我们的是一个 python shell，它可以执行各种很酷的数据科学任务。

```
Python 2.7.15 (default, Aug 16 2018, 14:17:09) [GCC 6.4.0] on linux2 Type "help", "copyright", "credits" or "license" for more information. >>>
```

我们的容器附带了 Python 2.7，但是如果您想使用 Python 3.6，请不要难过。请看 Python 3.6 的 docker 文件:

```
https://gist.github.com/faizanbashir/9443a7149cc53f81d84d0d356f871ec7#file-datascience-python3-6-dockerfile
```

构建并标记图像，如下所示:

```
FROM alpine:latest

LABEL MAINTAINER="Faizan Bashir <faizan.ibn.bashir@gmail.com>"

# Linking of locale.h as xlocale.h
# This is done to ensure successfull install of python numpy package
# see https://forum.alpinelinux.org/comment/690#comment-690 for more information.

WORKDIR /var/www/

# SOFTWARE PACKAGES
#   * musl: standard C library
#   * lib6-compat: compatibility libraries for glibc
#   * linux-headers: commonly needed, and an unusual package name from Alpine.
#   * build-base: used so we include the basic development packages (gcc)
#   * bash: so we can access /bin/bash
#   * git: to ease up clones of repos
#   * ca-certificates: for SSL verification during Pip and easy_install
#   * freetype: library used to render text onto bitmaps, and provides support font-related operations
#   * libgfortran: contains a Fortran shared library, needed to run Fortran
#   * libgcc: contains shared code that would be inefficient to duplicate every time as well as auxiliary helper routines and runtime support
#   * libstdc++: The GNU Standard C++ Library. This package contains an additional runtime library for C++ programs built with the GNU compiler
#   * openblas: open source implementation of the BLAS(Basic Linear Algebra Subprograms) API with many hand-crafted optimizations for specific processor types
#   * tcl: scripting language
#   * tk: GUI toolkit for the Tcl scripting language
#   * libssl1.0: SSL shared libraries
ENV PACKAGES="\
    dumb-init \
    musl \
    libc6-compat \
    linux-headers \
    build-base \
    bash \
    git \
    ca-certificates \
    freetype \
    libgfortran \
    libgcc \
    libstdc++ \
    openblas \
    tcl \
    tk \
    libssl1.0 \
    "

# PYTHON DATA SCIENCE PACKAGES
#   * numpy: support for large, multi-dimensional arrays and matrices
#   * matplotlib: plotting library for Python and its numerical mathematics extension NumPy.
#   * scipy: library used for scientific computing and technical computing
#   * scikit-learn: machine learning library integrates with NumPy and SciPy
#   * pandas: library providing high-performance, easy-to-use data structures and data analysis tools
#   * nltk: suite of libraries and programs for symbolic and statistical natural language processing for English
ENV PYTHON_PACKAGES="\
    numpy \
    matplotlib \
    scipy \
    scikit-learn \
    pandas \
    nltk \
    " 

RUN apk add --no-cache --virtual build-dependencies python3 \
    && apk add --virtual build-runtime \
    build-base python3-dev openblas-dev freetype-dev pkgconfig gfortran \
    && ln -s /usr/include/locale.h /usr/include/xlocale.h \
    && python3 -m ensurepip \
    && rm -r /usr/lib/python*/ensurepip \
    && pip3 install --upgrade pip setuptools \
    && ln -sf /usr/bin/python3 /usr/bin/python \
    && ln -sf pip3 /usr/bin/pip \
    && rm -r /root/.cache \
    && pip install --no-cache-dir $PYTHON_PACKAGES \
    && apk del build-runtime \
    && apk add --no-cache --virtual build-dependencies $PACKAGES \
    && rm -rf /var/cache/apk/*

CMD ["python3"]
```

[datascience-python3.6.Dockerfile](https://gist.github.com/faizanbashir/9443a7149cc53f81d84d0d356f871ec7#file-datascience-python3-6-dockerfile)

像这样运行容器:

```
$ docker container run --rm -it faizanbashir/python-datascience:3.6 python
```

有了这个，你就有了一个现成的容器来做各种很酷的数据科学的事情。

* * *

### 供应布丁

数字，你有时间和资源来设置所有这些东西。如果您没有，您可以使用以下命令将我已经构建好的现有图像推送到 Docker 的注册表 [Docker Hub](https://hub.docker.com/) :

```
# For Python 2.7 pull$ docker pull faizanbashir/python-datascience:2.7
```

```
# For Python 3.6 pull$ docker pull faizanbashir/python-datascience:3.6
```

提取图像后，您可以在 docker 文件中使用该图像或扩展该图像，或者将其用作 docker-compose 或 stack 文件中的图像。

* * *

### 后果

人工智能、人工智能的世界如今变得越来越令人兴奋，并将继续变得更加令人兴奋。大型企业正在这些领域大举投资。是时候开始利用数据的力量了，谁知道它可能会带来什么美妙的东西。

你可以在这里查看代码。

[**faizanbashir/python-data science**](https://github.com/faizanbashir/python-datascience)
[*带有 NumPy、SciPy、Scikit-learn、Matplotlib、nltk、pandas 包的 python datascience 容器的 Docker 映像…*github.com](https://github.com/faizanbashir/python-datascience)

我希望这篇文章有助于为您的数据科学项目构建容器。鼓掌如果它增加了你的知识，帮助它接触到更多的人。