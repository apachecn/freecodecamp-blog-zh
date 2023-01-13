# 如何用揭示意图的函数名使你的代码更好

> 原文：<https://www.freecodecamp.org/news/how-to-make-your-code-better-with-intention-revealing-function-names-6c8b5271693e/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

代码是与阅读它的开发人员交流的一种方式。名字透露意图的函数更容易阅读。我们读函数名，就能理解它的用途。函数名是我们表达一段代码意图的工具。

让我们看一个使用匿名函数以函数风格完成的[操作列表。](https://jsfiddle.net/cristi_salcescu/pujuve88/)

```
function getTodos(users){
  return todos
    .filter(todo => !todo.completed && todo.type === "RE")
    .map(todo => ({
      title : todo.title,
      userName : users[todo.userId].name
    }))
    .sort((todo1, todo2) =>  
      todo1.userName.localeCompare(todo2.userName));
}
```

现在检查使用带有意图揭示名称的函数重构的相同功能。

```
function isTopPriority(todo){
  return !todo.completed && todo.type === "RE";
}

function ascByUserName(todo1, todo2){
  return todo1.userName.localeCompare(todo2.userName);
}

function getTodos(users){
  function toViewModel(todo){
    return {
      title : todo.title,
      userName : users[todo.userId].name
    }
  }
  return todos.filter(isTopPriority)
              .map(toViewModel).sort(ascByUserName);
}
```

函数名使代码更加清晰。有了一个好的函数名，你只需要读这个名字——你不需要分析它的代码来理解它做什么。

> 人们普遍估计，开发人员将 70%的代码维护时间花在阅读和理解代码上。

> *Kyle Simpson in[Functional-Light JavaScript](https://www.amazon.com/Functional-Light-JavaScript-Pragmatic-Balanced-FP-ebook/dp/B0787DBFKH/ref=sr_1_1?ie=UTF8&qid=1519405569&sr=8-1&keywords=kyle+simpson+functional&dpID=41de4aNCSQL&preST=_SX342_QL70_&dpSrc=srch)*

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)