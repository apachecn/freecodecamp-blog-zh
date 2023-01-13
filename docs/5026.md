# 这里有一些你可以用来提升编码技能的应用想法

> 原文：<https://www.freecodecamp.org/news/here-are-some-app-ideas-you-can-build-to-level-up-your-coding-skills-39618291f672/>

你是否曾经想做点什么，但却不知道该怎么做？就像作者有时会有“文思枯竭”一样，开发人员也是如此。

我和我的朋友吉姆一起创建了一个 T2 应用创意集，旨在一劳永逸地解决这个问题！

这些应用是:

*   对提高您的编码技能很有帮助
*   非常适合尝试新技术
*   加入你的投资组合，给你的下一任雇主/客户留下深刻印象
*   非常适合在教程(文章或视频)中用作示例
*   易于完成，也易于扩展新功能

这不仅仅是一个简单的项目列表，而是一个足够详细地描述每个项目的集合，以便您可以从头开始开发它！

每个项目规范包含以下内容:

1.  一个清晰的描述性目标
2.  应该实现的*用户故事*列表(这些故事更像是一个指南，而不是一个强制的*待办事项*列表)。如果您愿意，可以随意添加自己的功能)
3.  一系列的*奖励特性*不仅提高了基础项目，同时也提高了你的技能
4.  帮助您找到完成项目所需的所有资源和链接

