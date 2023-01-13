# 这些是你应该知道的 ES6 的特性

> 原文：<https://www.freecodecamp.org/news/these-are-the-features-in-es6-that-you-should-know-1411194c71cb/>

ES6 为 JavaScript 语言带来了更多特性。一些新的语法允许您以更具表现力的方式编写代码，一些功能完善了函数式编程工具箱，而一些功能是有问题的。

### 让和 const

有两种方法可以声明一个变量(`let`和`const`)加上一个已经过时的变量(`var`)。

## 让

在当前作用域中声明并可选地初始化一个变量。当前作用域可以是模块、函数或块。未初始化的变量值为`undefined`。

范围定义了变量的生存期和可见性。变量在声明它们的范围之外是不可见的。

考虑下一个强调`let`块范围的代码:

```
let x = 1;
{ 
  let x = 2;
}
console.log(x); //1
```

相比之下，`var`声明没有块范围:

```
var x = 1;
{ 
  var x = 2;
}
console.log(x); //2
```

带有`let`声明的`for`循环语句为每次迭代创建一个新的局部变量。下一个循环在五个不同的`i`变量上创建了五个闭包。

```
(function run(){
  for(let i=0; i<5; i++){
    setTimeout(function log(){
      console.log(i); //0 1 2 3 4
    }, 100);
  }
})();
```

用`var`编写相同的代码将在相同的变量上创建五个闭包，因此所有闭包都将显示最后一个值`i`。

