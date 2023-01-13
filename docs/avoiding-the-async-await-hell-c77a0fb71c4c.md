# 如何逃离异步/等待地狱

> 原文：<https://www.freecodecamp.org/news/avoiding-the-async-await-hell-c77a0fb71c4c/>

async/await 把我们从回调地狱中解放出来，但是人们已经开始滥用它了——导致了 async/await 地狱的诞生。

在本文中，我将尝试解释什么是 async/await hell，并且我还将分享一些避免它的技巧。

### 什么是异步/等待地狱

在使用异步 JavaScript 时，人们经常一个接一个地编写多个语句，并在函数调用之前加上一个**wait**。这会导致性能问题，因为很多时候一个语句不依赖于前一个语句，但是您仍然必须等待前一个语句完成。

### 异步/等待地狱的一个例子

考虑一下，如果您编写了一个脚本来订购披萨和饮料。该脚本可能如下所示:

从表面上看，它是正确的，而且确实有效。但是这不是一个好的实现，因为它忽略了并发性。让我们了解它在做什么，以便我们可以确定问题。

#### 说明

我们已经将我们的代码包装在一个异步的生命中。以下内容按此顺序发生:

1.  拿到披萨的清单。
2.  去拿饮料单。
3.  从列表中选择一个比萨饼。
4.  从列表中选择一种饮料。
5.  将选择的披萨加入购物车。
6.  将选择的饮料加入购物车。
7.  订购购物车中的商品。

#### 那到底怎么了？

正如我前面强调的，所有这些语句都是逐个执行的。这里没有并发性。仔细想一想:为什么我们要等着拿到披萨的清单，然后才试图拿到饮料的清单？我们应该试着把两个列表放在一起。然而，当我们需要选择比萨饼时，我们需要事先有比萨饼的清单。饮料也是一样。

因此，我们可以得出结论，比萨饼相关工作和饮料相关工作可以并行发生，但比萨饼相关工作中涉及的各个步骤需要顺序发生(一个接一个)。

#### 糟糕实施的另一个例子

这个 JavaScript 代码片段将获取购物车中的商品，并发出订购请求。

```
async function orderItems() {
  const items = await getCartItems()    // async call
  const noOfItems = items.length
  for(var i = 0; i < noOfItems; i++) {
    await sendRequest(items[i])    // async call
  }
}
```

在这种情况下，for 循环必须等待`sendRequest()`函数完成，然后才能继续下一次迭代。然而，我们实际上不需要等待。我们希望尽快发送所有请求，然后等待所有请求完成。

我希望现在你越来越接近理解什么是异步/等待地狱，以及它对你的程序性能的影响有多严重。现在我想问你一个问题。

### 如果我们忘记了 await 关键字怎么办？

如果您在调用异步函数时忘记使用 **await** ，函数将开始执行。这意味着执行该函数不需要 await。异步函数将返回一个承诺，您可以稍后使用。

```
(async () => {
  const value = doSomeAsyncTask()
  console.log(value) // an unresolved promise
})()
```

另一个后果是编译器不知道你想等待函数完全执行。因此，编译器将在没有完成异步任务的情况下退出程序。所以我们确实需要 **await** 关键字。

```
(async () => {
  const promise = doSomeAsyncTask()
  const value = await promise
  console.log(value) // the actual value
})()
```

承诺的一个有趣特性是，你可以在一行中得到一个承诺，然后在另一行中等待它的解决。这是逃离异步/等待地狱的关键。

如你所见，`doSomeAsyncTask()`正在返回一个承诺。此时`doSomeAsyncTask()`已经开始执行。为了获得承诺的解析值，我们使用 await 关键字，这将告诉 JavaScript 不要立即执行下一行，而是等待承诺解析，然后执行下一行。

### 如何走出异步/等待地狱？

您应该按照这些步骤来逃离异步/等待地狱。

#### 查找依赖于其他语句执行的语句

在我们的第一个例子中，我们选择了比萨饼和饮料。我们的结论是，在选择比萨饼之前，我们需要有比萨饼的清单。在将披萨加入购物车之前，我们需要选择一个披萨。所以我们可以说这三个步骤是相互依存的。我们不能做一件事，直到我们完成了前一件事。

但是如果我们从更广的角度来看，我们会发现选择比萨饼并不取决于选择饮料，所以我们可以并行选择它们。这是机器比我们做得更好的一件事。

因此，我们发现有些语句依赖于其他语句的执行，而有些不依赖于其他语句的执行。

#### 异步函数中的组相关语句

正如我们所看到的，选择比萨饼涉及到从属语句，如获取比萨饼列表，选择一个，然后将选择的比萨饼添加到购物车中。我们应该在异步函数中对这些语句进行分组。这样我们得到了两个异步函数，`selectPizza()`和`selectDrink()`。

#### 并发执行这些异步函数

然后，我们利用事件循环来并发运行这些异步非阻塞函数。这样做的两种常见模式是**提前返回承诺**和**承诺所有方法**。

### 让我们修正这些例子

遵循这三个步骤，让我们将它们应用到我们的示例中。

```
async function selectPizza() {
  const pizzaData = await getPizzaData()    // async call
  const chosenPizza = choosePizza()    // sync call
  await addPizzaToCart(chosenPizza)    // async call
}

async function selectDrink() {
  const drinkData = await getDrinkData()    // async call
  const chosenDrink = chooseDrink()    // sync call
  await addDrinkToCart(chosenDrink)    // async call
}

(async () => {
  const pizzaPromise = selectPizza()
  const drinkPromise = selectDrink()
  await pizzaPromise
  await drinkPromise
  orderItems()    // async call
})()

// Although I prefer it this way 

Promise.all([selectPizza(), selectDrink()]).then(orderItems)   // async call
```

现在，我们将这些语句分为两个功能。在函数内部，每条语句都依赖于前一条语句的执行。然后我们同时执行函数`selectPizza()`和`selectDrink()`。

在第二个例子中，我们需要处理未知数量的承诺。处理这种情况非常简单:我们只需创建一个数组，并将承诺放入其中。然后使用`Promise.all()`我们同时等待所有的承诺解决。

```
async function orderItems() {
  const items = await getCartItems()    // async call
  const noOfItems = items.length
  const promises = []
  for(var i = 0; i < noOfItems; i++) {
    const orderPromise = sendRequest(items[i])    // async call
    promises.push(orderPromise)    // sync call
  }
  await Promise.all(promises)    // async call
}

// Although I prefer it this way 

async function orderItems() {
  const items = await getCartItems()    // async call
  const promises = items.map((item) => sendRequest(item))
  await Promise.all(promises)    // async call
}
```

我希望这篇文章能帮助您了解 async/await 的基础知识，并帮助您提高应用程序的性能。

如果你喜欢这篇文章，请拍手叫好。小贴士——你可以鼓掌 50 次！

也请在 Fb 和 Twitter 上分享。如果你想获得更新，请在 [Twitter](https://twitter.com/dev__adi) 和 [Medium](https://medium.com/@adityaa803/) 上关注我，或者订阅[我的简讯](https://buttondown.email/itaditya)！如果有什么不清楚或者你想指出什么，请在下面评论。