# React Conf 2018 的经验教训

> 原文：<https://www.freecodecamp.org/news/lessons-learned-at-react-conf-2018-bc390f5b1aa4/>

作者:杨顺泰

# React Conf 2018 的经验教训

![1*XxPm9SJhbJ_NHMdFRhbxeA](img/71201198b055fa6f23bc99b8e029c1f6.png)

Front row seat at React Conf 2018!

感谢我的经理们，我有幸参加了 React Conf 2018，这是一次令人敬畏的活动。我一直在网上观看过去的 React Conf 视频，能够参加这次活动并听一些业界最优秀的人现场直播，这令人兴奋！

React Conf 是一个为期两天的活动，有 20 多名演讲者在台上。以下是精彩片段的总结，以便那些未能出席的人可以从我的经历中吸取教训，并迅速决定是否值得花时间观看完整的视频，这些视频可以在 YouTube 上找到。

#### 向功能组件添加状态，并使用 React 挂钩重用生命周期逻辑

会议以脸书(当时)React 团队工程经理 Sophie Alpert 和 React 核心团队成员兼 Redux 创始人 Dan Abramov 的主题演讲拉开帷幕。Sophie 以一个有趣的琐事开始了演示，在 Google trends 上，React 也比可再生能源和橙汁更受欢迎！

她随后重申，自从 React 于 2013 年成立以来，它的使命就是让构建优秀的用户界面变得更加容易。React 试图简化难以做到的事情，例如通过悬念简化组件的异步数据依赖关系，通过时间切片提高渲染性能，确保 React 首先处理应用程序中最重要的渲染。

React 做得好的另一件事是，在您通过 React Devtools 扩展开发和调试应用程序时，拥有出色的开发人员体验和工具，React dev tools 扩展最近添加了一个分析器功能，以帮助开发人员了解应用程序中发生的事情。

React 中的一些东西仍然不理想——在多个组件之间重用逻辑传统上是通过使用更高阶的组件和渲染道具来完成的。它们解决了这个问题，但是对于不得不重构代码的开发人员来说，这是一个缺点。这可能导致包装器地狱，并使通过应用程序的数据流难以跟踪。

第二个问题是，在巨型组件中，逻辑有时会被各种生命周期方法混淆和分割，比如在`componentDidMount`中订阅一个存储，在`componentWillUnmount`中取消订阅。

分离逻辑往往会导致这样的情况，即在安装后忘记清理，这可能会导致内存泄漏。最后一个问题是课程。功能组件通常必须转换成类才能使用状态和生命周期方法，并且必须添加样板文件来支持它们。使用`this`和绑定回调也可能会令人困惑。

React 团队对以上三个问题有一个提议——React Hooks。

丹·阿布拉莫夫随后上台！每当有人想要对 React 进行实质性的添加或更改时，React 都会使用征求意见(RFC)流程，并且必须编写提案，详细说明更改的动机和设计。React 钩子的提议可以在这里找到。应该注意的是，钩子不包含任何破坏性的改变或废弃，它的使用是选择性的。

