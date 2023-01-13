# 何时使用 TypeScript:常见场景的详细指南

> 原文：<https://www.freecodecamp.org/news/when-to-use-typescript-a-detailed-guide-through-common-scenarios-b0a57e57905/>

系好安全带。在本指南中，我们比较了什么时候使用严格类型编程语言 TypeScript 是绝对重要的，什么时候坚持使用普通 JavaScript

你听说过那个叫做 **TypeScript** 的小编程语言吗？你知道，微软做的那个？有点像[炸掉](https://redmonk.com/sogrady/2019/03/20/language-rankings-1-19)的那个？

也许你和我一样，是一个真正的 JavaScript 纯粹主义者。我做的*很好*用 React 和 Node 构建没有类型的东西。道具类型和 [Joi 验证](https://github.com/hapijs/joi)一直对我很好，谢谢。

也许你在某个时候屈服了，尝试了一下。开始玩它。也许你讨厌它是因为它让你想起了 Java。也许你对自己不能马上高效工作感到恼火。

当我第一次开始使用 TypeScript 时，这些是我自己最初的感受。

我当然看不到它的好处…直到我开始经历一些真正烦人的事情。像构建没有在应该失败的时候失败，错误代码和错别字以某种方式进入生产代码，以及发现以一种真正干净的面向对象的方式表达我的设计越来越具有挑战性。

使用 TypeScript 9 个月后，我已经为客户在 Angular 应用程序中构建了新功能，我开始用 TypeScript 编译 [Univjobs](https://univjobs.ca) 的 React / Redux 前端，并将我们所有的后端服务从 vanilla Node.js 移植到 TypeScript，在此过程中重构了大量代码。

在本文中，我们将看看一些最常见的场景，并确定何时使用 TypeScript 可能是至关重要的，以及何时我们可以不使用它而坚持使用 *vanilla* JS。

### 为什么今天的讨论比以往任何时候都重要

我得出了一个非常重要的结论，根据你的情况、背景、项目、技能水平和其他因素，对你的项目来说，今天**不**使用 TypeScript 实际上是**危险的**。

首先，前端空间变得越来越复杂。某些曾经被认为是前沿的功能，现在已经成为非常标准的用户体验假设。

例如，几乎总是期望你的应用程序在某种程度上仍然可以离线工作。当用户在线时，通常也希望他们不用刷新页面就能得到实时通知。

这些要求相当苛刻(但在 2019 年肯定不是不现实的)。

在我们深入不同的场景之前，我们实际上应该讨论一下要解决的三类真正困难的软件问题。

### 3 类硬件软件问题

一般来说，有这三种。性能系统问题、嵌入式系统问题和复杂领域问题。

#### 1.表演系统问题

让我们来谈谈 Twitter。

Twitter 实际上是一个非常简单的概念。

你注册，你发微博，你喜欢别人的微博，差不多就是这样。

如果推特这么简单，为什么别人做不到呢？

很明显，对 Twitter 来说，真正的挑战实际上与其说是做什么，不如说是 T2 如何做什么。

Twitter 面临着一个独特的挑战，那就是每天要满足大约 5 亿用户的请求。

Twitter 解决的难题其实是一个*性能问题*。

当挑战是性能时，我们是否使用严格类型的语言就不那么重要了。

#### 2.嵌入式系统问题

嵌入式系统是计算机硬件和软件的组合，目的是实现对系统的机械或电气方面的控制。

我们今天使用的大多数系统都建立在一个非常复杂的代码层上，如果不是最初编写的，通常会编译成 C 或 C++。

用这些语言编码不适合胆小的人。

在 C 中，没有对象这种东西；作为人类，我们喜欢物体，因为我们可以很容易地理解它们。c 是程序性的，这使得我们用这种语言编写的代码更加难以保持整洁。这些问题还需要了解底层细节。

C++确实让生活变得更加美好，因为它具有面向对象的特性，但是挑战仍然是从根本上与底层硬件细节进行交互。

因为我们对于这些问题所使用的语言并没有太多的选择，所以在这里考虑 TypeScript 是不相关的。

#### 3.复杂域问题

对于一些问题来说，挑战不在于处理更多的请求，而在于代码库的大小。

企业公司有**个复杂的现实问题**要解决。在这些公司中，最大的工程挑战通常是:

*   能够从逻辑上把这一大块的各个部分分成更小的应用程序。然后，**在物理上**(用于受限环境的微服务)将它们分开，以便可以分配团队来维护它们
*   处理这些应用程序之间的集成和同步
*   对领域概念建模并实际解决领域的问题
*   创建一种被开发者和领域专家共享的无处不在的语言
*   不要迷失在大量编写的代码中，并且放慢速度，直到不可能在不破坏现有特性的情况下添加新特性

我本质上描述了[领域驱动设计](https://khalilstemmler.com/articles/domain-driven-design-intro/)解决的问题类型。对于这些类型的项目，您甚至不会考虑不使用严格类型的语言，如 TypeScript。

#### 面向对象的 JavaScript

对于**复杂领域**的问题，如果不选择 TypeScript 而是选择 JavaScript，那就需要付出一些额外的努力才能成功。你不仅要**非常熟悉**普通 JavaScript 的对象建模能力，还要知道如何利用面向对象编程的 4 个原则(封装、抽象、继承和多态)。

这可能**很难做到**。JavaScript 天生没有接口和抽象类的概念。

用普通的 JavaScript 很难实现与可靠设计原则的“接口分离”。

作为开发人员，单独使用 JavaScript 也需要一定程度的自律，以保持代码的整洁，一旦代码库足够大，这一点就至关重要。您还需要确保您的团队在如何用 JavaScript 实现通用设计模式方面拥有相同的原则、经验和知识水平。如果没有，你需要引导他们。

在像这样的领域驱动的项目中，使用严格类型语言的最大好处是 ***更少*** 关于表达 ***什么可以做*** ，但是更多的是关于使用封装和信息隐藏**通过限制哪些领域对象实际上被允许做*来减少 bug****的表面积。*

*我们可以在前端没有这个，但是在我的书里，这是对 **后端**的一个**硬语言要求。这也是我将 Node.js 后端服务转移到 TypeScript 的原因。***

*TypeScript 被称为“**可伸缩的 JavaScript】”是有原因的。***

*在所有三类硬软件问题中，只有复杂领域的问题是绝对需要打字稿的。*

*除此之外，还有其他因素可能会决定什么时候对您的 JavaScript 项目使用 TypeScript 是最好的。*

### *代码大小*

*代码大小通常与**复杂领域问题**联系在一起，大型代码库意味着复杂领域，但情况并非总是如此。*

*当一个项目的代码量达到一定规模时，跟踪存在的一切变得更加困难，而重新实现已经编码的东西变得更加容易。*

*重复是设计良好且稳定的软件的敌人。*

*当新开发人员开始在已经很大的代码库上编码时，这一点尤其突出。*

*Visual Studio 代码的自动完成和智能感知有助于在大型项目中导航。它在 TypeScript 上工作得很好，但是在 JavaScript 上有点受限。*

*对于我知道将保持简单和小型的项目，或者如果我知道它最终将被丢弃，我将不会那么迫切地推荐 TypeScript 作为必需品。*

### *生产软件与宠物项目*

***生产软件**是你关心的代码，或者是如果它不工作你会有麻烦的代码。这也是你为之编写测试的代码。一般的经验法则是“如果你关心代码，你需要对它进行单元测试”。*

*如果你不在乎，就不要做测试。*

***宠物项目**不言自明。做你喜欢的任何事情。你没有专业的承诺来维护任何工艺标准。*

*继续做东西吧！小事化了，大事化了。*

*也许有一天你会经历痛苦，当你的宠物项目变成你的主要项目，变成生产软件，这是错误的，因为它没有测试或类型？不像我去过那里或什么的…*

#### *缺少单元测试*

*并不总是有可能对所有事情都进行测试，因为，嗯——**生活**。*

*在这种情况下，我会说，如果你没有单元测试，下一个最好的办法就是用 TypeScript 进行编译时检查。在那之后，如果你使用 React，下一个最好的方法就是使用运行时检查和 Prop 类型。*

*然而，编译时检查不是单元测试的替代品。好的一面是单元测试可以用任何语言编写——所以这里关于 TypeScript 的争论是不相关的。重要的是测试已经写好了，我们对自己的代码有信心。*

### *创业公司*

*一定要使用任何能让你最有效率的方法。*

*这时，你选择的语言就不那么重要了。*

*你要做的最重要的事情是**验证你的产品**。*

*选择一种语言(比如 Java)或者一种工具(比如 Kubernetes)来帮助你在未来扩大规模(尽管你对它完全不熟悉),对于一家初创公司来说可能是也可能不是最好的选择。*

*取决于你有多早，你要做的最重要的事情是富有成效。*

*在保罗·格拉厄姆的著名文章《Python 悖论》中，他的主要观点是，初创公司的工程师应该使用能最大化他们生产力的技术。*

*总的来说，在这种情况下，使用你觉得最舒服的东西:类型或者没有类型。一旦你知道你已经构建了人们真正想要的东西，你总是可以向更好的设计重构。*

### *团队合作*

*根据您团队的规模和您使用的框架，使用 TypeScript 可能是成败的关键。*

#### *大型团队*

*当团队足够大时(因为问题足够大)，使用固执己见的框架是一个很好的理由，比如前端使用 Angular，后端使用 TypeScript。*

*使用固执己见的框架之所以有益，是因为你限制了人们完成某件事情的可能方式的数量。在 Angular 中，几乎有一种主要方法可以添加路由保护，使用依赖注入、挂接路由、延迟加载和反应式表单。*

*这里最大的好处是 API 被很好地指定了。*

*有了 TypeScript，我们还节省了大量的时间，并提高了沟通效率。*

*快速确定任何方法所需的参数及其返回类型的能力，或者仅通过公共、私有和受保护变量显式描述程序意图的能力非常有用。*

*是的，用 JavaScript 可以做到这一点，但是它很粗糙。*

#### *交流模式和实现设计原则*

*不仅如此，**设计模式**，软件中常见问题的解决方案，通过明确的严格类型语言更容易交流。*

*下面是一个常见模式的 JavaScript 示例。看看你能不能认出那是什么。*

```
 *`class AudioDevice {
  constructor () {
    this.isPlaying = false;
    this.currentTrack = null;
  }

  play (track) {
    this.currentTrack = track;
    this.isPlaying = true;
    this.handlePlayCurrentAudioTrack();
  }

  handlePlayCurrentAudioTrack () {
    throw new Error(`Subclasss responsibility error`)
  }
}

class Boombox extends AudioDevice {
  constructor () {
    super()
  }

  handlePlayCurrentAudioTrack () {
    // Play through the boombox speakers
  }
}

class IPod extends AudioDevice {
  constructor () {
    super()
  }

  handlePlayCurrentAudioTrack () {
    // Ensure headphones are plugged in
    // Play through the ipod
  }
}

const AudioDeviceType = {
  Boombox: 'Boombox',
  IPod: 'Ipod'
}

const AudioDeviceFactory = {
  create: (deviceType) => {
    switch (deviceType) {
      case AudioDeviceType.Boombox:
        return new Boombox();
      case AudioDeviceType.IPod:
        return new IPod();
      default:
        return null;
    }
  } 
}

const boombox = AudioDeviceFactory
  .create(AudioDeviceType.Boombox);

const ipod = AudioDeviceFactory
  .create(AudioDeviceType.IPod);`*
```

*如果你猜对了**工厂模式**，那你就对了。根据您对模式的熟悉程度，它对您来说可能不是那么明显。*

*现在让我们用 TypeScript 来看看。看看我们能在 TypeScript 中表达多少关于**音频设备**的意图。*

```
*`abstract class AudioDevice {
  protected isPlaying: boolean = false;
  protected currentTrack: ITrack = null;

  constructor () {
  }

  play (track: ITrack) : void {
    this.currentTrack = track;
    this.isPlaying = true;
    this.handlePlayCurrentAudioTrack();
  }

  abstract handlePlayCurrentAudioTrack () : void;
}`*
```

***即时改进***

*   *我们知道这个类是抽象的。我们需要在 JavaScript 示例中四处寻找。*
*   ***AudioDevice** 可以在 JavaScript 示例中实例化。这是不好的，我们打算将 **AudioDevice** 作为一个抽象类。抽象类不应该被实例化，它们只能被[具体类](https://khalilstemmler.com/wiki/concrete-class/)子类化和实现。在 TypeScript 示例中，这一限制设置正确。*
*   *我们已经标出了变量的范围。*
*   *在这个例子中， **currentTrack** 指的是一个接口。根据[依赖倒置](https://khalilstemmler.com/wiki/dependency-inversion/) **设计原则，**我们应该总是依赖抽象，而不是具体。这在 JavaScript 实现中是不可能的。*
*   *我们还发出信号表示， **AudioDevice** 的任何子类都需要自己实现**handlePlayCurrentAudioTrack**。在 JavaScript 示例中，我们揭示了有人在尝试从非法抽象类或不完整的具体类实现执行方法时引入运行时错误的可能性。*

*要点:如果您在一个大型团队中工作，并且您需要最小化某人可能滥用您的代码的潜在方式，TypeScript 是帮助解决这一问题的好方法。*

### *较小的团队和编码风格*

*较小的团队更容易管理编码风格和沟通。再加上林挺工具、关于事情如何完成的频繁讨论和预提交钩子，我认为小团队没有打字稿也能真正成功。*

*我认为成功是一个包含代码库规模和团队规模的等式。*

*随着代码库的增长，团队可能会发现他们需要依靠语言本身的帮助来记住事物的位置和它们应该是什么样子。*

*随着团队的成长，他们可能会发现他们需要更多的规则和限制来保持风格的一致性并防止重复代码。*

### *结构*

#### *反应和角度*

*吸引我和其他开发人员做出反应的主要原因是能够以优雅/巧妙的方式随心所欲地编写代码。*

*React 确实让你成为一名更好的 JavaScript 开发人员，因为它迫使你用不同的方式处理问题，它迫使你意识到 JavaScript 中的绑定是如何工作的，并使你能够将小组件组合成大组件。*

*React 也允许你有一点自己的风格。由于我可以实现任何给定任务的方式很多，所以在以下情况下，我会经常编写 vanilla React.js 应用程序:*

*   *代码库很小*
*   *只是我在编码*

*我会用 TypeScript 编译它，当:*

*   *超过 3 个人在编写代码，或者*
*   *代码库预计会非常大*

*出于同样的原因，我也可以选择使用 Angular 来编译 React 和 TypeScript。*

### *结论*

*总之，这些是我对什么时候打字稿是绝对必要的个人意见，我欢迎你不同意任何意见。*

*这就是我过去在决定是否使用 TypeScript 时的做法。然而，今天，因为我已经看到了光明，对我来说，使用 TypeScript 而不是普通的 JavaScript 并没有太多的努力，因为我对两者都同样满意，并且更喜欢类型安全。*

*我的最后一点是:*

#### *您总是可以逐渐开始使用 TypeScript*

*通过将 TypeScript 和 ts-node 添加到 package.json 并利用 tsconfig 文件中的选项 **allowjs: true** 来逐步开始。*

*这就是我如何将所有 Node.js 应用程序迁移到 TypeScript 的方法。*

#### *编译时错误比运行时错误好*

*你不能否认这一点。如果捕获生产代码中的 bug 对您特别重要，TypeScript 将帮助您最大限度地减少这些 bug。*

#### *如果你有能力学习它，那就去学吧。它为你的软件设计技能创造了奇迹*

*根据你在生活和职业中所处的位置，你可能没有时间去学习它。如果你有时间，我建议你开始学习它，开始学习**坚实的设计原则**和**软件设计模式**。在我看来，这是**提升为初级开发人员最快的方式**。*

*我希望这篇文章对你有用！你考虑在你的下一个项目中使用 TypeScript 吗？如果你同意/不同意，请在评论中告诉我。*

#### *[学习企业打字稿& JavaScript](https://khalilstemler.com)*

*现代 JavaScript 和 TypeScript 的基本软件开发模式、原则和教程。*

> *原载[4 月 6 日](http://khalilstemmler.com)@[T3【khalilstemmler.com】](https://khalilstemmler.com)**。***