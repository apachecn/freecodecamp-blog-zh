# 类与工厂功能:探索前进的道路

> 原文：<https://www.freecodecamp.org/news/class-vs-factory-function-exploring-the-way-forward-73258b6a8d15/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

ECMAScript 2015(又名 ES6)附带了`class`语法，所以现在我们有两种创建对象的竞争模式。为了比较它们，我将创建相同的对象定义(TodoModel)作为类，，然后作为工厂函数。

**[TodoModel 为一类](https://jsfiddle.net/cristi_salcescu/m9dhpzfx/)**

```
class TodoModel {
    constructor(){
        this.todos = [];
        this.lastChange = null;
    }

    addToPrivateList(){
        console.log("addToPrivateList"); 
    }
    add() { console.log("add"); }
    reload(){}
}
```

**[【todo model】作为工厂功能](https://jsfiddle.net/cristi_salcescu/bcta6yyv/)**

```
function TodoModel(){
    var todos = [];
    var lastChange = null;

    function addToPrivateList(){
        console.log("addToPrivateList"); 
    }
    function add() { console.log("add"); }
    function reload(){}

    return Object.freeze({
        add,
        reload
    });
}
```

### 包装

我们注意到的第一件事是，一个类对象的所有成员、字段和方法都是公共的。

```
var todoModel = new TodoModel();
console.log(todoModel.todos);     //[]
console.log(todoModel.lastChange) //null
todoModel.addToPrivateList();     //addToPrivateList
```

缺乏封装可能会产生安全问题。以可以直接从开发人员控制台修改的全局对象为例。

当使用工厂函数时，只有我们公开的方法是公共的，其他的都是封装的。

```
var todoModel = TodoModel();
console.log(todoModel.todos);     //undefined
console.log(todoModel.lastChange) //undefined
todoModel.addToPrivateList();     //taskModel.addToPrivateList
                                    is not a function
```

### 这

使用类时，丢失上下文的问题仍然存在。例如，`this`正在嵌套函数中丢失上下文。这不仅在编码过程中令人讨厌，而且也是 bug 的持续来源。

```
class TodoModel {
    constructor(){
        this.todos = [];
    }

    reload(){ 
        setTimeout(function log() { 
           console.log(this.todos);    //undefined
        }, 0);
    }
}
todoModel.reload();                   //undefined
```

或者`this`在方法被用作回调时会丢失上下文，比如在 DOM 事件上。

```
$("#btn").click(todoModel.reload);    //undefined
```

当使用工厂函数时就没有这样的问题，因为它根本不使用`this`。

```
function TodoModel(){
    var todos = [];

    function reload(){ 
        setTimeout(function log() { 
           console.log(todos);        //[]
       }, 0);
    }
}
todoModel.reload();                   //[]
$("#btn").click(todoModel.reload);    //[]
```

#### 这个和箭头功能

arrow 函数部分解决了`this`类中松散的上下文问题，但同时也产生了一个新问题:

*   `this`不再丢失嵌套函数中的上下文
*   当方法用作回调时，`this`正在丢失上下文
*   arrow 函数促进了匿名函数的使用

[我使用箭头函数](https://jsfiddle.net/cristi_salcescu/y0k18og2/)重构了`TodoModel`。值得注意的是，在重构 arrow 函数的过程中，我们可能会丢失一些对可读性非常重要的东西，即函数名。[以](https://jsfiddle.net/cristi_salcescu/y0k18og2/)为例:

```
//using function name to express intent
setTimeout(function renderTodosForReview() { 
      /* code */ 
}, 0);

//versus using an anonymous function
setTimeout(() => { 
      /* code */ 
}, 0);
```

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)