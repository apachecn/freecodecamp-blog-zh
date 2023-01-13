# 统一架构–构建全栈应用的更简单方式

> 原文：<https://www.freecodecamp.org/news/full-stack-unified-architecture/>

现代全栈应用——如单页应用或移动应用——通常有六层

*   数据存取
*   后端模型
*   API 服务器
*   API 客户端
*   前端模型
*   和用户界面。

通过以这种方式构建，您可以获得设计良好的应用程序的一些特征，例如[关注点分离](https://en.wikipedia.org/wiki/Separation_of_concerns)或[松散耦合](https://en.wikipedia.org/wiki/Loose_coupling)。

但是这并不是没有缺点的。这通常是以牺牲其他重要特征为代价的，比如简单性、[内聚性](https://en.wikipedia.org/wiki/Cohesion_(computer_science))和敏捷性。

似乎我们不能拥有一切。我们必须妥协。

问题是，开发人员通常将每一层都构建成一个完全不同的世界。

即使你用相同的语言实现这些层，它们也不能很容易地相互通信。

你将需要大量的[粘合代码](https://en.wikipedia.org/wiki/Glue_code)来连接它们，并且[域模型](https://en.wikipedia.org/wiki/Domain_model)在整个栈中被复制。结果，您的开发敏捷性受到了极大的影响。

例如，向模型中添加一个简单的字段通常需要修改堆栈的所有层。这可能有点荒谬。

嗯，这个问题我最近想了很多。我相信我已经找到了出路。

诀窍是:当然，应用程序的各层必须“物理”分离。但它们不需要“逻辑上”分开。

## 统一架构

![Traditional vs unified architecture](img/5d945e08e8051e56db49101e0d88d57d.png)

在面向对象编程中，当我们使用继承时，我们会得到一些类，这些类可以从两个方面来看:物理的和逻辑的。我这么说是什么意思？

假设我们有一个从类`A`继承而来的类`B`。那么，`A`和`B`可以看作是两个物理类。但逻辑上，它们并没有分开，`B`可以看做是用自己的属性组成`A`的属性的逻辑类。

例如，当我们调用一个类中的方法时，我们不必担心该方法是在这个类中实现的还是在父类中实现的。从调用者的角度来看，只需要担心一个类。父类和子类统一成一个逻辑类。

将相同的方法应用于应用程序的各个层怎么样？例如，如果前端可以以某种方式从后端继承，这不是很好吗？

这样，前端和后端将统一到一个逻辑层。这将消除所有的沟通和分享问题。事实上，后端类、属性和方法可以从前端直接访问。

当然，我们通常不希望将整个后端暴露给前端。但是对于类继承也是一样，有一个优雅的解决方案叫做“私有属性”。类似地，后端可以有选择地公开一些属性和方法。

能够从一个统一的世界中掌握应用程序的所有层并不是一件小事。它完全改变了游戏。这就像从一个 3D 世界到一个 2D 世界。一切都变得简单多了。

[传承不恶](https://liaison.dev/blog/articles/Do-We-Really-Need-to-Separate-the-Model-from-the-UI-9wogqr#composition-over-inheritance)。是的，它可能会被误用，而且在某些语言中，它可能相当严格。但如果使用得当，它是我们工具箱中一个无价的机制。

不过，我们有个问题。据我所知，没有一种语言允许我们跨多个执行环境继承类。但我们是程序员，不是吗？我们可以构建任何我们想要的东西，我们可以扩展语言来提供新的功能。

但在此之前，我们先来分解一下堆栈，看看每一层如何适应统一的架构。

### 数据存取

对于大多数应用程序，可以使用某种 ORM 对数据库进行抽象。因此，从开发人员的角度来看，不需要担心数据访问层。

对于更加雄心勃勃的应用程序，我们可能必须优化数据库模式和请求。但是我们不希望后端模型因为这些问题而变得混乱，这就是额外的一层可能是合适的地方。

我们构建一个数据访问层来实现优化问题，这通常发生在开发周期的后期，如果曾经发生过的话。

反正如果需要这样的层，我们可以以后再建。通过跨层继承，我们可以在后端模型层之上添加一个数据访问层，而几乎不需要修改现有的代码。

### 后端模型

通常，后端模型层处理以下职责:

*   塑造领域模型。
*   实现业务逻辑。
*   处理授权机制。

对于大多数后端来说，在一个层中实现它们就可以了。但是，如果我们想单独处理一些问题，例如，我们想将授权与业务逻辑分开，我们可以在相互继承的两个层中实现它们。

### API 层

为了连接前端和后端，我们通常会构建一个 web API (REST、GraphQL 等。)，而这让一切都变得复杂了。

web API 必须在两端实现:前端的 API 客户机和后端的 API 服务器。这需要担心两个额外的层，并且通常会导致复制整个领域模型。

web API 只不过是粘合代码，而且构建起来很麻烦。所以，如果我们能避免它，这是一个巨大的进步。

幸运的是，我们可以再次利用跨层继承。在统一架构中，不需要构建 web API。我们要做的就是从后端模型继承前端模型，就大功告成了。

然而，仍然有一些构建 web API 的好用例。这时我们需要向一些第三方开发者公开后端，或者需要与一些遗留系统集成。

但是说实话，大部分应用都没有这样的要求。当他们这样做时，事后处理就很容易了。我们可以简单地将 web API 实现到一个继承自后端模型层的新层中。

关于这个主题的更多信息可以在本文的[中找到。](https://liaison.dev/blog/articles/How-about-interoperability-oy3ugk)

### 前端模型

既然后端是真实的来源，就应该实现所有的业务逻辑，前端不应该实现任何。因此，前端模型只是从后端模型继承而来，几乎没有添加任何东西。

### 用户界面

我们通常在两个独立的层中实现前端模型和 UI。但是正如我在[这篇文章](https://liaison.dev/blog/articles/Do-We-Really-Need-to-Separate-the-Model-from-the-UI-9wogqr)中所展示的，它不是强制性的。

当前端模型由类组成时，可以将视图封装成简单的方法。如果你现在不明白我的意思，不要担心，在后面的例子中会变得更清楚。

由于前端模型基本上是空的(见上)，直接在其中实现 UI 就可以了，所以没有用户界面层*本身*。

当我们想要支持多个平台(例如，web 应用和移动应用)时，仍然需要在单独的层中实现 UI。但是，由于这只是继承一个层的问题，这可以在开发路线图的后面。

### 把所有东西放在一起

统一架构允许我们将六个物理层统一为一个逻辑层:

*   在一个最小的实现中，数据访问被封装到后端模型中，封装到前端模型中的 UI 也是如此。
*   前端模型继承了后端模型。
*   不再需要 API 层。

同样，下面是最终实现的样子:

![Traditional vs unified architecture](img/5d945e08e8051e56db49101e0d88d57d.png)

很壮观，你不觉得吗？

## 联络

要实现一个统一的架构，我们所需要的就是跨层继承，我开始构建[联络](https://liaison.dev)来实现这一点。

如果您愿意，您可以将 Liaison 视为一个框架，但是我更愿意将其描述为一种语言扩展，因为它的所有特性都位于尽可能低的级别——编程语言级别。

所以，联络不会把你锁在一个预定义的框架里，整个宇宙都可以在它的基础上被创造出来。你可以在本文的[中阅读更多关于这个话题的内容。](https://liaison.dev/blog/articles/Getting-the-Right-Level-of-Generalization-7xpk37)

在幕后，联络依赖于一个 [RPC](https://en.wikipedia.org/wiki/Remote_procedure_call) 机制。因此，从表面上看，它可以被视为类似于 [CORBA](https://en.wikipedia.org/wiki/Common_Object_Request_Broker_Architecture) 、 [Java RMI](https://en.wikipedia.org/wiki/Java_remote_method_invocation) 或[的东西。网 CWF](https://en.wikipedia.org/wiki/Windows_Communication_Foundation) 。

但是联络是完全不同的:

*   不是[分布式对象系统](https://en.wikipedia.org/wiki/Distributed_object)。事实上，联络后端是无状态的，所以没有跨层的共享对象。
*   它是在语言级别实现的(见上文)。
*   它的设计很简单，只公开了一个最小的 API。
*   它不涉及任何样板代码、生成的代码、配置文件或工件。
*   它使用一个简单但强大的序列化协议( [Deepr](https://deepr.io) )，该协议支持一些独特的特性，比如链式调用、自动批处理或部分执行。

Liaison 以 JavaScript 开始了它的旅程，但是它处理的问题是通用的，并且可以毫不费力地移植到任何面向对象的语言。

### 你好柜台

让我们通过将经典的“Counter”示例实现为单页面应用程序来说明 Liaison 是如何工作的。

首先，我们需要在前端和后端之间共享一些代码:

```
// shared.js

import {Model, field} from '@liaison/liaison';

export class Counter extends Model {
  // The shared class defines a field to keep track of the counter's value
  @field('number') value = 0;
} 
```

然后，让我们构建后端来实现业务逻辑:

```
// backend.js

import {Layer, expose} from '@liaison/liaison';

import {Counter as BaseCounter} from './shared';

class Counter extends BaseCounter {
  // We expose the `value` field to the frontend
  @expose({get: true, set: true}) value;

  // And we expose the `increment()` method as well
  @expose({call: true}) increment() {
    this.value++;
  }
}

// We register the backend class into an exported layer
export const backendLayer = new Layer({Counter}); 
```

最后，让我们构建前端:

```
// frontend.js

import {Layer} from '@liaison/liaison';

import {Counter as BaseCounter} from './shared';
import {backendLayer} from './backend';

class Counter extends BaseCounter {
  // For now, the frontend class is just inheriting the shared class
}

// We register the frontend class into a layer that inherits from the backend layer
const frontendLayer = new Layer({Counter}, {parent: backendLayer});

// Lastly, we can instantiate a counter
const counter = new frontendLayer.Counter();

// And play with it
await counter.increment();
console.log(counter.value); // => 1 
```

这是怎么回事？通过调用`counter.increment()`，我们增加了计数器的值。注意，`increment()`方法既没有在前端类中实现，也没有在共享类中实现。它只存在于后端。

那么，我们怎么可能从前端调用它呢？这是因为前端类注册在从后端层继承的层中。因此，当前端类中缺少一个方法，而后端类中暴露了一个同名的方法时，它会被自动调用。

从前端的角度来看，操作是透明的。它不需要知道方法是远程调用的。它只是工作。

实例的当前状态(即`counter`的属性)会自动来回传输。当在后端执行一个方法时，在前端已经被修改的属性被发送。相反，当一些属性在后端发生变化时，它们会在前端得到反映。

> 注意，在这个简单的例子中，后端并不完全是远程的。前端和后端都运行在同一个 JavaScript 运行时中。为了使后端真正远程，我们可以很容易地通过 HTTP 公开它。参见此处的[示例。](https://github.com/liaisonjs/liaison/tree/master/examples/counter-via-http/src)

向远程调用的方法传递值/从远程调用的方法返回值怎么样？可以传递/返回任何可序列化的东西，包括类实例。只要一个类在前端和后端都用相同的名字注册，它的实例就可以被自动传输。

跨越前端和后端重写一个方法怎么样？这与普通的 JavaScript 没有什么不同——我们可以使用`super`。例如，我们可以覆盖`increment()`方法，在前端的上下文中运行额外的代码:

```
// frontend.js

class Counter extends BaseCounter {
  async increment() {
    await super.increment(); // Backend's `increment()` method is invoked
    console.log(this.value); // Additional code is executed in the frontend
  }
} 
```

现在，让我们用前面显示的 [React](https://reactjs.org/) 和封装方法构建一个用户界面:

```
// frontend.js

import React from 'react';
import {view} from '@liaison/react-integration';

class Counter extends BaseCounter {
  // We use the `@view()` decorator to observe the model and re-render the view when needed
  @view() View() {
    return (
      <div>
        {this.value} <button onClick={() => this.increment()}>+</button>
      </div>
    );
  }
} 
```

最后，为了显示计数器，我们只需要:

```
<counter.View /> 
```

瞧啊。我们构建了一个具有两个统一层和一个封装 UI 的单页面应用程序。

### 概念证明

为了试验统一架构，我用 Liaison 构建了一个[真实世界示例应用](https://github.com/liaisonjs/react-liaison-realworld-example-app)。

我可能有偏见，但结果对我来说看起来相当惊人:简单的实现，高度的代码内聚性，100% [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) ，没有粘合代码。

就代码量而言，我的实现比我研究过的任何其他实现都要轻得多。点击查看[结果。](https://github.com/liaisonjs/react-liaison-realworld-example-app/blob/master/docs/comparison.md)

当然，真实世界的例子是一个小应用程序，但是由于它涵盖了所有应用程序共有的最重要的概念，我相信统一的架构可以扩展到更大的应用程序。

## 结论

关注点分离、松散耦合、简单性、内聚性和敏捷性。

似乎我们终于明白了一切。

如果你是一个有经验的开发人员，我想你在这一点上会感到有点怀疑，这完全没问题。很难抛弃多年来形成的惯例。

如果面向对象编程不适合你，你就不会想使用联络，这也完全没问题。

但是如果你对 OOP 感兴趣，请在你的头脑中保留一个小窗口，下次你必须构建一个全栈应用时，尝试看看它如何适应统一的架构。

[联络](https://liaison.dev/)仍处于早期阶段，但我正在积极地工作，我预计在 2020 年初发布第一个测试版。

如果你感兴趣，请启动[库](https://github.com/liaisonjs/liaison)并通过关注[博客](https://liaison.dev/blog)或订阅[时事通讯](https://liaison.dev/#newsletter)保持更新。

*在[Changelog News](https://changelog.com/news/how-to-simplify-fullstack-development-with-a-unified-architecture-XVOM)T3 上讨论这篇文章。*