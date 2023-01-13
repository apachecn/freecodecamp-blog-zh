# 如何在 JavaScript 中处理异步回调...没有复试？

> 原文：<https://www.freecodecamp.org/news/how-to-handle-async-callbacks-in-javascript/>

今天，同样的问题在几个不同的服务器上出现了几次。我认为这是一个很好的问题，似乎我的大脑并不像其他人想象的那样工作。

问题是:

> “所以我有一个`fetch`函数，我用它做一些`then`来解析 JSON 数据。我想把它还回去，但是我该怎么做呢？我们不能`return`从一个异步函数调用东西！”

这是一个很好的问题。那里发生了很多事。我们在 React 中有处理这种情况的方法，非常容易:我们可以`useState`创建一些有状态变量，我们可以在`useEffect`中运行我们的`fetch`并加载那个有状态变量，我们可以使用*另一个* `useEffect`来监听那个有状态变量的变化。当变化发生时，我们可以触发我们的自定义函数，并用它产生某种副作用。

对于纯 JavaScript、HTML 和 CSS，这变得有点棘手。对于那些喜欢先看推理小说的最后一页的人来说，这本书是我们的结尾。

## 丑陋的开端

假设我们想从服务器获取一些 todos，当我们加载了它们之后，我们想更新 DOM。我们可能需要重新加载它们，或者在以后追加它们——如果我们的异步函数对我们的*状态*进行某种更新，我们希望事情发生。

然而，我真的不知道我对此有什么感觉。当我们有这样的代码块时:

```
const load = () => {
  fetch("https://jsonplaceholder.typicode.com/todos")
    .then(res => res.json())
    .then(jsonObj => {
      const todoContainer = document.querySelector(".todos-container");
      // now, take each todo, create its DOM, and poke it in.
      jsonObj.forEach( (todo)=>{
        const todoEl = document.createElement("div");
        todoEl.classList.add("todo");
        const todoTitle = document.createElement("h3");
        todoTitle.classList.add("todo-title");
        todoTitle.textContent=todo.title;

        const todoStatus = document.createElement("div");
        todoStatus.classList.add("todo-status");
        todoStatus.textContent = todo.done ? "Complete" : "Incomplete";

        todoEl.append(todoTitle, todoStatus);
        todoContainer.append(todoEl)
    })
}
```

我们用*和*来填充`.then()`块中的 DOM，因为我们不能说“嘿，当这个完成后，启动这个函数。”

我们可以简单地等待每个承诺，而不是像这样将它们链接起来，并简单地返回最终解析的结果:

```
const load = async () => {
  const result = await fetch("https://jsonplaceholder.typicode.com/todos")
  const jsonObj = await result.json();
  const todoContainer = document.querySelector(".todos-container");

  jsonObj.forEach( (todo)=>{
    const todoEl = document.createElement("div");
    todoEl.classList.add("todo");
    const todoTitle = document.createElement("h3");
    todoTitle.classList.add("todo-title");
    todoTitle.textContent=todo.title;

    const todoStatus = document.createElement("div");
    todoStatus.classList.add("todo-status");
    todoStatus.textContent = todo.done ? "Complete" : "Incomplete";

    todoEl.append(todoTitle, todoStatus);
    todoContainer.append(todoEl)
  })
  // here, if we wanted, we could even return that object:
  return jsonObj;
}

// later, we can do this:
const todos = await load();
// fills the DOM and assigns all the todos to that variable
```

现在这样更好了，我们的`load()`函数不仅可以用于将这些元素放入 DOM，还可以将数据返回给我们。

然而，这仍然不理想——当结果被加载时，我们仍然必须填充 DOM，我们仍然必须等待加载发生。我们不知道*何时* `todos`会成为什么。最终会的，只是我们不知道什么时候。

## 有人想试镜吗？

我们有回调函数的选项。与其对 DOM 构造进行硬编码，不如将它传递给其他东西，这可能会有用。这使得`load`函数更加抽象，因为它没有连接到特定的端点。

让我们看看这可能是什么样子:

```
const load = async (apiEndpoint, callbackFn) => {
  const result = await fetch(apiEndpoint);
  if(!result.ok){
    throw new Error(`An error occurred: ${result.status}`)
  }
  // at this point, we have a good result:
  const jsonObj = await result.json();
  // run our callback function, passing in that object
  callbackFn(jsonObj)
}

// Let's use that. First, we'll make a callback function:
const todoHandler = (todos) => {
  const todoContainer = document.querySelector(".todos-container");

  todos.forEach( (todo)=>{
    const todoEl = document.createElement("div");
    todoEl.classList.add("todo");
    const todoTitle = document.createElement("h3");
    todoTitle.classList.add("todo-title");
    todoTitle.textContent=todo.title;

    const todoStatus = document.createElement("div");
    todoStatus.classList.add("todo-status");
    todoStatus.textContent = todo.done ? "Complete" : "Incomplete";

    todoEl.append(todoTitle, todoStatus);
    todoContainer.append(todoEl)
  })    
}

load("https://jsonplaceholder.typicode.com/todos", todoHandler);
```

这更好——我们现在告诉`load`装载什么，以及当获取完成时做什么。它工作了。这并没有什么错。尽管如此，它也有一些缺点。

我的回拨还没有完成。我们不处理错误，通过这种方法，我们真的没有*获得*任何东西。我们没有及时地从`load`函数中获取任何我们可以使用的数据。

我还是我，我想尝试不同的方式。

## 没有回调的回调

好吧，那个*是*有点误导。他们不是复试。我们将完全避免*出现*回调。我们吃什么来代替？事件监听器！

