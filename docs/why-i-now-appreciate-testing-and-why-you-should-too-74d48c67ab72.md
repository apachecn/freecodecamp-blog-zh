# 为什么我现在喜欢测试，为什么你也应该喜欢。

> 原文：<https://www.freecodecamp.org/news/why-i-now-appreciate-testing-and-why-you-should-too-74d48c67ab72/>

伊芙琳·陈

# 为什么我现在喜欢测试，为什么你也应该喜欢。

![V26crRt1no9-dFumjEK77jTLpwRKTtGBPKAb](img/19dc5bb24608b05f7ed12506aa2429c9.png)

Photo by Unsplash.com

有一种普遍的误解，认为编写测试会降低开发速度。虽然测试的好处有时可能不会立即显现出来，但是根据我目前的经验，从长远来看，测试可以让我工作得更快，即使是在一个小团队中。

在对一个全栈应用程序进行测试之后，我开始意识到有效测试你的应用程序是多么有用，以及它是如何影响你的编码能力的。

### 快速介绍测试堆栈的样子

Jest 是一个由脸书开发的测试库，它预先打包了一些很酷的方法来简化测试。在最近的一个项目中，我和我的团队选择了 Jest，因为它易于使用，并且有大量的内置函数可以帮助简化您的测试。为断言设置和使用 Expect 库非常简单。

酶是测试你的 React 应用程序的实际方法。它非常神奇，因为它在 Jest 中呈现 React 组件，允许您有效地测试您的前端 JSX 代码，而无需手动传输它。使用以下三种方法之一渲染组件:浅层、挂载或渲染。

SuperTest 允许你在不实际调用的情况下进行 API 调用，通过拦截它。对于单元测试来说，API 调用可能很昂贵和/或很慢，所以您不会想要进行调用并等待响应。否则，它会降低您的测试速度，并且永远无法运行。

### 以下是我所学到的

#### 测试防止回归

添加新特性经常会破坏现有的代码，进行测试可以防止这种情况发生。测试有助于确保您的代码如您所愿地运行。这听起来很简单，但却有着天壤之别。

听说不是所有的程序员都能清晰地说出他们编写的代码，这听起来似乎很傻，但实际上这很常见。有多少次你被要求解释你在严格的期限内匆忙写下的东西，却发现自己结结巴巴地回答？编写测试迫使你清楚地思考你的函数到底接受什么作为参数，返回什么作为输出。

即使你理解每一行代码的目的，随着时间的推移，你会不可避免地开始忘记。测试为文档提供了重要的补充，帮助您在代码库之间快速地来回移动。花时间编写好的、高效的测试可以让你更容易地重构代码，并且自信地进行开发。

#### 使用模拟和存根的单元测试。

在单元测试中，一次测试一段特定的代码。在任何应用程序中，您的代码可能会调用并依赖于其他类或模块。随着这些类之间的关系变得越来越复杂，这将混淆错误的来源。

为了有效地隔离和调试，您可以用 mock 或 stub 替换这些依赖项，以控制您期望的行为或输出。

例如，假设您想要测试以下方法:

```
import database from 'Database';import cache from 'Cache';
```

```
const getData = (request) => { if (cache.check(request.id)) { // check if data exists in cache  return cache.get(request.id); // get from cache } return database.get(request.id); // get from database};
```

存根:

```
test('should get from cache on cache miss', (done) => { const request = { id: 10 }; cache.check = jest.fn(() => false);
```

```
getData(request); expect(database.get).toHaveBeenCalledWith(request.id); done();});
```

模拟:

```
test('should check cache and return database data if cache data is not there', (done) => { let request = { id: 10 }; let dummyData = { id: 10, name: 'Foo' }  let cache = jest.mock('Cache'); let database = jest.mock('Database'); cache.check = jest.fn(() => false); database.get = jest.fn(() => dummyData);
```

```
expect(getData(request)).toBe(dummyData); expect(cache.check).toHaveBeenCalledWith(request.id); expect(database.get).toHaveBeenCalledWith(request.id); done();});
```

两者的主要区别在于状态 vs 行为操纵。

使用 mock 时，用一个 mock 对象替换整个模块。存根是一个函数的强制输出，与给定的输入无关。模拟用于测试函数是否使用正确的参数调用，存根用于测试函数如何对给定的响应进行操作。存根用于验证方法的状态，而模拟用于评估行为。

Jest 提供了`jest.fn`，它同时具有基本的嘲讽和存根功能。Jest mock 也可以存根方法输出，在这种情况下，它既是一个 mock 又是一个存根。

测试中的模拟概念以及它们与存根的不同之处可能会变得非常复杂，因此要深入了解，请查看最后的链接！

#### 知道自己不需要考什么。

你会想测试你写的每一个方法。但是请记住，根据您的代码库，100%的测试覆盖率是不太可能的，大多数时候甚至是不必要的。

使用 Jest，您可以通过在 CLI 上向您的测试脚本添加一个`--coverage`标签来轻松地跟踪您的测试覆盖率。虽然这是一个有用的衡量标准，但要有所保留 Jest 衡量测试覆盖率的方式是通过跟踪调用堆栈，因此较高的测试覆盖率并不一定意味着您的测试是有效的。

