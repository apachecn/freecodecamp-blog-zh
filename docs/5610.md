# 让我们在 JavaScript 中试验函数生成器和管道操作符

> 原文：<https://www.freecodecamp.org/news/lets-experiment-with-functional-generators-and-the-pipeline-operator-in-javascript-520364f97448/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

> *生成器是一个函数，它在每次被调用时返回序列中的下一个值。*

将函数生成器与管道操作符和纯函数相结合，目的是揭示名称，允许以更具表达性的方式编写代码，而无需创建中间列表:

```
import { sequence, filter, map, take, toList } from "./sequence";

const filteredTodos =
  sequence(todos) 
  |> filter(isPriorityTodo) 
  |> map(toTodoView)
  |> take(10)  
  |> toList;
```

让我们看看怎么做。

我将从一个简单的函数生成器开始，它在每次被调用时给出下一个整数。从 0 开始。

```
function sequence() {
  let count = 0;
  return function() {
    const result = count;
    count += 1;
    return result;
  }
}

const nextNumber = sequence();
nextNumber(); //0
nextNumber(); //1
nextNumber(); //2
```

`nextNumber()`是一个无限大的生成器。`nextNumber()`也是一个闭包函数。

### 有限生成器

生成器可以是有限的。查看下一个例子，其中`sequence()`创建了一个生成器，返回特定间隔内的连续数字。在序列结束时，它返回`undefined`:

```
function sequence(from, to){
 let count = from;
 return function(){
   if(count< to){
      const result = count;
      count += 1;
      return result;
    }
  }
}

const nextNumber = sequence(10, 15);
nextNumber(); //10
nextNumber(); //12
nextNumber(); //13
nextNumber(); //14
nextNumber(); //undefined
```

### toList()函数

当使用生成器时，我们可能希望创建一个包含序列中所有值的列表。对于这种情况，我们需要一个新函数`toList()`,它接受一个生成器，并将序列中的所有值作为数组返回。序列应该是有限的。

```
function toList(sequence) {
  const arr = [];
  let value = sequence();
  while (value !== undefined) {
    arr.push(value);
    value = sequence();
  }
  return arr;
}
```

让我们使用它与先前的发电机。

```
const numbers = toList(sequence(10, 15));
//[10,11,12,13,14]
```

### 管道运营商

管道是一系列数据转换，其中一个转换的输出是下一个转换的输入。

管道操作符`|>`使我们能够以更有表现力的方式编写数据转换。管道操作符为带有单个参数的函数调用提供了语法上的好处。考虑下一个代码:

```
const shortText = shortenText(capitalize("this is a long text"));

function capitalize(text) {
  return text.charAt(0).toUpperCase() + text.slice(1);
}

function shortenText(text) {
  return text.substring(0, 8).trim();
}
```

使用管道操作符，转换可以写成这样:

```
const shortText = "this is a long text" 
  |> capitalize 
  |> shortenText;
  //This is
```

目前，管道运营商还处于试验阶段。你可以用巴别塔试试:

*   在`package.json`文件中添加巴别塔管道插件:

```
{
  "dependencies": {
    "@babel/plugin-syntax-pipeline-operator": "7.2.0"
  }
}
```

*   在`.babelrc`配置文件中添加:

```
{
  "plugins": [["@babel/plugin-proposal-pipeline-operator", {
             "proposal": "minimal" }]]
}
```

### 集合上的生成器

