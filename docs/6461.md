# 如何在 JavaScript 中用工厂函数构建可靠的对象

> 原文：<https://www.freecodecamp.org/news/how-to-build-reliable-objects-with-factory-functions-in-javascript-9ec1c089ea6f/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

我建议考虑用 JavaScript 构建可靠对象的这些想法:

*   将对象一分为二:数据对象和行为对象
*   使数据对象不可变
*   在行为对象中公开行为和隐藏数据
*   构建可测试的行为对象

### 数据与行为对象

实际上，应用程序中有两种对象:

*   **数据对象—** 公开数据
*   **行为对象—** 暴露行为，隐藏数据

#### 数据对象

数据对象公开数据。它们用于在应用程序内部构建和传输数据。

让我们以待办事项列表应用程序为例。

从服务器获取的待办事项数据对象可能是这样的:

```
{ id: 1, title: "This is a title", userId: 10, completed: false }
```

这是用于在视图中显示信息的数据对象的外观:

```
{ id: 1, title: "This is a title", userName: "Cristi", completed: false };
```

如您所见，两个对象都只包含数据。它们之间有一点不同:视图的数据对象有`userName`而不是`userId`。

数据对象是普通对象，通常用对象文字构建。

#### 行为对象

行为对象公开方法并隐藏数据。

行为对象作用于数据对象。它们可以将数据对象作为输入或返回数据对象。

我将以`TodoStore`对象为例。对象的职责是存储和管理待办事项列表。它使用`dataService`对象与服务器同步。

用 React 和 Redux 阅读 [****功能架构，学习如何以功能风格构建应用。****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y&source=post_page---------------------------) ****。****

你可以在[媒体](https://medium.com/@cristiansalcescu)和[推特](https://twitter.com/cristi_salcescu)上找到我。