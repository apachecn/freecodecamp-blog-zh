# JavaScript 中的闭包、Curried 函数和很酷的抽象

> 原文：<https://www.freecodecamp.org/news/playing-around-with-closures-currying-and-cool-abstractions/>

在本文中，我们将讨论闭包和 curried 函数，我们将利用这些概念来构建很酷的抽象。我想展示每个概念背后的想法，但也想通过例子和重构代码使它变得非常实用，从而使它更有趣。

## 关闭

闭包是 JavaScript 中的一个常见话题，我们将从它开始。根据 MDN:

> 闭包是捆绑在一起(封闭)的函数与对其周围状态(词法环境)的引用的组合。

基本上，每次创建函数时，也会创建一个闭包，它提供对状态(变量、常量、函数等等)的访问。周围的状态称为`lexical environment`。

让我们看一个简单的例子:

```
function makeFunction() {
  const name = 'TK';
  function displayName() {
    console.log(name);
  }
  return displayName;
}; 
```

这是什么？

*   我们的主要函数叫做`makeFunction`
*   名为`name`的常数被赋予字符串`'TK'`
*   `displayName`函数的定义(它只记录了`name`常量)
*   最后，`makeFunction`返回`displayName`函数

这只是一个函数的定义。当我们调用`makeFunction`时，它将在其中创建一切:一个常量和另一个函数，在本例中。

正如我们所知，当`displayName`函数被创建时，闭包也被创建，它使函数知道它的环境，在本例中是`name`常量。这就是为什么我们可以在不破坏任何东西的情况下`console.log`这个`name`常数。该函数知道词法环境。

```
const myFunction = makeFunction();
myFunction(); // TK 
```

太好了！它像预期的那样工作。`makeFunction`的返回值是我们存储在`myFunction`常量中的函数。当我们呼叫`myFunction`时，它显示`TK`。

我们也可以让它像箭头一样工作:

```
const makeFunction = () => {
  const name = 'TK';
  return () => console.log(name);
}; 
```

但是如果我们想传递名称并显示它呢？简单！使用参数:

```
const makeFunction = (name = 'TK') => {
  return () => console.log(name);
};

// Or as a one-liner
const makeFunction = (name = 'TK') => () => console.log(name); 
```

现在我们可以玩这个名字了:

```
const myFunction = makeFunction();
myFunction(); // TK

const myFunction = makeFunction('Dan');
myFunction(); // Dan 
```

`myFunction`知道传入的参数，不管它是默认值还是动态值。

闭包确保创建的函数不仅知道常量/变量，还知道函数中的其他函数。

这也是可行的:

```
const makeFunction = (name = 'TK') => {
  const display = () => console.log(name);
  return () => display();
};

const myFunction = makeFunction();
myFunction(); // TK 
```

返回的函数知道`display`函数，并且能够调用它。

一种强大的技术是使用闭包来构建“私有”函数和变量。

几个月前，我正在学习数据结构(再次！)并想实现每一个。但是我总是使用面向对象的方法。作为一个函数式编程爱好者，我希望按照 FP 原则(纯函数、不变性、引用透明性等)构建所有的数据结构。).

我学习的第一个数据结构是堆栈。这很简单。主要的 API 是:

*   `push`:将一个项目添加到堆栈的第一个位置
*   `pop`:从堆栈中移除第一个项目
*   `peek`:从堆栈中获取第一个项目
*   `isEmpty`:验证堆栈是否为空
*   `size`:获取堆栈中的物品数量

我们可以清楚地为每个“方法”创建一个简单的函数，并将堆栈数据传递给它。然后它可以使用/转换数据并返回它。

但是我们也可以用私有数据创建一个堆栈，并且只公开 API 方法。我们开始吧！

```
const buildStack = () => {
  let items = [];

  const push = (item) => items = [item, ...items];
  const pop = () => items = items.slice(1);
  const peek = () => items[0];
  const isEmpty = () => !items.length;
  const size = () => items.length;

  return {
    push,
    pop,
    peek,
    isEmpty,
    size,
  };
}; 
```

