# 下面是一些具有封装的实用 JavaScript 对象

> 原文：<https://www.freecodecamp.org/news/here-are-some-practical-javascript-objects-that-have-encapsulation-fc4c1a79c655/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

封装意味着信息隐藏。它是关于隐藏尽可能多的对象内部部分，并暴露最小的公共接口。

用 JavaScript 创建封装的最简单也是最优雅的方法是使用闭包。闭包可以被创建为具有私有状态的函数。当创建许多共享相同私有状态的闭包时，我们创建一个对象。

我将构建一些在应用程序中有用的对象:堆栈、队列、事件发射器和计时器。所有都将使用工厂函数构建。

我们开始吧。

### 堆

Stack 是一个数据结构，有两个主要操作:`push`用于向集合中添加元素，`pop`用于删除最近添加的元素。它根据后进先出(LIFO)原则添加和删除元素。

看下一个例子:

```
let stack = Stack();
stack.push(1);
stack.push(2);
stack.push(3);
stack.pop(); //3
stack.pop(); //2
```

让我们使用工厂函数来实现堆栈。

```
function Stack(){
  let list = [];

  function push(value){ list.push(value); }
  function pop(){ return list.pop(); }

  return Object.freeze({
    push,
    pop
  });
}
```

stack 对象有两个公共方法`push()`和`pop()`。内部状态只能通过这些方法来改变。

```
stack.list; //undefined
```

我不能直接修改内部状态:

```
stack.list = 0;//Cannot add property list, object is not extensible
```

您可以在[探索功能 JavaScript](https://www.amazon.com/dp/B07PBQJYYG) 一书中找到更多信息。

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)