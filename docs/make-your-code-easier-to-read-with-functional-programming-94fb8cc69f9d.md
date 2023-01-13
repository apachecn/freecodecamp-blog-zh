# 用函数式编程让你的代码更容易阅读

> 原文：<https://www.freecodecamp.org/news/make-your-code-easier-to-read-with-functional-programming-94fb8cc69f9d/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

纯函数更容易阅读和理解。函数的所有依赖项都在它的定义中，因此更容易看到。纯函数也往往很小，只做一件事。他们不使用`this`，这是一个经常引起混乱的来源。

### 链接

> **链接**是一种用于简化代码的技术，其中多个方法被一个接一个地应用于一个对象。

[让我们来看看并比较一下](https://jsfiddle.net/cristi_salcescu/5va5dkq7/)这两种风格:命令式和功能式。在函数式风格中，我使用基本工具箱进行列表操作`filter()`和`map()`。然后我把它们连在一起。

我举了一个任务集合的例子。一个任务有一个`id`，一个描述(`desc`)，一个布尔`completed`，一个`type`和一个分配的`user`对象。用户对象有一个`name`属性。

```
//Imperative style
let filteredTasks = [];
for(let i=0; i<tasks.length; i++){
    let task = tasks[i];
    if (task.type === "RE" && !task.completed) {
        filteredTasks.push({ ...task, userName: task.user.name });
    }
}

//Functional style
function isPriorityTask(task){
   return task.type === "RE" && !task.completed;
}

function toTaskView(task) {
   return { ...task, userName: task.user.name };
}

let filteredTasks = tasks.filter(isPriorityTask).map(toTaskView);
```

请注意，`filter()`和`map()`的回调作为**纯函数，意图显示名称。**

> `*map()*`使用映射函数将一个值列表转换为另一个值列表。

这里有一个[性能测试](https://jsperf.com/make-code-easier-to-read-imperative-vs-functional)来衡量两种风格之间的差异。似乎函数式方法要慢 60%。当命令性流程在 10 毫秒内完成时，功能性方法将在 16 毫秒内完成。[在这种情况下](https://jsfiddle.net/cristi_salcescu/v5jegr61/)使用命令式循环将是一个不成熟的优化。

### 无点风格

在前面的例子中，我在构造函数时使用了无点风格。自由点是一种通过消除不必要的参数来提高可读性的技术。考虑下一个代码:

```
tasks.filter(task => isPriorityTask(task)).map(task => toTaskView(task));
```

在无点风格中，它是无参数的:

```
tasks.filter(isPriorityTask).map(toTaskView);
```

更多关于无点的内容，请看[无点合成如何让你成为更好的函数式程序员](https://medium.freecodecamp.org/how-point-free-composition-will-make-you-a-better-functional-programmer-33dcb910303a)

### 部分应用

接下来，我想看看我们如何提高可读性并重用现有的函数。在此之前，我们需要在工具箱中添加一个新函数。

> **局部应用**指的是给一个函数固定多个参数的过程。

> 这是一种从一般化到特殊化的方法。

对于部分应用程序，我们可以使用一个流行库中的`partial()`函数，如[下划线. js](http://underscorejs.org/#partial) 或[罗达什. js](https://lodash.com/docs/4.17.5#partial) 。`bind()`方法也可以做局部应用。

假设我们想要将下面的命令式代码重构为功能性的、更易于阅读的样式:

```
let filteredTasks = [];
for(let i=0; i<tasks.length; i++){
    let task = tasks[i];
    if (task.type === "NC") {
        filteredTasks.push(task);
    }
}
```

正如我所说的，这一次我们想要创建一个通用函数，它可以用于任何任务类型的过滤。`isTaskOfType()`是通用函数。函数`partial()`用于创建一个新的谓词函数`isCreateNewContent()`，它根据特定的类型进行过滤。

> **谓词函数**是以一个值作为输入，根据该值是否满足条件返回 true/false 的函数。

```
function isTaskOfType(type, task){
  return task.type === type;
}

let isCreateNewContent = partial(isTaskOfType, "NC");
let filteredTasks = tasks.filter(isCreateNewContent);
```

注意谓词函数。它有一个表达其意图的名字。当我阅读`tasks.filter(isCreateNewContent)`时，我清楚地知道我选择的是哪种`tasks`。

> `filter()` 根据决定应该保留哪些值的谓词函数从列表中选择值。

### 减少

我将开始一个使用购物清单的新例子。该列表可能如下所示:

```
let shoppingList = [
   { name : "orange", units : 2, price : 10, type : "FRT"},
   { name : "lemon", units : 1, price : 15, type : "FRT"},
   { name : "fish", units : 0.5, price : 30, type : "MET"}
];
```

我将只计算总价和水果的价格。下面是命令式:

```
let totalPrice = 0, fruitsPrice = 0;
for(let i=0; i<shoppingList.length; i++){
   let line = shoppingList[i];
   totalPrice += line.units * line.price;
   if (line.type === "FRT") {
       fruitsPrice += line.units * line.price;
   }
}
```

在这种情况下，采用功能方法将需要使用`reduce()`来计算总价。

> `reduce()` 将一列值缩减为一个值。

正如我们之前所做的，我们为所需的回调创建新的函数，并给它们命名为`addPrice()`和`areFruits()`。

```
function addPrice(totalPrice, line){
   return totalPrice + (line.units * line.price);
}

function areFruits(line){
   return line.type === "FRT";
}

let totalPrice = shoppingList.reduce(addPrice,0);
let fruitsPrice = shoppingList.filter(areFruits).reduce(addPrice,0);
```

### 结论

纯函数更容易阅读和推理。

函数式编程将把列表操作分解成如下步骤:过滤、映射、归约、排序。同时，需要定义新的纯粹的小函数来支持这些操作。

将函数式编程与给出意图揭示名称的实践相结合，极大地提高了代码的可读性。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)