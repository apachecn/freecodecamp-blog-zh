# 如何通过分解组合让复杂的问题变得简单

> 原文：<https://www.freecodecamp.org/news/how-to-make-complex-problems-easier-by-decomposing-and-composing-be57ce230c49/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

我们处理复杂事物的自然方式是把它分成更小的部分，然后再把所有的部分组合起来。

这是一个两步过程:

*   把问题分解成更小的部分
*   组合小部件来解决问题

我们分解成更小的部分，因为它们更容易理解和实现。较小的部分可以并行开发。

分解的过程就是分配责任，给名字。这使得谈论和推理变得容易。一旦我们确定了责任，我们就可以重用它。

构图是将小部分组合在一起，并在它们之间建立一种关系。我们决定这些部分通信的方式，它们执行的顺序，以及数据如何在它们之间流动。

我们发现一个系统很难理解，即使它被分割成更小的部分，如果这些部分之间有大量的关系。为了使一个系统更容易理解，我们需要尽量减少其各部分之间可能的联系。

### 对象分解

对象不仅仅是一起工作的状态和行为。对象是有责任的东西。

#### 分解

在[如何使用 React](https://medium.freecodecamp.org/how-to-create-a-three-layer-application-with-react-8621741baca0) 创建一个三层应用中，我采用了一个待办事项列表应用，并在以下对象之间划分职责:

*   `TodoDataService`:负责与服务器 Todo API 的通信
*   `UserDataService`:负责与服务器用户 API 的通信。
*   `TodoStore`:管理待办事项的域存储。这是关于待办事项的唯一来源。
*   `UserStore`:管理用户的域存储。
*   `TodoListContainer`:显示待办事项列表的根容器组件。

你可以看到，分解的时候，我分配责任，给名字。

#### 构成

接下来，我将它们组合在一个函数中。这是创建所有对象和注入依赖项的地方。叫做作文根。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)