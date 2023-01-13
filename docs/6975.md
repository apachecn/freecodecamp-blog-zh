# 如何决定是否应该链接或扩展 CSS 类

> 原文：<https://www.freecodecamp.org/news/how-to-decide-whether-you-should-chain-or-extend-css-classes-a8e17d7a7b0b/>

莎拉·达扬

# 如何决定是否应该链接或扩展 CSS 类

![DiJbWWgVzuJr0Rmgm4B1vUIHQPSQfewbwNji](img/ba922eb53396ac2a30856b9a97dceff4.png)

Photo by [Alexandru Tugui](https://unsplash.com/photos/-inuQpBGbgI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/chain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你正在构建一个经常变化的应用程序或网站，模块化的 CSS 方法可以解决很多问题。您不用在 CSS 中复制 HTML 结构并装饰它，而是创建可消费的组件库。这使得项目更具可伸缩性，并使 CSS 代码库处于控制之下。

CSS 模块化依赖于组合，这不可避免地使 HTML 变得臃肿。这种附带效应对许多人来说是一个很大的阻碍，因为它造成了“膨胀”。

在本文中，我们将比较两种技术:**链接**和**扩展**。我们会看到他们提供了什么，他们的缺点是什么，以便您可以做出更明智的选择。

### 链接

链接 CSS 类意味着**通过在 HTML 选择器**上添加粒度修饰符来组成想要的外观。复合风格创造了最终的视觉效果。这是大多数模块化 CSS 方法的默认行为。

让我们以下面的 OOCSS 代码为例:

```
.btn {  display: block;  box-shadow: 0 0 5px 0 rgba(0, 0, 0, .2);}.btn-default {  border: 3px solid grey;}.btn-primary {  background: purple; color: white;}
```

如果您要链接修饰符，您的 HTML 将如下所示:

```
<button class="btn btn-primary">Primary button</button><button class="btn btn-default">Default button</button>
```

现在让我们做一些更复杂的事情，这次用 BEM(块，元素，修改器):

```
<div class="media-object media-object--reverse media-object--outlined">  <div class="media-object__media">    <img class="media-object__img media-object__img--faded img img--square" src="..." alt="...">  </div>  <div class="media-object__body">...</div></div>
```

这里我们有更多的交互类:

*   `.media-object`块有几个修饰符(`.media-object--reverse`和`.media-object--outlined`)。
*   `.media-object__img`元素有一个修饰符(`.media-object__img--faded`)。
*   `.media-object__img`元素也是一个`.img`块，有自己的修饰符(`.img--square`)。

#### 赞成的意见

链接类的最大亮点是**分离责任**。它保持你的 CSS 代码库干净、简洁、易读、不重复。每个类做什么都一清二楚，你马上就知道什么该用，什么不该用。

它还能防止死代码:因为你在处理构建模块，所以一切都有潜在的用处。当你删除一个组件时，你只需要删除 HTML。

单独的修饰符非常适合表示状态。因此，这让 JavaScript 工程师的生活变得更加轻松。他们所要做的就是添加和删除类。

在大型项目上，**这种方法可以为你节省很多时间**。

#### 骗局

人们对模块化 CSS 最常见的一个问题是，它在 HTML 中创造了“类疯狂”。严格来说，这是真的。

划分责任的设计模式几乎总是导致更多的文件和冗长的代码。CSS 也不例外:**如果你选择一种方法来使你的代码库更易维护，对应的就是冗长的 HTML 文件**。

由于大多数编辑器和 ide 都提供了强大的自动完成功能，现在键入这么多代码已经越来越不成问题了。但是现在，每当你创建一个新的页面或者编写一个新的组件时，仍然需要编写更多的代码。随着时间的推移，这可能会导致混乱和冗余的感觉，从而让一些开发人员望而却步。

### 延伸

如果不想链类，可以扩展。我们仍然有相同的单独的块，但是没有在 HTML 中链接它们，**我们将基类的属性继承给它的修饰符**。这样，我们可以同时使用它们。

让我们使用 Sass 中的`@extend`函数来这样做:

```
.btn {  display: block;  box-shadow: 0 0 5px 0 rgba(0, 0, 0, .2);  &-default {    @extend .btn;    border: 3px solid grey;  }  &-primary {    @extend .btn;    background: purple;    color: white;  }}
```

这将变成下面的 CSS 片段:

```
.btn,.btn-default,.btn-primary {  display: block;  box-shadow: 0 0 5px 0 rgba(0, 0, 0, .2);}.btn-default {  border: 3px solid grey;}.btn-primary {  background: purple; color: white;}
```

有了上面的 CSS，我们的 HTML 看起来会像这样:

```
<button class="btn-primary">Primary button</button><button class="btn-default">Default button</button>
```

我们只有一个类，而不是许多看似重复的类。它有一个明确的名称，并保持代码可读。我们仍然可以单独使用`.btn`，但是如果我们需要它的变体，我们只需要在它上面添加修饰符部分，而不是链接一个新的类。

#### 赞成的意见

这种方法的亮点是一个整洁、可读性更好、更轻便的 HTML。当你选择模块化的 CSS 时，你也决定多做 HTML，少做 CSS。CSS 变成了一个库，而不是一个指令列表。因此，你在 HTML 上花了更多的时间，这就是为什么你可能想保持它的简洁易读。

#### 骗局

你的 CSS 可能**看起来**枯燥，尤其是如果你使用预处理器的话，但是**扩展类会导致 CSS 文件**更加沉重。另外，您对发生的事情没有太多的控制权:每次使用`@extend`，类定义都被移动到顶部，并被添加到共享相同规则集的选择器列表中。这个过程会导致奇怪的样式覆盖和更多的生成代码。

还有一种情况是想一起使用几个修饰语。使用 extend 方法，您不再需要在 HTML 中进行编写。如果要创建新的组合，您只有一个解决方案:通过扩展修饰符来创建更多的类。这很难维护，并且会产生更多的代码。每次你需要混合类时，你都需要编辑 CSS 并创建一个潜在的不可重用的新规则。如果你删除了使用它的 HTML，你也必须删除 CSS 类。

### 事后思考

模块化 CSS 以更冗长的 HTML 为代价，但对于它提供的所有好处来说，这并不算什么。如果你已经决定你需要模块化，不要用不兼容的实践搬起石头砸自己的脚。这将导致事倍功半。继承是诱人的，但是[组合已经不止一次被认为是一个好得多的策略](https://en.wikipedia.org/wiki/Composition_over_inheritance)。

当你看它的实际影响时，HTML“膨胀”并不是什么大不了的事情。模块化不可避免地会产生更多的代码——你选择的方法只决定了**的去向。从性能的角度来看，[更多的 HTML 远胜于更多的 CSS](https://frontstuff.io/in-defense-of-utility-first-css#it-bloats-the-html) 。**

不要关注无关紧要的小事。相反，利用工具来帮助你更有效地编写和导航代码。试着从大处着眼，根据事实做出选择，而不是个人喜好。

*最初发表于 [frontstuff.io](https://frontstuff.io/should-you-chain-or-extend-css-classes) 。*