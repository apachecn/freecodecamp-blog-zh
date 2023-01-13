# 如何在 Ubuntu 中安装 Java——JDK Linux 教程

> 原文：<https://www.freecodecamp.org/news/how-to-install-java-in-ubuntu-jdk-linux-tutorial/>

Java 是当今最流行的编程语言之一。全新的设置让您可以无缝安装 Java，并在构建应用程序时在不同版本之间切换。

在本教程中，您将学习如何:

*   安装任何 Java 版本，
*   在 Java 版本之间切换，
*   更新到最新的 Java 版本。

所提供的指南应该适用于大多数操作系统。我对以下 Linux 版本进行了测试:

*   人的本质
*   一种自由操作系统
*   马科斯

## Java 开发工具包

> Java 开发工具包(JDK)是一个使用 Java 编程语言构建应用程序、小程序和组件的开发环境。([来源](https://www.oracle.com/java/technologies/javase/jdk-jdk-7-readme.html))

JDK 包含不同的应用程序，包括

> `javac`， [Java 编译器](https://en.wikipedia.org/wiki/Java_compiler)，将源代码转换成 [Java 字节码](https://en.wikipedia.org/wiki/Java_bytecode)。
> 
> `java`，Java 应用程序的[加载器](https://en.wikipedia.org/wiki/Loader_(computing))。这个工具是一个解释器，可以解释由 [javac](https://en.wikipedia.org/wiki/Javac) 编译器生成的类文件。
> 
> 现在，开发和部署都使用单个启动器。Sun JDK 不再附带旧的部署启动程序 jre，取而代之的是这个新的 java 加载程序。([来源](https://www.javatpoint.com/jdk))

Java 构建工具(Maven、Gradle 等)和您的代码编辑器在幕后使用 Java 应用程序，为开发人员提供运行、创建和维护应用程序的良好体验。

让我们看看如何使用终端在 Linux 环境中安装 Java。这使您能够在自己的 Linux 环境和许多远程环境中使用这些步骤。

## 如何使用 SDKMan 管理 Java 版本

> SDKMAN！是一个工具，用于在大多数基于 Unix 的系统上管理多个软件开发工具包的并行版本。它提供了一个方便的命令行界面(CLI)和 API，用于安装、切换、删除和列出候选项。([来源](https://sdkman.io/))

SDKMan 自带安装程序，支持多种操作系统。确保之前安装 curl，之后执行安装程序脚本。

### 如何在 Ubuntu 22 上安装 SDKMan

```
# install curl
$ sudo apt install curl

# install sdkman
$ curl -s "https://get.sdkman.io" | bash
```

### 如何在 Debian 11 上安装 SDKMan

```
# login as root
$ su

# install curl
$ apt install curl zip

# exit root user session
$ exit

# install sdkman
$ curl -s "https://get.sdkman.io" | bash
```

### 如何在 MacOS 上安装 SDKMan

如果你还没有在 Mac 上安装 brew 和 curl，你需要安装它们来方便地安装和更新 sdkman。

```
# install brew package manager)
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# install curl
$ brew install curl

# install sdkman
$ curl -s "https://get.sdkman.io" | bash
```

现在，关闭并重新打开您的终端来使用 sdkman。

```
# print sdkman version to verify installation
$ sdk versionSDKMAN 5.15.0

# install latest java
$ sdk install java

# check your java installation and print your java’s version
$ java –version

Openjdk version "17.0.3" 2022-04-19OpenJDK Runtime Environment Temurin-17.0.3+7 (build 17.0.3+7)OpenJDK 64-Bit Server VM Temurin-17.0.3+7 (build 17.0.3+7, mixed mode, sharing)

# Show path to current java version$ which java/home/sesigl/.sdkman/candidates/java/current/bin/java
```

现在，您已经准备好使用 Java 了。

## 如何安装多个 Java 版本

安装多个 Java 版本非常有用。也许某些应用程序需要旧版本的 Java。或者您想玩一个全新的 Java 版本，然后轻松地切换回来。

接下来，您另外安装 Java 18:

```
$ sdk install java 18.0.1-tem
Done installing!

Do you want java 18.0.1-tem to be set as default? (Y/n): n
```

通过键入`n`，这意味着您不想使用 Java 18 作为您的默认版本。您可以通过执行`sdk use java <version>`在您的 shell 中临时手动启用版本。

```
$ sdk use java 18.0.1-tem
Using java version 18.0.1-tem in this shell.

$ java -version
openjdk version "18.0.1" 2022-04-19OpenJDK Runtime Environment Temurin-18.0.1+10 (build 18.0.1+10)OpenJDK 64-Bit Server VM Temurin-18.0.1+10 (build 18.0.1+10, mixed mode, sharing)
```

如果你关闭窗口或输入 Java `sdk use java 17.0.3-tem`你可以切换回来。

```
$ sdk use java 17.0.3-tem
Using java version 17.0.3-tem in this shell.

$ java -version
openjdk version "17.0.3" 2022-04-19OpenJDK Runtime Environment Temurin-17.0.3+7 (build 17.0.3+7)OpenJDK 64-Bit Server VM Temurin-17.0.3+7 (build 17.0.3+7, mixed mode, sharing)
```

## 如何自动切换 Java 版本

假设您有 2 个项目，一个使用 Java 17，一个使用 Java 18。通过在目录中创建一个`.sdkmanrc`文件，您可以自动切换版本，这将提高您的工作效率。

让我们为 Java 17 项目创建一个文件:

```
$ sdk env init
.sdkmanrc created.

$ tail .sdkmanrc
# Enable auto-env through the sdkman_auto_env config
# Add key=value pairs of SDKs to use below
java=17.0.3-tem
```

接下来，创建另一个目录，将 Java 版本切换到 Java 18，通过执行`sdk env init`创建另一个`.sdkmanrc`。

```
$ cd ..

$ mkdir my-java-18-project

$ cd my-java-18-project/

$ sdk use java 18.0.1-tem
Using java version 18.0.1-tem in this shell.

$ sdk env init
.sdkmanrc created.

$ tail .sdkmanrc
# Enable auto-env through the sdkman_auto_env config
# Add key=value pairs of SDKs to use below
java=18.0.1-tem
```

要自动切换 Java 版本，需要编辑文件`$HOME/.sdkman/etc/config`并设置`sdkman_auto_env=true`。已经有线路了，只需要把`false`改成`true`就可以了。

要启用配置更改，请重新启动您的终端。完成后，sdkman 会自动为您更改 Java 版本。

让我们也验证一下 Java 版本。

```
$ cd my-java-17-project/
Using java version 17.0.3-tem in this shell.

$ java -version
openjdk version "17.0.3" 2022-04-19
OpenJDK Runtime Environment Temurin-17.0.3+7 (build 17.0.3+7)
OpenJDK 64-Bit Server VM Temurin-17.0.3+7 (build 17.0.3+7, mixed mode, sharing)

$ cd ..
Restored java version to 17.0.3-tem (default)

$ cd my-java-18-project/
Using java version 18.0.1-tem in this shell.

$ java -version
openjdk version "18.0.1" 2022-04-19
OpenJDK Runtime Environment Temurin-18.0.1+10 (build 18.0.1+10)
OpenJDK 64-Bit Server VM Temurin-18.0.1+10 (build 18.0.1+10, mixed mode, sharing)
```

如果你想了解更多关于 sdkman 的信息，请查阅 [sdkman 使用文档](https://sdkman.io/usage)。

## 如何更新 Java 版本

一旦新的 Java 版本可用，它应该通过`sdk list java`列出。但是，您也可以使用`sdk upgrade java`让 sdkman 检查更新。

让我们安装一个旧版本的 Java:

```
$ sdk uninstall java 17.0.3-tem

$ sdk install java 17.0.2-tem

$ sdk install java 11.0.12-tem

$ sdk upgrade java
Available defaults:
java (local: 18.0.1-tem, 11.0.12-tem, 17.0.2-tem; default: 17.0.3-tem)

Use prescribed default version(s)? (Y/n): Y

Installing: java 17.0.3-tem
Done installing!
Setting java 17.0.3-tem as default.
```

通过`y`确认，它下载建议的默认版本`17.0.3-tem`，并将其设置为您系统上的默认版本。通过执行`sdk upgrade java`，这使得将来的更新变得容易。

## 摘要

在本文中，您了解了如何使用 sdkman 轻松管理 Java SDKs。这是一个非常有用的工具，它支持许多 Linux 发行版，包括 Ubuntu、Debian 和 MacOS。

SDKMan 使您能够安装和删除 Java 版本，在它们之间切换，以及用一个命令升级您的 Java 版本。这可以保持您的系统整洁，并使管理 Java SDKs 变得容易。

我希望你喜欢这篇文章。

如果你喜欢它，觉得有必要给我一点掌声👏或者只是想接触一下👋，[在 Twitter 上关注我](https://twitter.com/sesigl)。我在易贝·克莱南泽根公司工作，这是世界上最大的机密公司之一。顺便说一下，[我们正在招聘](https://www.ebay-kleinanzeigen.de/careers)！

### 参考

*   [https://en.wikipedia.org/wiki/Java_Development_Kit](https://en.wikipedia.org/wiki/Java_Development_Kit)
*   [https://adoptium.net/](https://adoptium.net/)
*   [https://phoenixnap.com/kb/create-a-sudo-user-on-debian](https://phoenixnap.com/kb/create-a-sudo-user-on-debian)
*   [https://stack overflow . com/questions/63336131/install-SDK man-in-an-alpine-based-docker-image](https://stackoverflow.com/questions/63336131/install-sdkman-in-an-alpine-based-docker-image)
*   [https://brew.sh/](https://brew.sh/)
*   [https://reflectoring.io/manage-jdks-with-sdkman/](https://reflectoring.io/manage-jdks-with-sdkman/)
*   [https://blog . jdriven . com/2020/10/automatic-switching-of-Java-versions-with-SDK man/](https://blog.jdriven.com/2020/10/automatic-switching-of-java-versions-with-sdkman/)