DOM 是关于交流的。事件无处不在——鼠标事件、键盘事件、手势、媒体和窗口...浏览器是一个嘈杂的地方。

但这一切都是由*控制的*，这一切都是*有意图的*，这一切都是*形式良好的*。这些东西封装得很好，完全独立，但是它们可以根据需要在 DOM 树中上下传递事件。通过`CustomEvent` API，我们可以利用这一点。

创建一个`CustomEvent`并没有那么难，只需提供一个字符串形式的事件名称和*有效负载*——包含在该事件中的信息。这里有一个例子:

```
const myShoutEvent = new CustomEvent('shout', {
  detail: {
    message: 'HELLO WORLD!!',
    timeSent: new Date() 
  }
})

// and later on, we can send that event:
someDomEl.dispatchEvent(myShoutEvent);
```

这就是自定义事件的全部内容。我们创建事件，包括定制的`detail`数据，然后我们在给定的 DOM 节点上`dispatchEvent`。当那个事件在那个 DOM 节点上被触发时，它加入正常的通信流，就像任何正常事件一样沿着冒泡和捕获阶段前进——因为它*是*一个正常事件。

## 这对我们有什么帮助？

如果我们让*监听*某处的自定义事件，并将处理该事件(及其`detail`)的责任交给接收者，而不是告诉`load`函数在我们获得数据时做什么，会怎么样？

使用这种方法，我们并不真正关心*何时*fetch 完成其处理，我们也不关心某个全局变量中的某个返回值——我们只是告诉 DOM 节点分派一个事件...*并将取出的数据作为`detail`* 传递。

让我们开始玩这个想法:

```
const load = (apiEndpoint, elementToNotify, eventTitle) => {
  fetch(apiEndpoint)
    .then( result => result.json() )
    .then( data => {
       // here's where we do this: we want to create that custom event
       const customEvent = new CustomEvent(eventTitle, {
         detail: {
           data
         }
       });
       // now, we simply tell the element to do its thing:
      elementToNotify.dispatchEvent(customEvent)
     })
};
```

就是这样。这就是全部情况。我们加载一些端点，解析它，将数据封装在一个定制的事件对象中，然后将其放入 DOM。

其余的不在那个`load`函数的关注范围之内。它不*关心*数据看起来像什么，它不*关心*它来自哪里，它不*返回*任何东西。它只做一件事——获取数据，然后大喊大叫。

现在，有了它，我们怎么从另一边把它接进来呢？

```
// a function to create the Todo element in the DOM...
const createTodo = ({id, title, completed}) => {
  const todoEl = document.createElement("div");
  todoEl.classList.add("todo");

  const todoTitle = document.createElement("h3");
  todoTitle.classList.add("todo-title");
  todoTitle.textContent=todo.title;

  const todoStatus = document.createElement("div");
  todoStatus.classList.add("todo-status");
  todoStatus.textContent = todo.done ? "Complete" : "Incomplete";

  todoEl.append(todoTitle, todoStatus);

  return todoEl;
}

// and when that load event gets fired, we want this to be
//  the event listener.
const handleLoad = (event)=>{
  // pull the data out of the custom event...
  const data = event.detail.data;
  // and create a new todo for each object
  data.forEach( todo => {
    event.target.append( createTodo(todo) )
  })
}

// finally, we wire in our custom event!
container.addEventListener("todo.load", handleLoad)
```

它连接`container`来监听自定义的`todo.load`事件。当事件发生时，它触发并执行那个`handleLoad`监听器。

它没有做任何特别神奇的事情:它只是从我们在`load`函数中创建的`event.detail`中获取`data`。然后`handleLoad`为`data`中的每个对象调用`createTodo`，为每个 todo 元素创建 DOM 节点。

使用这种方法，我们已经很好地将数据获取位和表示位分开了。唯一剩下的事情就是告诉一个人和另一个人说话:

```
// remember, the parameters we defined were:
// apiEndpoint: url,
// elementToNotify: HTMLDomNode,
// eventTitle: string
load("https://jsonplaceholder.typicode.com/todos", container, 'todo.load');
```

## 概括一下

我们从一堆难看的意大利面条式代码开始——获取逻辑与解析和表示混在一起。不好。我的意思是，我们都这样做，我们一直在使用它，但它只是感觉粗略。没有明确的分离，也没有办法处理那个`.then()`之外的数据。

使用`async/await`，我们*可以*返回该数据，并且如果需要的话，我们可以在获取之外使用它——但是我们没有真正的方法知道该数据何时被加载。我们仍然可以进行内联处理，通过读取加载表示层，但这与上一次相比没有任何好处。

使用回调，我们可以开始分离——通过回调，我们可以加载数据，当异步操作完成时，运行回调函数。它很好地将它们分开，并将数据作为参数传递给回调函数。它*比*在线混合演示要好，但是我们*可以*做一些不同的事情。

我的意思是*不同*——使用`CustomEvent` API 并不比使用回调更好或更差。两者各有优缺点。我喜欢`CustomEvent`系统的整洁，我喜欢我们可以扩展它。一些例子:

*   一个定时器类，触发一个`"timer.tick"`和`"timer.complete"`事件。计时器的 DOM 节点的父节点/容器可以监听这些事件，*异步触发*，并做出适当的响应，无论是更新显示的时间还是在计时器结束时做出反应。
*   我们的 Todos——我们可以让容器监听`"todo.load"`、`"todo.update"`，任何我们喜欢的自定义事件。我们可以通过找到相关的 DOM 节点并更新其内容来处理更新，或者删除所有节点并在加载时替换它们。

我们将模型逻辑与表示逻辑完全分离开来，并定义两者之间的接口。干净、清晰、可靠且简单。