Dan 演示了如何使用几个新的 React Hook API——`useState`、`useEffect`和`useContext`，将一个典型的带状态的类组件转换成功能组件，其好处是显而易见的。我们能够将生命周期逻辑放在一个`useEffect`钩子中，并且可以跨组件重用它们。这引发了一场社区运动，创建了有用钩子的 npm 包，它们可以在[这里](https://github.com/rehooks/awesome-react-hooks)找到。

使用钩子有几个注意事项:你不能在一个条件中调用钩子，它们必须在你的功能组件的顶层，如果你没有正确使用钩子，有一个 linter 插件会警告你。

这是因为 React 依赖于钩子的执行顺序来匹配钩子的状态值。您只能在功能组件中使用钩子或者其他定制钩子(按照惯例应该以`use`开头)。

脸书已经在生产中使用钩子四个月了，所以行为相对稳定。钩子可以与你现有的代码共存，你现在就可以开始使用它们，并逐渐将你的非钩子代码移植到钩子上。

React Router 的创建者 Ryan Florence 演示了如何重构一些真实的用例来使用钩子。Ryan 首先谈到了渲染道具如何给组件一种虚假的层次感，比如一个用于查询响应屏幕大小的组件，然后使用 lt；媒体>组件与渲染公关`ops usin` g 一个他当场创建的使用媒体挂钩。

下一个演示是关于重构/创建一个具有所有基本功能的可访问的旋转木马——播放/停止按钮、前进/后退按钮和进度条。

这里更详细地介绍了`useEffect`钩子，以及它们如何只在某些状态/道具改变时运行，这是你在`componentDidUpdate`中要做的事情。`useEffect`可以用来实现你所需要的`componentDidMount`、`componentDidUpdate`和`componentWillUnmount`一样的东西。

最后，Ryan 还演示了如何通过使用`useReducer`钩子在应用程序中采用 Flux/Redux 式的单向数据方法。挂钩返回两个变量，`state`和`dispatch`。

像 Redux 中一样，您提供给`useReducer`的 reduce 函数将接受当前状态和一个动作作为参数，并根据传入的动作产生一个新状态。

我建议你看看他的娱乐性和启发性的视频演示。他现场演示的代码也可以在他的 [GitHub repo](https://github.com/ryanflorence/react-conf-2018) 中找到。

另外，我还了解到改变一个组件的键会重置它的状态，因为改变键会卸载和重新装载一个组件。非常方便的提示！

#### 视频链接

*   [今天和明天做出反应——索菲·阿尔珀特&丹·阿布拉莫夫](https://www.youtube.com/watch?v=V-QO-KO90iQ&list=PLPxbbTqCLbGE5AihOSExAa4wUM-P42EIJ)
*   [90%的清洁工人使用钩子进行反应——Ryan Florence](https://www.youtube.com/watch?v=wXLf18DsV-I&index=2&list=PLPxbbTqCLbGE5AihOSExAa4wUM-P42EIJ)

#### 通过 React 中的并发渲染改善用户体验和开发人员满意度

会议的第二天由安德鲁·克拉克和布莱恩·沃恩主持开幕。在开发时，在让我们的应用程序更快的过程中降低用户体验并不罕见。Andrew 举了一个 Ads Manager 的[例子，作为这样的应用之一，因为组件代码分割或数据获取导致了创建流程中大量的旋转器。这些微调器也只出现一瞬间，因为在快速网络上加载数据不需要很长时间。](https://twitter.com/acdlite/status/991503599246098432)

在过去的一年里，React 团队一直致力于并发 React，旨在让开发者在默认情况下轻松构建高性能应用，并提供流畅的非 janky 用户体验。

并发 React(以前称为异步 React)允许 React 同时处理多件事情，并根据优先级在它们之间切换。今天，React 仍然同步工作。如果一个组件需要很长时间来更新，主线程将被阻塞，浏览器在工作完成之前无法响应用户输入。使用 concurrent React，可以暂停工作，主线程可以响应用户输入，然后恢复工作。这也被称为时间切片，将工作分成块并随时间分散执行的能力。

然后，Andrew 使用了一个由三个选项卡组成的应用程序作为例子。有了`React.lazy`，很容易对应用程序进行代码分割，并且只在组件被渲染时才在每个标签中加载组件。React 还附带了一个`<Suspen` se >组件，允许开发人员在组件代码尚未加载时呈现回退。

这些组件可以放在组件树的任何地方，如果树的任何部分没有被加载，将使用最近的`<Suspen` se >组件的回退。上述特性在同步模式下工作，不需要任何并发的 React 特性。前面提到的一个问题是，如果资源加载得很快，就会有一个闪烁的微调按钮。使用并发 React，可以避免这些不必要的闪烁微调器，因为您可以配置在显示回退微调器之前愿意等待的阈值。

Andrew 展示的最后一件事是，在用户读取第一个选项卡内容的空闲时间内，预加载和预呈现其他选项卡中的内容是多么容易。只需将一个`hidden` props 传递给一个 HTML 元素，React 就会将它的所有子元素的优先级降低到离屏优先级，并且只在页面上没有其他事情可做时才加载它们。当导航到其他选项卡时，它们会立即出现，因为它们已经在空闲时间加载。

Brian Vaughn 随后演示了 React Devtool 中内置的一个新的分析工具(它已经在您的浏览器中了)和一个新的分析 API。该配置文件的工作方式类似于 Chrome 性能配置文件，您可以记录一些交互，并可以查看渲染持续时间和哪些组件被渲染的火焰图。

这有助于调试不必要的重新渲染和检测渲染缓慢的组件。通过使用 experimental scheduler 的 trace API，还可以将性能信息归因于代码中的事件。注意，这个 API 还没有完成，所以要小心使用。点击了解更多关于 React 新交互追踪功能[的信息。](http://fb.me/react-interaction-tracing)

帕尔默总部的首席工程师贾里德·帕尔默演示了他如何通过使用新的并发 React 功能来改善他的 Spotify-clone(恰当地命名为 Suspensify)的用户体验。React cache 不仅可以用于缓存 API 响应数据，还可以用于缓存图像、音频文件和脚本等资产。

Jared 展示了他如何利用`unstable_createResource` API 和`<Suspense` / >来显示低分辨率占位符艺术家个人资料照片图像作为占位符，同时在后台下载高分辨率图像，然后在下载完成后显示高分辨率图像。使用`g the unstable_createRe`源 API 加载的数据也更容易阅读，因为开发人员不再需要显式地处理和编写加载状态的代码。悬念带来的好处是协调和加载状态容易。

最后，需要注意的是，使用`<Suspense` / >的代码在没有并发反应的情况下仍然可以工作；用户体验方面的优势减少了，但开发人员体验方面的优势依然存在。

#### 视频链接

*   [React 中的并发渲染——安德鲁·克拉克和布莱恩·沃恩](https://www.youtube.com/watch?v=ByBPyMBTzM0&list=PLPxbbTqCLbGE5AihOSExAa4wUM-P42EIJ&index=15)
*   [移动反应悬念——贾里德·帕尔默](https://www.youtube.com/watch?v=SCQgE4mTnjU&list=PLPxbbTqCLbGE5AihOSExAa4wUM-P42EIJ&index=16)

#### React Native 存在问题，但解决方案正在酝酿中

React Native 是脸书使用 JavaScript 和 React 构建原生移动应用的框架。React 的创作者詹姆斯·朗谈到了 React Native 的缺点，尤其是动画。在他的经验中，当编写与响应用户交互和动画相关的 React 本机代码时，用户体验是可怕的，因为滞后的动画。

原因是因为用户交互是在本地线程上处理的，但是交互效果是由 JavaScript 线程通过异步桥处理的。这个问题的一个解决方案是使用 [React Native 手势处理程序](https://kmagiera.github.io/react-native-gesture-handler/)库，它提供了一个声明式 API，将平台原生触摸和手势系统暴露给 React Native。对于更复杂的交互和动画，`[react-native-reanimated](https://github.com/kmagiera/react-native-reanimated)`(React Native 手势处理程序的同一作者)可以用来表示声明性 API 不能表示的逻辑。

在最坏的情况下，开发人员甚至可以更低级地编写本地语言代码和 API。总之，在处理动画时阻塞主线程，尽可能避免异步。声明式 API 对于许多用例来说是很好的，但是命令式 API 对于复杂用例来说是非常有用的。

React Native 团队的脸书工程师 Parashuram 简要介绍了 React Native 的当前架构以及当前 React Native 存在的问题，重申了 James 关于 React Native 上的交互的观点，即由于本机线程和 JavaScript 线程之间的异步桥，React Native 不能对用户交互做出快速响应，而人们习惯于在纯本机应用程序和 web 上进行用户交互。

React Native 团队对这个问题的解决方案是一个用于 JavaScript 和 native land 之间通信的新接口，称为 JavaScript 接口(JSI)。这基本上是 JavaScript 与 Objective-C 或 Java(或任何本地语言)对话的一种简单方式。

JavaScript 端访问一个与 HTML 元素(在 React Web 上)非常相似的主机对象，然后你可以调用它的方法和属性。

你也可以使用 JSI 来获得返回主机对象的本机模块，你可以调用它们的方法，类似于 RPC 调用。我期待的另一个变化是，React Native 可能会将核心库之外的一些视图管理器和本机模块转移到社区中，这使得 React Native 更轻量级，并且只要不依赖外部模块，升级就很容易。脸书内部使用 React Native，其他大公司如微软、Pinterest 和 Zynga 也是如此。

因此，脸书致力于改进 React Native 并与社区一起前进。

#### 视频链接

*   [继续，阻塞主线程——詹姆斯·龙](https://www.youtube.com/watch?v=ZXqyaslyXUw&index=24&list=PLPxbbTqCLbGE5AihOSExAa4wUM-P42EIJ)
*   [React Native 的新架构——Parashuram N](https://www.youtube.com/watch?v=UcqRXTriUVI&index=25&list=PLPxbbTqCLbGE5AihOSExAa4wUM-P42EIJ)

#### 您可以在没有 GraphQL 的情况下使用 GraphQL(的最佳特性)

网飞的工程师 Conor Hastings 详细介绍了 GraphQL 的设计原则，并解释了为什么 GraphQL 对工程师有吸引力。他打了一个很好的比方，传统的 REST 类似于订购比萨饼，但不能选择浇头，最终有 40 种浇头，而使用 GraphQL，你可以只选择你想要的浇头——这消除了过度获取数据的问题。GraphQL 的其他好处包括只需一次往返就可以获取分层数据，以及 GraphQL 令人敬畏的开发人员工具(graph QL)。

不是所有的软件都需要 GraphQL。

如果你的应用程序不需要维护，你的用户有高速互联网，你可能不值得花时间去创建一个完整的 GraphQL API。然而，你仍然可以在你的应用程序中使用 GraphQL 的一部分，并利用 GraphQL 的一些好的部分——也就是说，强大的查询系统可以很容易地调整数据以满足你的 UI 需求。

Conor 然后介绍了 [RouteQL](https://github.com/conorhastings/routeql) ，这是他构建的一个库，旨在使用 graphql-js(客户端上的 graphql 解析器)和 GraphQL 生态系统中其他流行库的工具，以便您可以在与任何后端对话的浏览器中编写 GraphQL 查询。通过做出一些牺牲和放弃 GraphQL 的一些好处，我们仍然可以在不使用 GraphQL 的情况下利用 GraphQL 的强大功能。GraphQL 非常适合没有太多客户端状态的服务器数据驱动的应用程序。

#### 视频链接

*   [无图形 SQL 的图形 SQL—Conor Hastings](https://www.youtube.com/watch?v=YSEUAi1dAdk&list=PLPxbbTqCLbGE5AihOSExAa4wUM-P42EIJ&index=10)

#### React 有助于使用户界面更加美观

马特·佩里讲述了如何使用现有的命令式动画库实现动画，如 [animated](https://github.com/animatedjs/animated) 和 [Popmotion](https://github.com/Popmotion/popmotion) 以及它们的缺点。通过从我们编写的命令式逻辑中识别模式，我们可以将命令式 API 简化为声明式 API。作为上述问题的解决方案，Matt 介绍了他的动画库 [Pose](https://popmotion.io/pose/) ，它使用了一种声明式的类似 CSS 的方法来制作动画，这使得编写普通动画变得非常简单。

在 Elizabet Oliveira 的另一个演讲中，她谈到了什么是 SVG，SVG 的好处，我们可以在 web 和 React 中使用它们的各种方式。当 SVG 插图需要动画和可定制时，将它们编写为带有道具的可组合组件尤其有益。

React-kawaii ，是伊丽莎白创建的可爱插图库，可以轻松定制。你可以通过改变道具来改变复杂的 SVG 插图的大小、颜色、情绪(内容)。在其网站上查看[演示](https://react-kawaii.now.sh/)。

#### 视频链接

*   [通向充满活力的未来之路——马特·佩里](https://www.youtube.com/watch?v=1e07uPWpvzI&list=PLPxbbTqCLbGE5AihOSExAa4wUM-P42EIJ&index=4)
*   [作为 React 组件的 SVG 插图— Elizabet Oliveira](https://www.youtube.com/watch?v=1gG8rtm-rq4&list=PLPxbbTqCLbGE5AihOSExAa4wUM-P42EIJ&index=17)

#### Create React App 2 的新增功能

创建 React 应用程序(CRA)是一个入门项目，它可以很容易地引导新的 React 应用程序，或者如果你想尝试 React。CRA 的维护者 Joe Haddad 介绍了 CRA 2 中的新特性:PostCSS 支持， [Babel 宏](https://github.com/kentcdodds/babel-plugin-macros)，Sass 和 CSS 模块，类型脚本支持。在 [React 博客](https://reactjs.org/blog/2018/10/01/create-react-app-v2.html)上阅读更多内容。

#### 视频链接

*   [Create React 应用程序的新特性—乔·哈达德](https://www.youtube.com/watch?v=He-m9gd6WyM&index=5&list=PLPxbbTqCLbGE5AihOSExAa4wUM-P42EIJ)

#### 网络非常适合制作复杂的用户界面

GDevelop 是一个游戏编辑器，由脸书的工程师 Florian Rival 在很多年前创建。他讲述了他如何通过利用 React、Electron 和 WebAssembly 将编辑器(最初用 C++编写)现代化到 web 的旅程。Florian 使用了 emscripten，它将自己的原生字节码编译成 WebAssembly 格式，并利用 React 庞大的组件库和工具生态系统重写了 React 中的 UI。Florian 使用虚拟化、新的 React profiler 和`shouldComponentUpdate`极大地优化了性能。游戏编辑器是非常复杂的应用程序，令人印象深刻的是，在 React 和开源生态系统中的其他工具的帮助下，Florian 能够在一年内将他的原生应用程序移植到 electronic。

#### 视频链接

*   [React、JavaScript 和 WebAssembly 移植传统原生应用——Florian 的竞争对手](https://www.youtube.com/watch?v=6La7jSCnYyk&list=PLPxbbTqCLbGE5AihOSExAa4wUM-P42EIJ&index=13)

#### 最后的想法

我最初发现 React 钩子在要被析构的数组中返回值很奇怪，但是在工作中使用了一个多月后，我逐渐习惯了。除了上面提到的钩子的好处之外，还有另一个巨大的好处就是构建规模[下降](https://twitter.com/sebmck/status/1055695821641924609) [下降](https://twitter.com/jamiebuilds/status/1056015484364087297)，因为大量的类方法被移除了。状态变量，因为它们只是组件中的局部变量，现在也被缩小了。

钩子很棒，但是它们也有缺点。

*   在每次渲染时运行。如果我们在`useEffect`回调中设置状态，我们可能会导致无限循环。一个例子可以详细说明这个陷阱可以在这个[栈溢出问题中找到，我写在这里](https://stackoverflow.com/q/53243203/1751946)。建议开发者在使用之前仔细阅读`useEffect` API，并充分理解。
*   `useEffect`和`useCallback`中的闭包(或代码)可能引用了过时的`state`和`props`。我也在[这个堆栈溢出问题](https://stackoverflow.com/q/53024496/1751946)中写过这个陷阱。如果你不确定你是否引用了旧的值，状态钩子更新器也有一个[回调表单](https://reactjs.org/docs/hooks-reference.html#functional-updates)，在这里你可以访问以前的状态值。

更多的缺点可以在 RFC [这里](https://github.com/reactjs/rfcs/blob/master/text/0068-react-hooks.md#drawbacks)找到。

我一直在工作中使用钩子，我们实现了一个小的表单输入抽象，帮助验证和跟踪表单的更改/脏状态。我的团队到目前为止都很喜欢！

我还没有玩转并发 React，但是从演示来看，在生产代码中使用它似乎非常容易。我相信并发反应、悬念和分析器特性与提高用户体验和开发人员的幸福感高度相关。

期待 React 未来版本中更多的 React 善良！？

如果你喜欢这篇文章，请别忘了留下？。(你知道可以不止一次鼓掌吗？自己试试看！)

你也可以在 [GitHub](https://github.com/yangshun) 和 [Twitter](https://twitter.com/yangshunz) 上关注我。