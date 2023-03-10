# 是的，JavaScript 发展很快。无论如何都要构建组件库。

> 原文：<https://www.freecodecamp.org/news/yep-javascript-moves-fast-build-your-component-library-anyway-a50576ab3031/>

有个问题我最近听过几次:

> “如果我们在 React/Vue/Angular/whatever 中创建一个组件库，用一种新的组件技术代替它，会怎么样？”

这不是如果的问题。这是一个何时的问题。这些技术已经变得非常流行，但它们并不是终极目标。像所有技术一样，更好的东西最终会出现并取代它们。

但这一事实在很大程度上无关紧要。今天，为您的公司建立一个可重用组件库仍然是绝对重要的。

原因如下。

### 今天行动更快

可重用组件通过创建更高层次的抽象帮助您的团队更快地前进。组件通过以编程方式实施标准化方法来消除决策疲劳。只考虑一个自以为是的形式`TextInput`组件。

它可以消除以下所有决策:

1.  我应该把标签放在输入的上面还是旁边？
2.  我应该在输入的右边还是下面显示验证错误？
3.  误差应该是什么颜色？
4.  我应该如何标记必填字段？
5.  我应该在模糊时还是提交时验证必填字段？
6.  我应该在标签和输入之间放置多少填充？

这样的例子不胜枚举。这些不是你的设计者和开发者每次创建新表单时应该问的问题。

### 强制一致性

可重用组件增强了用户界面的一致性。您的公司可能有许多开发人员。然而你的工作是开发一个看起来像是由一个开发者开发的应用。

为此，使用可重用的组件至关重要。复制/粘贴不是一种设计模式。如果设计人员和开发人员可以一次又一次地从头开始，那么您的应用程序将很快变成不同外观、感觉和技术的拼凑品。

### 提高性能

在客户端渲染的应用程序中，每次使用一个组件都会提高性能。为什么？因为它最小化了应用程序的包大小和内存占用。第二次使用一个组件**不需要额外的下载，也几乎不需要额外的内存**。

如果没有组件库，您的团队很可能会包含重复的 JavaScript，以稍微不同的方式解决相同的问题，这将使包膨胀并降低性能。更糟糕的是，他们很可能会抢占另一个竞争库，从而要求用户下载多个做同样事情的完整库。

### 维护更少

更多的代码导致更多的维护。更多的维护会导致更高的成本和更多的人员，从而产生额外的沟通开销，进一步降低您的速度。可重用组件可以最大限度地减少当前需要创建和维护的代码量。

### 以后更容易更新

最后，是的，最终你今天使用的组件技术将成为遗产。但是现在通过创建一个可重用的组件库，您可以最小化以后需要更新的表面区域。

迁移一个精心组件化的应用程序要容易得多，因为您可以一次一个组件地替换现有组件。当您的应用程序是不同技术和模式的拼凑时，这就不那么容易了。可重用组件最小化了以后需要更新的外围应用。

### 低投资

组件库实际上不需要那么多工作。例如，如果您选择 React，您不需要(通常也不应该)从头开始。有几十个成熟的组件库可供选择，也有上百个独立的组件。

使用一个流行的组件库作为您的起点，并根据您的需要进行调整。相信我，这不需要很长时间，而且好处很大。

或者，您可以选择创建普通的 CSS 组件作为基础。这种方法的一个例子是 StackOverflow 中的[栈。这种方法有两个优点:](https://stackoverflow.design)

1.  如果您将来转向一项新技术，您在 JavaScript 组件中使用的普通 CSS 基础可以重用。
2.  如果你的公司目前正在使用多个组件方法，比如 React、Angular 和/或 Vue，那么这个 CSS 方法可以作为所有方法的基础。

缺点是什么？你必须从头开始构建你的组件，这样它们才能利用你简单的 CSS 组件基础。

我的偏好？利用现有的 JavaScript 组件库作为基础，最大限度地减少运行所需的代码量。

### 摘要

不要让 JavaScript 的快速创新吓得你放弃为公司投资可重用组件库。是的，今天的技术最终会被取代，但变化是技术中唯一不变的。出于以上所有原因，可重用组件在今天是值得接受的。

寻找如何完成这项工作的更多细节？我最近在 Pluralsight 上发表了“[创建可重用的 React 组件库](https://app.pluralsight.com/library/courses/react-creating-reusable-components/table-of-contents)”。([免费试用](https://help.pluralsight.com/help/free-trial-10-days-andor-200-minutes))

### 想了解 React 的更多信息吗？⚛️

我已经在 Pluralsight 上创作了[多个 React 和 JavaScript 课程](http://bit.ly/psauthorpageimmutablepost)。

![ukD8krGR7M8AnnK7lzjsuLRcFnAOUcCPEIZr](img/9f37e31010842ba6daa837cd84fadd31.png)

Cory House 是多门 JavaScript、React、clean code 课程的作者。NET，以及更多关于 [Pluralsight](http://pluralsight.com/author/cory-house) 的内容。他是 reactjsconsulting.com[的首席顾问，软件架构师，微软 MVP，在前端开发实践方面培训国际软件开发人员。科里在 Twitter 上以](http://www.reactjsconsulting.com) [@housecor](http://www.twitter.com/housecor) 的身份发布关于 JavaScript 和前端开发的推文。