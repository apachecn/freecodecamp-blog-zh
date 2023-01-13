# 如何将应用程序分成三个主要层

> 原文：<https://www.freecodecamp.org/news/how-to-split-an-application-into-its-three-main-layers-fd18b11994a0/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

在应用程序中划分职责的一种常见方法是将它们分成三个主要层:

*   用户界面(又名演示)
*   领域(又名业务)
*   数据存取

这样做的一些好处是:

*   更好地理解图层的用途
*   层实现可以被替换。例如，我们可以在 UI 层的 AngularJS、VueJS 和 ReactJS 之间切换。
*   多个 UI 层可以在同一个域层上实现
*   用户界面可能很难测试。通过将逻辑转移到其他层，测试变得更加容易。

用 React 和 Redux 阅读 [****功能架构，学习如何以功能风格构建应用。****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y&source=post_page---------------------------) ****。****

你可以在[媒体](https://medium.com/@cristiansalcescu)和[推特](https://twitter.com/cristi_salcescu)上找到我。