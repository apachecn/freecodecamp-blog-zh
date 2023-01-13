# 无点合成如何让你成为更好的函数式程序员

> 原文：<https://www.freecodecamp.org/news/how-point-free-composition-will-make-you-a-better-functional-programmer-33dcb910303a/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

> "无点风格——旨在通过移除不必要的参数映射来减少视觉混乱."——Kyle Simpson 在[Functional-Light JavaScript](https://www.amazon.com/Functional-Light-JavaScript-Pragmatic-Balanced-FP-ebook/dp/B0787DBFKH/ref=sr_1_1?ie=UTF8&qid=1519405569&sr=8-1&keywords=kyle+simpson+functional&dpID=41de4aNCSQL&preST=_SX342_QL70_&dpSrc=srch)

考虑下面的代码:

```
let newBooks = books.filter(point => isTechnology(point))
```

现在看一下剔除点(参数/自变量)后的相同代码:

```
let newBooks = books.filter(isTechnology)
```

### 列表操作中的无指针

让我们用无点方式来做列表操作。

假设我们需要在书籍列表中找到技术标题，准备包含视图所有信息的 book 对象，并按照作者姓名对书籍进行排序。

[这里的](https://jsfiddle.net/cristi_salcescu/j2mzyvau/)是代码的样子:

```
function getBooks(){
  return books.filter(isTechnology)
              .map(toBookView)
              .sort(ascByAuthor);
}

//Small functions with points
function isTechnology(book){
   return book.type === "T";
}

function toBookView(book){
  return Object.freeze({
    title : book.title,
    author : authors[book.authorID].name
  });
}

function ascByAuthor(book1, book2){
  if(book1.author < book2.author) return -1;
  if(book1.author > book2.author) return 1;
  return 0;
}
```

回调函数`isTechnology()`、`toBookView()`、`ascByAuthor()`都是名字透露意图的小函数。它们不是以无点的方式构建的。

在`getBooks()`中组装所有这些函数的代码是无指针的。

#### 分解和合成

我们处理问题的自然方式是把它分成更小的部分，然后再把所有的部分组合起来。

我们将较大的任务分解成几个完成较小任务的功能。然后我们重新组合这些更小的函数来解决最初的问题。

让我们再看一遍需求:

> 我们需要在书籍列表中找到技术标题，为视图准备包含所有信息的 book 对象，并按照作者姓名对书籍进行排序。

我们创造了:

*   `isTechnology()`判断这是否是一本科技书籍
*   `toViewBook()`用视图的所有信息构建一个对象
*   按作者姓名对两本书进行升序排序
*   将所有这些小功能以一种无点的方式组合在一起

```
function getBooks(){
  return books.filter(isTechnology)
              .map(toBookView)
              .sort(ascByAuthor);
}
```

### 走向无点构图的步骤

做无点构图的时候没有额外的匿名回调。没有`function`关键字，没有箭头语法`=&`gt；。我们看到的都是函数名。

*   在大多数情况下，提取出命名函数中的回调。
*   在简单的情况下，只需使用工具箱中的实用函数来动态创建回调。[以](#2428)中的`prop()`函数为例。
*   用无点风格写协调器函数。

#### 小功能

以这种方式编写代码的结果是许多小函数有意暴露名称。命名这些小函数需要时间，但如果做得好，会使代码更容易阅读。

将有两种功能:

*   完成一项任务的函数:它们是纯函数或闭包函数。通常它们不是以无点风格构建的，而是有好的名字。
*   协调大量任务的功能:将这些小任务以无要点的方式连接起来，这样更容易阅读。

#### 不是所有的事情都是没有点的

我的目标不是让一切都毫无意义。我的目标是在特定的地方没有点，尤其是在构造函数的时候。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)