例如，在以前的项目中，我使用一个库来实现一个 carousel 组件。组件中有一个基于数组呈现列表的函数。为了增加测试覆盖率，我编写了一个测试来计算和比较数组中呈现的项目数。carousel 组件将呈现在 DOM 上的项目数量修改为大于 1:1 的输出，尽管实现在浏览器中直观地显示了正确的元素数量。我选择放弃测试覆盖，因为它本质上是在测试 carousel 库，而不是我的代码。

让我们假设一个具有方法`renderCarousel`的`Listings`组件从外部库呈现一个转盘:

**无效测试:**

```
test('should return the same number of elements as the array', (done) => {    // Full DOM render    let mountWrapper = mount(<Listings />);
```

```
 // State change to trigger re-render    mountWrapper.instance().setState({ listings: [listing, listing, listing] });
```

```
 // Updates the wrapper based on new state    mountWrapper.update();
```

```
 expect(mountWrapper.find('li').length).toBe(3);    done();  })
```

**有效测试:**

```
test('should call renderCarousel method when state is updated', (done) => {    // Mock function inside component to track calls    wrapper.instance().renderCarousel = jest.fn();
```

```
 // State change to trigger re-render    wrapper.instance().setState({ listings: [listing, listing, listing] });
```

```
 expect(wrapper.instance().renderCarousel).toHaveBeenCalled();    done();  });
```

两者的区别在于测试实际测试的是什么。

第一个示例评估 renderCarousel 函数，该函数调用外部库。第二个测试评估 renderCarousel 是否只是被调用。由于外部库有点类似于魔术的黑盒，并且由他们自己的开发人员测试，所以没有必要编写测试来确保它正常工作。

在这种情况下，我们只需要测试库是否被调用，并且相信库的开发人员正在处理测试。

理解你做什么和不需要测试，让你最大化你的时间来消除冗余。

#### 设计良好的测试导致设计良好的代码。

设计您的代码时，知道您必须为它编写测试会改进您的代码。这就是所谓的测试驱动开发，它得到了编码社区的广泛支持。

为了恰当地单元测试功能，您需要剥离逻辑，一次测试一个方法。这种结构迫使您编写模块化代码，并从组件中抽象出逻辑。

当你从编写测试的角度考虑你的代码时，你将开始养成代码解耦的习惯。我发现，以这种方式编写代码似乎会产生一种类似故事的结构，更容易编写，也更容易理解。

让我们考虑一个场景，我们调用一个端点从数据库中获取数据，并在返回数据之前格式化数据。

**逻辑太多:**

```
// Define endpointapp.get('/project', (request, response) => {  let { id } = request.params;  let query = `SELECT * FROM database WHERE id = ${id}`;    database.query(query)    .then((result) => {      const results = [];      for (let index = 0; index < data.length; index++) {        let result = {};        result.newKey = data[index].oldKey;        results.push(result);      }      response.send(results);    })    .catch((error) => {      response.send(error);    })  })
```

**更容易测试:**

```
// Make call to database for dataconst getData = (request) => {  return new Promise((resolve, reject) => {    let { id } = request.params;    let query = `SELECT * FROM database WHERE id = ${id}`;    database.query(query)      .then(results => resolve(results))      .catch(error => reject(error));    };}
```

```
// Format data to send backconst formatData = (data) => {  const results = [];  for (let index = 0; index < data.length; index++) {    let result = {};    result.newKey = data[index].oldKey;    results.push(result);  }  return results;}
```

```
// Send back dataconst handleRequest = (request, response) => {  getData(request)  .then((result) => {    let formattedResults = formatData(result)    response.send(formattedResults);  .catch((error) => {    response.send(error);}
```

```
// Define endpointapp.get('/project', handleRequest);
```

虽然第二个例子比较长，但是很容易理解。逻辑显然是抽象和隔离的，这使得测试更加容易。

如果你刚刚开始测试/编码，很难辨别什么是设计良好的测试。如果你的代码设计得不好，你就写不出有效的测试，但是你不知道是什么让代码适合你写不出的测试！幸运的是，这引出了我的最后一个技巧…

#### 编码时编写测试

合并测试的最好方法是把它写在你的代码旁边。否则，当你一次写完所有的测试和方法文件时，你会感到不知所措。

许多初学者错误地认为测试是在所有代码都写完之后才进行的。如果你把它当作一项和代码一起做的任务，它不仅会改进你的代码，还会让编写代码变得更容易实现。

在一个具有清晰抽象的 API 的系统中，您可以在继续之前测试每个类，这样您就知道哪个逻辑部分被破坏了。例如，我的“get”端点调用 getData 与数据库交互。我会首先为 getData 编写测试，并确保它们是绿色的。这样，我知道如果我的任何控制器测试失败，它可能与我调用 getData 的方式有关。

希望这篇文章已经帮助你理解了为什么测试如此有用，并且为你提供了一些如何开始的提示。不过，这只是测试的冰山一角！如果您想了解更多信息，以下是一些资源:

[https://martinfowler.com/articles/mocksArentStubs.html](https://martinfowler.com/articles/mocksArentStubs.html)

[https://medium . com/@ rickhanlonii/understanding-jest-mocks-f 0046 c 68 e 53 c](https://medium.com/@rickhanlonii/understanding-jest-mocks-f0046c68e53c)

如果您喜欢这篇文章，请点击？？并分享出来帮助别人找到。感谢阅读！