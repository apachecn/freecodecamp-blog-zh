# 面向初学者的 Kotlin 编程基础

> 原文：<https://www.freecodecamp.org/news/kotlin-programming-basics-for-beginners/>

## 科特林是什么？

Kotlin 是由 T2 Jetbrains 公司开发的一种编程语言，该公司开发了一些世界上最流行的 ide，如 IntelliJ 和 Pycharm。

它作为 Java 的替代品，运行在 JVM 上。它已经开发了将近 6 年，仅仅在一年前才达到 1.0。

开发者社区对 Kotlin 的接受程度如此之高，以至于谷歌在 2017 年谷歌 I/O 大会上宣布为 Android 开发提供一流的语言支持。

## **版本**

在撰写本文时，Kotlin 的最新稳定版本恰好是[版本 1.2.71](https://blog.jetbrains.com/kotlin/2018/09/kotlin-1-2-70-is-out/)

## **安装**

在继续 Kotlin 的安装说明之前，您需要确保已经在您的系统上设置了****【JDK(Java 开发工具包)】**** 。

如果你的电脑上没有安装 JDK，请点击[上的 ****安装部分**** 来学习](https://guide.freecodecamp.org/java)如何安装。

科特林与 ****JDK 1.6+**** 一起工作，所以确保你安装了正确的版本。一旦完成了 JDK 的设置，请继续执行以下步骤。

## ****IntelliJ IDEA****

让 Kotlin 在你的机器上运行的最快方法是把它和 ****IntelliJ IDEA**** 一起使用。这是推荐给 Kotlin 的 IDE，因为 Jetbrains 提供了工具支持。你可以从 [JetBrains](https://www.jetbrains.com/) 下载 IntelliJ 的[社区版](http://www.jetbrains.com/idea/download/index.html)。

一旦你安装了 IntelliJ，你就可以开始你在 Kotlin 的第一个项目，而不需要任何进一步的配置。

创建一个 ****新项目**** ，并确保选择 Java 模块。选择屏幕上的 Kotlin 复选框

![new project screen](img/caad9217976da029033a9d564fa8b648.png)

为您的项目命名，然后单击 Finish。

![project name ](img/a7ef26912dc42d6006514e46bf530340.png)

现在您将被带到主编辑器，在那里您将看到您的项目文件以如下方式组织。

![project structure ](img/deaf51a55efa502fe25d42c0d1494454.png)

为了验证您的安装，在 ****src**** 文件夹中创建一个新的 Kotlin 文件，并将其命名为 ****app**** (或者其他任何适合您的名称)

![project structure ](img/e84ce18e4d1bf15b1bb7488b92f8e54c.png)

一旦创建了文件，就可以键入下面的 cremonial Hello World 代码。如果它现在没有意义，不要担心，它将在本指南的后面部分详细讨论。

```
fun main (args: Array<String>) {
    println("Hello World!")
}
```

![project structure ](img/57d30d80b81c2861fba847c326fe2c76.png)

你现在可以运行这个程序了，方法是:点击装订线上的 Kotlin 图标(在编辑器的左边，有行号)

![hello world ](img/be405127d2bd08f75194165d06f1ba69.png)

如果一切正常，您应该会看到消息 Hello World！在如下所示的运行窗口中

![run window ](img/f8fad370e58d5b73a462e68ae8249a68.png)

## ****月食****

虽然 IntelliJ 是用 Kotlin 开发的推荐 IDE，但它绝对不是唯一的选择。Eclipse 恰好是 Java 开发人员选择的另一个流行的 IDE，Eclipse 也支持 Kotlin。

在您的系统上设置好 ****JDK**** 之后，按照下面的说明操作。

为您的操作系统下载[****Eclipse Neon****](https://www.eclipse.org/downloads/)，一旦您成功地将它安装到您的系统上，从[****Eclipse market place****](http://marketplace.eclipse.org/content/kotlin-plugin-eclipse)下载 Eclipse 的 ****Kotlin 插件**** 。

![eclipse marketplace ](img/97d70e8a22476b7685b247f3e7e1841d.png)

*****注意:你也可以通过进入帮助- > Eclipse Marketplace，然后搜索 Kotlin 插件***** 来完成

一旦插件安装完毕，你就基本上完成了，但是用一个快速的 Hello World 示例来测试一下 IDE 是个不错的主意。

通过点击文件->新建-> Kotlin 项目创建一个新的 Kotlin 项目

![new kotlin project ](img/aea7b2c35543900de36c576b92693377.png)

将创建一个空项目，其目录结构与 Java 项目非常相似。它看起来会像这样

![empty kotlin project ](img/9442729d699188a483e0af71ffc75278.png)

继续在 ****src**** 文件夹中创建一个新的 Kotlin 文件

完成后，继续输入下面的代码。如果它现在没有意义，不要担心，它将在本指南的后面部分介绍。

```
fun main (args: Array<String>) {
    println("Hello World!")
}
```

![eclipse hello world ](img/ec4fe1a82bb9a2d8b6cd4d292d46e064.png)

现在您已经完成了 Hello World 代码的输入，继续运行它。要运行该文件，右击编辑器内的任意位置，点击 *****运行方式- >科特林应用*****

![eclipse run app](img/82191b1aa24d7e3ba36acdbc949c579b.png)

如果一切顺利，控制台窗口将会打开，向您显示输出。

![eclipse run app](img/eafc286b599f5ab7f69e23a87aee2293.png)

## ****使用终端上的独立编译器****

如果你喜欢用更手工的方式做事，不想把自己绑在编辑器/IDE 上，你可能想用 Kotlin 编译器。

### **下载编译器**

在 Kotlin 的每个版本中，Jetbrains 都提供了一个独立的编译器，可以从 GitHub 的版本中下载。在撰写本文时，版本 1.1.51 恰好是最新的。

### 手动安装

下载完编译器后，您需要将其解压缩，并使用安装向导继续进行标准安装。将 ****bin**** 目录添加到系统路径是一个可选步骤。它包含在 Windows、Linux 和 macOS 上编译和运行 Kotlin 所必需的脚本。

### 通过自制软件安装

你可以使用 macOS 的软件包管理器 [Homebrew](http://brew.sh/) 在 macOS 上安装编译器。启动终端应用程序并发出以下命令

```
$ brew update
$ brew install kotlin
```

### 通过 SDKMAN 安装！

在 macOS、Linux、Cygwin、FreeBSD 和 Solaris 上安装 Kotlin 编译器的另一个简单方法是使用 [SDKMAN！](http://sdkman.io/)。启动终端并发出以下命令

`$ curl -s https://get.sdkman.io | bash`

按照屏幕上的指示，一旦 SDKMAN！安装程序是否在终端内部发出以下命令

`$ sdk install kotlin`

与所有以前的安装选项一样，测试运行安装是一个好主意。

打开你选择的文本编辑器，写一个下面给出的基本 Kotlin 程序

```
fun main(args: Array<String>) {
    println("Hello, World!")
}
```

用 ****保存这个文件。kt**** 分机。您现在可以编译它并查看结果了。为此，发出以下命令

`$ kotlinc hello.kt -include-runtime -d hello.jar`

`-d`选项告诉编译器您希望输出被称为什么。`-include-runtime`选项使结果。通过在 jar 文件中包含 Kotlin 运行时库，jar 文件是自包含的和可运行的。

如果没有编译错误，使用以下命令运行应用程序

`$ java -jar hello.jar`

如果一切顺利，你应该会看到 ****Hello World！**** 印在你的终端屏幕上

```
$ java -jar hello.jar       
Hello, World!
```

恭喜您已经成功地在您的系统上设置了 Kotlin 编译器和开发环境。我们将在本指南中涵盖 Kotlin 的所有复杂和有趣的部分，但是如果你想的话，你可以先去 [Try Kotlin](https://try.kotlinlang.org/) 网站并浏览那里的练习。

## **文档**

Kotlin 最大的优点之一是它的全面和结构良好的文档。即使你是编程新手，你也会发现自己对这些文档了如指掌。他们非常出色地以一种结构良好的方式展示了这一切。你可以在[这个链接](https://kotlinlang.org/docs/reference/)查看官方文档。

## 更多关于科特林的信息

*   [使用 Kotlin 开发原生 Android 应用——全程](https://www.freecodecamp.org/news/learn-how-to-develop-native-android-apps-with-kotlin-full-tutorial/)
*   [为什么你应该尝试 Kotlin 而不是 Java](https://www.freecodecamp.org/news/still-using-java-for-android-development-you-should/)
*   [如何用 Kotlin 构建 Android messenger 应用](https://www.freecodecamp.org/news/how-to-build-an-android-messenger-app-with-online-presence-using-kotlin-fdcb3ea9e73b/)