在[用函数式编程让你的代码更容易阅读](https://medium.freecodecamp.org/make-your-code-easier-to-read-with-functional-programming-94fb8cc69f9d)中，我有一个处理列表`todos`的例子。代码如下:

```
function isPriorityTodo(task) {
  return task.type === "RE" && !task.completed;
}

function toTodoView(task) {
  return Object.freeze({ id: task.id, desc: task.desc });
}

const filteredTodos = todos.filter(isPriorityTodo).map(toTodoView);
```

在这个例子中，`todos`列表经历了两次转换。首先创建一个过滤列表，然后创建包含映射值的第二个列表。

使用生成器，我们可以进行两种转换，并且只创建一个列表。为此，我们需要一个生成器`sequence()`来给出集合中的下一个值。

```
function sequence(list) {
  let index = 0;
  return function() {
    if (index < list.length) {
      const result = list[index];
      index += 1;
      return result;
    }
  };
}
```

#### 过滤器()和地图()

接下来，我们需要两个装饰器`filter()`和`map()`，它们与函数生成器一起工作。

`filter()`获取一个生成器并创建一个新的生成器，该生成器只返回满足谓词函数的序列中的值。

`map()`获取一个生成器并创建一个返回映射值的新生成器。

以下是实现方式:

```
function filter(predicate) {
  return function(sequence) {
    return function filteredSequence() {
      const value = sequence();
      if (value !== undefined) {
        if (predicate(value)) {
          return value;
        } else {
          return filteredSequence();
        }
      }
    };
  };
}

function map(mapping) {
  return function(sequence) {
    return function() {
      const value = sequence();
      if (value !== undefined) {
        return mapping(value);
      }
    };
  };
}
```

我希望将这些装饰器与管道操作符一起使用。因此，我没有创建带有两个参数的`filter(sequence, predicate){ }`，而是创建了它的一个简化版本，将会像这样使用:`filter(predicate)(sequence)`。这样，它可以很好地与管道运营商合作。

现在我们有了工具箱，它由`sequence`、`filter`、`map`和`toList`函数组成，用于处理集合上的生成器，我们可以把它们都放在一个模块中(`"./sequence"`)。有关如何使用此工具箱和管道运算符重写前面的代码，请参见下文:

```
import { sequence, filter, map, take, toList } from "./sequence";

const filteredTodos =
  sequence(todos) 
  |> filter(isPriorityTodo) 
  |> map(toTodoView) 
  |> toList;
```

这里有一个[性能测试](https://jsperf.com/functional-generators-vs-array-methods/1)，测量使用数组方法和使用函数生成器之间的差异。使用函数生成器的方法似乎要慢 15–20%。

#### 减少()

让我们再举一个例子，从购物清单中计算水果的价格。

```
function addPrice(totalPrice, line){
   return totalPrice + (line.units * line.price);
}

function areFruits(line){
   return line.type === "FRT";
}

let fruitsPrice = shoppingList.filter(areFruits).reduce(addPrice,0);
```

如您所见，它要求我们首先创建一个过滤列表，然后计算该列表的总数。让我们用函数生成器重写计算，避免创建过滤列表。

我们需要工具箱中的一个新函数:`reduce()`。它使用一个生成器，将序列简化为一个值。

```
function reduce(accumulator, startValue) {
  return function(sequence) {
    let result = startValue;
    let value = sequence();
    while (value !== undefined) {
      result = accumulator(result, value);
      value = sequence();
    }
    return result;
  };
}
```

`reduce()`已立即执行。

下面是用生成器重写的代码:

```
import { sequence, filter, reduce } from "./sequence";

const fruitsPrice = sequence(shoppingList) 
  |> filter(areFruits) 
  |> reduce(addPrice, 0);
```

#### 采取()

另一个常见的场景是只从序列中取出第一个`n`元素。对于这种情况，我们需要一个新的装饰器`take()`，它接收一个生成器并创建一个新的生成器，该生成器只返回序列中的第一个`n`元素。

```
function take(n) {
  return function(sequence) {
    let count = 0;
    return function() {
      if (count < n) {
        count += 1;
        return sequence();
      }
    };
  };
}
```

还是那句话，这是`take()`的咖喱版应该这么叫:`take(n)(sequence)`。

下面是如何在一个无限数列上使用`take()`:

```
import { sequence, toList, filter, take } from "./sequence";

function isEven(n) {
  return n % 2 === 0;
}

const first3EvenNumbers = sequence()  
  |> filter(isEven) 
  |> take(3) 
  |> toList;
  //[0, 2, 4]
```

我重新制作了之前的[性能测试](https://jsperf.com/functional-generators-vs-array-methods/4)，并使用`take()`只处理前 100 项。事实证明，带有函数生成器的版本要快得多(大约快 170 倍)。

```
let filteredTodos = todos
 .filter(isPriorityTodo)
 .slice(0, 100)
 .map(toTodoView);
//320 ops/sec

let filteredTodos =
const filteredTodos =
  sequence(todos) 
  |> filter(isPriorityTodo) 
  |> map(toTodoView)
  |> take(100)
  |> toList;
//54000 ops/sec
```

### 自定义生成器

我们可以创建任何自定义生成器，并将其与工具箱和管道操作符一起使用。让我们创建斐波那契自定义生成器:

```
function fibonacciSequence() {
  let a = 0;
  let b = 1;
  return function() {
    const aResult = a;
    a = b;
    b = aResult + b;
    return aResult;
  };
}

const fibonacci = fibonacciSequence();
fibonacci();
fibonacci();
fibonacci();
fibonacci();
fibonacci();

const firstNumbers = fibonacciSequence()  
  |> take(10) 
  |> toList;
  //[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

### 结论

管道运算符使数据转换更具表现力。

可以在有限或无限的值序列上创建函数生成器。

有了生成器，我们可以在每一步都不创建中间列表的情况下进行列表处理。

你可以在 [codesandbox](https://codesandbox.io/s/rj2r9mxl0n) 上查看所有的样本。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)