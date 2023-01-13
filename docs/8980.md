# 如何在 PyPi 上发布自己的 Python 包

> 原文：<https://www.freecodecamp.org/news/how-to-publish-a-pyton-package-on-pypi-a89e9522ce24/>

马可·马森齐奥

# 如何在 PyPi 上发布自己的 Python 包

![hjsxCTEsrUvORaRML1X395Uf5zKplgBW9iZC](img/5b401b8d4d2437bdd0ebdf1dcb143142.png)

Python is one the most powerful, yet friendly programming languages today.

想和其他开发者分享你的 Python 代码吗？想让你的用户在安装你的软件包时生活更轻松吗？那么您应该将您的 Python 包发布到 PyPi。

好消息是，现在比以往任何时候都容易。这是一个简短的指南，它将引导您完成整个过程，并指引您找到相关的文档。

### 步骤 1:创建一个 setup.py 文件

*setup()* 的参数在这里的[中有记录，并且不是微不足道的。一个很好的例子是我的](https://packaging.python.org/distributing/#setup-args) [filecrypt](https://github.com/massenz/filecrypt) 项目的 *setup.py* 文件。

以下是一个简短的摘录。同样，请务必[阅读文档](https://packaging.python.org/distributing/#setup-args)以获得更多信息，因为它对所有这些论点的含义的解释比我更好，即使是用一篇完整的中型文章也能做到:

```
setup(name='crytto',      description='An OpenSSL-based file encryption                    and decryption utility',      long_description=long_description,      version='0.2.0',      url='https://github.com/massenz/filecrypt',      author='M. Massenzio',      author_email='marco@alertavert.com',      license='Apache2',      classifiers=[          'Development Status :: 4 - Beta',          'Intended Audience :: System Administrators',          'License :: OSI Approved :: Apache Software License',          'Programming Language :: Python :: 3'      ],      packages=['crytto'],      install_requires=[          'PyYAML>=3.11',          'sh>=1.11'      ],      entry_points={          'console_scripts': [              'encrypt=crytto.main:run'          ]      })
```

**注意:****不要**混淆 setuptools 和 distutils——下面是 setup.py 的正确导入:

```
from setuptools import setup
```

最棘手的部分是为您的*脚本文件找出包名和正确的配置。*最好提前决定这些，但是你可以在制作你的 setup.py 时修改这些。

最大的挑战是想出一个不与现有名称冲突的顶级包名称。据我所知，目前主要是一个试错的过程。

一旦 setup.py 有了合适的形状，你就可以试着做一个轮子了:

```
python setup.py bdist_wheel
```

这样做之后，最好创建一个新的 virtualenv，并尝试在其中安装新的包:

```
virtualenv test_env./test_env/bin/activatepip install dist/my-project.whl
```

这对于测试*控制台脚本*是否已经正确配置特别有用。

如果您使用*量词*,如:

```
classifiers=[     'Development Status :: 4 - Beta',     'Intended Audience :: System Administrators',     'License :: OSI Approved :: Apache Software License',    'Programming Language :: Python :: 3']
```

…然后确保查阅[分类器列表](https://pypi.python.org/pypi?%3Aaction=list_classifiers)，否则会导致错误并阻止注册。

### 注册您的项目

![0kPl1058z7-o9Hw-0VLNSkuHpuIjLTsx7R6i](img/2e0288608c00d4ad5dc1ed99bcd2b671.png)

**注意:**文档告诉我在这一步使用 twine，但是对我不起作用。您的里程可能会有所不同。

除非您已经在 PyPi 上有一个帐户，否则您需要[创建一个](https://pypi.python.org/pypi?%3Aaction=register_form)，然后登录。

然后你可以前往[注册表格](https://pypi.python.org/pypi?%3Aaction=submit_form)并上传你的 **PKG 信息**文件。这是在一个*【prj 名称】中创建的。鸡蛋信息/* 目录。当您试图安抚 PyPi 的神来接受您的配置选择时，可能需要一些来回。

特别是，想出一个不冲突但有意义的包名可能需要比你想象的更多的尝试。我再次建议您计划，因为我还没有找到一种简单的方法来列出所有的包名。如果你知道一个，一定要留下评论。你会注意到，根据 PyPi…

```
There are currently 88906 packages here.
```

(“此处”为 PyPi，截至 2016 年 9 月 16 日)。

### 上传到 PyPi

注册成功后，使用 twine 进行实际上传相当容易:

```
twine upload dist/*
```

前提是您有有效的~/。pypirc，它只会问你的密码。那么你只需要:

```
$ cat ~/.pypirc [distutils] index-servers=pypi
```

```
[pypi] repository = https://upload.pypi.org/legacy/ username = [your username]
```

就是这样。享受构建和共享 Python 包的乐趣吧！

我最初在 codetrips.com 的博客上发表了这篇文章。