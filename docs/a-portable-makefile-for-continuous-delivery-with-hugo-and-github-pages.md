# 一个用于 Hugo 和 GitHub 页面连续交付的可移植 Makefile

> 原文：<https://www.freecodecamp.org/news/a-portable-makefile-for-continuous-delivery-with-hugo-and-github-pages/>

![cover](img/e928a135898a7d1a422c5c5e187e066a.png)

有趣的事实:1018 天前，我第一次推出了我的 GitHub 页面网站。

从那以后，我们一起成长。从早期值得畏缩的提交消息，通过 86 个版本的 [Hugo](https://gohugo.io/) ，直到上周，一个不太精简的多应用持续集成和部署(CI/CD)工作流。

如果你了解我，你就会知道我喜欢自动化。我一直在使用 AWS Lambda、Netlify 和 Travis CI 的组合来自动构建和发布这个站点。我的任务工作流程包括:

*   与 Hugo 一起按计划(Netlify 和 Lambda)构建 push to master
*   优化和调整图像大小(Netlify)；
*   用 [HTMLProofer](https://github.com/gjtorikian/html-proofer) 测试(Travis CI)；和
*   部署到我的[独立的、公共的 GitHub 页面库](https://victoria.dev/blog/two-ways-to-deploy-a-public-github-pages-site-from-a-private-hugo-repository/) (Netlify)。

多亏了 GitHub Actions 的引入，我可以只用一个可移植的 Makefile 来完成以上所有工作。

下周我将介绍我的行动设置；今天，我将带您了解我的 Makefile 的本质，以便您可以编写自己的 Makefile。

## Makefile 可移植性

[POSIX-standard-flavor Make](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/make.html)运行在所有类似 Unix 的系统上。Make 的衍生产品，如 [GNU Make](https://www.gnu.org/software/make/) 和几种风格的 BSD Make 也可以在类 Unix 系统上运行，尽管它们的特殊用途需要安装各自的程序。为了编写一个真正可移植的 Makefile，我的遵循 POSIX 标准。(对于 POSIX 兼容 makefile 的更全面总结，我发现这篇文章很有帮助:[关于可移植 makefile 的教程](https://nullprogram.com/blog/2017/08/20/)。)我运行 Ubuntu，所以我用 BSD Make 程序`bmake`、`pmake`和`fmake`测试了可移植性。与非 Unix 类系统的兼容性稍微复杂一些，因为 shell 命令不同。有了 Nmake 这样的衍生工具，最好用适当的 Windows 命令编写一个单独的 Makefile。

虽然我的许多特定用例可以通过 shell 脚本实现，但我发现 Make 提供了一些有价值的优势。我喜欢使用变量和[宏](https://en.wikipedia.org/wiki/Make_(software)#Macros)的便利，以及在组织我的步骤时[规则](https://en.wikipedia.org/wiki/Makefile#Rules)的模块化。

规则的编写主要归结于 shell 命令，这是 Makefiles 如此可移植的主要原因。最好的部分是你可以在终端中做几乎所有的事情，当然也可以处理上面列出的所有工作流程步骤。

## 我的连续部署生成文件

这是处理我的工作流的可移植 Makefile。是的，我在里面放了表情符号。我是个怪物。

```
.POSIX:
DESTDIR=public
HUGO_VERSION=0.58.3

OPTIMIZE = find $(DESTDIR) -not -path "*/static/*" \( -name '*.png' -o -name '*.jpg' -o -name '*.jpeg' \) -print0 | \
xargs -0 -P8 -n2 mogrify -strip -thumbnail '1000>'

.PHONY: all
all: get_repository clean get build test deploy

.PHONY: get_repository
get_repository:
	@echo "? Getting Pages repository"
	git clone https://github.com/victoriadrake/victoriadrake.github.io.git $(DESTDIR)

.PHONY: clean
clean:
	@echo "? Cleaning old build"
	cd $(DESTDIR) && rm -rf *

.PHONY: get
get:
	@echo "❓ Checking for hugo"
	@if ! [ -x "$(command -v hugo)" ]; then\
		echo "? Getting Hugo";\
	    wget -q -P tmp/ https://github.com/gohugoio/hugo/releases/download/v$(HUGO_VERSION)/hugo_extended_$(HUGO_VERSION)_Linux-64bit.tar.gz;\
		tar xf tmp/hugo_extended_$(HUGO_VERSION)_Linux-64bit.tar.gz -C tmp/;\
		sudo mv -f tmp/hugo /usr/bin/;\
		rm -rf tmp/;\
		hugo version;\
	fi

.PHONY: build
build:
	@echo "? Generating site"
	hugo --gc --minify -d $(DESTDIR)

	@echo "? Optimizing images"
	$(OPTIMIZE)

.PHONY: test
test:
	@echo "? Testing HTML"
	docker run -v $(GITHUB_WORKSPACE)/$(DESTDIR)/:/mnt 18fgsa/html-proofer mnt --disable-external

.PHONY: deploy
deploy:
	@echo "? Preparing commit"
	@cd $(DESTDIR) \
	&& git config user.email "hello@victoria.dev" \
	&& git config user.name "Victoria via GitHub Actions" \
	&& git add . \
	&& git status \
	&& git commit -m "? CD bot is helping" \
	&& git push -f -q https://$(TOKEN)@github.com/victoriadrake/victoriadrake.github.io.git master
	@echo "? Site is deployed!" 
```

接下来，该工作流:

1.  克隆公共页面存储库；
2.  清除(删除)以前的生成文件；
3.  下载并安装 Hugo 的指定版本(如果 Hugo 尚未安装);
4.  构建网站；
5.  优化图像；
6.  使用 HTMLProofer 测试生成的网站，并
7.  准备新的提交并推送到公共页面存储库。

如果您熟悉命令行，这其中的大部分可能看起来很熟悉。这里有几个可能需要解释一下的地方。

### 检查程序是否已经安装

我认为这一点很整洁:

```
if ! [ -x "$(command -v hugo)" ]; then\
...
fi
```

我使用一个否定的`if`条件和`command -v`来检查一个叫做`hugo`的可执行文件(`-x`)是否存在。如果没有，脚本会获取指定版本的 Hugo 并安装它。这个堆栈溢出答案很好地总结了为什么`command -v`比`which`更便于移植。

### 图像优化

我的 Makefile 使用`mogrify`来批量调整特定文件夹中图像的大小和压缩图像。它会使用文件扩展名自动查找它们，并且只修改任何维度上大于 1000px 目标大小的图像。在这篇文章中，我写了更多关于[批处理一行程序的内容。](https://victoria.dev/blog/how-to-quickly-batch-resize-compress-and-convert-images-with-a-bash-one-liner/)

有几种不同的方法可以完成同样的任务，理论上，其中一种方法是利用 Make 的[后缀规则](https://en.wikipedia.org/wiki/Make_(software)#Suffix_rules)只对图像文件运行命令。我发现 shell 脚本可读性更好。

### 使用 Dockerized HTMLProofer

HTMLProofer 是用`gem`安装的，用的是 Ruby 和 [Nokogiri](https://nokogiri.org/tutorials/ensuring_well_formed_markup.html) ，对于 CI 工作流来说加起来要花很多安装时间。谢天谢地， [18F](https://github.com/18F) 有一个[的 Dockerized 版本](https://github.com/18F/html-proofer-docker)，实现起来要快得多。它的使用需要启动容器，将构建的站点目录[挂载为数据卷](https://docs.docker.com/storage/volumes/#start-a-container-with-a-volume)，这可以通过添加`docker run`命令轻松实现。

```
docker run -v /absolute/path/to/site/:/mounted-site 18fgsa/html-proofer /mounted-site
```

在我的 Makefile 中，我使用默认环境变量 `GITHUB_WORKSPACE`来指定绝对站点路径。我将在下一篇文章中深入探讨这个和其他 GitHub Actions 特性。

同时，祝你快乐！