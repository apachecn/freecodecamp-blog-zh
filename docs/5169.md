# 如何用六行 python 代码构建图像类型转换器

> 原文：<https://www.freecodecamp.org/news/how-to-build-an-image-type-convertor-in-six-lines-of-python-d63c3c33d1db/>

通过 AMR

# 如何用六行 Python 代码构建图像类型转换器

![Wqg8dtilWWQd3TlCJxa7vyIAkJqGPnvLCmZJ](img/2d7eb65dd2df3f7a93dae3578a9eebaf.png)

Photo by [Keagan Henman](https://unsplash.com/photos/eifiZb9245c?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/brevity?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

作为一名程序员的优势之一是你有能力构建实用工具来改善你的生活。不像一个非程序员，你可能不会花几个小时在多个谷歌搜索结果页面中寻找一个工具，这个工具本来应该提高你的工作效率。这可能会让你觉得了解一门编程语言更强大——特别是如果这门编程语言像 Python 一样通用和出色的话。

[Python 之禅](https://www.python.org/dev/peps/pep-0020/#id3)中有一点说:

> 简单比复杂好。

有了这种理念，许多使用 Python 的利基工具开发可以如此简洁地完成，这让我怀疑是否值得称之为工具。有时候用`script`这个词会更准确。不管怎样，我们在这里开始构建一个这样的`script`,将图像从一种文件格式(图像类型)转换成另一种格式——只用了 6 行 Python 代码。

> *免责声明:行数(6)不包括空行和注释*

在本教程中，我们将建立一个图像类型转换器，将 PNG 图像转换为 JPG 图像。在你的灰质细胞急于判断我是否疯狂地建造这个工具之前，让我说这不仅仅是针对一个图像——而是针对一个文件夹中的所有图像。如果不编码的话，那肯定需要更多的手工劳动(我知道你能感觉到)。

#### Python 包

为此，我们将使用 Python 包`PIL`(代表 Python 图像库)。最初的`PIL`没有得到最新 Python 版本的任何更新，所以一些善良的灵魂创造了[一个友好的分叉，叫做`Pillow`](https://python-pillow.org/) ，它甚至支持> Python 3.0。

使用`pip3 install Pillow`安装。

#### **开始脚本**

该代码有两个主要部分。第一部分是我们导入所需包的地方，第二部分是实际操作发生的地方。实际操作可以进一步细分如下:

*   遍历具有给定扩展名的所有文件——在我们的例子中是`.png`——并重复以下所有操作:
*   打开图像文件(作为图像文件)
*   将图像文件转换为不同的格式(`RGB`)
*   最后保存文件——使用新的扩展名`.jpg`

**第 1 行和第 2 行:**

```
from PIL import Image  # Python Image Library - Image Processing
```

```
import glob
```

这个部分只是导入所需的包。`PIL`用于图像处理，而`glob`用于遍历操作系统中给定文件夹的文件。

**第 3–6 行:**

```
# based on SO Answer: https://stackoverflow.com/a/43258974/5086335
```

```
for file in glob.glob("*.png"):
```

```
 im = Image.open(file)
```

```
 rgb_im = im.convert('RGB')
```

```
 rgb_im.save(file.replace("png", "jpg"), quality=95)
```

#### 鳍状物

这就是我们工具的结尾！您可以将这 6 行保存为一个`.py`文件，然后在您的计算机中调用它们，在那里您可以转换图像。

#### 进一步发展

如果您计划进一步改进这个脚本，您可以将整个脚本转换成一个命令行界面工具——然后所有这些细节如`File Format`和`Folder Path`都可以作为参数给出，从而进一步扩展它的功能。

#### **参考文献**

*   这里使用的完整代码可以在 [my github](https://github.com/amrrs/py_img_convertor) 上找到
*   [Python 之禅](https://www.python.org/dev/peps/pep-0020/#id3)
*   [枕头](https://python-pillow.org/)