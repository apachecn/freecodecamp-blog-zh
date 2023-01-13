# Python 虚拟环境举例说明

> 原文：<https://www.freecodecamp.org/news/python-virtual-environments-explained-with-examples/>

虚拟环境可以描述为独立的安装目录。这种隔离允许您本地化项目依赖项的安装，而不强制您在系统范围内安装它们。

假设您有两个应用程序，App1 和 App2。两者都使用 package，但是需要不同的版本。如果为 App1 安装 Pak 2.3 版，您将无法运行 App2，因为它需要 3.1 版。

这就是虚拟环境派上用场的地方。

好处:

*   您可以有多个环境，有多组包，它们之间没有冲突。这样，可以同时满足不同项目的需求。
*   您可以很容易地发布您的项目及其独立的模块。

这里有两种方法可以创建 Python 虚拟环境。

## **虚拟人**

`[virtualenv](https://virtualenv.pypa.io/en/stable/)`是一个用来创建隔离 Python 环境的工具。它创建一个文件夹，其中包含使用 Python 项目所需的包所需的所有可执行文件。

可以用`pip`安装:

```
pip install virtualenv
```

使用以下命令验证安装:

```
virtualenv --version
```

### **创造环境**

要创建虚拟环境，请使用:

```
virtualenv --no-site-packages my-env
```

这将在当前目录中创建一个文件夹，其名称为环境名称(`my-env/`)。该文件夹包含安装模块和 Python 可执行文件的目录。

您还可以指定想要使用的 Python 版本。就用论点`--python=/path/to/python/version`吧。比如，`python2.7`:

```
virtualenv --python=/usr/bin/python2.7 my-env
```

### **列出环境**

您可以使用以下命令列出可用的环境:

```
lsvirtualenv
```

### **激活环境**

在开始使用环境之前，您需要激活它:

```
source my-env/bin/activate
```

这确保了只使用`my-env/`下的包。

您会注意到环境的名称显示在提示符的左侧。这样你就可以看到哪个是活动环境。

### **安装包**

您可以逐个安装软件包，或者为您的项目设置一个`requirements.txt`文件。

```
pip install some-package
pip install -r requirements.txt
```

如果您想从已经安装的软件包中创建一个`requirements.txt`文件，请运行以下命令:

```
pip freeze > requirements.txt
```

该文件将包含当前环境中安装的所有软件包的列表，以及它们各自的版本。这将有助于您发布带有独立模块的项目。

### **停用环境**

如果您已经完成了虚拟环境的工作，您可以通过以下方式停用它:

```
deactivate
```

这将使您回到系统的默认 Python 解释器及其所有已安装的库。

### **删除环境**

只需删除环境文件夹。

## **康达**

[`Conda`](https://conda.io/docs/index.html) 是许多语言的包、依赖和环境管理，包括 Python。

要安装 Conda，请遵循这些[说明](https://conda.io/docs/user-guide/install/index.html)。

### **创造环境**

要创建虚拟环境，请使用:

```
conda create --name my-env
```

Conda 将在 Conda 安装目录中创建相应的文件夹。

您还可以指定想要使用哪个版本的 Python:

```
conda create --name my-env python=3.6
```

### **列出环境**

您可以使用以下命令列出所有可用的环境:

```
conda info --envs
```

### **激活环境**

在开始使用环境之前，您需要激活它:

```
source activate my-env
```

### **安装包**

与`virtualenv`相同。

### **停用环境**

如果您已经完成了虚拟环境的工作，您可以通过以下方式停用它:

```
source deactivate
```

### **移除环境**

如果要从 Conda 使用中删除环境:

```
conda remove --name my-env
```