因为我们在`buildStack`函数内部创建了`items`栈，所以它是“私有”的。它只能在函数中访问。在这种情况下，只有`push`、`pop`等一个人能够接触到数据。这正是我们要找的。

我们如何使用它？像这样:

```
const stack = buildStack();

stack.isEmpty(); // true

stack.push(1); // [1]
stack.push(2); // [2, 1]
stack.push(3); // [3, 2, 1]
stack.push(4); // [4, 3, 2, 1]
stack.push(5); // [5, 4, 3, 2, 1]

stack.peek(); // 5
stack.size(); // 5
stack.isEmpty(); // false

stack.pop(); // [4, 3, 2, 1]
stack.pop(); // [3, 2, 1]
stack.pop(); // [2, 1]
stack.pop(); // [1]

stack.isEmpty(); // false
stack.peek(); // 1
stack.pop(); // []
stack.isEmpty(); // true
stack.size(); // 0 
```

因此，当堆栈被创建时，所有的函数都知道`items`数据。但是在函数之外，我们无法访问这些数据。这是隐私。我们只是通过使用栈的内置 API 来修改数据。

## **咖喱**

> “Currying 是将一个有多个参数的函数转换成一系列只有一个参数的函数的过程。”
> - [前端面试](https://frontendinterview.co/question/what-is-currying-function?tech=javascript)

假设你有一个多参数的函数:`f(a, b, c)`。使用 currying，我们实现了返回函数`h(c)`的函数`f(a)`。

基本上:`f(a, b, c)`——>

让我们构建一个简单的将两个数相加的例子。但首先，不要奉承:

```
const add = (x, y) => x + y;
add(1, 2); // 3 
```

太好了！超级简单！这里我们有一个带有两个参数的函数。为了将它转换成一个可定制的函数，我们需要一个接收`x`并返回一个接收`y`并返回两个值之和的函数。

```
const add = (x) => {
  function addY(y) {
    return x + y;
  }

  return addY;
}; 
```

我们可以将`addY`重构为一个匿名箭头函数:

```
const add = (x) => {
  return (y) => {
    return x + y;
  }
}; 
```

或者通过构建一个线性箭头函数来简化它:

```
const add = (x) => (y) => x + y; 
```

这三个不同的 curried 函数具有相同的行为:构建一个只有一个参数的函数序列。

我们如何使用它？

```
add(10)(20); // 30 
```

起初，这可能看起来有点奇怪，但它背后有一个逻辑。`add(10)`返回函数。我们用`20`值调用这个函数。

这与以下内容相同:

```
const addTen = add(10);
addTen(20); // 30 
```

这很有趣。我们可以通过调用第一个函数来生成专门的函数。假设我们想要一个`increment`函数。我们可以通过将`1`作为值传递来从我们的`add`函数中生成它。

```
const increment = add(1);
increment(9); // 10 
```

当我实现 [Lazy Cypress](https://github.com/leandrotk/lazy-cypress) 时，一个 npm 库记录表单页面上的用户行为并生成 Cypress 测试代码，我想构建一个函数来生成这个字符串`input[data-testid="123"]`。所以我有了元素(`input`)、属性(`data-testid`)和值(`123`)。在 JavaScript 中插入这个字符串会是这样的:`${element}[${attribute}="${value}"]`。

我的第一个实现是接收这三个值作为参数，并返回上面的插值字符串:

```
const buildSelector = (element, attribute, value) =>
  `${element}[${attribute}="${value}"]`;

buildSelector('input', 'data-testid', 123); // input[data-testid="123"] 
```

感觉很棒。我实现了我所期待的。

但同时，我想构建一个更习惯的函数。我可以写“G *et 元素 X，属性为 Y，值为 Z* ”。所以如果我们把这个短语分成三步:

*   *得到一个元素 X*:`get(x)`
*   *带属性 Y*:`withAttribute(y)`
*   *和 Z 值*:`andValue(z)`

我们可以用奉承的概念将`buildSelector(x, y, z)`转化为`get(x)``withAttribute(y)``andValue(z)`。

```
const get = (element) => {
  return {
    withAttribute: (attribute) => {
      return {
        andValue: (value) => `${element}[${attribute}="${value}"]`,
      }
    }
  };
}; 
```

这里我们使用了一个不同的想法:用 function 作为键值返回一个对象。那么我们就可以实现这个语法:`get(x).withAttribute(y).andValue(z)`。

对于每个返回的对象，我们有下一个函数和参数。

重构时间！删除`return`语句:

```
const get = (element) => ({
  withAttribute: (attribute) => ({
    andValue: (value) => `${element}[${attribute}="${value}"]`,
  }),
}); 
```

我觉得它看起来更漂亮。我们是这样使用它的:

```
const selector = get('input')
  .withAttribute('data-testid')
  .andValue(123);

selector; // input[data-testid="123"] 
```

`andValue`函数知道`element`和`attribute`的值，因为它知道词法环境，就像我们之前讨论的闭包一样。

例如，我们也可以通过将第一个参数从其余参数中分离出来，使用“部分 currying”来实现函数。

做了很久的 web 开发，对[事件监听器 Web API](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) 真的很熟悉。下面是它的使用方法:

```
const log = () => console.log('clicked');
button.addEventListener('click', log); 
```

我想创建一个抽象来构建专门的事件侦听器，并通过传递元素和回调处理程序来使用它们。

```
const buildEventListener = (event) => (element, handler) => element.addEventListener(event, handler); 
```

这样，我可以创建不同的专用事件侦听器，并将它们用作函数。

```
const onClick = buildEventListener('click');
onClick(button, log);

const onHover = buildEventListener('hover');
onHover(link, log); 
```

有了这些概念，我可以使用 JavaScript 语法创建一个 SQL 查询。我想像这样查询 JSON 数据:

```
const json = {
  "users": [
    {
      "id": 1,
      "name": "TK",
      "age": 25,
      "email": "tk@mail.com"
    },
    {
      "id": 2,
      "name": "Kaio",
      "age": 11,
      "email": "kaio@mail.com"
    },
    {
      "id": 3,
      "name": "Daniel",
      "age": 28,
      "email": "dani@mail.com"
    }
  ]
} 
```

所以我构建了一个简单的引擎来处理这个实现:

```
const startEngine = (json) => (attributes) => ({ from: from(json, attributes) });

const buildAttributes = (node) => (acc, attribute) => ({ ...acc, [attribute]: node[attribute] });

const executeQuery = (attributes, attribute, value) => (resultList, node) =>
  node[attribute] === value
    ? [...resultList, attributes.reduce(buildAttributes(node), {})]
    : resultList;

const where = (json, attributes) => (attribute, value) =>
  json
    .reduce(executeQuery(attributes, attribute, value), []);

const from = (json, attributes) => (node) => ({ where: where(json[node], attributes) }); 
```

有了这个实现，我们可以用 JSON 数据启动引擎:

```
const select = startEngine(json); 
```

并像 SQL 查询一样使用它:

```
select(['id', 'name'])
  .from('users')
  .where('id', 1);

result; // [{ id: 1, name: 'TK' }] 
```

今天到此为止。我可以继续向你们展示很多不同的抽象例子，但是我会让你们自己来研究这些概念。

你可以在我的博客上发表类似这样的文章。

我的 [Twitter](https://twitter.com/leandrotk_) 和 [Github](https://github.com/leandrotk) 。

## 资源

*   [博文源代码](https://github.com/tk-notes/blog/tree/master/closures-currying-and-cool-abstractions)
*   [Closures | MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
*   [curry | Fun Fun 函数](https://www.youtube.com/watch?v=iZLP4qOwY8I)
*   [通过构建 App 学习 React](https://alterclass.io/?ref=5ec57f513c1321001703dcd2)