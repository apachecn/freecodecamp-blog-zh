# Django 项目最佳实践，让您的开发人员满意

> 原文：<https://www.freecodecamp.org/news/django-project-best-practices-for-happy-developers/>

你希望你的团队*享受*你的开发工作流程吗？你认为构建软件应该是*有趣且有意义的吗？*如果是这样，这是给你的帖子。

我已经和 Django 一起开发了很多年，我从来没有像现在这样对我的 Django 项目感到高兴。

在本文中，我将解释我是如何让用 Django 进行开发的一天成为我和我的工程团队最放松和愉快的开发经历的。

## Django 项目的定制 CLI 工具

不要键入:

```
python3 -m venv env
source env/bin/activate
pip install -r requirements.txt
python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py collectstatic
python3 manage.py runserver 
```

键入以下内容不是更好吗:

```
make start 
```

…让这一切发生在你身上？我想是的！

我们可以用一个自文档化的 Makefile 来实现。这是我在开发 Django 应用程序时经常使用的一个，比如[ApplyByAPI.com](https://applybyapi.com/):

```
SHELL := /bin/bash

include .env

.PHONY: help
help: ## Show this help
    @egrep -h '\s##\s' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $1, $2}'

.PHONY: venv
venv: ## Make a new virtual environment
    python3 -m venv $(VENV) && source $(BIN)/activate

.PHONY: install
install: venv ## Make venv and install requirements
    $(BIN)/pip install -r requirements.txt

migrate: ## Make and run migrations
    $(PYTHON) manage.py makemigrations
    $(PYTHON) manage.py migrate

db-up: ## Pull and start the Docker Postgres container in the background
    docker pull postgres
    docker-compose up -d

db-shell: ## Access the Postgres Docker database interactively with psql
    docker exec -it container_name psql -d $(DBNAME)

.PHONY: test
test: ## Run tests
    $(PYTHON) $(APP_DIR)/manage.py test application --verbosity=0 --parallel --failfast

.PHONY: run
run: ## Run the Django server
    $(PYTHON) $(APP_DIR)/manage.py runserver

start: install migrate run ## Install requirements, apply migrations, then start development server 
```

你会注意到上面那条线`include .env`的存在。这确保了`make`可以访问存储在名为`.env`的文件中的环境变量。

这允许 Make 在其命令中使用这些变量，例如，我的虚拟环境的名称，或者传入`$(DBNAME)`到`psql`。

那个奇怪的“`##`”注释语法是怎么回事？这样的 Makefile 为您提供了一套方便的命令行别名，您可以将其签入到您的 Django 项目中。只要你能记住所有这些别名，它就非常有用。

上面的`help`命令是默认运行的，当您运行`make`或`make help`时，它会打印一个有用的可用命令列表:

```
help                 Show this help
venv                 Make a new virtual environment
install              Make venv and install requirements
migrate              Make and run migrations
db-up                Pull and start the Docker Postgres container in the background
db-shell             Access the Postgres Docker database interactively with psql
test                 Run tests
run                  Run the Django server
start                Install requirements, apply migrations, then start development server 
```

所有常见的 Django 命令都包括在内，我们有一个`test`命令，它用我们喜欢的选项运行我们的测试。太棒了。

你可以在这里阅读我关于自文档化 Makefile 的完整[帖子，其中还包括一个使用`pipenv`的 Makefile 示例。](https://victoria.dev/blog/how-to-create-a-self-documenting-makefile/)

## 使用预提交挂钩节省您的脑力

我之前写过一些关于技术人类工程学的文章，这些文章可以让团队更容易开发出优秀的软件。

一个显而易见的地方是在检入 lint 代码之前使用预提交钩子。

这有助于保持开发人员签入的代码的质量。但最重要的是，它确保你的团队中没有人花时间去记住应该是单引号还是双引号，或者在哪里换行。

令人困惑地命名为[的预提交框架](https://pre-commit.com/)是保持钩子(不包括在克隆的仓库中)在本地环境中一致的另一种奇妙方式。

这是我的 Django 项目的配置文件`.pre-commit-config.yaml`:

```
fail_fast: true
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.1.0
    hooks:
      - id: detect-aws-credentials
  - repo: https://github.com/psf/black
    rev: 19.3b0
    hooks:
      - id: black
  - repo: https://github.com/asottile/blacken-docs
    rev: v1.7.0
    hooks:
      - id: blacken-docs
        additional_dependencies: [black==19.3b0]
  - repo: local
    hooks:
      - id: markdownlint
        name: markdownlint
        description: "Lint Markdown files"
        entry: markdownlint '**/*.md' --fix --ignore node_modules --config "./.markdownlint.json"
        language: node
        types: [markdown] 
```

这些钩子检查意外的秘密提交，使用[黑色](https://github.com/psf/black)格式化 Python 文件，使用 [`blacken-docs`](https://github.com/asottile/blacken-docs) 格式化 Markdown 文件中的 Python 片段，以及 [lint Markdown 文件](https://github.com/igorshubovych/markdownlint-cli)。

对于您的特定用例，甚至可能有更有用的钩子可用:参见[支持的钩子](https://pre-commit.com/hooks.html)来探索。

## 有用的 gitignores

改善团队日常开发体验的一个不被重视的方法是确保您的项目使用一个全面的`.gitignore`文件。

这有助于防止包含秘密的文件被提交，并且通过确保您永远不会筛选生成的文件，可以额外节省开发人员几个小时的乏味时间。

为了有效地为 Python 和 Django 项目创建一个 [gitignore，Toptal 的](https://www.toptal.com/developers/gitignore/api/python,django) [gitignore.io](https://gitignore.io/) 可以成为生成一个健壮的`.gitignore`文件的良好资源。

我仍然建议您自己检查生成的结果，以确保被忽略的文件适合您的用例，并且您希望被忽略的内容都不会被注释掉。

## 用 GitHub 动作进行连续测试

如果你的团队在 GitHub 上工作，建立一个带有动作的测试过程是容易实现的。

在一致的环境中对每个拉请求运行测试可以帮助消除“在我的机器上工作”的难题。他们还可以帮助确保没有人坐在那里等待测试在本地运行。

GitHub Actions 这样的托管 CI 环境在运行需要使用托管服务资源的集成测试时也会有所帮助。

您可以在存储库中使用[加密的秘密来授权 Actions runner 访问测试环境中的资源，而不用担心为您的每个开发人员创建测试资源和访问密钥。](https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets)

我在很多场合写过关于设置动作工作流的文章，包括[使用一个来运行你的 Makefile](https://victoria.dev/blog/a-lightweight-tool-agnostic-ci/cd-flow-with-github-actions/) ，以及[如何集成 GitHub 事件数据](https://victoria.dev/blog/publishing-github-event-data-with-github-actions-and-pages/)。GitHub 甚至[就行动](https://github.blog/2020-06-26-github-action-hero-victoria-drake/)采访过我一次。

对于 Django 项目，这里有一个简单的 GitHub Actions 工作流，它使用一致的 Python 版本运行测试。

```
name: Run Django tests

on: push

jobs:
  test:

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.8'
      - name: Install dependencies
        run: make install
      - name: Run tests
        run: make test 
```

对于安装和测试命令，我简单地利用了已经登记到存储库中的 [Makefile](https://victoria.dev/blog/my-django-project-best-practices-for-happy-developers/#a-custom-cli-tool-for-your-django-project) 。

在 CI 测试工作流中使用 Makefile 命令的一个好处是，您只需要在一个地方更新它们——您的 Makefile！不再问“为什么这在本地有效，但在 CI 中无效？？！?"头疼。

如果你想升级你的安全游戏，你也可以添加 [Django 安全检查](https://github.com/victoriadrake/django-security-check)作为一个动作。

## 为成功建立您的 Django 项目

想让你的开发团队开心吗？用这些 Django 开发的最佳实践帮助他们取得成功。

记住，一盎司的脑力抵得上一磅的软件。