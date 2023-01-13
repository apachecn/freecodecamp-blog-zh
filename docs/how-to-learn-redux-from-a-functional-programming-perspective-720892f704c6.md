# 如何从函数式编程的角度学习 Redux

> 原文：<https://www.freecodecamp.org/news/how-to-learn-redux-from-a-functional-programming-perspective-720892f704c6/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

Redux 是一个状态容器，它提倡使用函数式编程来管理状态。

我认为 Redux 生态系统已经在一种架构模式中发展，这种模式给出了如何组织应用程序的最佳实践。

### 纯函数

给定相同的输入，纯函数产生相同的输出值。纯函数没有副作用。

纯函数不会使数据发生突变，所以问题是我们如何改变状态，同时使用纯函数。Redux 提出了一个解决方案:我们编写纯函数，并让库应用它们并进行状态更改。

应用程序确实发生了状态变化，但是这种变化被封装在 Redux 存储之后。

### 不变

不可变值是指一旦创建就不能更改的值。

状态值是不可变的，所以每次我们想要改变状态时，我们需要创建一个新的不可变值。

状态的值是不可变的，但是状态可以改变。使用库来管理不变的状态是没有意义的。我们可以使用普通对象来存储这种数据。

### 体系结构

Redux 建议我们将一个实际应用拆分成以下几个部分:

*   演示组件
*   动作创建者(又名同步动作创建者)
*   还原剂
*   异步动作创建者
*   API 实用程序/网关
*   选择器
*   容器组件

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)