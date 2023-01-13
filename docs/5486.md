# 如何使用纯函数创建商店

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-store-using-pure-functions-2c19b678552f/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

给定相同的输入，纯函数产生相同的输出值。它们没有副作用，更容易阅读、理解和测试。

鉴于所有这些，我想创建一个隐藏状态但同时使用纯函数的存储。

### 不变

纯函数不修改它们的输入。他们将输入值视为不可变的。

不可变值是指一旦创建就不能更改的值。

[Immutable.js](https://facebook.github.io/immutable-js/) 提供了类似`List`的不可变数据结构。不可变的数据结构将在每个动作中创建一个新的数据结构。

考虑下一个代码:

```
import { List } from "immutable";
const list = List();
const newList = list.push(1);
```

创建一个包含新元素的新列表。它不会修改现有列表。

`delete()`返回一个新的`List`，其中指定索引处的元素被移除。

`List`数据结构为以不可变的方式处理列表提供了一个很好的接口，所以我将使用它作为状态值。

### 故事

存储管理状态。

状态是可以改变的数据。该存储隐藏了状态数据，并提供了一组公共的方法来处理这些数据。

我想用`add()`、`remove()`和`getBy()`的方法创建一个书店。

我希望这些函数都是纯函数。

商店将使用两种纯功能:

*   读取和过滤状态的函数。我称他们为吸气剂。
*   将修改状态的函数。我称他们为 setters。

这两种函数都将状态作为它们的第一个参数。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)