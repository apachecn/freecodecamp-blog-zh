# 如何创建一个响应式滑动菜单

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-responsive-sliding-menu-97b90852a455/>

普拉尚·亚达夫

# 如何创建一个响应式滑动菜单

我开了一个名为[learnersbucket.com](https://learnersbucket.com/)的博客，在那里我写了关于 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro/) 、[数据结构](https://learnersbucket.com/tutorials/topics/data-structures/)和[算法](https://learnersbucket.com/examples/topics/algorithms/)的文章，帮助其他人破解编码面试。在推特[上关注我](https://twitter.com/LearnersBucket)定期更新。

当我用移动优先的方法设计我的博客时，我决定把我的侧边栏导航菜单放在右下角。没有必要粘贴标题，用户可以阅读全部内容。

这是我的手机菜单的简单版本。

![1*_qfdprsLd9bgo4VB1gTWyA](img/b10b1d144242202c2ba019e2e5f89bbd.png)

Sliding Sidebar Navigation Menu

以下是你如何创建自己的响应侧边栏导航菜单。

### 概观

在我们开始设计菜单之前，让我们想象一下我们需要哪些组件。

*   一个汉堡？将滑动菜单进行操作的按钮。
*   汉堡包按钮上的动画代表菜单的当前状态。
*   侧面导航菜单。

因为侧面导航菜单会在点击汉堡菜单时切换，所以我们可以将它们放在一个容器中。

### 属国

我喜欢使用 jQuery 进行 DOM 操作，因为它减少了我需要编写的代码量。

### 汉堡纽扣

#### HTML 结构

制作汉堡菜单有一个简单的技巧。

我们将使用一个`<d` iv > w `ith a .ham` burger 类来创建汉堡按钮包装器。然后我们将放置`three` <跨度>秒来创建汉堡的层次。

### 设计汉堡按钮

现在我们的按钮的 HTML 结构已经准备好了，我们需要设计它看起来像一个汉堡包。在设计时，我们需要记住，当用户点击它时，我们必须提供打开和关闭的动画。

当我们创建一个固定尺寸的汉堡包按钮时，我们将为包装器提供固定的尺寸。

*   我们创建了一个固定的父节点`.hamburger{position:fixed}`,将它放在屏幕上我们想要的任何地方。
*   然后我们把所有的`<sp`和>设计成小的矩形盒子 `with position:ab`溶质。
*   由于我们需要显示三个不同的条带，我们改变了第二个跨度`.hamburger > span:nth-child(2){ top: 16px`的顶部位置；}&3 号 sp`an .hamburger > span:nth-child(3){ top: 2`7px；}.
*   我们还为所有跨度提供了`transition: all .25s ease-in-out;`,这样它们的属性变化应该是平滑的。

### 用 jQuery 打开和关闭汉堡按钮

每当点击 hamburger 按钮时，它会切换到一个`open`类。我们现在可以使用这个类来添加开始&结束效果。

`.hamburger.open > span:nth-child(2){ transform: translateX(-100%); opacity:` 0；}将汉堡中间的长条向左滑动，使其透明。

`.hamburger.open > span:nth-child(1){ transform: rotateZ(45deg); top:16px`；} `& .hamburger.open > span:nth-child(2){ transform: rotateZ(-45deg); top:1`6px；}将使第一个和最后一个跨度达到相同的顶部位置，并旋转它们以形成 x。

![0*wV23krL_L1ewHBrk](img/cc97a3e52faab402e30e244b10ded4d5.png)

Hamburger Button

荣誉？我们有汉堡包了吗？按钮准备好了，现在让我们创建侧面导航。

### 响应侧面导航菜单

#### HTML 结构

我们将创建一个简单的导航菜单。

我们使用了一个`nav`元素来创建导航菜单，并将链接放在了`ul`中。

### 设计导航菜单

我创建了一个全屏侧菜单，你可以根据你的需要改变尺寸。我们用的是 `&`gt；选择器，以避免覆盖其他元素的样式。

现在，我们已经准备好了导航菜单和汉堡按钮，所以我们可以将它们包装在一个包装器中，使它们具有功能性。

### 滑动导航菜单

#### HTML 结构

我们已经在`.mobile-menu`包装器中放置了汉堡按钮和导航菜单。

### 设计滑动导航菜单

我们通过提供`.hamburger`到`.mobile-menu`的一些属性对设计进行了一点更新，使其固定，并使`.hamburger`相对于`<sp`和>保持相同的设计。

因为可以有多个`nav`元素，所以我们也更新了所有的选择器`.mobile-menu >` nav，以确保我们只指向所需的元素。

### 用 jQuery 实现侧边栏菜单的功能

我们现在将我们的`.open`类添加到`.mobile-menu`中，这样我们可以通过一个简单的改变来处理汉堡按钮和滑动菜单。

我们的动画 CSS 也相应地更新。

干得好？？？我们涵盖了一切。

点击这里查看工作演示

### 结论

这篇文章是关于一个简单的滑动菜单。我试着把它分成不同的组件，这样你就可以独立使用它们了。

感谢你有耐心阅读这篇文章。如果你今天学到了新的东西，那就给一些？。此外，与你的朋友分享，这样他们也可以学到新的东西。

就是这样，关注我 [Twitter](https://twitter.com/LearnersBucket) 进行知识分享。我写的是关于 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro/) 、Nodejs、[数据结构](https://learnersbucket.com/tutorials/topics/data-structures/) & [算法](https://learnersbucket.com/examples/topics/algorithms/)和使用 JavaScript 的全栈 web 开发。

*最初发布于[learnersbucket.com](https://learnersbucket.com/examples/html/how-to-create-responsive-sidebar-menu/)2019 年 4 月 14 日。*