`log()`函数是一个闭包。关于闭包的更多信息，请看一下[在 JavaScript 中发现闭包的力量](https://medium.freecodecamp.org/discover-the-power-of-closures-in-javascript-5c472a7765d7)。

## 常数

`const`声明了一个不能被重新赋值的变量。只有当赋值不可变时，它才成为常数。

不可变值是指一旦创建就不能更改的值。原始值是不可变的，对象是可变的。

`const`冻结变量，`Object.freeze()`冻结对象。

`const`变量的初始化是强制性的。

## 模块

在模块之前，在任何函数之外声明的变量都是全局变量。

对于模块，在任何函数外声明的变量都是隐藏的，除非显式导出，否则其他模块无法使用。

导出使函数或对象对其他模块可用。在下一个例子中，我从不同的模块中导出函数:

```
//module "./TodoStore.js"
export default function TodoStore(){}

//module "./UserStore.js"
export default function UserStore(){}
```

导入使其他模块中的函数或对象可用于当前模块。

```
import TodoStore from "./TodoStore";
import UserStore from "./UserStore";

const todoStore = TodoStore();
const userStore = UserStore();
```

## 传播/休息

`…`操作符可以是 spread 操作符或 rest 参数，这取决于它的使用位置。考虑下一个例子:

```
const numbers = [1, 2, 3];
const arr = ['a', 'b', 'c', ...numbers];

console.log(arr);
["a", "b", "c", 1, 2, 3]
```

这是扩展运算符。现在看下一个例子:

```
function process(x,y, ...arr){
  console.log(arr)
}
process(1,2,3,4,5);
//[3, 4, 5]

function processArray(...arr){
  console.log(arr)
}
processArray(1,2,3,4,5);
//[1, 2, 3, 4, 5]
```

这是 rest 参数。

### 争论

使用 rest 参数，我们可以替换`arguments`伪参数。rest 参数是数组，`arguments`不是。

```
function addNumber(total, value){
  return total + value;
}

function sum(...args){
  return args.reduce(addNumber, 0);
}

sum(1,2,3); //6
```

### 克隆

spread 操作符使对象和数组的克隆变得更简单、更有表现力。

ES2018 中将提供对象扩散属性操作符。

```
const book = { title: "JavaScript: The Good Parts" };

//clone with Object.assign()
const clone = Object.assign({}, book);

//clone with spread operator
const clone = { ...book };

const arr = [1, 2 ,3];

//clone with slice
const cloneArr = arr.slice();

//clone with spread operator
const cloneArr = [ ...arr ];
```

### 串联

在下一个示例中，spread 运算符用于连接数组:

```
const part1 = [1, 2, 3];
const part2 = [4, 5, 6];

const arr = part1.concat(part2);

const arr = [...part1, ...part2];
```

### 合并对象

像`Object.assign()`一样，扩展操作符可用于将一个或多个对象的属性复制到一个空对象，并组合它们的属性。

```
const authorGateway = { 
  getAuthors : function() {},
  editAuthor: function() {}
};

const bookGateway = { 
  getBooks : function() {},
  editBook: function() {}
};

//copy with Object.assign()
const gateway = Object.assign({},
      authorGateway, 
      bookGateway);

//copy with spread operator
const gateway = {
   ...authorGateway,
   ...bookGateway
};
```

## 财产短缺

考虑下一个代码:

```
function BookGateway(){
  function getBooks() {}
  function editBook() {}

  return {
    getBooks: getBooks,
    editBook: editBook
  }
}
```

对于属性缩写，当属性名和用作值的变量名相同时，我们可以只写一次键。

```
function BookGateway(){
  function getBooks() {}
  function editBook() {}

  return {
    getBooks,
    editBook
  }
}
```

这是另一个例子:

```
const todoStore = TodoStore();
const userStore = UserStore();

const stores = {
  todoStore,
  userStore
};
```

## 解构分配

考虑下一个代码:

```
function TodoStore(args){
  const helper = args.helper;
  const dataAccess = args.dataAccess;
  const userStore = args.userStore;
}
```

使用析构赋值语法，可以这样写:

```
function TodoStore(args){
   const { 
      helper, 
      dataAccess, 
      userStore } = args;
}
```

或者更好，在参数列表中使用析构语法:

```
function TodoStore({ helper, dataAccess, userStore }){}
```

下面是函数调用:

```
TodoStore({ 
  helper: {}, 
  dataAccess: {}, 
  userStore: {} 
});
```

## 默认参数

函数可以有默认参数。看下一个例子:

```
function log(message, mode = "Info"){
  console.log(mode + ": " + message);
}

log("An info");
//Info: An info

log("An error", "Error");
//Error: An error
```

## 模板字符串文字

模板字符串用`` ` ``字符定义。使用模板字符串，前面的日志记录消息可以写成这样:

```
function log(message, mode= "Info"){
  console.log(`${mode}: ${message}`);
}
```

模板字符串可以在多行中定义。然而，更好的选择是将长文本消息作为资源保存，例如保存在数据库中。

下面是一个生成跨多行 HTML 的函数:

```
function createTodoItemHtml(todo){
  return `<li>
    <div>${todo.title}</div>
    <div>${todo.userName}</div>
  </li>`;
}
```

## 适当的尾音

> **当递归调用是函数做的最后一件事时，递归函数是尾递归的。**

尾部递归函数的性能优于非尾部递归函数。优化的尾部递归调用不会为每个函数调用创建新的堆栈框架，而是使用单个堆栈框架。

ES6 带来了严格模式下的尾调用优化。

[下面的函数](https://jsfiddle.net/cristi_salcescu/4t2j3uho/)应该受益于尾部调用优化。

```
function print(from, to) 
{ 
  const n = from;
  if (n > to)  return;

  console.log(n);

  //the last statement is the recursive call 
  print(n + 1, to); 
}

print(1, 10);
```

注意:主流浏览器还不支持尾调用优化。

## 承诺

**承诺是对异步调用的引用。它可能会在未来的某个地方解决或失败。**

承诺更容易结合。[正如你在下一个例子](https://jsfiddle.net/cristi_salcescu/eqyhq2e3/)中看到的，当所有承诺都被解析时，或者当第一个承诺被解析时，很容易调用一个函数。

```
function getTodos() { return fetch("/todos"); }
function getUsers() { return fetch("/users"); }
function getAlbums(){ return fetch("/albums"); }

const getPromises = [
  getTodos(), 
  getUsers(), 
  getAlbums()
];

Promise.all(getPromises).then(doSomethingWhenAll);
Promise.race(getPromises).then(doSomethingWhenOne);

function doSomethingWhenAll(){}
function doSomethingWhenOne(){}
```

`fetch()`函数是 Fetch API 的一部分，它返回一个承诺。

`Promise.all()`返回一个承诺，当所有输入承诺都已解决时，该承诺也将解决。`Promise.race()`当输入承诺之一解决或拒绝时，返回解决或拒绝的承诺。

承诺可以处于三种状态之一:待定、已解决或已拒绝。该承诺将处于待定状态，直到被解决或拒绝。

Promises 支持一个链接系统，允许您通过一组函数传递数据。[在下一个例子](https://jsfiddle.net/cristi_salcescu/kgxnay46/)中，`getTodos()`的结果作为输入传递给`toJson()`，然后其结果作为输入传递给`getTopPriority()`，然后其结果作为输入传递给`renderTodos()`函数。当一个错误被抛出或者一个承诺被拒绝时,`handleError`被调用。

```
getTodos()
  .then(toJson)
  .then(getTopPriority)
  .then(renderTodos)
  .catch(handleError);

function toJson(response){}
function getTopPriority(todos){}
function renderTodos(todos){}
function handleError(error){}
```

在前面的例子中，`.then()`处理成功场景，`.catch()`处理错误场景。如果任何一步出现错误，链控制会跳到链中最近的拒绝处理器。

`Promise.resolve()`返回已解决的承诺。`Promise.reject()`退回被拒绝的承诺。

## 班级

类是用自定义原型创建对象的糖语法。它有一个比前一个更好的语法，函数构造器。[看看下一个例子](https://jsfiddle.net/cristi_salcescu/aLg8t632/):

```
class Service {
  doSomething(){ console.log("doSomething"); }
}

let service = new Service();
console.log(service.__proto__ === Service.prototype);
```

在`Service`类中定义的所有方法都将被添加到`Service.prototype`对象中。`Service`类的实例将拥有相同的原型(`Service.prototype`)对象。所有实例都将方法调用委托给`Service.prototype`对象。方法在`Service.prototype`上定义一次，然后被所有实例继承。

### 遗产

“类可以从其他类继承”。下面是一个继承的[示例，其中`SpecialService`类从`Service`类“继承”:](https://jsfiddle.net/cristi_salcescu/1xo96yt8/)

```
class Service {
  doSomething(){ console.log("doSomething"); }
}

class SpecialService extends Service {
  doSomethingElse(){ console.log("doSomethingElse"); }  
}

let specialService = new SpecialService();
specialService.doSomething();
specialService.doSomethingElse();
```

在`SpecialService`类中定义的所有方法都将被添加到`SpecialService.prototype`对象中。所有实例都将方法调用委托给`SpecialService.prototype`对象。如果在`SpecialService.prototype`中没有找到该方法，将在`Service.prototype`对象中进行搜索。如果仍未找到，将在`Object.prototype`中搜索。

### 类可能成为一个不好的特征

即使它们看起来是封装的，类的所有成员都是公共的。您仍然需要管理`this`丢失上下文的问题。公共 API 是可变的。

如果你忽略了 JavaScript 的功能性，它可能会成为一个不好的特性。当 JavaScript 既是函数式编程语言又是基于原型的语言时，可能会给人一种基于类的语言的印象。

封装的对象可以用工厂函数创建。考虑下一个例子:

```
function Service() {
  function doSomething(){ console.log("doSomething"); }

  return Object.freeze({
     doSomething
  });
}
```

默认情况下，这一次所有成员都是私有的。公共 API 是不可变的。没有必要管理`this`丢失上下文的问题。

如果组件框架需要，可以将`class`用作例外。React 就是这种情况，但 React 钩子不再是这种情况了。

关于为什么偏爱工厂函数的更多信息，请看一下[类与工厂函数:探索前进的道路](https://medium.freecodecamp.org/class-vs-factory-function-exploring-the-way-forward-73258b6a8d15)。

### 箭头功能

箭头函数可以动态创建匿名函数。它们可以用较短的语法创建小型回调。

让我们收集一些待办事项。待办事项有一个`id`、一个`title`和一个`completed`布尔属性。现在，考虑下一个只从集合中选择`title`的代码:

```
const titles = todos.map(todo => todo.title);
```

或者下一个例子只选择未完成的`todos`:

```
const filteredTodos = todos.filter(todo => !todo.completed);
```

### 这

箭头函数没有自己的`this`和`arguments`。因此，您可能会看到箭头函数用于修复`this`丢失上下文的问题。我认为避免这个问题的最好方法是根本不使用`this`。

### 箭头功能可能会成为一个不好的特性

当使用箭头函数损害命名函数时，它可能会变成一个不好的特性。这将产生可读性和可维护性问题。看下一个只用匿名箭头函数编写的代码:

```
const newTodos = todos.filter(todo => 
       !todo.completed && todo.type === "RE")
    .map(todo => ({
       title : todo.title,
       userName : users[todo.userId].name
    }))
    .sort((todo1, todo2) =>  
      todo1.userName.localeCompare(todo2.userName));
```

[现在，看看同样的逻辑](https://jsfiddle.net/cristi_salcescu/pm7n2ab5/)被重构为纯粹的函数，目的是揭示名字，并决定哪一个更容易理解:

```
const newTodos = todos.filter(isTopPriority)
  .map(partial(toTodoView, users))
  .sort(ascByUserName);

function isTopPriority(todo){
  return !todo.completed && todo.type === "RE";
}

function toTodoView(users, todo){
  return {
    title : todo.title,
    userName : users[todo.userId].name
  }
}

function ascByUserName(todo1, todo2){
  return todo1.userName.localeCompare(todo2.userName);
}
```

更有甚者，匿名箭头函数会在调用栈中以`(anonymous)`的形式出现。

关于为什么偏爱命名函数的更多信息，请看一下[如何用揭示意图的函数名](https://medium.freecodecamp.org/how-to-make-your-code-better-with-intention-revealing-function-names-6c8b5271693e)使你的代码更好。

更少的代码不一定意味着更可读。[看下一个例子](https://jsfiddle.net/cristi_salcescu/wc8be2gn/)，看看哪个版本对你来说更容易理解:

```
//with arrow function
const prop = key => obj => obj[key];

//with function keyword
function prop(key){
   return function(obj){
      return obj[key];
   }
}
```

返回对象时要注意。在下一个例子中，`getSampleTodo()`返回`undefined`。

```
const getSampleTodo = () => { title : "A sample todo" };

getSampleTodo();
//undefined
```

## 发电机

我认为 ES6 生成器是一个不必要的特性，它使得代码更加复杂。

ES6 生成器创建一个具有`next()`方法的对象。`next()`方法创建一个具有`value`属性的对象。ES6 发电机促进了回路的使用。[看看下面的代码](https://jsfiddle.net/cristi_salcescu/edq7vfwm/):

```
function* sequence(){
  let count = 0;
  while(true) {
    count += 1;
    yield count;
  }
}

const generator = sequence();
generator.next().value;//1
generator.next().value;//2
generator.next().value;//3
```

相同的生成器可以简单地用闭包来实现。

```
function sequence(){
  let count = 0;
  return function(){
    count += 1;
    return count;
  }
}

const generator = sequence();
generator();//1
generator();//2
generator();//3
```

关于函数生成器的更多例子，请看[让我们在 JavaScript](https://medium.freecodecamp.org/lets-experiment-with-functional-generators-and-the-pipeline-operator-in-javascript-520364f97448) 中试验函数生成器和管道操作符。

# 结论

`let`和`const`声明并初始化变量。

模块封装功能，只暴露一小部分。

spread 操作符、rest 参数和属性简写使事情更容易表达。

承诺和尾部递归完成了函数式编程工具箱。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)