# 我在 React Europe 2017 学到的东西

> 原文：<https://www.freecodecamp.org/news/what-ive-learned-from-react-europe-2017-c433468890d6/>

尼古拉斯·库里尔

# 我在 React Europe 2017 学到的东西

![1*LI8kcoWrhCJUoAXdqz6vaQ](img/c532d55d728d6d9bb0d3aa92b5a24e3d.png)

几天前，第三届欧洲最大的 React 会议在巴黎举行。今年没有热浪或交通罢工——只有精彩的谈话和有趣的人。

这是我对这个版本最欣赏的演讲的反馈。

说到有趣的人，我要衷心感谢刚刚在观众中认识的潘辰，感谢他对本文的评论，也感谢我在 M6 网的队友。

### 主旨

那些专门来听 React 新闻和更新的人应该会感到满意: [Andrew Clark](https://www.freecodecamp.org/news/what-ive-learned-from-react-europe-2017-c433468890d6/undefined) 用 React 的路线图开始了会议。所有这些更新都可以用一个名字来恢复:React Fiber。

Andrew 用新闻提要中的视频流的性能分析说明了 React Fiber。他们必须处理由于其他任务阻塞了主 JS 线程而导致的低性能。这是一个调度问题，通过拆分任务的执行得到了解决:主线程能够处理到达的数据块，因此视频不再被中断。React Fiber 背后的想法是通过异步渲染在组件级别调度这些任务。

在 React 16 中，改进还将包括片段(一组兄弟组件，不再有 div 包装，只是在渲染函数中返回一个数组)。此外，还将提供更好的错误处理，包括错误边界(即组件的 try/catch)以及关键错误与其他错误的区别。在严重错误的情况下，组件将卸载，以避免降级的用户界面，损坏的数据…

React 16 已经投入生产，准备安装下一个标签:

`yarn add react@next`

除 React 16 之外，增强的渲染调度允许根据视口位置确定渲染的优先级:这允许屏幕外或隐藏元素最后渲染，而不会延迟“立即可见”组件的渲染。当在当前屏幕中使用代码分割时，它还可以改善用户体验。代码分割会导致屏幕呈现不平滑的层叠效果。为了避免这种副作用，React 16 引入了一个“提交阶段”，在渲染阶段结束时延迟所有的 DOM 更新，以避免不一致的 DOM。

介绍 React Fiber 是开启会议的绝佳方式。它的新功能备受社区期待。了解更多关于 React Fiber 的信息:[https://github.com/acdlite/react-fiber-architecture](https://github.com/acdlite/react-fiber-architecture)

### 我所学到的基准反应

来自脸书的 Dominic Gannaway 展示了 React 软件包最近的优化。受 React like Inferno(由他创建)& Preact 的轻量级克隆体的设计启发，他致力于使 React 包更轻、加载更快。

得益于由 [Rollup](https://github.com/rollup/rollup) 支持的新构建系统，React 包的重量增加了 10%，加载速度提高了 9%。

> Webpack 用于应用程序，Rollup 用于库

他们还具体化了对`PropTypes`的支持，以及像`React.createClass`这样被否决的代码(参见[这里的](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html#new-deprecation-warnings))。

Dominic 指出了在尝试优化 React 包时使用强大且一致的基准测试工具的重要性。他展示了一个有趣的“快照基准测试”工具，将当前指标与之前的指标(包大小、初始加载时间、交互时间)进行比较，该工具运行在 [Google Lighthouse](https://developers.google.com/web/tools/lighthouse/) 上。

在移动 React 代码库时，这个工具对获得信心有很大的帮助。让 React 更小更快是前提。

了解更多 Dominic 的作品:
新构建系统:[https://github.com/facebook/react/pull/9327](https://github.com/facebook/react/pull/9327)基准:[https://github.com/facebook/react/pull/9465](https://github.com/facebook/react/pull/9465)

### 高质量的 JavaScript 工具

作为 Jest 的早期采用者，我可以说两年前使用 Jest 就像穿越沙漠一样。我也经历过 Jest 的变异，Jest 是如今一个伟大而广泛使用的工具。这就是为什么我很高兴听到克里斯托弗·波杰尔讲述这个故事。

![1*RGql2QYssW6i_RPldPED_A](img/2b32984a8d9ceec6fcacb0657b7ae340.png)

Sad jester (“Stańczyk” by [Jan Matejko](https://en.wikipedia.org/wiki/Jan_Matejko "Jan Matejko"))

那个故事就是:Jest 是如何从一个工具变成一个“产品”的。一个产品关注的是带来可采用性的性能和特性，对于一个测试运行人员来说，两种体验必须是令人愉快的:运行测试和编写测试。

通过实现并行测试运行，使用所有的 CPU，并测量执行时间以便为下一次运行更好地调度它们，性能得到了提高。

Christoph 和社区做了大量工作，将有趣的功能引入 Jest，如相关输出(有用的 diff 而不是无意义的错误堆栈跟踪)，沉浸式 watchmode(交互式 CLI，在测试用例列表中导航以重新运行它们)。

Christoph 提到了快照测试:除了常规测试(这不是替代！)，这是一个可视化组件综合输出(当前与预期)的好方法。

Jest 20 将与多项目运行器一起提供。通过使用选项`--projects`, Jest 能够运行多个测试套件，并在单个终端窗口中整合测试结果。对于在多回购代码库上使用监视模式非常有用。

Jest 已经被转移到一个多包架构中，其中一些被用于脸书的几个项目:jest-haste-map，jest-snapshot，jest-validate(一个 CLI 选项解析器，如 [Commander](https://github.com/tj/commander.js) )。在内部，整合基础架构和共享最佳实践非常有用。

值得一提的是:Jest 现在由 2 个核心开发者+社区维护。我们强烈鼓励贡献，Jest 项目是适当的，很容易作出贡献。

关于 Jest 20 的博文:[https://Facebook . github . io/Jest/blog/2017/05/06/Jest-20-pleasant-testing-multi-project-runner . html](https://facebook.github.io/jest/blog/2017/05/06/jest-20-delightful-testing-multi-project-runner.html)

### 越差越好:JavaScript 疲劳的好处

这个演讲的重点是 JavaScript 疲劳是一件好事，对生态系统有益。凯文·莱克首先提醒我们什么是 [JavaScript 疲劳](http://thefullstack.xyz/javascript-fatigue/)。

每月都有数百家新图书馆在黑客新闻上发布。不可能面面俱到。好消息是你不必这样做:只需选择**正确的东西**。

根据 Richard P. Gabriel 在八十年代提出的原则，正确的事情有四个黄金特性:

*   简单
*   正确性
*   一致性
*   完全

但是历史表明，正确的事情有时…不是正确的事情。实际上，只有简单才重要。

> 简单的东西更容易整合。

**越差越好**的概念由此诞生。如果事情很容易处理，我们可以忍受缺乏正确性、一致性或完整性！

在这一点上，出现了与 React 的提升相类似的东西:简单高于所有其他特征。

简单导致流行，流行导致贡献者。今天，我们可以享受 React 推出时所缺少的一切:状态管理、路由、入门工具(create-react-app)。

说到 create-react-app，它真的收集了 3 年来社区带来的所有最好的东西，如果它能在 react 发布会上发布就更好了。

那么，JavaScript 疲劳就是大量开源贡献者的结果。

总之，推出一些简单的东西，让它流行起来(记录在案，经过战斗考验)，然后填补漏洞。

我发现 Kevin 在现代 JavaScript 生态系统和“越差越好”理论之间做了一个有趣的类比。

关于理查德·p·加百列理论的优秀文章:[https://en.wikipedia.org/wiki/Worse_is_better](https://en.wikipedia.org/wiki/Worse_is_better)

### 瓦比·萨比

这个日语单词表示“美入不完美”的艺术。[程露](https://www.freecodecamp.org/news/what-ive-learned-from-react-europe-2017-c433468890d6/undefined)做了一个关于权衡取舍的富有哲理且非常鼓舞人心的演讲([连续第二年](https://www.youtube.com/watch?v=mVVNJKv9esE))。

作为软件工程师，权衡是我们工作的一大部分，因为我们没有无限的时间和金钱。因此，我们使用不完全满足我们需求的软件。假设我们 80%的需求:它对应于帕累托曲线上的**最佳点**，也就是 80/20 法则。意味着合理的时间量(约 20%)足以覆盖我们 80%的需求。

我们的 IT 世界充满了 80%的系统。例如，图像压缩:如果你以 20%的大小得到 80%的原始图像，这是一个很好的权衡。

关于 React，权衡可能是与兄弟组件的通信远比父/子组件难…原因是树模型，方便了 80%的情况。100%方法是一个图表模型。

通量也是一个 80%的系统。有些任务很难，比如管理副作用或外部变化源。Redux 通常很棒，但是 20%的不幸程序员需要肮脏的变通方法。这让我想起了[死投模式](https://medium.com/@erikras/redux-dead-drop-1b9573705bec)的例子。

> 有些东西本来就不是要优化的。

重要的是要指出，有时，这种权衡是有意的，因为我们处于最佳状态，我们不想移动。让我们回到上面的 React 例子，如果 React 转移到处理 100%用例的图模型，整个库将更加复杂，这不是一件好事。

处于一个 80%的系统中给了我们回旋的空间，它增强了我们的创造力和适应性。我用 [redux-observable](https://redux-observable.js.org/) 工作了几个月，我现在可以把它描述为“redux 没有覆盖的 20%的情况的一种权衡”。

![1*DBGdHlkB6DVjBy62d3TANQ](img/0941925b3cb94dd99ace171c88c4f9cb.png)

And redux-observable is also a 80% system!

80%的方法有缺点。首先:很明显是 20%。它可能不会被选择来建立基础。此外，80%的设计通常会让作曲成为一场噩梦(突变、副作用等等)。

有些东西属于 100%的世界。例如，使用 Flow 进行类型检查:Flow 的一个很大的优势是能够增量地对代码库进行类型检查。一个文件一个文件地添加流是可以的，但是你不能在一个文件中部分地添加流，因为你不能说“好的，我的应用的这一部分是经过严格的类型检查的，我相信它”。您将失去类型检查的好处(可信度)。

有趣的是，80%和 100%的系统可以互补:您可以使用 80%的工具，因为您在幕后有一个编译器(100%，编译器从不依赖您)。

阅读文档是 80%的行动，阅读源代码是 100%。

最后，程提到了莱斯利·兰波特的一个出版物，作为他演讲的灵感来源。他说，80%方法的可信度因科学领域而异，从数学的 0%到社会学/心理学的 100%。

再说一次，程拥有事后审视大局的高超技巧。我发现在这样一个技术会议上，他的讲话很有启发性，而且……有点令人耳目一新。

Leslie Lamport 的出版物:[http://Lamport . azure websites . net/pubs/future-of-computing . pdf](http://lamport.azurewebsites.net/pubs/future-of-computing.pdf)

### 增压器如何流动能反应

[Sasha Aickin](https://www.freecodecamp.org/news/what-ive-learned-from-react-europe-2017-c433468890d6/undefined) 用一些关于初始渲染时间的指标介绍了他的演讲(在商业网站上+1s 的页面加载时间导致-20%的转化率)，强调了 SSR(服务器端渲染)的重要性。

但是 SSR 带来了另一个问题:它导致了第一次呈现(浏览器立即绘制服务器端呈现的标记)和“交互时间”(浏览器已经加载了包，SPA 已经打开)之间的延迟。保罗·刘易斯用一种有趣的方式将这段时间称为“恐怖谷”。

此外，SSR 的伸缩性也不好:你的页面越复杂，在服务器端渲染的时间就越长。而且很难对你的 PO 说:“不不不，我不能这么做，这会让整个页面变慢！”

在服务器端呈现页面时会发生多种情况:

*   浏览器向服务器发送请求，
*   服务器从 API 获取数据，
*   服务器呈现标记，
*   服务器返回带有标记的页面。
*   浏览器绘制标记并发送对 CSS 和 JS 文件的请求。我们已经进入了恐怖谷。
*   服务器应答所请求的文件。
*   浏览器加载并执行该包。
*   我们终于到达了恐怖谷的尽头。

问题是所有这些任务都是按顺序发生的。如果我们能把事情并行化呢？

服务器可以逐段处理页面的呈现，而不是一个巨大的单片 SSR。让我们考虑一个由标题、主要内容和评论组成的页面。例如，Sasha 展示了服务器可以返回一个“chunk”，也就是为页面的每个部分呈现页面的一部分(HTML + CSS + JS + JSON)所需的所有片段。

使用这种方法(SCR 或服务器块渲染)，初始渲染时间和不可思议的山谷持续时间都减少了(特别是在移动设备上),因为 SSR 现在更像是一个“流”,它可以并行化:

*   浏览器向服务器发送请求，
*   服务器处理并返回第一个块(标题)
*   当浏览器绘制并加载它时，服务器开始处理第二个块(主要内容)，甚至可以同时向 API 请求第三个块(评论)的数据。
*   服务器返回第二个块
*   诸如此类…

另一个好处是:如果对 API 的多个调用中的一个陷入黑洞，它不会关闭整个页面，而只是关闭与块相关的区域。

演讲结束后，我脑海中有很多疑问:如何定义应用程序每个页面中的块？这是否暗示着老式 HTML 模板的回归？(我想我必须看看 Sasha 的 lib [react-dom-stream](https://github.com/aickin/react-dom-stream) )，但是，是的，我们似乎已经看到了 SSR 的未来:

> 重命名 react server-> ReactDOMServerStream 该文件将替换 ReactDOMServer。

最近的拉动请求显示，官方反应回购中有东西要来了:[https://github.com/facebook/react/pull/9710](https://github.com/facebook/react/pull/9710)

关于恐怖谷的博文:[https://aero twist . com/blog/when-everything-is-nothing-is/](https://aerotwist.com/blog/when-everything-is-important-nothing-is/)

### 闪电对话(第一天)

#### 返回 null 约书亚·科莫

Joshua Comeau 谈到了“无渲染”组件。这些组件不呈现任何东西，但却享受 React 的生命周期。

他展示了一个非常简单的`Log`组件，由于 React 生命周期，每当它发生变化时，它都会在控制台中打印其子组件。

然后，一个更严肃的例子:一个包装了 Web 语音 API 的组件，它大声读出文本输入的内容。该组件能够在读取新内容之前中断当前的语音:因为新内容是一个道具，这可以很容易地在`componentWillReceiveProps`中实现。

约书亚在他的 Github 库中收集了大量的解释(还有幻灯片和例子):[https://github.com/joshwcomeau/return-null](https://github.com/joshwcomeau/return-null)

#### Detox:用于 React Native 的 Graybox 端到端测试和自动化库

即使是从未听说过排毒的人也知道，当演讲开始前观众鼓掌时，就在标题出现在屏幕上时，事情就要发生了！

在移动开发领域，手动 QA 非常耗时，Wix 应用程序需要长达 10 天的时间。所以 [Tal Kol](https://www.freecodecamp.org/news/what-ive-learned-from-react-europe-2017-c433468890d6/undefined) 和 Wix 的人们致力于自动化测试，并使用了排名第一的框架: [Appium](http://appium.io/) 。它并没有让人满意，因为测试必须在真实的设备上运行，它们非常慢，写起来很复杂，而且不可靠(它们最终到处都是*睡眠()*)。所以他们决定创造一种新工具:排毒！

虽然 Appium 是一个黑匣子，但 Detox 更像是一个“灰箱”:设备(或模拟器)上的一切都受到监控(网络、JS 线程等。)，保持受控和同步以避免剥落。

演讲以一个展示快速测试套件执行的视频结束。

在我的客户公司 [M6 网络](http://tech.m6web.fr/)中，我们遇到了与 Appium 完全相同的问题。由于闪电谈话的形式，不可能详细解释排毒的内部，所以我们仍然有很多问题(它与布朗菲尔德应用程序一起工作吗？)，但我们一定会自己尝试排毒！

回购:[https://github.com/wix/detox](https://github.com/wix/detox)

#### React Native 上的严肃图形

结束第一天的好方法:充满酷东西的视觉闪电演讲，如视频游戏、图像过滤器、数据即，...来自世博会的詹姆斯·伊德。

James 展示了 Expo 的 GLView 组件，该组件能够在移动设备上显示 OpenGL GPU 加速的图形。

GLView 组件架构与其他 React 本机组件略有不同。它使用 JavaScriptCore(移动 JavaScript 执行环境)的能力来调用 C 原生 API，而不使用主桥。所以图形不受桥上占领的影响。

然后，James 展示了一些图形界面，如来自相机的视频流的效果、视频游戏和一个使用现有库 [gl-react](https://github.com/gre/gl-react) 创建的 3D 视图示例，该库与 React Native 兼容。

GLView 文档:[https://docs.expo.io/versions/v17.0.0/sdk/gl-view.html](https://docs.expo.io/versions/v17.0.0/sdk/gl-view.html)

### 组成:一个超级大国解释

我发现函数式编程通常非常有用、简洁和优雅，但是我们不应该陷入做函数式编程的陷阱……当代码变得不如以前可读和可理解时。尼克·格拉芙(Nik Graf)在介绍他关于构图的演讲时没有落入那个陷阱，他的灵感来自于他在[波兰](https://github.com/styled-components/polished)的色彩 API 上的工作。

为了说明最初的问题，Nik 展示了一个使用 [Lodash](https://lodash.com) 的命令式代码的例子(由于现场视频已经暂时从 YouTube 上删除，我已经重写了类似的代码片段):

充满临时变量的代码可以用 chain API 来改进:

这更简洁，但仍然不完美，仍然有`chain`和`value`操作符(说真的，谁没有忘记在显式 Lodash 链末端的`.value()`？).此外，链式 API 扼杀了部分导入 Lodash 的能力(参见[这篇精彩的文章](https://medium.com/making-internets/why-using-chain-is-a-mistake-9bc1f80d51ba))。

真实组合允许定义一个带有 [Ramda](http://ramdajs.com/) 、 [lodash/fp](https://github.com/lodash/lodash/wiki/FP-Guide) 的管道:

我们可以在 React with the HoC(High-Order Component)模式中找到类似的难看的语法，同样，组合可以提高可读性:

这里可以使用 Ramda、lodash/fp、recompose 或 react-apollo 中的`compose`函数(哦，我已经在本文中放入了令人印象深刻的 gif，对吧？).所以这里似乎有一个真正的模式。

这是 Nik 创建 polish 的颜色 API 的灵感来源:一种颜色可以由色调、亮度和饱和度组成！他们开发了一个很酷的 api，比如 Ramda 或 Lodash/fp，可以组合，但由于 currying，也可以在命令式风格中使用。

回到 React，Nik 展示了我们可以编写自己的 HoC 函数来避免重复。例如，当我们有一堆组件需要一个布尔属性为真来显示内容时:

我们可以用一个特殊的函数`withErrorMessage`来做同样的事情，比如用一个错误消息来修饰一个文本输入。

Nik 以 React VR 屏幕显示行星旋转结束了讲话，旋转是用类似 React Native 动画的语法实现的。他解释了动画是如何包含在一个可重用的特设功能`withRotation`中的。

这个讲座提供了相关的例子来说明 FP 概念，如 currying，高阶函数以及它们如何应用于反应(高阶分量)。我非常期待分析这个`withRotation` HoC，看看我是否能把它移植到我的 React 本地项目上。

react doc about composition & HoC:[https://Facebook . github . io/react/docs/higher-order-components . html](https://facebook.github.io/react/docs/higher-order-components.html)

### 作为一个平台做出反应

众所周知，Airbnb 大量使用 React 和 React Native。利兰·理查森指出这是一大进步:

不是由 3 个不同的团队(JavaScript / Java / Swift)编写 3 次 Airbnb(针对 Web、iOS 和 Android)，React 和 React Native 允许由同一个团队(JavaScript)编写 2 次(Web 和 Native)。但是现在，工程师写了两次几乎相同的代码。如果他们只写一次不是更好吗？

> 写一个，跑到哪里？

利兰认为我们可以做到这一点。React Native 令人大开眼界:React 可以处理多个平台。然后， [React Native for Web](https://github.com/necolas/react-native-web) 来了，接着是 Windows，Ubuntu，VR，…

平台间组件共享的最大障碍是平台实现的导入，例如 React Native:

```
import { View } from 'react-native';
```

请注意，由于遗留原因，React DOM 组件没有这样的导入，但这难道不是为了一致性吗？

```
import { Div } from 'react-dom'; //To discuss only, don't do that ;)
```

根据 Airbnb 的 React 原生组件库的经验，只有 7 个 React 原生 API 被广泛使用。70 多个 API 中的很多都是由这 7 个主要 API 组成的。

Leland 提出了一个建议:这 7 个 API 被包装在一个名为 react-primitives 的 NPM 包中。

*   `View`
*   `Image`
*   `Text`
*   `Animated`
*   `Touchable`
*   `Platform`
*   `StyleSheet`

利兰意识到它并不完美:`Animated`的存在是有疑问的。相反，`TextInput`不包括在内，尽管它不可能是 7 个基元的组合。

但是也许一个可选的平台扩展就是答案(受 React Native 通过在文件名末尾添加后缀来定义特定于平台的实现的能力的启发)。

以 Checkbox 为例: *Checkbox.js* 可以包含一组原语，用于在不存在 checkbox 的每个平台上设计 checkbox。作为一种优化， *Checkbox.web.js* 可以只包含来自 react-dom 的输入组件。

```
<input type="checkbox">
```

如果这还不够令人信服，利兰结束了他与 [react-sketchapp](https://github.com/airbnb/react-sketchapp) 的谈话，展示了他们如何将设计工具 [Sketch](https://www.sketchapp.com/) 作为一个额外的平台，由于原语，无需修改代码库。Airbnb 设计师可以使用 React 原生组件的真实库来设计故事板。

反应-原语:[https://github.com/lelandrichardson/react-primitives](https://github.com/lelandrichardson/react-primitives)

### 闪电对话(第二天)

#### 世博小吃

在这两天里，除了为每位演讲者做了精彩的介绍外， [Brent Vatne](https://www.freecodecamp.org/news/what-ive-learned-from-react-europe-2017-c433468890d6/undefined) 还在一次闪电般的演讲中展示了世博会应用程序的点心。

零食是之前 React 原生游乐场( [rnplay-web](https://github.com/rnplay/rnplay-web) )的替代。rnplay-web 的问题是反馈循环非常慢。代码更改被发送到服务器，服务器必须运行 React 本机打包程序并将输出发送回浏览器。

使用 Snack，来自 web 编辑器的代码更改被发送到设备并直接重新执行。

小吃还包括一个二维码生成，让您的代码在自己的设备上运行博览会应用程序。

官方 React 本地文档的例子已经转移到了 Snack。比如:[http://Facebook . github . io/react-native/releases/next/docs/text . html](http://facebook.github.io/react-native/releases/next/docs/text.html)

要玩二维码生成，只需将世博会的桥接组件拖放到您自己的设备上即可:[https://snack.expo.io/](https://snack.expo.io/)

#### React 应用程序更智能的代码拆分和预加载

在他的闪电演讲中， [Brandon Dail](https://www.freecodecamp.org/news/what-ive-learned-from-react-europe-2017-c433468890d6/undefined) 谈到了 react-loadable 的代码拆分以及它使用静态方法预加载组件的能力`preload()`。

关于预加载有两种方法。“被动”预加载基于用户的**意图**。例如，列表页面可以预加载细节页面，因为用户很有可能想要打开细节页面。这种方法应该基于分析，以避免浪费的预加载。

相反，“主动”方法包括基于用户的**动作**的预加载，就像鼠标接近按钮(`onMouseEnter`事件)。对于这种情况，Brandon 创建了库 react-perimeter，它定义了组件周围的边界，并在违反边界时调用回调(类似于`onMouseEnter`事件):这对于调用 preload 方法很有用。

react-loaded:[https://github.com/thejameskyle/react-loadable](https://github.com/thejameskyle/react-loadable)
react-perimeter:[https://github.com/aweary/react-perimeter](https://github.com/aweary/react-perimeter)

会议结束仅两天，会议 YouTube 频道就开始发布会谈视频，敬请关注[https://www . YouTube . com/channel/ucorln 2 ozfgoj-fucf 2 ez 1a/videos](https://www.youtube.com/channel/UCorlLn2oZfgOJ-FUcF2eZ1A/videos)！