# React 和 Material UI 课程–编写字典

> 原文：<https://www.freecodecamp.org/news/code-a-dictionary-with-react-and-material-ui/>

React 仍然是一个流行的前端 JavaScript 库。你可以通过建立一个字典项目来提高你的技能。

我们刚刚在 freeCodeCamp.org YouTube 频道上发布了一门课程，将教你如何用 React 和 Material UI 创建一个字典应用程序。

你将学习创建一个支持超过 12 种语言的字典。该应用程序还允许用户在明暗主题之间切换。

路边编码器开发了这个课程。他在他的 YouTube 频道上创建了许多受欢迎的课程。

以下是本课程涵盖的主题:

*   项目概述
*   初始化新的 React 应用
*   谷歌字典 API
*   材料用户界面介绍
*   安装材料用户界面
*   构建应用程序
*   部署到网络
*   什么是 PWA？
*   将字典转换为 PWA
*   测试最终 PWA

观看 freeCodeCamp.org YouTube 频道的全部课程(2 小时观看)。

[https://www.youtube.com/embed/ToXna81iij0?feature=oembed](https://www.youtube.com/embed/ToXna81iij0?feature=oembed)

## 副本

(自动生成)

通过学习构建一个字典应用程序来提高你的反应技能。

在这个来自路边编码器的课程中，字典使用了 material UI 并支持 12 种语言。

嘿，各位，欢迎回到自由代码营。

而今天我们要用 react js 和 material UI 搭建一个超赞的字典 app。

这个视频有两个部分。

在第一个项目中，我们将在 netlify 上构建和部署这个应用程序。

在第二部分中，我们将把它转换成 pw a 或 progressive web 应用程序，以便您可以将此应用程序作为原生 Android 或 iOS 应用程序安装到您的手机中。

所以我们先来探索一下这个 app。

所以这本字典支持超过 12 种语言。

让我们快速搜索一个词。

让我们搜索平原。

现在你可以看到它提供了这个特定单词的定义、例子和同义词。

而且不仅仅是一个定义，它提供了那个特定单词的所有定义。

更棒的是这个发音音频，你可以播放这架飞机，它会告诉你那个特定单词的发音。

所以，是的，这太棒了。

我想只有英语才支持发音。

所以让我们来看看印度语。

我要搜索我的名字，这是 bu ish。

给你，你知道这个名字的含义。

所以这是一个神奇的应用。

现在，这个应用程序中更好的是你在这个小角落看到的 Tara，亮模式和暗模式，他有这个应用程序的惊人功能，你可以通过使用材质 UI 和沼泽来实现它。

这就是为什么我选择了 material UI，因为在 material UI 中实现这种黑暗模式和光明模式非常容易。

此外，如果你对 react js 的更多教程感兴趣，一定要看看我的频道，它被称为路边编码器。

最近，我在我的频道上传了一个完全成熟的 myrn stack 项目教程，链接将在下面的描述中。

所以事不宜迟，让我们开始吧。

我们要做的是快速打开一个新项目，让我选择我的文件夹。

我将在这里选择这个特定的文件夹。

我是说，让我尽快把这些文件关了。

是的，所以我们需要去我们的终端，键入和 PX 创建反应应用程序。

我们要把这个命名为，我们要把这个应用命名为单词，hunt，哎呀，我犯了一个错误。

让我把这个 NPM 修正为 NP x，然后回车。

现在它要做的是，它将转到 NPM 存储库，它将获取创建我们的 create react 应用程序所需的所有文件，我将把它们放到这个 word hunt 文件夹中。

与此同时，这是初始化。

让我向你们展示一下我们将在这个应用中使用的 API。

所以我们要用的 API 叫做谷歌字典 API。

这是一个官方的谷歌字典 API，由 me 开发者创建。

非常感谢这个 API 的开发者。

所以这个 API 为我们提供了对这些语言的支持。

我们将如何使用这个 API，我们将为这个 API 提供一个语言代码和一个特定的单词。

所以它会以这种形式获取数据。

所以我们要，我们要看看我们以后如何处理这些数据。

所以你可以看到我们有一个语言支持，或者这里有很多语言，英语和西班牙语，法语。

我喜欢这里的语言。

这是一个令人惊叹的 API。

所以，让我们快速探索一下材质 UI。

那么什么是 material 呢？为什么 material UI 是一个 react 组件，用于更快更容易的 web 开发。

这是一个 react 组件库。

现在什么是 react 组件库。

因此，react 组件库为您提供了一个预构建的组件，在这个特定的库中没有预构建的组件，您可以在自己的项目中使用它。

让我通过点击这里的“开始”向您展示。

我要关闭这个已经打开的项目。

所以，是的，我们可以通过使用 NPM，安装材料，用户界面斜线核心来安装它。

所以我们也要安装这个。

让我看看我们的应用程序是否已安装。

不，还在安装中。

我们就等着吧。

所以也给我们提供了素材 UI 图标。

所以这个，我们以后再说吧。

我将向您展示如何使用这个库。

现在。

让我们转到组件，然后转到按钮。

好的，你可以看到这里有不同类型的按钮。

所以你这些是在材质 UI 库中预建的按钮，你只需要输入一行就可以直接导入它们。

当你输入这一行时，这个按钮会很重要。

如果您将颜色更改为原色，此按钮将被导入。

所以，是的，这是一个了不起的图书馆。

你可能会想可能还有其他的库。

是的，有。

有一个名为 react bootstrap 的库。

我一直在使用这个图书馆。

但是我决定使用这个材质 UI 库，因为它也很容易处理明暗特征。

您还将在 react 中探索一些新的东西。

所以，bootstrap 确实是图书馆的主流类型，但在材料方面，它在市场上发挥了作用。

所以我们正在实现一个非常好的图书馆。

那么我们将从这个素材库中使用什么，我们将使用一个叫做文本域的东西作为文本，通过那里的文本域进行限制。

实际上，我们将在这里使用这个组件。

它要做的是，为我们提供一个预建的输入组件，我们可以在那里搜索我们的特定单词，我们将使用它，我们将使用这个选择去哪里，是的，在这里，在这里，我们也将使用这个选择组件。

好了，让我们检查一下我们的应用程序是否已经成功初始化。

是的，它已经成功初始化了，让我通过键入 cd，word hunt 切换到这里。

或者，您可以转到“文件”,打开“文件夹”,然后转到您的特定文件夹，在我的情况下，就是这个文件夹，然后选择这个文件夹。

在这里，它已经在一个单独的窗口中打开了那个特定的文件夹。

让我们开始吧。

我们将首先安装材料用户界面。

好的，让我们看这里，那条线在哪里？让我们转到安装，将这一行复制到这里，复制并粘贴到这里。

所以安装 material UI 需要一点时间，因为 material wire 是一个巨大的库。

所以这需要一点时间。

所以一旦它安装好了，我会回来的，永远以后。

好了，终于，我们的素材 UI 库安装成功了。

因此，我们现在要用 react 应用程序做的是，删除我们现在不需要的无用文件。

让我们删除这些文件，这四个文件。

我们根本不需要这些文件。

但首先，让我第一次运行这个应用程序，我将向你们展示它第一次运行时的样子。

这就是我们成功启动的 react 应用。

所以我要做的，就是把它停靠在这里。

我将删除这些文件，正如我之前提到的，所以我们只删除这四个文件。

让我们转到，你可以看到，这个应用程序现在会抱怨，因为它在某个地方使用了这些文件。

所以让我们检查一下。

这里使用了 logo，我们不需要它，我们只需要删除这个文件中的所有信息。

我们要去索引 j，s，我们要删除它。

我们要移除这个。

差不多就是这样。

上我们的 app 吧。

你好，世界。

你好，世界。

没错。

给你。

你好，世界，但它在中央启动。

因为我认为我们还没有移除这些瓷砖。

让我们移除应用程序 j s 中的所有图块，我们现在不打算触及索引 CSS 或索引 j s。

因为我们现在或任何地方、任何时间都不需要，因为我们不需要对他们做任何事情。

所以，是的，HelloWorld 已经被成功地印在这里了。

好吧，那又怎样？我们首先要做什么？所以，让我们快速导入我们的应用程序中的 API。

让我们去我们的 API 网站。

那是字典吗？API？让我检查一下。

是的，在那里。

革命性的 API，我们需要这个特殊的链接。

但是我们如何获取这个链接，我们将使用一个叫做 axios 的包。

所以我们需要安装这个包。

因此，我将在这里打开另一个终端。

我们要去 NPM I axios。

我们开始吧。

axios 已成功安装。

我们现在要做什么呢我们要创建一个函数。

我们称这个函数为字典 API。

稍等一下。

是的，我们要把这个函数叫做字典 API。

我们将在这里创建一个箭头函数。

此外，由于我们正在获取 API，我们需要使这个函数异步。

我们需要使它成为一个异步函数。

我们将在这里键入 try catch，以捕捉我们的应用程序中是否出现任何错误。

所以我要把这个错误记录在这里。

在 try、block 中，我们要做的是，首先，让我关闭这里的终端。

是的，我们又回到了 terminal 中，那么我们在这个 try 块中要做什么，我们要获取那个 API。

所以 const，让我们输入数据等于 await。

所以我们在这个异步函数中需要这个 await 关键字，否则，它会给我们一个错误。

所以 axios 没有得到。

所以让我们从网站上复制链接。

这是链接。

让我们把这个链接复制到这里。

好的，你看，你可以看到这里有一些默认的语言代码，一个英语单词，对不起，英语关键词，等等。

所以，逐字逐句，我会给它，让我们简单明了。

好的，我们还需要在某个地方调用这个函数。

所以我们要调用这个函数，我们要用一个叫做使用效果的东西。

使用效果。

那么什么是使用效果呢？让我来澄清一下。

Use it use effect 是，当你在这里保持这些括号为空时，use effect 在你的组件第一次被渲染时被调用。

但是如果你把任何东西放在这些括号里，或者那些叫做依赖关系的东西，那么将要做的事情，每次变量改变的时候都会被调用。

一路上你会知道的，所以我要在这个 use effect 里面调用这个字典 API。

让我们看看会发生什么。

useeffect 没有定义，很明显，因为我们没有导入你是事实。

是的。

我要进口。

使用效果。

哎呀，刚刚发生了什么事？使用来自 react 的效果。

没错。

我什么都得不到。

不，是的。

数据是价值的标志，但永远不会使用。

显然，我们已经调用了这个数据变量，但是我们没有使用它。

我们把这个记录下来。

让我们看看在这个 API 中我们得到了什么。

我将在这里记录这些，然后转到我们的浏览器和应用程序。

你可以看到我们的应用程序目前是空的。

我们就印字典吧。

是的，就这些。

是的，字典，那个词。

让我们快速检查一下控制台。

我们的控制台里有什么？所以我们得到了我们得到的这些数据，我们得到了所谓的假数据头，等等，这将需要这个数据数组在这里。

在这些数组中，每个数组中，plain 这个词有不同的含义。

所以我们去里面吧。

让我们进入一个项目。

在一个项目中，我们可以看到有两个部分。

首先是意义，另一个是语音。

所以在语音学里面，我们得到了这个联系。

是的，这是这个单词的发音。

所以我们稍后会用到它。

首先，我们需要在数组项中使用这个含义项。

那么让我们来看意义项，我们可以看到另一个数组对象。

所以在这些物体的内部，我们有一个定义。

我们有例子。

我们有这个词的同义词。

这就是这个 API 的工作方式。

所以就用它吧。

首先，我们需要的是什么，我们将进入其中并获取这些数据，以便使用这些数据并将其存储在其他地方，我们将在 react 中创建一个名为 state 的东西，我们将键入 use state，哎呦，use est，enter，您可以看到它给了我们这段代码。

我们将命名这些含义。

因为这是一个数组，所以我们给它一个空数组。

这部分包含了这个特殊状态的初始状态。

这将是我们用来改变状态的函数。

这将是一个实际变量。

现在让我展示一下它会抱怨，因为我们还没有导入美国州。

所以让我们从 react 导入使用状态。

好了，我们要把所有的数据导入到这个集合的含义中。

所以让我们用一秒钟来设定，设定意义。

在既定的意义之内。

我们要输入数据。

呜呜，数据点数据。

没错。

让我们记录下这个意思。

我要在这里记录下这些含义。

是啊。

让我们检查一下我们的浏览器。

让我们刷新这个页面一次。

您可以看到，我们只获得 API 表的数据部分，因此我们需要这些含义。

总之。

所以让我们看看接下来要做什么。

首先，我们需要做的是创造另一个国家。

我们的承诺将会被默认为空。

让我们从创建应用程序的标题部分开始。

首先，我们将转到材质 UI，同样，我将向您介绍材质 UI 中的容器。

因此，这个容器在使你的应用程序响应方面帮助很大。

每当你的应用程序的屏幕尺寸改变时，它都会适应特定的尺寸。

让我很快展示给你们看。

在这个应用程序 Dev 中，我要写的是包含这个容器标签，你可以看到 VS 代码已经自动导入了这个容器。

如果您点击“CTR space ”,它会向您显示建议，您可以单击特定会话来自动导入这些建议。

我要给这个容器一个中等的最大宽度 MD，你可以在这个材料 UI 的文档中通过中等小的最大宽度，你可以看到他们给了你一个小尺寸，接受，接受 Trump。

现在我们来看看我们的应用程序会发生什么，我们只需输入 dictionary。

让我们看看会发生什么。

好吧，让我关闭这个终端。

你可以看到你的字典正对着正中心。

你可以看到当我调整这个窗口的大小时，你可以看到它在适应那个特定的，容器在帮助我们的屏幕适应屏幕大小。

这就是容器如何帮助我们的。

让我也给这几个样式。

我将为它提供一些高度为 100 的内联样式，视口。

和背景颜色。

它所在位置的背景颜色是的，在这里。

我给它哈希 282 c 三，四。

让我们看看那是什么样子。

是的，看起来不错。

但是我们的字典被藏起来了。

所以我也要给它一个颜色。

所以颜色，会是白色的。

是的，差不多就是这样。

看起来很漂亮。

好的，所以我们也要给我们的容器一些样式。

你们可能都知道 Flexbox，Flexbox 在 CSS 中的作用，我们要给它一种显示风格，flex，这样所有的项目都从上到下对齐，我们要给它 flex 方向，列。

开始了。

我们将赋予它 100 的高度和对齐内容。

好的，我现在不打算给出它，我稍后会告诉你们为什么它对我们有用。

首先，让我在这个标题部分工作，这里是我们的标题，我们的搜索和选择框将要放置的地方。

好了，接下来我要做的是转到 SRC，创建一个名为 components 的新文件夹。

在这个组件中，我将创建一个名为 header 的新文件或新文件夹。

现在在 zardoz 标题文件夹中，当你创建一个名为 header dot j s 的新文件，和另一个 CSS 文件。

让我们来看看 header dot j s，我会给你看一个快捷键，r A，F，C E，如果你输入这个然后按 enter，它会给你一个基本的 react 函数的样板代码。

让我们快速创建这个头部组件。

首先，我要输入 header。

或者我在这里用一个标题。

我要给它一个 span 标签。

给它一个类名称的标题。

我要去好吧。

一言为定亨特。

是的，看起来不错。

让我们给它一个类名。

不顶球。

是的，看起来不错。

让我们将它导入我们的应用程序 your app.js。

让我把这本字典扔掉。

我要输入 header。

现在你可以看到它给我们汽车进口的建议。

所以我们要点击它。

您可以看到标题已经自动导入到这里。

所以我们要做一个自我关闭标签。

让我们看看它有没有渲染。

开始了。

字数已渲染。

桑德拉把这个单词打了 100 个。

很快的。

我要去头头点 CSS。

但是首先，我们需要将这个 CSS 文件导入到这个头中。

我们要怎么做呢？我们需要输入导入点斜杠点意味着当前目录斜杠头点 CSS。

哎呦。

嗯，看起来不错。

现在我们要提供这个标题，一些样式，只是提供这个标题，一点点样式。

假设字体大小为 7 个视口宽度。

视区宽度是您当前在电话或计算机上的屏幕宽度。

是那种特别的宽度。

我要把文本转换成大写。

所以这是资本，我要给它一个字体系列。

首先，我们不要给它字体系列。

现在让我们检查一下。

好吧，看起来不错，但是有点丑。

所以我们想让它漂亮一点。

所以我要做的是我要去谷歌，谷歌。

抱歉，谷歌，我要输入蒙塞拉特字体，如果我拼写没错的话，是的，蒙塞拉特。

所以我们需要这种蒙特塞拉特字体。

我要去那边。

10 100.

选择此样式，然后转到导入。

我要把这个字体放在这里。

我要复制这个。

我将返回我们的文本编辑器，转到 index . CSS。

就在最上面加上吧。

我要粘贴这种字体样式。

现在让我们看看会发生什么。

如果我们输入字体系列。

我们需要纠正拼写 m o n d e s e r r a t。

没错。

正确。

而无衬线呢？让我们去检查一下。

听着，这不管用。

为什么呢？希望我没有在这里写错拼写。

也许是我干的。是的，我做到了。

好吧，现在让我们看看。

是啊，看起来真不错。

所以给大家一个建议，如果你想让世界变得更大一点，标题变得更大一点，就减少字体的粗细。

所以看起来会很漂亮。

这是一些设计，你知道，暂时的提示。

所以是的，看起来不错。

那么我们需要什么，除了我们的标题里面的这个标题，我们需要文本组件，你们都知道。

因此，我将在这里创建另一个 div。

好的，我将给出这个 div 的类名作为输入。

没错。

此外，我认为我们还需要设计一下我们的标题。

我要为我们的页眉引入一些风格。

我在这里要做的是，我在这里要做的是，提供一个 flex 的显示，将项目居中对齐，并平均对齐空间内容。

那么接下来要做的是，让我向你们展示所有的幻灯片，让我说一句你好。

如果你回到这里，你可以看到它从上到下对齐。

发生这种情况是因为这个伸缩方向列，我们已经将显示设置为伸缩伸缩方向列，并且我们正在设置显示均匀调整内容空间。

所以它们之间会有均匀的间距。

并且将具有 35 个视窗高度的高度。

如果我把这个高度增加到 50，你可以看到高度已经增加了。

所以我们想要的是 35 的视口高度，你可以看到这些组件之间有一个空间。

所以，这将是 100%。

所以这些都是款式。

我不打算深入研究 CSS，因为它是一个 react 项目，而不是 CSS 项目。

所以是的，请原谅我。

好，那么在输入里面，我们现在需要什么？我们需要一个文本字段。

让我们快速浏览一下我们的浏览器。

你真的知道为什么吗？让我们转到文本字段文本字段。

哎呀，开始了。

我们需要这个标准文本字段。

所以我要把它复制到这里，然后转到我们的文本编辑器。

我要把这个贴在这里。

您可以看到它会给我们带来错误，因为文本字段我们还有一个重要的文本字段，所以我们将我转到这里，按 ctrl+space，它将向我显示建议，我将单击这里，您可以看到文本字段已被导入。

所以是的，VS 代码在编码的时候帮助很大。

好的，它是暗的，现在你可以看到，因为背景也是暗的，组件也是暗的。

所以没成功。

我们在这里需要做什么？我们将在这里导入一个黑暗主题。

我们在哪里找到这个黑暗的主题？我们将转到材质用户界面，我们将搜索黑暗模式。

开始了。

我要把这个复制下来。

这个还是另一个？让我查一下。

不，不是这个。

我们要复制。

是啊，我想我要复制那个。

是的，我们要对这个做一点小小的改动。

所以这里是黑暗主题，它会抱怨，因为我们还没有导入创建 em UI 主题。

让我们点击导入那个。

所以字体是黑色的。

所以我要做的就是，我要复制这个名称，然后进入它的内部，导入一个叫做主题提供者的东西。

所以主题提供者允许我们，你知道，将主题应用于材料 UI 组件。

我们要去这里，主题。

因此，我通过键入 CTL space 自动导入了它，您可以看到它已被自动导入。

我要去这里，类型主题等于黑暗主题。

现在让我们快速检查一下我们的组件。

有改善吗？是的，它已经。

但是，你可以看到当我点击这里时，它变得有点深蓝色。

所以我们不想那样。

那我们在这里要做什么？在调色板内部，我们将为它提供一种原色。

主要的主要的，这将是如何散列，F F F。

此外，这里需要逗号。

我觉得这个应该可以。

开始了。

当我们点击它时，它会以白色激活。

所以，是的，我们在这里引入了我们的黑暗主题。

除了这个我们还需要什么？我们需要一个精选的组件。

让我们找到我们选择的组件。

让我回到文本字段。

没错。

让我向下滚动到这里。

这是我们需要的。

好吧。

好吧。

哎呦。

是的，我要点击这里。

你可以看到它为我们提供了大量的代码。

那么我们需要什么？我们需要第一个，对吗？是的，我们需要第一个。

我将把这个文本字段从这里复制到这里。

让我们复制它。

粘贴到这里。

它会抱怨，因为它错过了很多东西。

首先，这个文本字段是非常重要的。

我们不需要这个菜单项，我将进入 CTL space 并按回车键。

所以我的新项目很重要。

但是你可以看到这些货币并处理零钱。

这些都是他们的数据，他们没有展示文件，你可以看到货币处理改变货币。

那么我们需要这个选择做什么呢？首先，我们需要为我们的国家选择这些。

让我把这个拿掉。

把这个拿走。

这张地图也起作用。

是的，我们待会要用 desc。

是的，在菜单项里面，我要输入，比如说英语。

我也将删除这些键和值。

看看到底行不行。

确实如此。

是的，你可以看到，但它只有一个组成部分。

所以我们需要做什么，我们需要导入所有这些语言。

所以我已经为你创建了一个文件。

在这里，让我转到文本编辑器，创建另一个名为 data 的文件夹。

我将创建一个名为 category 的新文件。

类别点 j s。

是的，类别点 j 在这个类别里面。

我要去提供这个特殊的数据。

因此，如果您想要这个特定的数据，您需要做什么，我将在描述中提供到我的 GitHub repo 的链接，您将转到我的 GitHub repo，您将转到这个特定的 GitHub repo，您将转到 SRC，转到 category.js 的数据，您将复制这个文件，我将把它复制到您的代码中，所以我们只需做它是否提供值， 而且标签就在那个 API 的文档里，你可以自己创建，也可以从我的 GitHub 复制。

所以我们创建了这个 category.js。

还是省省吧。

在我们的 header.js 里面，我要把它导入。

从这里导入类别，我要后退一步。

再后退一步，在数据内部，你需要分类，我们开始。

所以我们需要这个类别变量。

所以我们要，我们要在这个菜单项里面映射。

我现在要做的是，这个和类别路线图，我们要浏览一下，我要在其中提供这个菜单项。

我们给它起个名字，变量名。

将其命名为 option。

在这里，我们将输入选项，点值，因为我们希望值在我们的选择框中。

看看到底行不行。

是的，确实如此。

但是我们也需要为它提供一个键和值。

所以我们到这边来。

在这个键里面，我要输入选项点值。

抱歉，选项点标签。

并且该值将是选项点标签。

让我们尽快解决这个问题。

让我们来看看。

还是老样子？我们现在需要做什么？我们还需要在这里提供一个值。

我们之前已经为此创建了一个状态，我想我们还没有创建状态。

所以让我们创建一个叫做类别的状态。

所以对不起，哎呀，我在做什么？美国州？类别？是的，看起来不错。

我将提供英语的默认值。

没错。

我们需要将这两种东西都带到那个 header 组件中。

我们要怎么做呢？我们将在这里转到标题组件。

对此，我们要把这个作为一个道具，这个类别和这个集合类别一样。

是啊。

所以我们将进入我们的头，我们将在这里接收这两个变量。

但是通过输入花括号，回车和 Diggory 来进行析构。

是的，这两个都在这里。

让我把它重新命名为类别。

参加一场围棋比赛。

所以我们不会混淆这个和这个。

所以分类，是的，很好。

好吧。

所以价值就是价值就是类别。

当它在链上改变时，它将在那个东西上设置特定的类别。

因此，在更改时，我们将有一个事件集类别，e 点目标，点值。

让我们保存这个，看看这个。

希望应该先显示语言。

是的，它在这里显示英语。

好吧，让我们把事情做好。

它的显示器显示标准目前，我们还需要命名一个。

所以我们不需要这个身份证。

我们将输入搜索词来代替标签。

我们将为它命名为 search。

哎呀，在错误的地方做这个。

它应该在这里。

这两样东西都应该在这里。

有一个精选的。

所以我们要做的不是选择，而是给它贴上语言的标签。

我们不需要这个帮助文本。

我们把它拿掉。

好吧。

好的，所以在这个里面，我们需要把我们创建的单词状态。

让我给你看看。

它在哪里？是的，单词和集合单词。

我要把这两个都拿走，寄给海德。

所以词等于词。

集合词等于集合词。

这里不会有任何逗号。

是的，我们已经发送了这个，让我们在这里接收它们。

Word 和 set word。

是的，我要给这个单词赋值。

关于变化，变化会发生什么？它会改变那个固定的词。

所以他设置了 word，e 点目标，点值，让我们看看这个东西的作用。

我在这里要做的是，我要检查一下，在 Word 中有什么吗？我要这样用花括号打出来？Word 问号里有什么吗？如果 Word 中有任何内容，我们会翻译出来。

否则，我们要做的，就是渲染，文字搜索。

它会实时更新。

在那里，检查一下。

让我们在这里打字。

你看，你可以看到，它的工作非常好。

每当我们改变它。

它调用这个 on change 函数，来改变这个状态，并把这个特定的值提供给这个特定的盒子。

改变这个，如果里面什么都没有，即使有也不会改变。

是的，很好。

好吧，我看到一个问题。

这是什么？这个为什么压缩？这么少，因为它没有办法到这里来。

这件事也不会结束吗？这里有一点。

所以让我们看看我要做什么。

我要给它一个搜索的类名，我要给它一个类名。

让我们给它一个类名 select。

是的，那很好。

现在我要去头点 CSS，首先，我需要提供输入一点点的样式。

此外，我们需要确定这里有一件事，我要让这两个也做出响应。

因为当你走到这里，当你把它变小，你可以看到亨特变得有点太小了。

所以我们希望这个看起来比那个好一点。

所以我们要让它有反应。

因此，我们将通过使用媒体查询来提高响应速度。

所以添加媒体，哎呀，媒体，我们是类型最大宽度。

如果最大宽度小于 900 像素，会发生什么？它要做的是，点页眉将使页眉均匀地对齐内容空间。

身高是 25 岁，我们要降低一点身高。

第二件事，我们要做的是在标题中，我们要增加这个单词搜索的字体大小。

比如说 11 号的 VH 字体。

哎呀，这里发生了什么？我想我需要让它更高一点。

让我查一下。

不，没关系。

让我们看看。

如果我到这里为输入提供样式，我将首先为输入提供样式。

我将为它提供 100%的宽度。

我将提供 flex 和 justify 内容的显示。

我将在这些空间之间提供一个空间，所以你可以看到它在这些空间之间。

让我们检查一下，如果我没有错的话，我想我需要用 100%的宽度来制作它，但是我已经提供了 100%的重量，那么这里有什么问题呢？这是正确的，也很好。

在中间，你好，对不起，间隔均匀。

我不认为我需要在这里再次提供它，所以我要删除它。

一切看起来都很好。

以后再想办法吧。

现在我要做的是，这是我给它的问题。

h，我需要给它 v w。

是的，这就是问题所在，因为这与屏幕的高度有关。

好吧，那是个问题。

所以是的，看起来不错。

现在。

你可以看到，它不会收缩太多。

有反应的时候。

所以是的，我们正在讨论这个，让我们在这里给它一点重量。

首先，我要在这里设计这个输入框的样式。

所以搜索，搜索的宽度是 43%。

select 也是如此。

所以我要做的是逗号，点选。

这两个班级都有 43%的学生。

是的，看起来很漂亮，你可以看到看起来真的很好。

如果你提供任何东西，它会改变这个状态。

好了，我们完成了头球。

让我们再次访问我们的应用程序 JS，看看我们的标题中是否还有任何内容。

我不认为我们在标题里有任何东西留下。

好的，我想我要做的另一件事是，无论何时我们输入，我们都要搜索一个文本，比方说，如果我搜索 plain，如果我改变类别，那么它应该做的是，它应该清除这个，以及含义部分。

意义状态也是如此。

所以我要做的是，在这里创建另一个函数，const，处理变化。

所以我们要在这里调用这个函数，这个文本字段，我要在这里调用这个函数。

在这个里面，因为它发送给我们这个 e 点目标点值，我们将在这里接收它，这个值有它的语言值。

我们将提供这种语言来设置类别。

语言应该还是一样的，设定的世界应该是空的。

现在。

让我们试试这个。

我能改变我们的类别吗？是的，是空的。

如果我输入了什么，就改变它。

是的，是空的。

起作用了。

太好了。

好的，我们需要做的另一件事是，我们忽略了如何改变我们的 API？当我们改变这个框，或者这个特定的选择框。所以我们要在这里用反勾号，去掉这些普通的字符串标签。

我们将使用反勾号，转到语言部分。

我们要输入美元符号和这些花括号，我要输入类别。

Antara 代替这个词。

我们要做同样的事情，输入单词。

太好了。

让我们检查一下我们的 API 是否对此做出了响应。

哎呀，刚刚发生了什么事？是啊。

我们去控制台吧。

是的，它，它显示一个错误，因为里面什么也没有。

我们就打 Hello 吧。

又来了，看到了吗？没错。

它仍然显示什么好吧，因为我想我没有话说。

我没担心过。

很好，看起来很好。

让我们再次重新加载这个页面。

并键入 Hello。

好吧。

好吧，React hooks 有一个缺失的依赖项，当然，因为我们每次改变我们的世界或类别都需要调用这个 API。

所以我们要给它提供单词和类别。

这就是将要发生的事。

这就是上一个错误的地方。

每次换这个词的时候，都要打个招呼。

又要调用 API 了。

你可以看到它在调用 API。

你可以看到它为我们提供了意义和我们需要使用的一切。

是的，太好了。

这就是我们所需要的。

所以让我们开始努力吧。

我们要把这个控制台拿走，放在这里。

我们将在派克山下解决这个问题。

所以我要做的是，为此创建另一个组件。

我将转到这里，我们将创建一个名为“假设定义”的新文件夹，而不是我们的组件。

我要把它变成首都。

这很重要。

不是文件夹，而是您创建的文件。

应该是在首都护卫 GS 里，CSS 的另一个文件。

我想我已经在标题中创建了它。

所以我要把它从标题中去掉。

是啊，现在没事了。

所以我们的 definition.js，让我们键入一个 f c，然后得到样板代码。

我们就打 Hello 吧。

我要导入它。

首先，让我也为它创建一个 CSS 文件。

定义点 CSS 并将 CSS 文件导入到这里。

导入点斜线定义。

点 CSS？哎呦。

是啊。

让我们在应用程序 j s .中导入定义。

在这个标题下面。

我要输入定义。

是的。

展品来自汽车进口。

是的，它已经。

大家去我们的 app 看看吧。

是的，你好，这里打印成功了。

太好了。

那么我们需要在定义里面创建什么呢，我们需要创建一个框，就像我之前给你们展示的那样，在里面，我们要为这个特定的，你知道的，单词和类别，呈现我们所有的含义。

那么让我们看看我们需要发送到定义中的所有东西是什么？所以我认为我们需要发送这个单词和类别，不，哈，是的，我们需要发送这个单词和含义，因为那里不需要类别。

因为品类在改变 API。

这就是它所做的一切。

所以我们要发送消息，首先，发送消息。

我们会把意思状态。

好吧。

我们还要设置类别。

让我们看看我们是否需要它。

类别。

让我们关闭无用的文件夹。

是的，让我们回到定义，在这里接受所有的词。

词，类别。

和意义。

好吧，我们省省吧。

同样，我会给出这个定义，只有当这个定义里面有东西的时候。

你知道，如果州内什么都没有，那我们就不需要在这里看到任何东西。

所以我要做的是，当 a 用花括号括起来，当他们说意思的时候，只有意思里面有东西，然后定义就会被呈现出来。

让我们深入定义。

让我们看看。

首先，我们还需要一个音频组件，但我会把音频组件留到后面。

所以我们就从打字开始吧。

如果单词里面有任何东西，那么如果单词里面没有任何东西，首先，它要呈现的，是一个 span 标签。

一个 span 标签，比如 sub，title 的类名。

标题。

它会说先在搜索中输入一个词。

是啊。

如果单词里面有什么东西要呈现呢？把这个渲染成别的东西？我们说点什么吧。

这里有东西。

哎呀。

我就要这么做。

是啊。

让我们看看。

是的，从输入一个词开始。

所以让我稍微设计一下。

我将为这个 div 提供一个类名，比如说含义，或者说含义。

我们也在 CSS 中进行了导入。

这两者都有一种风格，所以我们先完成它。

我要去输入意思。

我们要做什么首先，我们需要一个边界，就像我之前展示的那样，但首先我们要做的是键入 Display flex，太棒了，我们需要把它做成 flex box。

我们需要一切从上到下都保持一致。

从上到下。

我们要做的是弯曲方向有柱。

是的，我们要做的是，当有很多物品在这里展示时，我们要做溢出。

为什么是卷轴？还是流量？为什么是卷轴？你知道我在想什么，我们以后就应用这种风格吧。

所以，等等，有点不对劲。

号码

30，要看的东西，首先我要导入一些东西，而不是这个写在这里的东西。

所以，好吧，首先，让我来演示一下它是否有效。

你能看到的任何东西都打印出来了，如果没什么，它会说从搜索一个词开始。

让我们来设计这个字幕。

我要去设计这个字幕了。

暂时如此。

因此，标题，我要给它一个类似的字体大小，我们之前提供的，五，视口宽度，字体家族将是相同的。

蒙塞拉特，让我抄一遍，这样我这次就不会犯任何拼写错误了。

没错。

嗯，看起来不错。

对我来说，字号五 VH。

我在想什么？不，是的，那很好。

不管怎样，所以我们要做的，是渲染这个意思，而不是这个东西。

所以我们到这边来。

我要输入含义。

我要穿过这里。

和地图。

我要给它一个词的意思。

就这么做。

我们把它记录下来吧。

让我们看看里面有什么。

我是说，有些不对劲。

对，分号。

让我们看看，我们得到了什么？如果我输入任何简单的东西？嗯，你可以看到我们这里有很多东西，我们有一系列单词和语音，好的，这不是最后的单词，这是最后的单词。

我们得到了意义和语音，所以我们需要快速地理解这些意义。

至于去，我现在要把这个拿掉。

好吧，所以我要做的就是离开这里，你的意思，点的意思。

点阵图。

我将把这个变量命名为 item。

让我们看看里面有什么。

让我们把它记录下来。

项目。

现在，让我们快速记录一下。

首先，我需要从这里删除这个日志，这样就不会造成混乱。

让我关闭这个终端。

是啊，腾出点空间。

让我们看看我们得到了什么。

我来刷新一下。

我要打飞机。

好了，我们可以在这里看到很多东西，我们可以看到我们的定义。

所以现在我们需要渲染，你知道，通过这个定义映射。

所以我们得到了每一个定义，其他的必须是同义词和例子。

好吧，这里没有。

就在这里的某个地方。

定义。

这是怎么回事？让我检查一下。

它一定在这里面的某个地方。

它就在这里面的某个地方。

词类词类定义。

你可以看到例子。

这个有一个额外的东西，它是同义词。

我们可以选择用同义词来举这个例子，因为你可以看到它在某处出现，也可以不出现。

所以我们要把它变成可选的。

所以让我们回到我们的代码。

让我们把这个从里面取出来。

我想我拿走了别的东西。

做错事。

号码

我应该把这个拿掉。

是的，所以在这里面。

我们拿到了我们的东西。

对于我们的项目，让我把它倒过来。

对于我们的项目，我们要做什么，我们需要定义。

我们将映射到定义。

我知道这里有很多映射。

这不是绘制地狱地图。

所以我要做的肯定是项目点定义点地图，我们给它 D，f 处的变量名。

好了，现在我们需要创建一个 div。

哎呦，IB。

是的，这是我们的 div，我会给它一个单一含义的类名，或者单一含义，不管你想叫它什么，我会给它一个样式。

一点点造型。

我要给它一个背景色。

比方说，单个组件的背景颜色是白色。

我们还需要什么，我会给它，因为它很宽，我会让内容为黑色，彩色，黑色。

让我们看看我是否在里面输入了什么。

让我们先打印我们的定义。

所以我要用粗体打印这个定义。

所以 def 点定义。

让我们检查一下。

好了，我们开始吧。

这些是我们从 API 中得到的关于单词 plane 的不同定义。

如果我输入 ball，我们开始。

我们对球有了不同的理解。

所以我们不仅仅需要定义，我们还需要例子。

所以我要在这两者之间画一条线。

我要给它一个背景颜色的样式。

黑色和宽度，而不是 100%的宽度。

比方说。

我希望这会造成一点点分离。

是的，确实如此。

意思是看到这个溢出也在这里。

一会儿我要把它藏起来。

所以我们得到了这个 HR 号。

好吧。

现在这些可选部分定义点示例。

如果有一个例子，那么我们将在哪里打印它，我们将创建一个 span 标签。

首先，我们需要键入 example，而不是 span 标签。

所以我们要举一个大胆的例子。

好的，我们要打印 d f 点的例子。

我们去看看。

好吧，这不合适。

它有一些可疑的含义。

是啊。

你可以看到例子定义。

定义示例。

是啊。

给你。

那么我们还需要什么？我们需要同义词。

就在例子的正下方。

我要输入 def 点同义词。

如果同义词存在，同义词 s 也会存在。

如果不存在，我们要做什么，我们要做和这里一样的事情，我们要复制这个。

我要把它粘贴到这里。

我只是要改变同义词。

和 d f，点同义词。

同义词，因为它们有很多同义词，我要做的是我可以通过同义词做一个地图。

南

我要用反勾号。

我要做的是这个。

我要打印 s 和逗号。

让我们看看那是什么样子。

你可以看到同义词的例子，但看起来有点丑，你知道它在同一行。

所以我们需要做的是，我们需要做的是，我们将在所有这些不同的行中有这个。

首先，让我们先设计一下外部容器的样式。

我将首先设计这个含义容器的样式，对吗？当我讲到定义的时候，让我们从这里继续。

我要输入这个难看的滚动条，我需要把它变小一点。

所以我要输入 scroll，我想，是的，滚动条宽度，我给它 10。

现在你可以看到滚动条看起来好多了。

你可以看到侧滚动条变细了。

我要给它 55 的视口高度和边框，得到 10 像素 10 像素的纯色边框。我在这里有一种颜色，我们要复制它，你可以复制这种颜色，你可以通过点击这里并选择一种颜色来试验这些颜色。

我选择了这种颜色，它看起来很好。

我将它的边框半径设为 10 像素。

让我们看看它看起来有多远。

好吧，这不是很好，我想我需要给它一些填料。

我要给它一个衬垫。

在顶部和底部，我给它 10 个像素，在左侧和右侧，我给它 20 个像素。

并且溢出 x 会被隐藏，我们不希望底栏溢出 x 会被隐藏。

是的，我们只是需要这个酒吧。

此外，我们需要在这两者之间建立一些隔离。

现在我要提供这种风格。

但让我看看它现在是否有反应。

所以如果你能看见，我们把它变小。

这之间有一点点差距。

我是说，低于这个。

当它变小的时候，我们会把它做得高一点。

所以我要做一个媒体查询。

最大宽度为 900 像素。

让我们理解他的意思。

是的，意思，我给它的高度 60，视口高度。

是的，看起来好多了。

和溢出。

看，为什么我在这里留了一点空间，因为我们将在顶部有一个组件，它将负责切换我们的主题。

所以我就把空间留给它了。

溢出，x 将被隐藏，溢出将被滚动。

是啊。

好多了。

所以在这个单一意义的部分，让我回到这里。

在这个单一意义的组件中，我们也需要对这个 div 进行样式化，你知道，每个 dev 也需要对它进行样式化。

我要去那边。

我要输入 display flex。

当我输入这个的时候，你可以看到它只是一条水平线。

看起来真的很丑。

所以我要把挠曲方向输入为列。

是的，好多了。

我给它的边框半径比如说 10 个像素。

让我们试试看，看起来不错。

是啊，看起来好多了。

我们将通过上下左右各填充 10 个像素来创建它们之间的间距，这将是 20 个像素。

没错。

好的，现在我需要四个空格。

因为我只需要顶部和底部的间距，左边和右边的间距将为零。

是的，这样好多了。

你可以看到。

我们已经完成了词典应用的一半。

看看我们的印地语到底行不行。

我要打我的名字。

没错。

是长相。

是劳动人民。

太好了。

轻拍自己的后背。

你做得很好。

不管怎样，把畏缩放在一边。

那么我们下一步要做什么？我们要创建一个主题切换器组件？我们需要这个类别吗？现在吗？我不认为我们需要它。

让我检查一下。

我想我们的音频组件需要这个。

是的，我要做的是在创建音频组件时，如果类别仅为英语，那么我们要打印音频组件，因为音频组件在其他语言中不可用。

所以只要快速创建它。

所以意义。

它只在这个含义数组的第一项中可用。

所以我要输入 if 含义，第一项可用。

如果单词里面有东西，如果这是真的，第三件需要为真的事情是，如果 Kanu 里面有东西，而不是我的意思是，如果它是英语，如果类别是英语，如果这三个条件为真，那么只有它会去渲染我们的应用程序。

抱歉，音频组件。

我要进去了。

现在我要渲染音频部分。

好的，如果音频组件不被支持，我会输入你的浏览器不支持音频元素。

那更好。

我只需要给一些信息或类似的东西。

抱歉。

是啊。

所以在这个瓷砖里面，我们要命名为背景色。

我们将使用哈希 F F F 的背景色，白色，边框半径为 10。

让我们看看是否下雨了。

当然不下雨是因为我们什么都没打。

还是不渲染。

好吧，我们还没有提供。

显然，我们没有把它定义为提供 src 的 SRC。

所以 SRC 将在我们的含义之内。

我们意义的第一个要素。

这就是为什么我把第一个元素放在了语音学里面，正如我之前给你们展示的，语音学，对不起，语音学，而不是语音学的第一个元素。

好吧，有时会发生的是语音也丢失了。

所以我要测试语音是否存在。

所以语音是存在的，它是存在的，然后我们要再次输入同样的东西。

在这里面，有一种叫做音频的东西。

此外，我将提供 it 控制。

所以我们可以控制暂停和播放音频等。

现在，我们开始清理，你有它，你有音频组件，你有你的控制增加或减少音频，等等。

这是音频部分。

我们需要做的最后一件事是什么，我们需要创建一个主题切换器组件，我们需要在这里创建一个主题切换器组件。

好吧，让我们回到我们的应用程序。

我要做的是，首先，我要回到这个材料，你们为什么我要引入一个叫做开关的东西，我们需要，因为显然当我们需要开关来切换主题的时候。

这些是普通的开关，有二级开关，一级开关，不受控制的开关等等，我们需要一个开关来改变它的颜色。

这是自定义颜色组件，我将复制它，我将到这里复制这个特定的组件，这是组件，您可以浏览我们的文档，您也会了解这里发生了什么。

所以我要把这个复制过来。

让我们去看看 drlc 已经把它复制好了。

因此，在我们的应用程序 j . s .中，我将把它粘贴到这里。

它会抱怨，因为有些东西不是进口的，比如样式。

让我们用样式导入它。

还有别的东西，紫色，但是我们不需要紫色。

我们要把它换成灰色。

所以进入，你可以看到在灰色的颜色也很重要。

未定义开关。

是的，开关，我们还需要导入开关，重要的是开关，希望它能工作，否则不会在这里显示。

直到我们在应用程序中呈现出来。

让我在这里创造一个空间层次。

让我们面向那边，所以我要在我们的通讯器里面，把它渲染出来。

集装箱。

是的，就在头上。

我要做的是再给它一次机会。

因为我们要让这个 div 成为绝对的。

所以就是这个 div 的位置永远是右上角。

使其成为绝对的，样式将被定位为绝对的。

顶部将为零。

让我在里面写点东西，比如说主题。

你会看到你的队伍出现在这里。

我们会弥补的。

比如说 15 年。

是啊，那是个好地方。

我们将在 10 的顶部提供一点填充。

太好了。

好的，现在我们需要使用这个紫色开关，它的名字仍然是紫色开关。

让我把主题改变的名字改一下。

希望不是预建组件，material UI。

我就叫它黑暗模式吧。

是的，我要叫它黑暗模式。

那更好。

为了代替这个主题，我将在这里渲染黑暗模式。

开始了。

希望现在能渲染出来。

是的，我们走吧。

是的，大约在这个黑暗模式之前，我需要提供一个 span 标记，它将表示某个特定的模式，比方说现在它表示光模式，要切换光模式，您只需按下它，它就会切换到那个特定的模式。

此外，在这种黑暗模式中，当我们需要提供一些组件时，让我们提供 checked。

c，h，e，k，Ed，是的，选中，等于，我们要在里面提供一个特定的状态。

所以我们需要创建一个状态 four checked，就像 HTML 中的一个普通开关一样，它的工作方式就是这样，我们将提供一个值，并且在发生变化时也是如此。

因此，让我们快速地在亮模式中为我们暗模式创建状态，我将去创建一个用户状态，我将把它称为亮模式，我也应该把组件称为亮模式。

但不管怎样，灯光模式默认为假。

好吧，因为你可以看到，我们将在黑暗模式下加载这个应用程序，因为我们的眼睛是有用的，我们不想在光明模式下失明。

所以你的灯光模式在这里开始改变，它会打开开关。

所以我要做的是设置灯光模式，和当前值相反。

与光模式相反。

是的。

我们走吧。

我认为我们需要为这个东西提供造型。

这东西的柔韧性很好。

所以我要去我们的内部这里，我想我要提供一点点的造型。

或者我们的容器，伸缩方向，列高，100，VH。

是的，我们没有提供均衡的调整内容。

对齐内容，间距均匀。

均匀间隔。

现在，你会看到更漂亮的。

现在。

如果你在代码中有任何困惑，你可以去我的 GitHub repo，描述中提供了链接来查看所有的代码更改，如果你有任何困惑。

我说过两次了。

是的。

是啊。

我说到哪里了？

是的，黑暗模式。

所以我要做的是，如果这个人切换到灯光模式，这个 led 文本也应该改变。

首先，让我们解决这个问题。

我将在这里提供一个 JavaScript 标签。

比方说灯光模式，如果打开灯光模式，它会变暗。

否则，它会渲染光线。

让我们看看这是否有效。

默认情况下，它是黑暗模式。

所以我要打开它。看，如果我关闭它，它将显示黑暗模式。

好了，最后，关键时刻到了。

我们如何实现这个特性？所以我们需要做的是，在我们说过这些颜色的地方，我们只需要在那里放一个条件。

让我们从应用程序组件的最佳位置开始，我将让它全屏显示。

下一张支票我会过牌。

是不是更兴奋了？如果是，那么我要给它提供一个白色的颜色，三 f。

否则，那颜色。

颜色也会受到灯光模式打开的影响，我们会再次问这个问题。

如果听起来不错，那我就提供黑色。

哎呀，我不会打字。

我们走吧。

正在编译。

让我们来测试一下。

哇，像魔法一样管用。

但是我们还有一些事情需要处理。

此外，我们只是不希望这只是在我们脸上拍打光模式，只是我们想提供一种过渡线程，它很软，看起来很好。

我们将提供所有点五秒，您可以根据自己的需要进行配置。

勒尼奥，现在会有一点软了，好了。

看起来好多了。

所以我也要设计这个。

或者那东西在哪里？首先，让我看看是否有任何需要改变的地方，或者这里有什么关于暗模式或亮模式的地方？不，我想我们也要把光模式放在我们的标题里。

所以我们可以用这个东西，对吧，我要把这个也放在这里，伙计。

哎呦，不是光的主题。

灯光模式。

哇哦。

显然我不能输入光模式等于光模式。

我要把它提供给那个主题。

然后就去哎呀。

我要去我们的主题了。

我的意思是现在，我不能说话我不能，对不对？我太傻了。

是啊。

来不要用 snort 一个很大的教程。

灯光模式看到了吗？光会把它糊掉的。

灯光模式。

在这里，我将检查灯光模式是否打开？问号。

如果它打开了，我将为它提供黑色。

哈希 000。

否则，白色。

我要再次检查它，因为它已经打开了。

如果是的话，我会给它一个轻松的主题。

不然呜呜。

否则就是黑暗。

哟，让我们看看这个。

又来了。

很漂亮。

但是让我们在这里输入一些东西。

看到了吗？嗯，看起来不太好。

我们需要创造它一点点，只是需要给一些黑色的主题或东西。

让我们看看这里是否还有剩余的东西？我不这么认为。

所以我们也要为我们的定义组件提供同样的东西。

好吧。

没错。

比如风尚。

我要去我已经来过的地方。

所以我要在这里做什么，让我们看看。

是的，就在这里，我们要检查灯光模式是否打开。

问号是灯模式打开了。

因此，我们将为它提供一种颜色，这是我们的背景，三个 b 5360。

否则，白色。

我们将在这里再次测试这一点，是否有一个光模式，然后我们将提供它一个白色，否则，一个黑色？我们去看看。

开始了。

该死，看起来真漂亮。

非常好。

好吧。

我认为有了这个，我们已经成功地创建了我们的应用程序。

好吧，让我们快速进入 netlify。

我要主持这个网站。

那么我们如何在 netlify 上托管这些网站呢？

所以我已经在网上托管了他的网站。

已经，这个，这个单词搜索，我将从 get 中点击 new site，然后点击这里的 GitHub 按钮，然后我将登录。

所以你可以看到我已经授权了 GitHub，所以它将显示我的库，然后我将选择我的代码所在的 react 字典库。

所以推送到 GitHub 之后，你只需要点击这个 deploy side 按钮，站点就部署好了。

这就是你需要做的。

比方说，如果你想更改你的网站的域名，你要去哪里做，你要去你的网络应用程序，并点击域设置。

点击选项添加网站名称，你可以在这里提供一个自定义的网站名称。

因此，在我们的下一个视频中，我将让这个应用程序成为一个进步的 web 应用程序。

那么什么是 pw a 或者渐进式 web app 呢？所以 pwd 允许你在手机上安装一个特定的网络应用。

我的意思是，当你进入手机浏览器或桌面浏览器时，它会提示你一个安装图标。

因此，当你按下安装提示时，它会开始将该应用程序安装到你的手机上。

因此，您可以像在手机或桌面上使用原生应用一样使用该应用。

所以这是一个进步的网络应用程序的惊人特性。

所以让我们开始吧。

如你所见，我们正在浏览器中运行字典应用程序，我也打开了这个项目文件。

那我们现在要做什么？因此，我们将通过点击 inspect 转到 Chrome 开发者工具，然后转到 lighthouse。

因此，lighthouse 允许我们做的是，它将扫描我们的应用程序，并告诉我们它缺少哪些功能来使它成为一个进步的 web 应用程序。

因此，在您的情况下，它会显示所有这些被勾选的图标。

所以你需要保留除了 progressive web 应用程序之外的所有图标，然后点击 Generate Report。

因此，它会扫描您的网站，并告诉您创建 it 渐进式 web 应用程序所缺少的内容。

所以让我们检查一下。

好了，我们开始吧。

这些是红色错误，是我们需要完成的事情。

所以首先，我们从底层开始。

所以首先，它要求我们 manifest 没有一个可屏蔽的图标。

所以我们需要创造一个可掩盖的图标。

但首先，我要做的是，你可以看到，首先，让我们转到应用程序选项卡，你可以看到这个图标有这些反应图标。

所以我们不想要这个反应图标，我们要把这个图标改成我们自己的应用程序图标。

所以让我们去谷歌搜索字典，我可以 PNG，给你。

让我们来看图像。

我们将拥有该图标，在其中我们拥有完全的使用权。

我们去点击这里。

让我们选择这个图标。

我要点击这个图标，它会把我们带到网站，从这里，我们可以免费下载这个图标，哦，我们需要注册。

让我快速地做那件事。

好的，我们已经下载了这个图标，它是免费的，不需要任何费用。

所以让我们把这个图标转换成一个 favicon。

所以我们要去浏览器，输入 PNG 到 favicon。

我们去这个网站吧。

我们需要将文件拖放到这里。

让我们把这个文件放在这里。

让我们点击下载。

所以它为我们提供了这个 zip 文件。

让我们把 zip 文件放在这里，然后提取出来。

好了，我们到了。

那么我们需要哪些文件呢？我们需要这个。

好吧，我们需要这些。

还有这个。

是啊。

让我们把这些图标切掉。

我们将转到这里的“公共文件夹”,我们将它粘贴到这里并替换该文件。

在这里，我们有它，我们有这些织物的文件。

所以我们在这里，我们要删除这两个文件。

是的，我们不需要它们，我们要把它们拿掉。

好了，让我们回到我们的代码。

在 index . HTML 的公共文件夹中，我们需要添加几行。

就在这里，你可以看到这里有一个苹果触摸图标。

所以我们要把 H ref 改成别的。

让我们看看我们的文件名称是什么。

这是我们文件的名字，苹果触摸图标点 png。

所以苹果，触摸图标点 png，来了。

它已经自动建议了这个名称。

我们需要添加的另一件事是我们的 favicon。

图标。

所以链接，短。

明白了吗？好了，现在我们需要给它一个 href。

所以会很老套。

如果我没错的话。

让我查一下。

是的，网站图标图标。

是的，我们需要它。

我们走吧。

怎么了?好的，我们还没有关闭标签。

现在我们结束了。

我们去我们的网站查一下。

给你。

你可以看到图标已经改变。

让我重新加载这个页面，然后检查应用程序。

现在，我们可以在这里看到，我们的结构代码已成功更改。

对于第二步，我们需要做的是生成一个清单文件。

那么什么是清单点 JSON 文件，如果你去你的 react 应用，你可以在清单点 Jason 上看到这个东西。

它有它的图标点图标。

它有我们删除的徽标、不同的徽标或徽标文件。

我们不会使用它，我们没有在这里删除它。

我也要把这个删掉。

我们不需要这个。

我也要把那些删掉。

基本上我们要删除的所有内容。

首先，让我生成您的清单文件数据。

所以我要做的是我要去这个网站曼尼，快速发电机。

让我们看看。

开始了。

嘿，你拿到了。

让我们给我们的应用程序起个名字。

单词搜索。

我将把它简称为单词搜索。

团队色彩。

假设我要选黑色。

好吧。

显示模式将是独立的，因为我们希望这是作为一个本地应用程序。

所以我们要做的就是孤军奋战。

方向。

这取决于你的应用程序。

如果你的应用程序支持支持率以及景观，你可以选择任何。

在我的例子中，我将选择任何应用程序范围，这里没有任何要更改的内容。

所以我们开始吧。

我们已经生成了清单数据。

所以我要把它抄上来。

我将把我们的 VS 代码粘贴到这里。

那边需要逗号。

是啊。

好了，现在我们需要生成我们的图标。

那么我们要怎么做呢？之前，我们已经下载了那个文件。

它在哪里？是的，单词搜索，我们要用它来生成不同的尺寸。

所以你需要生成四种尺寸。

第一个是 190 2x 192，第二个是 256 3354，第四个是 512。

让我们去我们的浏览器。

让我们输入图像大小。

是啊，让我们去想象 resizer.com。

好了，我们到了，我们要在这里提供我们的图像。

首先，我们需要什么，我们需要生成 192 x 192。

开始了。

现在调整图像大小。

好了，我们到了，下载这张图片。

开始了。

所以我们要去我们的项目文件夹。

我们要把它粘贴到这里。

所以我们就在这里拖放吧。

这就是了。

现在是第二个。

还有，我要给它起个名字。

我们把它命名为 logo 吧。

192.

我们将遵循这个惯例。

好吧，让我们生成另一个。

让我们拿那个。

我将为所有内容生成，然后将它们粘贴到一起。

第二个是 256 进 256。

有下载那个。

第三个。

是的，第三个是 384 进 384。

开始了。

最终文件将是 512 和 251251 2x。

五个一个来调整图像大小。

好了，我们开始吧。

看到曾经误下载了两次？要删除它吗？哦，不，没关系。

好了，我们要把所有这些文件复制到这个文件夹，公共文件夹。

然后我会用我给这个标志一和二命名的方式给它们命名。

所以这个是 logo 256。

这个是 logo。

384.

这个是 logo。

512.

我们到了。

好了，我们已经成功地生成了所有的图标。

好了，让我们来看看 VS 代码。

让我们创造我们的图标。

所以我们现在应该做的是。

我们要进去了。

我在这里做了什么？让我们看看。

是的，所以我们要提供图标所在位置的 SRC。

我要把这个拿掉。

和类型标志 192。

这个也一样。

标志 256。

Logo 384。

最后，logo 512。

让我们看看什么应用程序。

还是省省吧。

是啊，让我们去铬合金。

让我们刷新这个页面。

编译成功。

让我们去检查和应用。

我们找到了。

我们这里有所有的图标。

太好了。

我们还需要添加一名服务人员。

稍后我会解释什么是 ServiceWorker。

我们去那边的灯塔吧。

让我们生成报告。

现在让我们开始努力吧。

好吧，首先是清单上没有吉祥物铃铛图标。

那么吉祥物铃铛图标是什么呢？

所以，当你在手机上安装这个应用程序时，这个图标就会显示在你的菜单上。

这是我最擅长的。

那么我们如何生成一个可屏蔽的图标呢？

所以有个网站叫大众有线点 app。

我将关闭这两个应用程序，我将转到 Mass cable、dot app slash 编辑器。

我们到了。

所以我们需要提供它。

首先，我们需要提供我们的图标或我们需要使用的图像。

所以这一个，我要拖放到这里。

所以我们在这里。

现在我们需要根据我们的，你知道，根据我们的喜好，所以我要做的是，比方说，圆角矩形。

嗯，看起来不错。

我会给它提供一点衬垫。

我要把它移过来一点。

看起来不错。

我认为那很好。

我觉得那看起来不错。

是的，所以我要把它导出来。

它已经下载了。

让我们去我们的公共文件夹。

我将把它拖放到这里。

让我们拖放到这里。

我要给它起名叫可掩蔽的。

mashable

好极了。

所以现在在我们的清单文件中，我们需要提供这个可屏蔽文件的位置。

我们去看看清单或者 JSON。

在这里的顶部，我将打开另一个花括号。

就像其他人一样，我们需要首先以 RC 的形式提供它，哎呀，以 c 的形式。

那么什么是 SRC 可屏蔽的？

我们给的名字不能出现在。

jpg。

那么大小是多少，大小是 1-967-219-6196。

2196 年。

这是我们需要添加的类型类型将是图像斜线 PNG。

目的是可以掩饰的。

在这里。

我们省省吧。

让我们看看我们的网站。

我们去这边，那边，刷新一下。

我将关闭它并打开一个新标签页。

去灯塔检查并生成报告。

再一次。

好吧，给你。

清单上有一个吉祥物铃铛图标。

让我们看看还剩下什么。

好多了。

好，接下来，我们需要做的是不要将 HTTP 流量重定向到 HTTPS。

所以当我们把它部署到 netlify 的时候它就会被修复。

因为 netlify 把所有的流量都导向 HTTPS，所以这不是问题。

所以让我们暂时离开这一个。

第三个是不注册 ServiceWorker，控制页面和启动 URL。

所以这三者是联系在一起的。

所以我们需要提供让我们看这里。

它在哪里？如您所见，我们目前没有任何 ServiceWorker 文件。

那么我们需要如何生成 ServiceWorker 文件呢？所以让我们去一个新的窗口。

我要做的是，打开一个文件夹。

让我们在这里打开一个文件夹。

我将把这个文件夹命名为 service。

工作什么的，什么都不重要。

所以我要做的是初始化一个新的 react 应用程序。

但这不会是传统的方式。

我要做的是进入 NP x create react app。

我们需要做什么？是的，我们需要写任何名字，比如说我的应用程序。

我要为 CR 写一个模板。

因此，当您键入这个时，它将生成一个 react 应用程序，特别是为 pw a 应用程序配置的应用程序。

所以让我们输入。

让我们看看会发生什么。

好了，我们开始吧。

它已经成功初始化了一个新的 react 应用程序。

那么我们需要从这个 react 应用中得到什么呢？我们需要这两个文件 serviceworker.js 和 service worker registration dot . js，我们将选择这两个文件，并将它们复制到我们的项目中，复制到 src 文件夹中，我将粘贴它。

所以我们有这两个 ServiceWorker 文件。

让我们回到那个项目。

让我们进入 index dot j s，看看这些文件是如何被使用的。

所以它在这里被使用。

我们还需要添加这一行。

所以我要先添加这一行，让我们导入它，复制并粘贴到我们的 index.js 文件中。

开始了。

我们回去吧。

让我们也复制这一部分。

复制并粘贴到这里。

我们省省吧。

让我们看看应用程序已经编译成功。

让我们回到我们的本地主机。

我将再次在新标签页中打开它。

让我们去视察和检查灯塔。

让我们看看报告。

好吧，让我们看看发生了什么。

好的，它仍然给出同样的错误。

为什么？因为这一点，当你进入 index 时，对不起，ServiceWorker，有一个叫做 inside of register 的东西，你可以看到它只在一个生产应用上工作。

所以我们的应用程序目前正在开发中。

所以让我们建立这个应用程序。

让我们看看它是否有效。

所以我要做的是输入 NPM，运行，建立。

因此，它将构建我们的应用程序，一个生产就绪的应用程序。

好吧，首先，哇，哇，哇，我们忘了做这个。

而不是注销。

我们需要把它改成挂号。

是的，以便它可以注册我们的服务人员。

现在让我们运行构建。

好了，应用程序已经建立成功。

它说你可以运行这个命令来运行生产版本。

但首先，如果你没有这个，先生，你需要做什么？你可以运行 NPM I，当你运行这个命令时，用破折号 g，抱歉，不是 om，是 NPM。

所以当你运行这个命令时，它会在你的机器上安装这个命令服务器。

所以现在让我们运行这个命令。

先生，达什的身材。

我要把它复制下来，然后在这里运行。

它说它在我们的 localhost 5000 中的 localhost 上提供服务。

所以我们来看看 localhost 5000。

现在。

localhost，我要把这个改成 5000。

我们的应用程序正在运行，让我们去检查和灯塔。

让我们再次生成报告。

好吧，给你。

这些检查结束了专家的工作。

现在你可以看到它点亮了这个渐进式网络应用图标。

因此，我们现在需要做的就是不要将 HTTP 流量重定向到 HTTPS。

因此，当您将此应用程序部署到 netlify 时，它将成功地将所有流量从 HTTP 定向到 HTTPS，然后它将被创建为一个渐进式 web 应用程序。

所以如果你不知道如何在 netlify 上部署这个应用程序，你可以查看我们之前的视频，我在哪里创建了这个字典应用程序。

因此，让我们检查我们部署的网站。

现在。

我们去搜字点 netlify 点 app。

这是我们部署的网站。

最后一项检查首先，你可以看到这里有一个安装按钮。

如果我单击这个安装按钮，它会要求我安装这个应用程序，就像我之前在手机中向您展示的一样。

这就是你如何在你的桌面上安装它。

让我们点击安装。

这个应用程序已经成功安装。

给你。

我们的应用程序就在这里，当我们点击它时，它已经作为本机应用程序安装在我们的电脑上。

检查有新报告的最终时间，它将向我们显示我们的应用程序现在是成功的 pw a。

嘿，你去吧。

这就是了。

我们的 progressive 应用程序运行良好。