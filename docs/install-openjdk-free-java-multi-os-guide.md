# 如何安装 OpenJDK(免费 Java)-多操作系统指南

> 原文：<https://www.freecodecamp.org/news/install-openjdk-free-java-multi-os-guide/>

简而言之，Java 有两个共存的分支:专有的闭源 Oracle Java 和社区维护的[开源 OpenJDK](https://github.com/openjdk/jdk) 。

OpenJDK 在 [GPL-2.0](https://github.com/openjdk/jdk/blob/master/LICENSE) 下获得许可，它由一个 java 虚拟机和一个 Java 字节码编译器组成。因为这是更容易和更便宜的方法，这是我们在本教程中要使用的方法。

在这里，您将学习如何以几种不同的方式在 Windows、Mac 和 Linux 上安装 OpenJDK。

# 如何安装 OpenJDK

## 非常简单的半自动模式–适用于 Windows 和 macOS

请记住，这需要管理员访问权限。

如果你很着急，只是想要一个简单的卸载程序和自动设置的即插即用安装，这很好——我不会评判。:)

前往社区驱动、 [Eclipse Foundation 支持的](https://blog.adoptopenjdk.net/2021/03/transition-to-eclipse-an-update/) [采用开放的 JDK](https://adoptopenjdk.net/) 网站，获取安装程序的链接(如果您有疑问，请使用 HotSpot JVM 上的 OpenJDK 11 LTS)。

另外，如果您不知道的话，Eclipse 是主要的开源 Java IDE。

您将被重定向到一个带有安装链接列表的页面。找到你的操作系统，选择打包的安装程序(`.msi`用于 Windows 或者`.pkg`用于 macOS)并下载。**记得安装所有功能**，因为如果你不允许安装程序设置 JAVA_HOME，它就不能开箱即用。然后运行它，瞧！你完了。

## 非常简单的半自动模式——适用于 Linux

当然，这种方法也需要管理员权限。

首先，记住运行以下命令:

`sudo apt-get update`

您的操作系统很可能在存储库管理器中有自己的 OpenJDK 包。

对于 Ubuntu/Debian 来说，包名一般都是像`openjdk-<version_number>-jre-headless`这样命名的。例如:

`sudo apt install openjdk-8-jre-headless # installs for java 8`
或
`sudo apt install openjdk-13-jre-headless # installs for java 13`

就是这样，开源社区再次拯救了世界。

## 仍然很简单，主要是手动模式——适用于 Windows、macOS 和 Linux

您可以从许多不同的供应商那里获得压缩的 OpenJDK，比如微软、Red Hat、英特尔或任何提供 OpenJDK 的供应商。他们甚至可能提供自己的安装文件。但是为了简单起见，我们再次使用[采用开放 JDK](https://adoptopenjdk.net/) 。

选择您的首选版本和 JVM(如果您不确定，请选择 HotSpot JVM 上的 OpenJDK 11 LTS)并下载压缩的 JDK。

为什么您会选择这个选项而不是上面描述的更简单的方法呢？也许您在当前机器上没有管理员权限，或者您正在设置自己的策略来管理多个 Java 版本。我不知道，但它有它的使用案例。

### Windows 的步骤

1.  将提取的文件存储在目录树中

首先，将 zip 文件解压到一个文件夹中(`C:\Program Files\OpenJDK`将是[受过教育的选择](https://www.makeuseof.com/tag/default-windows-files-folders/))。注意`\OpenJDK`是手动添加的)。它将为 JDK 安装创建一个文件夹，其中的一个子目录是`\bin`。

您需要管理员权限才能将 zip 文件解压缩到该位置。

如果您因为任何原因不能使用管理员权限，请将其提取到您的用户空间下的某个位置，比如`C:\Users\%YOUR_USERNAME%\OpenJDK`。

2.开放环境变量

打开控制面板>系统&安全>系统>高级系统设置(在 Windows 10+中会在‘设备规格’下)。

在“系统属性”窗口中，选择“高级”选项卡，然后选择“环境变量”。

3.设置 JAVA_HOME:

在“系统变量”下，单击“新建”。输入变量名 JAVA_HOME。输入变量值作为 JDK 的安装路径(在路径末尾附加`\bin`子文件夹)。我的是`C:\Program Files\OpenJDK\OpenJDK11U-jdk_x64_windows_hotspot_11.0.15_10\jdk-11.0.15+10\bin`。

单击确定并应用更改。如果您以非管理员身份执行此过程，请选择`User Variables`。

4.将二进制可执行文件添加到路径:

停留在环境变量窗口中。点击名为`Path`的变量(系统或用户，取决于您在最后一节的选择)。

你会看到一个清单。这些是您可以从 CLI 访问的可执行文件(如 Windows 终端、命令提示符或 Poweshell)。

点击右上角的“新建”并添加`%JAVA_HOME%`作为变量。

单击确定并应用更改。

5.试验设备

打开命令行界面。类型`java -version`。如果输出是版本，一切正常，恭喜！

如果不是，请重新启动计算机，然后再试一次。如果它仍然不起作用，请仔细检查本教程，尝试读取 JAVA_HOME 路径，并查看它是否指向下载文件夹路径中的 bin 文件夹。

### Linux/macOS 的步骤:

1.  将提取的文件存储在目录树中:

然后，提取适合您的操作系统的压缩文件。万一你不能或者不想使用管理员权限，在你的用户空间中提取它(比如`~/.openjdk`)。

如果您想要一个更传统的位置，将其提取到`/usr/local/`，这是 POSIX 系统中用户手动安装软件的传统位置。

我的命令(对于 Linux)是这样的:`sudo tar -xf OpenJDK11U-jdk_x64_linux_hotspot_11.0.16.1_1.tar.gz -C /usr/local`。

2.设置 JAVA_HOME 并将其添加到路径:

将 JAVA_HOME 设置为您解压缩 OpenJDK 安装的位置。将它指向 OpenJDK 目录，而不是它的`/bin`子文件夹，因为 JAVA_HOME 将不仅仅用于确定可执行文件的位置。

这应该位于您的 shell 初始化文件中。例如，让我们假设这是我的`~/.zshrc`文件的最后两行:

```
 export JAVA_HOME="/usr/local/jdk-11.0.16.1+1"
   export PATH="$JAVA_HOME/bin:$PATH" 
```

3.验证安装

现在，通过获取 init 文件或打开另一个选项卡/窗口来刷新您的 shell。

可以用`java -version`检查安装。如果没有显示错误，那么恭喜您！是时候做些 Java 了。

## 包扎

就是这样！现在，您应该已经安装了 OpenJDK，可以在您的机器上使用了。感谢阅读。