[https://www.youtube.com/embed/TNzCfgwIDCY?feature=oembed](https://www.youtube.com/embed/TNzCfgwIDCY?feature=oembed)

Video about the app-ideas repository

### 项目

根据完成这些项目所需的知识和经验，所有的项目被分为三个层次。这些层是:

1.  **初学者** —处于学习旅程早期的开发人员。那些通常专注于创建面向用户的应用程序的人。
2.  **中级** —处于学习和经验中级阶段的开发人员。他们在 UI/UX 中得心应手，使用开发工具，构建使用 API 服务的应用程序。
3.  **高级** —开发人员具备以上所有条件，并且正在学习更高级的技术，如实现后端应用程序和数据库服务。

在下面你会发现每层有 **5 个项目**(**总共有**15 个)，但是在[这个 GitHub 库](https://github.com/florinpop17/app-ideas)中有超过 **30 个项目**(目前)。请务必检查它，因为我们计划在未来添加更多的项目。欢迎您的帮助！(在下面的*贡献*部分有更多关于这个的内容？)

### 1.便签应用

**等级:**1-初学者

**描述**:创建并存储您的笔记以备后用！

#### 用户故事

*   用户可以创建注释
*   用户可以编辑注释
*   用户可以删除注释
*   当关闭浏览器窗口时，笔记将被存储，当用户返回时，数据将被检索

#### 额外功能

*   用户可以创建和编辑降价格式的注释。保存时，它会将 Markdown 转换为 HTML
*   用户可以看到他创建便笺的日期

#### 有用的链接和资源

*   [本地存储](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
*   [降价指南](https://www.markdownguide.org/basic-syntax/)
*   [标记—降价解析器](https://github.com/markedjs/marked)

#### 示例项目

[https://codepen.io/nickmoreton/embed/preview/gbyygq?height=300&slug-hash=gbyygq&default-tabs=css,result&host=https://codepen.io](https://codepen.io/nickmoreton/embed/preview/gbyygq?height=300&slug-hash=gbyygq&default-tabs=css,result&host=https://codepen.io)

### 2.圣诞灯

**等级:**1-初学者

**描述**:圣诞灯应用程序依靠你的开发才能来创建一个迷人的灯光显示。你的任务是连续画七个彩色圆圈，并根据计时器改变每个圆圈的亮度。当一个圆变亮时，它的前身恢复正常亮度。

这模拟了一串涟漪灯光的效果，类似于圣诞节期间显示的灯光。

#### 用户故事

*   用户可以按下按钮来开始和停止显示
*   用户可以改变控制强度变化的时间间隔

#### 额外功能

*   用户可以选择用于填充每个圆圈的颜色
*   用户可以指定强度值
*   用户可以改变行中任何圆的大小
*   用户可以指定显示的行数。可以选择一到七行

#### 有用的链接和资源

*   [样本图像](https://previews.123rf.com/images/whiterabbit/whiterabbit1003/whiterabbit100300020/6582600-seven-color-balls-red-orange-yellow-green-cyan-blue-and-magenta-in-a-row-on-a-white-background.jpg)
*   [Adafruit LED 矩阵](https://cdn-shop.adafruit.com/970x728/1487-02.jpg)

#### 示例项目

[https://codepen.io/tobyj/embed/preview/QjvEex?height=300&slug-hash=QjvEex&default-tabs=css,result&host=https://codepen.io](https://codepen.io/tobyj/embed/preview/QjvEex?height=300&slug-hash=QjvEex&default-tabs=css,result&host=https://codepen.io)

### 3.飞行图像

**等级:**1-初学者

描述:对于 web 开发人员来说，理解操作图像的基础知识是很重要的，因为富 Web 应用程序依赖图像来增加用户界面和用户体验(UI/UX)的价值。

FlipImage 探索了图像处理的一个方面——图像旋转。这个应用程序显示一个正方形面板，其中包含一个 2x2 矩阵形式的图像。使用与每个图像相邻的一组上、下、左和右箭头，用户可以垂直或水平翻转它们。

您必须只使用原生 HTML、CSS 和 Javascript 来实现此应用程序。不允许使用映像包和库。

#### 用户故事

*   用户可以看到包含在 2x2 矩阵中重复的单个图像的窗格
*   用户可以使用图像旁边的一组上、下、左、右箭头垂直或水平翻转任何一个图像

#### 额外功能

*   用户可以通过在输入字段中输入不同图像的 URL 来更改默认图像
*   用户可以通过点击输入字段旁边的“显示”按钮来显示新图像
*   如果找不到新图像的 URL，用户会看到一条错误消息

#### 有用的链接和资源

*   [如何翻转图像](https://www.w3schools.com/howto/howto_css_flip_image.asp)
*   [创建 CSS 翻转动画](https://davidwalsh.name/css-flip)

#### 示例项目

[https://codepen.io/seyedi/embed/preview/gvqYQv?height=300&slug-hash=gvqYQv&default-tabs=html,result&host=https://codepen.io](https://codepen.io/seyedi/embed/preview/gvqYQv?height=300&slug-hash=gvqYQv&default-tabs=html,result&host=https://codepen.io)

### 4.测验应用程序

**等级:**1-初学者

**描述**:通过回答测验应用程序中的问题来练习和测试您的知识。

作为开发人员，您可以创建一个测验应用程序来测试其他开发人员的编码技能。(HTML、CSS、JavaScript、Python、PHP 等…)

#### 用户故事

*   用户可以通过按下`button`开始测验
*   用户可以看到一个有 4 个可能答案的问题
*   选择答案后，向用户显示下一个问题。这样做，直到测验结束
*   最后，用户可以看到以下统计数据:

1.  完成测验所用的时间
2.  他得到了多少正确答案
3.  显示他是否`passed`或`failed`参加测验的信息

#### 额外功能

*   用户可以在社交媒体上分享测验结果
*   向应用程序添加多个测验。用户可以选择哪一个
*   用户可以创建一个帐户，并在他的仪表板上保存所有的分数。用户可以多次完成测验

#### 有用的链接和资源

*   [打开琐事数据库](https://opentdb.com/api_config.php)

#### 示例项目

[https://codepen.io/FlorinPop17/embed/preview/qqYNgW?height=300&slug-hash=qqYNgW&default-tabs=css,result&host=https://codepen.io](https://codepen.io/FlorinPop17/embed/preview/qqYNgW?height=300&slug-hash=qqYNgW&default-tabs=css,result&host=https://codepen.io)

[用 React](http://tranquil-beyond-43849.herokuapp.com/) 构建的测验应用(等待它加载，因为它托管在 Heroku 上)

### 5.罗马数字到十进制数字转换器

**等级:**1-初学者

由罗马数字代表的数字系统起源于古罗马，并在中世纪晚期一直是整个欧洲书写数字的常用方式。今天使用的罗马数字使用七个符号，每个符号有一个固定的整数值。

见下表*符号-值*对:

*   I-1
*   第五卷
*   x-10
*   l-50
*   c-100
*   d-500
*   M — 1000

#### 用户故事

*   用户应该能够在输入字段中输入一个罗马数字
*   用户可以在单个输出字段中看到结果，该字段包含通过按键输入的罗马数字的十进制(以 10 为基数)等效值
*   如果输入了错误的符号，用户应该会看到一个错误

#### 额外功能

*   用户可以看到我输入时自动进行的转换
*   用户应该能够从十进制转换为罗马(反之亦然)

#### 有用的链接和资源

*   [罗马数字的解释](https://en.wikipedia.org/wiki/Roman_numerals)

#### 示例项目

[罗马数字转换器](https://www.calculatorsoup.com/calculators/conversions/roman-numeral-converter.php)

### 6.图书查找应用程序

**层:**2-中间

**描述**:创建一个应用程序，允许用户通过输入查询(标题、作者等)来搜索书籍。在页面上的列表中显示生成的书籍以及所有相应的数据。

#### 用户故事

*   用户可以在`input`字段中输入搜索查询
*   用户可以提交查询。这将调用一个 API，该 API 将返回包含相应数据的书籍数组(**书名**、**作者**、**出版日期**、**图片**等)
*   用户可以看到页面上出现的图书列表

#### 额外功能

*   为列表中的每一项添加一个链接，将用户带到一个外部网站，那里有关于这本书的更多信息
*   实施响应式设计
*   添加加载动画

#### 有用的链接和资源

你可以使用[谷歌图书 API](https://developers.google.com/books/docs/overview)

#### 示例项目

[https://codepen.io/chasebank/embed/preview/wpQBKV?height=300&slug-hash=wpQBKV&default-tabs=css,result&host=https://codepen.io](https://codepen.io/chasebank/embed/preview/wpQBKV?height=300&slug-hash=wpQBKV&default-tabs=css,result&host=https://codepen.io)

[BookSearch-React](https://fethica.github.io/BookSearch-React/)

### 7.卡片记忆游戏

**层:**2-中间层

**描述**:卡片记忆是一种游戏，你必须点击一张卡片，看看它下面是什么图像，并试图找到其他卡片下面的匹配图像。

#### 用户故事

*   用户可以看到一个由 n x n 张卡片组成的网格(`n`是一个整数)。所有的牌最初都面朝下(`hidden`状态)
*   用户可以点击按钮开始游戏。点击此按钮时，计时器将启动
*   用户可以点击任何一张卡片来揭开它下面的图像(将其改变为`visible`状态)。该图像将一直显示，直到用户点击第二张卡

当用户点击第二张卡时:

*   如果有匹配，这 2 张卡将从游戏中删除(隐藏/移除它们，或者让它们处于`visible`状态)
*   如果没有匹配，这 2 张牌将翻回到它们的原始状态(`hidden`状态)
*   当所有的匹配都被找到后，用户可以看到一个显示祝贺信息的对话框，其中有一个计数器显示完成游戏所用的时间

#### 额外功能

*   用户可以在多个难度级别(简单、中等、困难)之间进行选择。增加难度意味着:减少完成时间和/或增加卡片数量
*   用户可以看到游戏统计数据。他赢/输的次数，每个级别的最佳时间)

#### 有用的链接和资源

*   [维基百科](https://en.wikipedia.org/wiki/Concentration_(game))

#### 示例项目

[翻牌记忆游戏](https://codepen.io/zerospree/full/bNWbvW)

[SMB3 记忆卡游戏](https://codepen.io/hexagoncircle/full/OXBJxV)

### 8.降价表生成器

**层:**2-中间层

**描述**:创建一个应用程序，将用户提供的数据从一个常规表格(可选)转换成一个 Markdown 格式的表格。

#### 用户故事

*   用户可以用给定数量的**行**和**列**创建一个`HTML table`
*   用户可以在`HTML table`的每个单元格中插入文本
*   用户可以生成一个包含来自`HTML table`的数据的`Markdown formatted table`
*   用户可以预览`Markdown formatted table`

#### 额外功能

*   用户可以通过按下按钮将`Markdown formatted table`复制到剪贴板
*   用户可以在指定位置插入新的**行**或**列**
*   用户可以完全删除一个**行**或一个**列**
*   用户可以将一个**单元格**、**列**、**行**或整个**表格**对齐(到*左*、*右*或*中心*

#### 有用的链接和资源

*   [降价指南](https://www.markdownguide.org/)
*   [标记—降价解析器](https://github.com/markedjs/marked)
*   [如何复制到剪贴板](https://www.w3schools.com/howto/howto_js_copy_clipboard.asp)

#### 示例项目

[表格生成器/降价表格](https://www.tablesgenerator.com/markdown_tables)

### 9.弦乐艺术

**层:**2-中间

**描述**:String Art 的目的是为开发者提供创建简单动画图形的练习，在动画算法中使用几何图形，并创建视觉上令人愉快的东西。

字符串艺术画一个单一的多色线，平滑移动，直到一端接触到封闭窗口的一面。在这一点上，它触摸了一个“反弹”效果来改变它的方向。

当线条移动时，只保留 10-20 个图像就产生了涟漪效应。旧图像逐渐褪色，直到消失。

不允许使用动画库。只使用普通的 HTML/CSS/JavaScript。

#### 用户故事

*   首先在封闭窗口边界内的任意位置画一条多色线
*   每 20 毫秒根据轨迹在新位置绘制一个新的线条副本，即根据端点与前一条线条的增量距离
*   当线的任一端点触及封闭窗口的边界时，改变其方向并随机改变其角度
*   逐渐淡化旧线条的强度，以便只有最近的 10-20 条线条可见，从而产生运动感或“波纹”

#### 额外功能

*   用户可以指定线的长度及其速度
*   用户可以在窗口内指定多条线，所有线都沿着不同的轨迹和速度移动

#### 有用的链接和资源

*   [使用多步动画&过渡](https://css-tricks.com/using-multi-step-animations-transitions/)
*   [动画基础知识](https://www.khanacademy.org/computing/computer-programming/programming/animation-basics/a/what-are-animations)

#### 示例项目

这个项目非常接近，但有一个小的封闭窗口，是单色的。丹尼尔·科尔特斯

### 10.待办事项应用

**层:**2-中间层

**描述**:经典的待办应用程序，用户可以写下所有他想完成的事情。

#### 用户故事

*   用户可以看到一个`input`字段，他可以在其中输入待办事项
*   通过按回车键(或按钮)，用户可以提交待办事项，并可以看到它被添加到待办事项列表中
*   用户可以将待办事项标记为`completed`
*   用户可以通过按下按钮(或待办事项本身)来删除待办事项

#### 额外功能

*   用户可以编辑待办事项
*   用户可以看到所有已完成的待办事项列表
*   用户可以看到所有活动待办事项的列表
*   用户可以看到他创建待办事项的日期
*   当关闭浏览器窗口时，待办事项将被存储，当用户返回时，数据将被检索

#### 有用的链接和资源

*   [本地存储](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

#### 示例项目

[https://codepen.io/yesilfasulye/embed/preview/eJIuF?height=300&slug-hash=eJIuF&default-tabs=css,result&host=https://codepen.io](https://codepen.io/yesilfasulye/embed/preview/eJIuF?height=300&slug-hash=eJIuF&default-tabs=css,result&host=https://codepen.io)

[用 React 构建的 Todo 应用](http://todomvc.com/examples/react/#/)

### 11.战舰游戏引擎

**等级:** 3 —高级

**描述**:战舰游戏引擎(BGE)将经典的回合制棋盘游戏实现为一个独立于任何表示层的包。这是一种在许多应用程序中都很有用的架构模式，因为它允许任意数量的应用程序使用相同的服务。

BGE 本身是通过一系列函数调用来调用的，而不是通过直接耦合的最终用户动作。在这方面，使用 BGE 类似于使用 API 或由 web 服务器公开的一系列路由。

这项挑战要求您开发 BGE 和一个非常薄的、基于文本的表示层，用于与引擎本身分离的测试。由于这个原因，下面的用户场景被分为两组——一组用于 BGE，另一组用于基于文本的表示层。

BGE 负责维护游戏状态。

#### 用户故事

#### 丁基缩水甘油醚

*   调用者可以调用`startGame()`函数开始单人游戏。该函数将生成一个 8×8 的游戏棋盘，由 3 艘船组成，宽度为一个正方形，长度为:

1.  毁灭者:2 个方块
2.  巡洋舰:3 个方格
3.  战舰:4 格

将随机地将这些船放置在棋盘上的任何方向，并将返回一个代表船位置的数组。

*   调用者可以调用一个`shoot()`函数，传递游戏板上目标单元格的目标行和列坐标。`shoot()`将返回指示符，表示射击是命中还是未命中、剩余的船只数量(即尚未沉没)、船只放置数组以及更新的命中和未命中数组。

命中和未命中数组中的单元将包含一个空格，如果它们还没有被作为目标，`O`如果它们被作为目标但是没有船的一部分在那个位置，或者`X`如果单元被船的一部分占据。

#### 基于文本的表示层

*   用户可以看到由`startGame()`函数返回的命中和未命中数组显示为游戏板的二维字符表示。
*   可以提示用户输入游戏板上目标方块的坐标。
*   用户可以看到一个更新的击中和未击中数组显示后，采取了一杆。
*   用户可以在每次射击后看到一条消息，指示该射击是命中还是未命中。
*   玩家可以在击沉最后一艘船后看到一条祝贺信息。
*   用户可以被提示在每场比赛结束时再玩一次。拒绝再次参与游戏将停止游戏。

#### 额外功能

#### 丁基缩水甘油醚

*   调用者可以指定游戏板的行数和列数，作为`startGame()`函数的参数。
*   调用者可以调用一个`gameStats()`函数，该函数返回一个包含当前游戏指标的 Javascript 对象。比如玩的回合数，当前的命中和未命中数等。
*   当调用`startGame()`时，调用者可以指定玩家的数量(1 或 2 ),这将为随机填充有船只的每个玩家生成一个棋盘。

`shoot()`将接受正在拍摄的玩家编号以及拍摄坐标。它返回的数据将是那个玩家的。

#### 基于文本的表示层

*   用户可以通过输入短语`stats`来代替目标坐标，在任意点看到当前的游戏静态。(注意，这需要 BGE 中的`gameStats()`功能)
*   用户可以指定要玩的双人游戏，每个玩家在同一个终端会话中轮流玩(注意，这需要 BGE 中相应的奖励功能)
*   用户可以在每轮输入的提示中看到玩家的号码。
*   玩家可以在每回合结束时看到两个玩家的棋盘。

#### 有用的链接和资源

*   [战舰游戏(维基百科)](https://en.wikipedia.org/wiki/Battleship_(game))
*   [战舰游戏规则(孩之宝)](https://www.hasbro.com/common/instruct/battleship.pdf)

#### 示例项目

这段 YouTube 视频展示了一款基于文本的[战舰游戏](https://www.youtube.com/watch?v=TKksu3JXTTM)的玩法。

如果你对战舰游戏不熟悉的话，下面的例子是一个战舰游戏的演示。请记住，您要实现一个基于文本的表示层来进行测试。克里斯·布罗迪的战舰游戏

### 12.聊天应用

**等级:** 3 —高级

**描述**:实时聊天界面，多个用户可以通过发送消息进行互动。

作为一个 MVP(最小可行产品),你可以专注于建立聊天界面。实时功能可以在以后添加(额外的功能)。

#### 用户故事

*   当用户访问聊天应用程序时，系统会提示用户输入用户名。用户名将存储在应用程序中
*   用户可以看到一个`input field`,他可以在这里输入新消息
*   按下`enter`键或点击`send`按钮，文本将显示在用户名旁边的`chat box`中(如`John Doe: Hello World!`

#### 额外功能

*   聊天应用程序中的所有用户都可以看到这些消息(使用 WebSockets)
*   当新用户加入聊天时，会向所有现有用户显示一条消息
*   消息保存在数据库中
*   用户可以发送图像，视频和链接，将正确显示
*   用户可以选择并发送表情符号
*   用户可以私下聊天
*   用户可以加入特定主题的`channels`

#### 有用的链接和资源

*   [Socket.io](https://socket.io/)
*   [如何在 10 分钟内搭建一个 React.js 聊天 app 文章](https://medium.freecodecamp.org/how-to-build-a-react-js-chat-app-in-10-minutes-c9233794642b)

[https://www.youtube.com/embed/tHbCkikFfDE?feature=oembed](https://www.youtube.com/embed/tHbCkikFfDE?feature=oembed)

#### 示例项目

[https://codepen.io/iremlopsum/embed/preview/ZWEdZj?height=300&slug-hash=ZWEdZj&default-tabs=css,result&host=https://codepen.io](https://codepen.io/iremlopsum/embed/preview/ZWEdZj?height=300&slug-hash=ZWEdZj&default-tabs=css,result&host=https://codepen.io)

[聊天 2](https://web-chatty.herokuapp.com/)

### 13.GitHub 时间轴

**等级:** 3 —高级

描述:API 和信息的图形化表示是现代网络应用的标志。GitHub Timeline 将两者结合起来，创建用户 GitHub 活动的可视化历史。

GitHub Timeline 的目标是接受一个 GitHub 用户名，并生成一个包含每个回购协议的 Timeline，并用回购协议名称、创建日期及其描述进行注释。时间表应该是一个可以与未来雇主分享的时间表。它应该易于阅读，并有效地利用颜色和排版。

应该只显示公共 GitHub repos。

#### 用户故事

*   用户可以输入 GitHub 用户名
*   用户可以单击“生成”按钮来创建和显示指定用户的回购时间表
*   如果 GitHub 用户名不是有效的 GitHub 用户名，用户会看到一条警告消息。

#### 额外功能

*   用户可以看到按创建年份统计的回购数量摘要

#### 有用的链接和资源

GitHub 提供了两个 API 来访问回购数据。您也可以选择使用 NPM 包来访问 GitHub API。

GitHub API 的文档可以在以下位置找到:

*   [GitHub REST API V3](https://developer.github.com/v3/)
*   [GitHub GraphQL API V4](https://developer.github.com/v4/)

展示如何使用 GitHub API 的示例代码如下:

您可以使用这个 CURL 命令查看 V3 REST API 为您的 repos 返回的 JSON:

```
curl -u "user-id" https://api.github.com/users/user-id/repos
```

#### 示例项目

[https://codepen.io/NilsWe/embed/preview/FemfK?height=300&slug-hash=FemfK&default-tabs=css,result&host=https://codepen.io](https://codepen.io/NilsWe/embed/preview/FemfK?height=300&slug-hash=FemfK&default-tabs=css,result&host=https://codepen.io)

[https://codepen.io/tutsplus/embed/preview/QNeJgR?height=300&slug-hash=QNeJgR&default-tabs=css,result&host=https://codepen.io](https://codepen.io/tutsplus/embed/preview/QNeJgR?height=300&slug-hash=QNeJgR&default-tabs=css,result&host=https://codepen.io)

### 14.拼一下

**等级:** 3 —高级

了解如何拼写是流利使用任何语言的基础。无论你是一个正在学习拼写的年轻人还是一个正在学习一门新语言的人，练习都有助于巩固你的语言技能。

Spell-It 应用程序通过播放一个单词的录音来帮助用户练习拼写，然后用户必须使用电脑键盘拼写这个单词。

#### 用户故事

*   用户可以点击“播放”按钮来听将要输入的单词
*   当在键盘上输入字母时，用户可以看到文字输入文本框中显示的字母
*   用户可以点击“输入”按钮，提交在单词输入文本框中输入的单词
*   当键入正确的单词时，用户可以看到一条确认消息
*   当单词拼写错误时，用户可以看到一条消息，要求重新输入单词
*   用户可以看到正确拼写的数量，尝试的单词总数，以及成功输入的百分比。

#### 额外功能

*   当单词拼写正确时，用户可以听到确认声音
*   当单词拼写错误时，用户可以听到警告声
*   用户可以单击“提示”按钮来突出显示单词输入文本框中的错误字母
*   用户可以按键盘上的“Enter”键提交键入的单词，或者在应用程序窗口中单击“Enter”按钮

#### 有用的链接和资源

*   [德州仪器说话和拼写](https://en.wikipedia.org/wiki/Speak_%26_Spell_(toy))
*   [网络音频 API](https://codepen.io/2kool2/full/RgKeyp)
*   [点击说话](https://codepen.io/shangle/full/Wvqqzq)

#### 示例项目

[iOS 版 Word 向导](https://itunes.apple.com/app/id447312716)

[在 Google Play 上念 N 个咒语](https://play.google.com/store/apps/details?id=au.id.weston.scott.SpeakAndSpell&hl=en_US)

### 15.调查应用程序

**等级:** 3 —高级

**描述**:调查是任何开发者工具箱中有价值的一部分。它们有助于从用户那里获得各种主题的反馈，包括应用程序满意度、需求、即将到来的需求、问题、优先级，以及简单的问题等等。

调查应用程序为您提供了一个学习的机会，您可以开发一个功能全面的应用程序，将它添加到您的工具箱中。它提供了定义调查的能力，允许用户在预定义的时间范围内做出响应，并将结果制成表格并呈现出来。

该应用程序的用户分为两个不同的角色，每个角色都有不同的要求:

*   *调查协调员*定义并开展调查。这是普通用户无法使用的管理功能。
*   *调查受访者*完成调查并查看结果。他们在应用程序中没有管理权限。

商业调查工具包括向调查对象群发电子邮件调查的分发功能。为简单起见，该应用程序假设将从应用程序的网页访问开放的调查。

#### 用户故事

#### 一般

*   调查协调员和调查受访者可以从一个公共网站定义、进行和查看调查和调查结果
*   调查协调员可以登录应用程序访问管理功能，如定义调查。

#### 定义调查

*   调查协调员可以定义包含 1-10 个选择题的调查。
*   调查协调员可以为每个问题定义 1-5 个互斥选项。
*   调查协调员可以输入调查的标题。
*   调查协调员可以单击“取消”按钮返回主页，而不保存调查。
*   调查协调员可以点击“保存”按钮来保存调查。

#### 进行调查

*   调查协调员可以通过从先前定义的调查列表中选择一个调查来打开调查
*   调查协调员可以通过从打开的调查列表中选择调查来关闭该调查
*   调查回答者可以通过从打开的调查列表中选择调查来完成调查
*   调查回答者可以通过点击复选框来选择对调查问题的回答
*   调查受访者可以看到，如果单击不同的回答，先前选择的回答将自动取消选中。
*   调查受访者可以单击“取消”按钮返回主页，而无需提交调查。
*   调查受访者可以点击“提交”按钮提交他们对调查的答复。
*   如果单击“提交”,调查受访者会看到一条错误消息，但并非所有问题都已得到回复。

#### 查看调查结果

*   调查协调员和调查受访者可以从已关闭的调查列表中选择要显示的调查
*   调查协调员和调查受访者可以以表格形式查看调查结果，该表格显示了每个问题选项的回复数量。

#### 额外功能

*   调查受访者可以在应用程序中创建一个独特的帐户
*   调查受访者可以登录该应用程序
*   调查受访者不能多次完成同一项调查
*   调查协调员和调查受访者可以查看调查结果的图形表示(如饼图、条形图、柱形图等)。图表)

#### 有用的链接和资源

用于建筑测量的库: [SurveyJS](https://surveyjs.io/Overview/Library/)

一些商业调查服务包括:[调查猴](https://www.surveymonkey.com/)和[字体](https://www.typeform.com/)

#### 示例项目

[https://codepen.io/amyfu/embed/preview/oLChg?height=300&slug-hash=oLChg&default-tabs=js,result&host=https://codepen.io](https://codepen.io/amyfu/embed/preview/oLChg?height=300&slug-hash=oLChg&default-tabs=js,result&host=https://codepen.io)

### *投稿*

欢迎您对 [GitHub 资源库](https://github.com/florinpop17/app-ideas)中的项目做出贡献！任何贡献都非常感谢。

您可以通过两种方式做出贡献:

1.  制造一个问题，告诉我们你的想法。确保在这种情况下使用**新想法**标签；
2.  叉项目，并提交一份公关。在此之前，请确保您阅读并遵循了投稿指南(您可以在资源库中找到它)；

#### 添加您自己的示例

您也可以在完成后将自己的示例添加到项目中。我强烈鼓励你这样做，因为这将向其他人展示你创造了多么神奇的东西！？

### 把话传出去！

如果这篇文章和回购协议中的信息对你有用，一定要给它打个星？，这样别人也能发现，也能受益！我们可以一起成长，让我们的社区变得更好！

你对我们如何改进这个项目有什么建议吗？让我们知道！我们希望听到您的反馈！

#### 主要贡献者？？

**Florin Pop** : [推特](https://twitter.com/florinpop1705)和[网站](https://florin-pop.com)。

**吉姆·梅德洛克** : [推特](https://twitter.com/jd_medlock)和[媒体](https://medium.com/@jdmedlock)

### **每周编码挑战？**

作为奖励，每周有一次编码挑战，你可以通过在真实项目中练习你的技能来学到更多。阅读[完整指南](https://www.florin-pop.com/blog/2019/03/weekly-coding-challenge/)了解如何参与！？

*最初发表于[www.florin-pop.com](https://www.florin-pop.com/blog/2019/03/15-plus-app-ideas-to-build-to-level-up-your-coding-skills/)。*