# 如何使用具有工厂功能的装饰器

> 原文：<https://www.freecodecamp.org/news/how-to-use-decorators-with-factory-functions-373fb972b6d4/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

方法装饰器是重用公共逻辑的工具。它们是面向对象编程的补充。装饰者封装了由不同对象分担的责任。

[考虑下面的代码](https://jsfiddle.net/cristi_salcescu/0tv3y06p/):

```
function TodoStore(currentUser){
  let todos = [];

  function add(todo){
    let start = Date.now();
    if(currentUser.isAuthenticated()){
      todos.push(todo);
    } else {
      throw "Not authorized to perform this operation";
    }

    let duration = Date.now() - start;
    console.log("add() duration : " + duration);
  }

  return Object.freeze({
    add
  });  
}
```

`add()`方法的目的是给内部状态添加新的待办事项。除此之外，该方法需要检查用户授权并记录执行持续时间。这两件事是次要的，实际上可以在其他方法中重复。

想象一下，我们可以将这些次要职责封装在函数中。那么我们可以按照下面的方式编写代码:

```
function TodoStore(){
  let todos = [];

  function add(todo){
    todos.push(todo);
  }

  return Object.freeze({
     add:compose(logDuration,authorize)(add) 
  }); 
}
```

现在，`add()`方法只是将`todo`添加到列表中。其他责任通过修饰方法来实现。

`logDuration()`和`authorize()`是装修工。

> 一个**函数装饰器**是一个高阶函数，它将一个函数作为自变量，返回另一个函数，返回的函数是自变量函数的变体。

> 雷金纳德·布莱斯维特在 [Javascript Allongé](https://leanpub.com/javascript-allonge/read#decorators)

### 日志持续时间

一个常见的场景是记录方法调用的持续时间。下面的装饰器记录了同步调用的持续时间。

```
function logDuration(fn){
  return function decorator(...args){
    let start = Date.now();
    let result = fn.apply(this, args);
    let duration = Date.now() - start;
    console.log(fn.name + "() duration : " + duration);
    return result;
  }
}
```

注意原始函数是如何被调用的——通过传入当前值`this`和所有参数:`fn.apply(this, args)`。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)