# 反应式编程主题介绍

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-subjects-in-reactive-programming-bbdc8fed7b6/>

主题是一种“特殊”类型的可观察对象，它允许我们向多个订阅者传播值。受试者很酷的一点是，它提供了实时反应。

例如，如果我们有一个有 10 个订阅者的主题，每当我们向主题推送值时，我们可以看到每个订阅者获取的值

这带来了一些挑战；如果我们推行一些价值观，然后订阅，或者反过来呢？时间起着重要的作用，如果我们订阅晚了，我们将无法访问这些值，就像有人在 30 分钟后进入电视直播体育赛事一样。

幸运的是，我们有 4 种类型的主题允许我们进行“时间旅行”,即使我们订阅晚了或没有订阅，我们也可以在其中访问值。

#### 我们将涉及的主题:

1.  什么是有实际例子的主题
2.  行为主题:获取最后一条消息
3.  重播主题:时间旅行
4.  AsyncSubject:一旦完成，获取最后一条消息

### 1.什么是主题？

如前所述，一个主体只不过是多了几个特征的可观察对象。根据定义，一个可观察对象是一个[可调用的集合](https://rxjs.dev/guide/overview)，一旦被订阅就发出数据。同时，主题是我们控制“何时向多个订阅者发出数据”的状态。

Subject 允许我们在外部调用像`.next()`、`.complete()`和`.error()`这样的方法，而在 observable 中，我们调用这些方法作为回调。

```
// Creating an Observable
const observable = new Observable((observer) => {
    observer.next(10);
    observer.next(5);
    observer.complete();
});

// Creating a Subject
const subject = new Subject();
subject.next(10);
subject.next(5);
subject.complete();
```

#### 实际例子:让我们用一个主题建立一个简单的聊天组

假设我们正在构建一个简单的聊天应用程序，人们可以在其中向聊天组发布消息。第一步是创建 Subject 的实例，然后将其分配给一个`chatGroup`。

```
// Create subject "Observable"
const chatGroup = new Subject();
```

现在我们的聊天组(主题)已经创建好了，接下来要做的就是添加消息。让我们创建一个两个朋友之间的典型对话。

```
// Push values to the stream
chatGroup.next('David - Hi, which hot series do you recommend?');
chatGroup.next('Peter - Game of Thrones, Bodyguard or Narcos are few of the good ones');
chatGroup.next('David - Interesting, which one is the hottest?');
chatGroup.next('Peter - Game of Thrones!');
```

到目前为止还不错——现在我们的聊天组里已经发布了 4 条消息，那么如果我们订阅了会发生什么呢？或者假设一个名叫约翰的新朋友想要加入对话。他能看到以前的信息吗？

```
// Print messages
chatGroup.subscribe((messages) => {
    console.log(messages)
})
```

不幸的是，John 错过了对话，因为他订阅晚了。这是反应式编程如何工作的一个完美例子——值随时间传递的思想，因此，我们必须在正确的时间订阅以访问值。

为了进一步阐述前面的例子，如果约翰在对话中插话会怎样？

```
// Push values to the stream
chatGroup.next('David - Hi, which hot series do you recommend?');
chatGroup.next('Peter - Game of Thrones, Bodyguard or Narcos are few of the good ones');

// John enters the conversation 
chatGroup.subscribe((messages) => {
    console.log(messages)
});

chatGroup.next('David - Interesting, which one is the hottest?');
chatGroup.next('Peter - Game of Thrones!');

// OUTPUT
// David - Interesting, which one is the hottest?
// Peter - Game of Thrones!
```

一旦约翰订阅了，他就会看到最后两条消息。主体在做它想做的事。但是，如果我们希望 John 查看所有消息，或者只查看最后一条消息，或者在有新消息发布时得到通知，该怎么办呢？

总的来说，这些主题大多相似，但每一个都提供了一些额外的功能，让我们逐一描述它们。

### 2.行为主题:获取最后一条消息

BehaviorSubject 类似于 Subject，只是它需要一个初始值作为参数来标记数据流的起点。原因是因为当我们订阅时，它返回最后一条消息。当处理数组时，这是类似的概念；在这里我们做`array.length-1`来获得最新的值。

```
import {BehaviorSubject } from "rxjs";

// Create a Subject
const chatGroup = new BehaviorSubject('Starting point');

// Push values to the data stream
chatGroup.next('David - Hi, which hot series do you recommend?');
chatGroup.next('Peter - Game of Thrones, Bodyguard or Narcos are few of the good ones');
chatGroup.next('David - Interesting, which one is the hottest?');
chatGroup.next('Peter - Game of Thrones!');

// John enters the conversation
chatGroup.subscribe((messages) => {
    console.log(messages)
})

// OUTPUT
// Peter - Game of Thrones!
```

### 3.重播主题:时间旅行

ReplaySubject，顾名思义，一旦订阅，它就广播所有消息，不管我们是否延迟订阅。这就像时间旅行，我们可以访问所有被广播的值。

```
 import { ReplaySubject } from "rxjs";

// Create a Subject
const chatGroup = new ReplaySubject();

// Push values to the data stream
chatGroup.next('David - Hi, which hot series do you recommend?');
chatGroup.next('Peter - Game of Thrones, Bodyguard or Narcos are few of the good ones');
chatGroup.next('David - Interesting, which one is the hottest?');
chatGroup.next('Peter - Game of Thrones!');

// John enters the conversation
chatGroup.subscribe((messages) => {
    console.log(messages)
})

// OUTPUT
// David - Hi, which hot series do you recommend?'
// Peter - Game of Thrones, Bodyguard or Narcos are few of the good ones'
// David - Interesting, which one is the hottest?'
// Peter - Game of Thrones!'
```

### 4.AsyncSubject:完成后，获取最后一条消息

AsyncSubject 类似于 BehaviorSubject，一旦订阅就发出最后一个值。唯一的区别是它需要一个`complete()`方法来将流标记为完成。一旦完成，就发出最后一个值。

```
import { AsyncSubject } from "rxjs";

// Create a Subject
const chatGroup = new AsyncSubject();

// Push values to the data stream
chatGroup.next('David - Hi, which hot series do you recommend?');
chatGroup.next('Peter - Game of Thrones, Bodyguard or Narcos are few of the good ones');
chatGroup.next('David - Interesting, which one is the hottest?');
chatGroup.next('Peter - Game of Thrones!');

chatGroup.complete(); // <-- Mark the stream as completed

// John enters the conversation
chatGroup.subscribe((messages) => {
    console.log(messages)
})

// OUTPUT
// Peter - Game of Thrones!'
```

### 摘要

回到我们之前关于 John 的例子，我们现在可以决定是希望 John 访问整个对话(ReplaySubject)、最后一条消息(BehaviorSubject)还是对话完成后的最后一条消息(AsyncSubject)。

如果你曾经纠结于确定一个主语是否是正确的方式，戴夫·西斯顿的文章“[使用主语还是不使用主语](http://davesexton.com/blog/post/To-Use-Subject-Or-Not-To-Use-Subject.aspx)”描述了何时使用主语的两个标准:

1.  只有当一个人想把一个冷的可观测值转换成热的可观测值时。
2.  **生成**持续传递数据的热可观察对象。

简而言之，只有创造力限制了反应式编程的潜在用途。在某些场景中，Observables 会做大部分繁重的工作，但是理解什么是主题，存在什么类型的主题，肯定会提高你的反应式编程技能。

如果你有兴趣了解更多关于网络生态系统的知识，这里有几篇我写的文章，请欣赏。

*   【Angular 和 React 之间的比较
*   [ES6 模块实用指南](https://medium.freecodecamp.org/how-to-use-es6-modules-and-why-theyre-important-a9b20b480773)
*   [如何使用获取 API 执行 HTTP 请求](http://A practical ES6 guide on how to perform HTTP requests using the Fetch API)
*   [需要学习的重要网络概念](https://medium.freecodecamp.org/learn-these-core-javascript-concepts-in-just-a-few-minutes-f7a16f42c1b0?gi=6274e9c4d599)
*   [用这些 JavaScript 方法提升你的技能](https://medium.freecodecamp.org/7-javascript-methods-that-will-boost-your-skills-in-less-than-8-minutes-4cc4c3dca03f)
*   [创建自定义 bash 命令](https://codeburst.io/learn-how-to-create-custom-bash-commands-in-less-than-4-minutes-6d4ceadd9590)

你可以在我每周发表文章的媒体上找到我。或者你可以在 Twitter 上关注我，我会在那里发布相关的 web 开发技巧和诀窍。