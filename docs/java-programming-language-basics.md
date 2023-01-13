# Java 编程语言基础

> 原文：<https://www.freecodecamp.org/news/java-programming-language-basics/>

[Java](https://www.oracle.com/java/index.html) 是由[太阳微系统](https://en.wikipedia.org/wiki/Sun_Microsystems)在 1995 年开发的一种编程语言，后来被[甲骨文](http://www.oracle.com/index.html)收购。它现在是一个完整的平台，拥有许多标准 API、开源 API、工具和庞大的开发人员社区，被大大小小的公司用来构建最值得信赖的企业解决方案。Android 应用程序开发完全由 Java 及其生态系统完成。想了解更多关于 Java 的知识，请看[这个](https://java.com/en/download/faq/whatis_java.xml)和[这个](http://tutorials.jenkov.com/java/what-is-java.html)。

## **版本**

最新版本是 2018 年发布的 [Java 11](http://www.oracle.com/technetwork/java/javase/overview) ，在之前版本 Java 10 的基础上做了[各种改进](https://www.oracle.com/technetwork/java/javase/11-relnote-issues-5012449.html)。但是出于所有的意图和目的，我们将在这个 wiki 的所有教程中使用 Java 8。

Java 也分为几个“版本”:

*   标准版-用于桌面和独立服务器应用
*   企业版——用于开发和执行嵌入在 Java 服务器中运行的 Java 组件
*   ME(微型版)，用于在手机和嵌入式设备上开发和执行 Java 应用程序

## 安装:JDK 还是 JRE？

从[官网](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载最新的 Java 二进制文件。这里你可能会面临一个问题，到底下载 JDK 还是 JRE？JRE 代表 Java 运行时环境，它是运行 Java 代码的平台相关的 Java 虚拟机，JDK 代表 Java 开发工具包，它包括大多数开发工具，最重要的是编译器`javac`，还有 JRE。因此，对于普通用户来说，JRE 就足够了，但是因为我们要用 Java 开发，所以我们要下载 JDK。

## **平台特定安装说明**

### **窗户**

*   下载相关的[。msi](https://en.wikipedia.org/wiki/Windows_Installer) 文件(32 位 x86 / i586，64 位 x64)
*   运行。msi 文件。这是一个自解压的可执行文件，它将在你的系统中安装 Java！

### **Linux**

*   为您的系统下载相关的[tar.gz](http://www.cyberciti.biz/faq/linux-unix-bsd-extract-targz-file/)文件并安装:

`bash $ tar zxvf jdk-8uversion-linux-x64.tar.gz`

*   [基于 RPM 的 Linux 平台](https://en.wikipedia.org/wiki/List_of_Linux_distributions#RPM-based)下载相关的[。rpm](https://en.wikipedia.org/wiki/RPM_Package_Manager) 文件和安装:

`bash $ rpm -ivh jdk-8uversion-linux-x64.rpm`

*   用户可以选择安装开源版本的 Java、OpenJDK 或 Oracle JDK。虽然 OpenJDK 正在积极开发中，并与甲骨文 JDK 公司保持同步，但它们只是在许可方面有所不同。然而，很少有开发商抱怨开放的 JDK 的稳定性。****Ubuntu**指令:**

打开 JDK 安装:
`bash sudo apt-get install openjdk-8-jdk`

Oracle JDK 安装:
`bash sudo add-apt-repository ppa:webupd8team/java sudo apt-get update sudo apt-get install oracle-java8-installer`

### **Mac**

*   要么下载麦克·OSX。来自 Oracle 下载的 dmg 可执行文件
*   或者用[自制](http://brew.sh/)到[安装](http://stackoverflow.com/a/28635465/2861269):

```
brew tap caskroom/cask  
brew install brew-cask  
brew cask install java
```

### **验证安装**

通过打开命令提示符(Windows) / Windows Powershell /终端(Mac OS 和*Unix)并检查 Java 运行时和编译器的版本，验证 Java 是否已正确安装在您的系统中:

```
$ java -version
java version "1.8.0_66"
Java(TM) SE Runtime Environment (build 1.8.0_66-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.66-b17, mixed mode)

$ javac -version
javac 1.8.0_66
```

****提示**** :如果在`java`或`javac`或两者上出现诸如“未找到命令”之类的错误，不要惊慌，这只是您的系统路径设置不正确。对于 Windows，参见 [this StackOverflow answer](http://stackoverflow.com/questions/15796855/java-is-not-recognized-as-an-internal-or-external-command) 或[这篇文章](http://javaandme.com/)如何做。也有针对 Ubuntu 和 Mac 的指南。如果你还是不明白，不要担心，就在我们的[聊天室](https://gitter.im/FreeCodeCamp/java)问我们！

## JVM

好了，现在我们已经完成了安装，让我们首先开始理解 Java 生态系统的本质。Java 是一种[解释和编译的](http://stackoverflow.com/questions/1326071/is-java-a-compiled-or-an-interpreted-programming-language)语言，也就是说，我们编写的代码被编译成字节码并被解释运行。我们把代码写进去。java 文件，Java 将它们编译成在 Java 虚拟机或 JVM 上运行的[字节码](https://en.wikipedia.org/wiki/Java_bytecode)。这些字节码通常有一个. class 扩展名。

Java 是一种非常安全的语言，因为它不允许你的程序直接在机器上运行。相反，您的程序运行在一个名为 JVM 的虚拟机上。这个虚拟机为你可以进行的低级机器交互提供了几个 API，但是除此之外，你不能显式地使用机器指令。这大大增加了安全性。

此外，一旦您的字节码被编译，它可以在任何 Java 虚拟机上运行。该虚拟机依赖于机器，即它在 Windows、Linux 和 Mac 上有不同的实现。但是多亏了这个虚拟机，你的程序可以在任何系统上运行。这种哲学被称为[“一次编写，随处运行”](https://en.wikipedia.org/wiki/Write_once,_run_anywhere)。

## 你好，世界！

让我们编写一个简单的 Hello World 应用程序。打开任意编辑器/ IDE 并创建一个文件`HelloWorld.java`。

```
public class HelloWorld {

    public static void main(String[] args) {
        // Prints "Hello, World" to the terminal window.
        System.out.println("Hello, World");
    }

}
```

****N.B.**** 记住在 Java 中文件名应该与 ****公有类的名称**** 完全相同以便编译！

现在打开终端/命令提示符。在终端/命令提示符下，将当前目录更改为文件所在的目录。并编译该文件:

```
$ javac HelloWorld.java
```

现在使用`java`命令运行文件！

```
$ java HelloWorld
Hello, World
```

恭喜你。您的第一个 Java 程序已经成功运行。这里我们只是打印一个字符串，并将其传递给 API `System.out.println`。我们将涵盖代码中的所有概念，但是欢迎您仔细研究一下！如果您有任何疑问或需要额外的帮助，请随时在我们的 [Gitter 聊天室](https://gitter.im/FreeCodeCamp/java)联系我们！

## **文档**

Java 有大量的文档记录，因为它支持大量的 API。如果您正在使用任何主流的 IDE，比如 Eclipse 或 IntelliJ IDEA，您会发现其中包含了 Java 文档。

另外，这里有一个 Java 编码的免费 ide 列表:

*   [NetBeans](https://netbeans.org/)
*   [月食](https://eclipse.org/)
*   [IntelliJ IDEA](https://www.jetbrains.com/idea/features/)
*   [安卓工作室](https://developer.android.com/studio/index.html)
*   [蓝色](https://www.bluej.org/)
*   [绝地武士](http://www.jedit.org/)
*   [Oracle JDeveloper](http://www.oracle.com/technetwork/developer-tools/jdev/overview/index-094652.html)