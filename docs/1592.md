# 关系数据库课程——如何使用 Docker 和 freeCodeCamp 学习 VSCode 中的 SQL

> 原文：<https://www.freecodecamp.org/news/how-to-run-freecodecamps-relational-databases-curriculum-using-docker-vscode-and-coderoad/>

现在，您可以在 VSCode 编辑器中学习关系数据库概念和 SQL。本教程将引导你如何使用 Docker 安装它。

在这个全长 300 小时的课程中，您将学习构建十几个项目。其中一些将涉及一步一步的指示，而其他的将是开放式的，带有精心设计的测试套件。

您将使用真正的开发人员工具和软件，如 VS Code、PostgreSQL 和 Linux / Unix 命令行来完成交互式教程和构建项目。

### 你将学到什么

*   Linux / Unix 命令行
*   关系数据库
*   SQL 和 PostgreSQL
*   Bash 和 Bash 脚本
*   Git 和 GitHub
*   毫微；纤（10 的负九次方）
*   以及许多其他的概念和工具

这门课程是由在线课程的搜索引擎和复习网站 [Class Central](https://www.classcentral.com/) 资助的。

## 如何安装 Docker 并运行关系数据库课程

Docker 将在您的计算机上运行一个容器，其中包含这些教程所需的软件和文件结构。

您将使用 VSCode 和 Dev Containers 扩展在该容器中工作。一旦运行，CodeRoad 扩展将运行我们创建的教程。

### 先决条件

在开始之前，您需要安装一些东西:

1.  [Docker 引擎](https://docs.docker.com/engine/install/)
2.  [VS 代码](https://code.visualstudio.com/download)
3.  VS 代码的[开发容器](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)扩展
4.  [去](https://git-scm.com/downloads)

### 如何在 Docker 中运行项目

按照以下步骤运行 docker 容器并启动教程:

1.  用`git clone https://github.com/freeCodeCamp/rdb-alpha`将 RDB 阿尔法回购克隆到你的电脑上
2.  打开终端，导航到`rdb-alpha`目录，用`code .`打开 VS 代码
3.  在 VS 代码中，用`Ctrl / Cmd + Shift + P`打开命令面板。然后，进入并运行`Dev Containers: Rebuild and Reopen in Container`
4.  一个新的 VS 代码窗口将会打开，并开始构建 Docker 映像。第一次构建需要几分钟时间
5.  一旦映像构建完成，用`Ctrl / Cmd + Shift + P`再次打开命令面板，进入并运行`CodeRoad: Start`。在扩展安装到您的容器中之前，该命令不可用
6.  在 CodeRoad 窗口中，单击“开始新教程”
7.  点击`URL`选项卡，输入您想要启动的项目的`tutorial.json`文件的 URL(例如:[https://raw . githubusercontent . com/freeCodeCamp/learn-bash-by-building-a-boilerplate/main/tutorial . JSON](https://raw.githubusercontent.com/freeCodeCamp/learn-bash-by-building-a-boilerplate/main/tutorial.json))下面是可用教程的完整列表。
8.  单击“开始”按钮开始课程

### 如何重启或切换项目

如果您重新启动或切换项目，您将会丢失已经开始的教程的进度以及已经创建的任何文件或文件夹。

1.  用`Ctrl / Cmd + Shift + P`打开命令面板，输入并运行`Dev-Containers: Rebuild Container`
2.  等待 VS 代码重新打开重新加载容器
3.  像以前一样，从命令面板打开 CodeRoad，单击“开始新教程”,并输入您想要做的项目的`tutorial.json`文件的 URL

### 可用课程

以下是当前可用的教程列表。打开其中一个并使用它的 URL，如上面的说明所述，来启动它。

*   [通过构建样板文件学习 Bash](https://raw.githubusercontent.com/freeCodeCamp/learn-bash-by-building-a-boilerplate/main/tutorial.json)
*   [通过构建马里奥数据库学习关系数据库](https://raw.githubusercontent.com/freeCodeCamp/learn-relational-databases-by-building-a-mario-database/main/tutorial.json)
*   [天体数据库](https://raw.githubusercontent.com/freeCodeCamp/learn-celestial-bodies-database/main/tutorial.json)
*   [通过构建五个程序学习 Bash 脚本](https://raw.githubusercontent.com/freeCodeCamp/learn-bash-scripting-by-building-five-programs/main/tutorial.json)
*   [通过构建学生数据库学习 SQL:第 1 部分](https://raw.githubusercontent.com/freeCodeCamp/learn-sql-by-building-a-student-database-part-1/main/tutorial.json)
*   [通过构建学生数据库学习 SQL:第二部分](https://raw.githubusercontent.com/freeCodeCamp/learn-sql-by-building-a-student-database-part-2/main/tutorial.json)
*   [世界杯数据库](https://raw.githubusercontent.com/freeCodeCamp/learn-world-cup-database/main/tutorial.json)
*   [通过构建 Kitty Ipsum 翻译器学习高级 Bash](https://raw.githubusercontent.com/freeCodeCamp/learn-advanced-bash-by-building-a-kitty-ipsum-translator/main/tutorial.json)
*   [通过建立自行车租赁店学习 Bash 和 SQL](https://raw.githubusercontent.com/freeCodeCamp/learn-bash-and-sql-by-building-a-bike-rental-shop/main/tutorial.json)
*   [沙龙预约时间表](https://raw.githubusercontent.com/freeCodeCamp/learn-salon-appointment-scheduler/main/tutorial.json)
*   [通过建造城堡学习纳米](https://raw.githubusercontent.com/freeCodeCamp/learn-nano-by-building-a-castle/main/tutorial.json)
*   [通过构建 SQL 引用对象学习 Git](https://raw.githubusercontent.com/freeCodeCamp/learn-git-by-building-an-sql-reference-object/main/tutorial.json)
*   [周期表数据库](https://raw.githubusercontent.com/freeCodeCamp/learn-periodic-table-database/main/tutorial.json)
*   通过建立一个灵感引语列表来学习 GitHub(即将推出)
*   [猜数字游戏](https://raw.githubusercontent.com/freeCodeCamp/learn-number-guessing-game/main/tutorial.json)

#### 这是我在 13 分 38 秒内完成“通过构建样板文件学习 Bash”的视频:

[https://www.youtube.com/embed/VQmCwzfSM-k?feature=oembed](https://www.youtube.com/embed/VQmCwzfSM-k?feature=oembed)

## 该课程将很快在您的浏览器中于[freeCodeCamp.org/learn](https://www.freecodecamp.org/learn)推出

在那里，你将能够获得关系数据库认证。

## 另外，下载 VS 代码的 freeCodeCamp 黑暗主题

如果你喜欢这些教程使用的配色方案，你可以从 Visual Studio Marketplace 下载 [freeCodeCamp Dark 主题扩展](https://marketplace.visualstudio.com/items?itemName=freeCodeCamp.freecodecamp-dark-vscode-theme)。

你可以在这里了解更多关于黑暗主题的信息。

## 通过提问和给我们反馈来帮助我们改进这些课程

如果您对这些新的关系数据库课程有任何问题，在某个时候遇到困难，或者只是对它们有一般性的反馈，您可以在 freeCodeCamp 论坛上创建一个帖子。

我们也有自己的 Slack-like 聊天室系统，在那里你可以提出问题，帮助我们的开源项目。加入我们的聊天室系统。

快乐编码。