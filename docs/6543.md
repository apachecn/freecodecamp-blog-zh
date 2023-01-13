# 让我们探索 JavaScript 中的对象

> 原文：<https://www.freecodecamp.org/news/lets-explore-objects-in-javascript-4a4ad76af798/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

对象是属性的动态集合，对于对象的原型具有“隐藏”属性。

属性有一个键和值。

### 属性键

属性键是唯一的字符串。

有两种方法可以访问属性:点符号和括号符号。当使用点符号时，属性键必须是有效的标识符。

```
let obj = {  message : "A message"}
```

```
obj.message //"A message"obj["message"] //"A message"
```

访问不存在的属性不会抛出错误，但会返回`undefined`。

```
obj.otherProperty //undefined
```

JavaScript 像对待对象一样对待原语、对象和函数。

对象本质上是动态的，可以用作地图。

对象从其他对象继承。构造函数和类是创建从其他原型对象继承的对象的糖语法。

`Object.create()`可用于单继承，`Object.assign()`可用于多继承。

工厂函数可以构建封装的对象。

用 React 和 Redux 阅读 [****功能架构，学习如何以功能风格构建应用。****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y&source=post_page---------------------------) ****。****

你可以在[媒体](https://medium.com/@cristiansalcescu)和[推特](https://twitter.com/cristi_salcescu)上找到我。