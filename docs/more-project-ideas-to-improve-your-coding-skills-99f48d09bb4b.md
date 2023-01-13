# 更多的项目想法来提高您的编码技能

> 原文：<https://www.freecodecamp.org/news/more-project-ideas-to-improve-your-coding-skills-99f48d09bb4b/>

[https://www.youtube.com/embed/TNzCfgwIDCY?feature=oembed](https://www.youtube.com/embed/TNzCfgwIDCY?feature=oembed)

Video about the app-ideas repository

两周前，我发表了一篇文章，其中包含了 15 个项目想法，你可以通过这些想法来提升你的编码技能，人们对这一资源感到非常兴奋。

此外，自从我发表那篇文章以来， [app-ideas](https://github.com/florinpop17/app-ideas) 知识库已经获得了近 3000 颗星。这太疯狂了。？

为此感谢大家！？

在本帖中，我们将回顾一些从那时起添加到库中的**新**项目。

快速提醒一下，根据完成项目所需的知识和经验，所有项目被分为三个*层*。查看存储库中的*层*描述。

下面你会发现 2 个**初级**，4 个**中级**和 3 个**高级**项目创意。

### 1.计算器

**等级:**1-初学者

计算器不仅是可用的最有用的工具之一，而且也是理解应用程序中 UI 和事件处理的好方法。在这个问题中，您将创建一个支持整数基本算术计算的计算器。

造型取决于你，所以发挥你的想象力和创造力吧！您可能还会发现值得花时间在移动设备上试验计算器应用程序，以更好地理解基本功能和边缘情况。

#### 限制

*   您不能使用`eval()`功能来执行计算

#### 用户故事

*   用户可以看到一个显示屏，显示当前输入的数字或上次操作的结果。
*   用户可以看到一个输入板，其中包含数字 0-9、操作按钮—“+”、“-”、“/”和“=”、“C”按钮(用于清除)和“AC”按钮(用于全部清除)。
*   用户可以通过点击输入板中的数字来输入最长为 8 位数的数字序列。输入任何超过 8 位的数字都将被忽略。
*   用户可以点击一个操作按钮来显示该操作的结果:前一个操作的结果和最后输入的数字或最后输入的两个数字或最后输入的数字
*   用户可以点击“C”按钮清除最后一个数字或最后一次操作。如果用户的最后一次输入是一个操作，显示将被更新为之前的值。
*   用户可以点击“AC”按钮清除所有内部工作区，并将显示设置为 0。
*   如果任何操作超过最大 8 位数，用户可以看到显示“ERR”。

#### 额外功能

*   用户可以点击“+/-”按钮来改变当前显示的数字的符号。
*   用户可以看到小数点(。)输入板上的按钮，允许输入多达 3 位的浮点数，并对任何一个数字输入的最大小数位数执行操作。

#### 有用的链接和资源

*   [计算器(维基百科)](https://en.wikipedia.org/wiki/Calculator)
*   [MDN](https://developer.mozilla.org/en-US/)

#### 示例项目

[https://codepen.io/giana/embed/preview/GJMBEv?height=300&slug-hash=GJMBEv&default-tabs=css,result&host=https://codepen.io](https://codepen.io/giana/embed/preview/GJMBEv?height=300&slug-hash=GJMBEv&default-tabs=css,result&host=https://codepen.io)

[https://codepen.io/mjijackson/embed/preview/xOzyGX?height=300&slug-hash=xOzyGX&default-tabs=js,result&host=https://codepen.io](https://codepen.io/mjijackson/embed/preview/xOzyGX?height=300&slug-hash=xOzyGX&default-tabs=js,result&host=https://codepen.io)

### 2.食谱应用程序

**等级:**1-初学者

你可能没有意识到这一点，但食谱只不过是烹饪算法。就像程序一样，食谱是一系列必不可少的步骤，如果遵循正确，就会产生一道美味的菜肴。

菜谱应用程序的目标是帮助用户以一种易于遵循的方式管理菜谱。

#### 限制

*   对于该应用的初始版本，配方数据可能被编码为 JSON 文件。在实现了这个应用程序的初始版本后，您可以在此基础上进行扩展，将配方保存在文件或数据库中。

#### 用户故事

*   用户可以看到配方标题列表
*   用户可以单击食谱标题来显示食谱卡，其中包含食谱标题、膳食类型(早餐、午餐、晚餐或小吃)、服务人数、难度级别(初级、中级、高级)、配料列表(包括它们的数量)以及准备步骤。
*   用户点击新的配方标题，用新的配方替换当前卡片。

#### 额外功能

*   用户可以看到一张照片，显示项目准备后的样子。
*   用户可以通过在搜索框中输入膳食名称并点击“搜索”按钮来搜索不在食谱标题列表中的食谱。任何开源配方 API 都可以作为配方的来源(参见下面的 MealDB)。
*   用户可以看到与搜索词匹配的食谱列表
*   用户可以点击配方的名称来显示其配方卡。
*   如果没有找到匹配的配方，用户会看到一条警告消息。
*   用户可以点击通过 API 找到的配方卡上的“保存”按钮，将副本保存到该应用程序的配方文件或数据库中。

#### 有用的链接和资源

*   [使用 Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
*   [轴](https://www.npmjs.com/package/axios)
*   [meal db API](https://www.themealdb.com/api.php)

#### 示例项目

[https://codepen.io/eddyerburgh/embed/preview/xVeJvB?height=300&slug-hash=xVeJvB&default-tabs=js,result&host=https://codepen.io](https://codepen.io/eddyerburgh/embed/preview/xVeJvB?height=300&slug-hash=xVeJvB&default-tabs=js,result&host=https://codepen.io)

[https://codepen.io/inkblotty/embed/preview/oxWRme?height=300&slug-hash=oxWRme&default-tabs=js,result&host=https://codepen.io](https://codepen.io/inkblotty/embed/preview/oxWRme?height=300&slug-hash=oxWRme&default-tabs=js,result&host=https://codepen.io)

### 3.绘图应用程序

**层:**2-中间层

在网上的画布上创建数字作品，以便在线共享，也可以导出为图像。

#### 用户故事

*   用户可以使用鼠标绘制一个`canvas`
*   用户可以改变颜色
*   用户可以改变工具的大小
*   用户可以按下按钮清除`canvas`

#### 额外功能

*   用户可以将作品保存为图像(`.png`、`.jpg`等格式)
*   用户可以绘制不同的形状(`rectangle`、`circle`、`star`等)
*   用户可以在社交媒体上分享作品

#### 有用的链接和资源

*   [了解如何使用 p5js 创建绘图应用程序](https://www.florin-pop.com/blog/2019/04/drawing-app-built-with-p5js/)

#### 示例项目

[https://codepen.io/FlorinPop17/embed/preview/VNYyZQ?height=300&slug-hash=VNYyZQ&default-tabs=css,result&host=https://codepen.io](https://codepen.io/FlorinPop17/embed/preview/VNYyZQ?height=300&slug-hash=VNYyZQ&default-tabs=css,result&host=https://codepen.io)

[https://codepen.io/t0mm4rx/embed/preview/dLowvZ?height=300&slug-hash=dLowvZ&default-tabs=js,result&host=https://codepen.io](https://codepen.io/t0mm4rx/embed/preview/dLowvZ?height=300&slug-hash=dLowvZ&default-tabs=js,result&host=https://codepen.io)

### 4.表情符号翻译器

**层:**2-中间层

表情符号已经成为现代社会的通用语言。它们是一种有趣而快速的交流方式，同时也是一种极具表现力的交流情感和反应的机制。

Emoji Translator 应用程序的目标是将用户输入的文本翻译成等效的 Emoji 字符串，该字符串由原文中的一个或多个单词以及没有相应 emoji 的单词翻译而成。

#### 用户故事

*   用户可以在文本框中输入一串单词、数字和标点符号
*   用户可以点击“翻译”按钮，将输入文本中的单词翻译成相应的表情符号。
*   如果单击了“翻译”,但输入文本框为空或与上次翻译相比没有变化，用户会看到一条警告消息。
*   用户可以在输出文本框中看到输入文本中的文本元素被翻译成相应的表情符号。没有表情符号的文本元素将保持不变。
*   用户可以点击“清除”按钮来清除输入和输出文本框。

#### 额外功能

*   开发者将实现一个表情符号同义词功能，允许应用程序将更多种类的单词翻译成表情符号。
*   用户可以从语言下拉列表中选择输入文本的语言。

#### 有用的链接和资源

[完整表情列表 v12.0](https://unicode.org/emoji/charts/full-emoji-list.html)

#### 示例项目

[表情符号翻译](https://emojitranslate.com/)

### 5.热图生成器应用程序

**层:**2-中间

允许用户通过在图片上添加文字来生成自定义的迷因。

#### 用户故事

*   用户可以上传将出现在画布上的图像
*   用户可以在图像的顶部添加文本
*   用户可以在图像的底部添加文本
*   用户可以选择文本的颜色
*   用户可以选择文本的大小
*   用户可以保存生成的迷因

#### 额外功能

*   用户可以选择文本的字体系列
*   用户可以在社交媒体(twitter、reddit、facebook 等)上分享迷因
*   用户可以拖动文本，并把它放在任何他想放在图片上面的地方
*   用户可以在图像上绘制形状(圆形、矩形或用鼠标随意绘制)

#### 有用的链接和资源

通过 [p5js](http://p5js.org/) 库，使用 canvas 变得非常容易。

#### 示例项目

[img flip 的迷因生成器](https://imgflip.com/memegenerator)

[https://codepen.io/ninivert/embed/preview/BpLKRx?height=300&slug-hash=BpLKRx&default-tabs=js,result&host=https://codepen.io](https://codepen.io/ninivert/embed/preview/BpLKRx?height=300&slug-hash=BpLKRx&default-tabs=js,result&host=https://codepen.io)

### 6.打字练习

**层:**2-中间层

有些事情显而易见，很容易被忽视。作为开发人员，快速准确地输入是影响开发效率的一个因素。打字练习应用程序的目标是为您提供打字练习和指标，让您可以衡量自己的进步。

键入练习显示一个单词，然后您必须在特定的时间间隔内键入该单词。如果单词输入错误，它会留在屏幕上，时间间隔保持不变。但是当单词被正确键入时，会显示一个新单词，并且时间间隔会稍微缩短。

希望这种重复的练习能帮助你提高打字速度和准确度。

#### 用户故事

*   用户可以看到在应用程序窗口中显示的单词必须输入的时间间隔。
*   用户可以在分数框中看到成功尝试的次数和总尝试次数。
*   用户可以点击“开始练习”按钮开始练习。
*   用户可以看到文本框中显示的提示词。
*   用户可以开始在文本输入框中键入单词。
*   如果输入了不正确的字母，用户可以看到输入的字母闪烁，文本输入框将被清除。
*   用户可以看到文本输入框旁边的一条消息，指示如果输入了不正确的字母，用户应该重试。
*   用户可以在分数框中看到增加的总尝试次数。
*   如果单词输入正确，用户可以看到一条祝贺消息。
*   如果单词被正确键入，用户可以看到单词必须被键入的时间间隔被少量减少。
*   如果单词输入正确，用户可以在分数框中看到增加的成功尝试次数。
*   用户可以点击“停止练习”按钮来停止练习。

#### 额外功能

*   当显示新单词、正确输入单词或在单词中键入错误的字母时，用户可以听到独特的音频信号。
*   用户可以登录到应用程序
*   用户可以查看他/她所有练习课的累积成绩统计数据。

#### 有用的链接和资源

*   [按键](https://developer.mozilla.org/en-US/docs/Web/Events/keydown)
*   [设定间隔](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval)

#### 示例项目

[旋转打字导师](http://twiddler.tekgear.com/tutor/twiddler.html)

### 7.电梯

**等级:** 3 —高级

很难想象一个没有电梯的世界。尤其是如果你每天不得不上下 20 层楼梯的话。但是，如果你仔细想想，在 web 应用程序出现之前，电梯是事件和事件处理器的原始实现之一。

电梯应用程序的目标是模拟电梯的运行，更重要的是，如何处理建筑物居住者使用电梯时产生的事件。这个应用程序模拟居住者从任何楼层呼叫电梯，并按下电梯内的按钮来指示他们希望访问的楼层。

#### 限制

*   您必须为每个楼层的 up 和 down 按钮实现一个事件处理程序。例如，如果有 4 层，应该实现一个事件处理器，而不是 8 个(每层两个按钮)。
*   类似地，应该为电梯中控制面板上的所有按钮实现单个事件处理程序，而不是为每个按钮实现唯一的事件处理程序。

#### 用户故事

*   用户可以看到具有四层楼的建筑物的横截面图、电梯井、电梯、在第一层的向上按钮、在第二层和第三层的向上和向下按钮、以及在第四层的向下按钮。
*   用户可以在图的一侧看到电梯控制面板，上面有每个楼层的按钮。
*   用户可以在任何楼层点击上下按钮来呼叫电梯。
*   用户可以预期，点击任何楼层上的向上和向下按钮来请求电梯，将会按照他们被点击的顺序来排队和服务。
*   用户可以看到电梯在井道上下移动到它被呼叫的楼层。
*   用户可以点击电梯控制面板来选择它应该运行到的楼层。
*   用户预计电梯会暂停 5 秒钟，等待控制面板上的楼层按钮被点击。如果在这段时间内没有点击楼层按钮，电梯将处理下一个呼叫请求。
*   当没有请求要处理时，用户可以期望电梯返回到一楼。

#### 额外功能

*   如果电梯请求的数量超过允许的最大数量，用户可以看到警告通知。这个限制留给开发者。
*   当电梯到达某一楼层时，用户可以听到声音。
*   用户可以看到居住者随机到达某一楼层，以指示用户何时应该点击该楼层的上行或下行电梯呼叫按钮。
*   用户可以指定新乘客到达呼叫电梯的时间间隔。

#### 有用的链接和资源

[先进先出队列(维基百科)](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics))

#### 示例项目

[https://codepen.io/nibalAn/embed/preview/prWdjq?height=300&slug-hash=prWdjq&default-tabs=css,result&host=https://codepen.io](https://codepen.io/nibalAn/embed/preview/prWdjq?height=300&slug-hash=prWdjq&default-tabs=css,result&host=https://codepen.io)

### 8.快餐模拟器应用程序

**等级:** 3 —高级

快餐应用程序模拟了一个简单的外卖餐馆的运作，旨在帮助开发人员将他们的承诺和坚实的设计原则付诸实践。

这个应用程序模拟外卖餐馆的顾客下订单，等待他们准备好并送到取货柜台。下订单后，顾客在领取订单并前往用餐区之前等待订单通知。

构成该应用的用户故事围绕四个不同的角色展开:

*   用户—使用应用程序的最终用户
*   客户——模拟客户
*   接单员——模拟接单员
*   厨师——模拟厨师
*   服务器—模拟的服务器

这个应用有很多用户故事。但是不要被压倒。花时间不仅仅勾画出 UI，还要勾画出不同的参与者(角色)如何交互，以及如何遵循敏捷原则逐步构建应用程序。

#### 限制

*   订单可以表示为两种不同类型的承诺——一种是厨师准备订单时服务员等待的承诺，另一种是顾客排队等候的承诺。
*   无论您选择哪种语言进行开发，都要使用 JS Promises 的本机等价物。JS 开发人员应该使用本机承诺，而不是`async/await`。
*   使用本地语言功能创建此应用程序。您可能不使用模拟包或库。
*   新客户以固定的时间间隔到达订单行。换句话说，新客户以恒定的速度到来。
*   订单也以固定的时间间隔完成。它们以恒定的速度完成。

#### 用户故事

**应用操作**

*   用户可以看到一个输入区域，允许输入顾客到达的时间间隔和厨师完成*订单*的时间间隔。
*   如果客户到达间隔或订单履行间隔输入错误，用户可以看到定制的警告消息。
*   用户可以通过点击开始按钮开始模拟。
*   用户可以看到一个订单行区域，其中包含一个文本框，显示等待下订单的客户数量。
*   用户可以看到一个包含文本框的订单区域，显示当前正在接受的*订单编号*。
*   用户可以看到厨房区域包含一个显示正在准备的*订单号*的文本框和一个按顺序列出等待订单的文本框，以及等待订单的数量。
*   用户可以看到一个提货区，其中包含一个文本框，显示当前可供客户提货的*订单号*，以及一个文本框，显示在服务队列中等待的客户数量。
*   用户可以通过单击停止按钮随时停止模拟。

#### 额外功能

*   用户可以指定订单接受者创建一张*订单单*需要多长时间。
*   用户可以指定服务器将订单交付给客户需要多长时间。
*   一旦点击开始按钮，用户可以指定模拟运行的总时间。
*   用户可以在工作流程中移动时看到客户和订单的动画视图。

#### 有用的链接和资源

*   [快餐模拟器—逻辑工作流程](https://drive.google.com/file/d/1Thfm5cFDm1OjTg_0LsIt2j1uPL5fv-Dh/view?usp=sharing)
*   [敏捷宣言&敏捷软件的 12 条原则](http://agilemanifesto.org/)
*   [每个开发者都应该知道的坚实原则](https://blog.bitsrc.io/solid-principles-every-developer-should-know-b3bfa96bb688)
*   [利用承诺](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
*   [承诺](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

### 9.骗局

**等级:** 3 —高级

贝壳游戏是一种经典的赌博游戏，可以追溯到古希腊。玩它需要三个壳，一个豌豆，操作者的手灵巧，以及玩家敏锐的观察技巧。这也是一个经典的骗局，因为运营商很容易欺骗玩家。谢天谢地，后者不是我们这个应用程序的目的。

这个外壳游戏旨在为经典外壳游戏提供一个图形界面，并为玩家提供一个诚实的游戏。我们的游戏在画布上绘制三个贝壳和豌豆，将豌豆移到其中一个贝壳下，并在特定的时间间隔内洗牌。用户必须点击他们认为藏着豌豆的外壳。允许用户继续猜测，直到找到豌豆。

#### 用户故事

*   用户可以看到带有三个贝壳和豌豆的画布。
*   用户可以点击隐藏豌豆的外壳。
*   用户可以看到豌豆在被点击的外壳下移动。
*   用户可以点击“洗牌”按钮开始 5 秒钟的贝壳动画洗牌。
*   当动画停止时，用户可以点击她认为隐藏豌豆的外壳。
*   用户可以看到被点击的壳上升，显示豌豆是否隐藏在它下面。
*   用户可以继续点击外壳，直到找到豌豆。
*   当豌豆被找到时，用户可以看到一条祝贺消息
*   用户可以通过点击隐藏豌豆的壳来开始新游戏(上面的步骤 2)。然后重复上述步骤。

#### 额外功能

*   用户可以看到一个分数面板，其中包含获胜次数和游戏次数。
*   当豌豆被藏在壳下时，用户可以看到玩游戏的次数增加了
*   当第一次猜测发现 pea 时，用户可以看到增加的获胜次数。
*   用户可以看到壳隐藏豌豆动画(颜色，大小，或其他一些效果)时，点击(正确的猜测)。

#### 有用的链接和资源

*   [贝壳游戏(维基百科)](https://en.wikipedia.org/wiki/Shell_game)
*   [Javascript HTML DOM 动画](https://www.w3schools.com/js/js_htmldom_animate.asp)
*   [p5js 动画库](https://p5js.org/)

#### 示例项目

[https://codepen.io/RedCactus/embed/preview/dwEjXy?height=300&slug-hash=dwEjXy&default-tabs=js,result&host=https://codepen.io](https://codepen.io/RedCactus/embed/preview/dwEjXy?height=300&slug-hash=dwEjXy&default-tabs=js,result&host=https://codepen.io)

### 结论

如果你想找到更多的应用/项目想法，不要忘记查看[以前的文章](https://www.freecodecamp.org/news/here-are-some-app-ideas-you-can-build-to-level-up-your-coding-skills-39618291f672/)和[资源库](https://github.com/florinpop17/app-ideas)。

另外，如果这篇文章和回购协议中的信息对你有用，一定要给它打个星？；这样其他人也能发现它并从中受益！谢谢大家！

你对我们如何改进这个项目有什么建议吗？让我们知道！我们希望听到您的反馈！

欢迎大家贡献自己的想法！我们可以让这个资源库成为应用创意的首选资源。

*最初发表于[www.florin-pop.com](https://www.florin-pop.com/blog/2019/04/more-project-ideas-to-improve-your-coding-skills/)。*