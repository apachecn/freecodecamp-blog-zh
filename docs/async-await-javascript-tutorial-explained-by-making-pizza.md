# JavaScript 中的 Async 和 Await 通过制作披萨来解释

> 原文：<https://www.freecodecamp.org/news/async-await-javascript-tutorial-explained-by-making-pizza/>

异步和等待听起来可能很复杂...但是一旦你开始吃，它们就像披萨饼一样简单。

我们在日常生活中都使用异步和等待。

## 什么是异步任务？

异步任务允许您在异步任务仍接近完成时完成其他任务。

### 以下是一些日常异步任务示例

#### 示例 1:

您在“得来速”点餐，这将启动您的点餐请求(异步任务)。

当你的食物准备好的时候，你在汽车餐厅排队等候(下一个任务)。

你不必等你的食物准备好了再往前拉。

您正在等待您的食物，您的请求在提货窗口得到满足。

#### 示例 2:

你在厨房拖地板。

当你等待厨房地板变干时，你用吸尘器清扫卧室的地毯。

最初的任务是清洁你的厨房地板，当地板干燥时，任务就完成了。

站在那里等着地板变干效率不高，所以你在厨房地板变干的时候用吸尘器打扫了卧室地板。

这也是 Javascript 处理异步函数的方式。

### 异步/等待示例–烘焙冷冻披萨

你决定在你的烤箱里烤一个比萨饼，你的第一步是预热烤箱。

所以你设置好想要的温度，开始预热烤箱。

当烤箱预热时，你把冷冻披萨从冰箱里拿出来，打开盒子，放在披萨盘上。

你还有时间！

当你等待烤箱准备好发出哔哔声的时候，可以拿一杯饮料，看会儿电视。

下面是模拟这个例子的一些代码:

```
// This async function simulates the oven response
const ovenReady = async () => {
  return new Promise(resolve => setTimeout(() => {
    resolve('Beep! Oven preheated!')
  }, 3000));
}

// Define preheatOven async function
const preheatOven = async () => {
  console.log('Preheating oven.');
  const response = await ovenReady();
  console.log(response);
}

// Define the other functions
const getFrozenPizza = () => console.log('Getting pizza.');
const openFrozenPizza = () => console.log('Opening pizza.');
const getPizzaPan = () => console.log('Getting pan.');
const placeFrozenPizzaOnPan = () => console.log('Putting pizza on pan.');
const grabABeverage = () => console.log('Grabbing a beverage.');
const watchTV = () => console.log('Watching television.');

// Now call the functions
preheatOven();
getFrozenPizza();
openFrozenPizza();
getPizzaPan();
placeFrozenPizzaOnPan();
grabABeverage();
watchTV();

// Output sequence in console:
Preheating oven.
Getting pizza.
Opening pizza.
Getting pan.
Putting pizza on pan.
Grabbing a beverage.
Watching television.
Beep! Oven preheated!
```

上面的过程正是 async 和 await 的意义所在。

当我们`await`为异步`preheatOven`函数完成时，我们可以执行同步任务，如`getFrozenPizza`、`openFrozenPizza`、`getPizzaPan`、`placeFrozenPizzaOnPan`、`grabABeverage`甚至`watchTV`。

### 我们一直在执行这样的异步任务

这也是 Javascript 的工作方式。

注意，当我们从一个`async`函数中`await`一个响应时，需要在另一个`async`函数中调用它。这就是我们在上面看到的在`preheatOven`内部调用`ovenReady`的情况。

### 要记住两个要点:

1.  Javascript 不会等待像`preheatOven`这样的`async`函数完成，然后继续执行像`getFrozenPizza`和`openFrozenPizza`这样的任务。
2.  Javascript 将让像`ovenReady`这样的`async`函数`await`完成并返回数据，然后继续执行父异步函数中的下一个任务。当`console.log(response)`语句直到`ovenReady`返回响应后才执行时，我们会看到这种情况。

## 如果比萨饼的例子不切实际...

我知道日常的例子对我们有些人有帮助，但是其他人可能更喜欢真实的代码。

因此，我将在下面提供一个不太抽象的 async 和 await JavaScript 示例，它使用 Fetch API 请求数据:

## JavaScript 中 Async/Await 的代码示例

```
const getTheData = async () => {
    try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users');
    if (!response.ok) throw Error();
    const data = await response.json();
    // do something with this data... save to db, update the DOM, etc.
    console.log(data);
    console.log('You will see this last.')
    } catch (err) {
        console.error(err);
    }
} 

getTheData();
console.log('You will see this first.'); 
```

# 结论

我希望我已经帮助你理解了 JavaScript 中的 async 和 await。

我知道完全理解需要一段时间。

开始预热你想吃的比萨饼的烤箱，看看更多的异步和等待的例子，同时等待哔哔声！

我将从我的 Youtube 频道给你留下一个教程。该视频给出了更深入的解释和更多的代码示例，包括对回调、承诺、属性和 Fetch API 以及 async & await 的讨论:

[https://www.youtube.com/embed/VmQ6dHvnKIM?feature=oembed](https://www.youtube.com/embed/VmQ6dHvnKIM?feature=oembed)