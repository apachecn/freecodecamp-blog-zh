# React 中的状态处理:要考虑的四种不可变方法

> 原文：<https://www.freecodecamp.org/news/handling-state-in-react-four-immutable-approaches-to-consider-d1f5c00249d5/>

科里·豪斯

也许 React 今天最常见的困惑点是:状态。

假设您有一个编辑用户的表单。创建一个单独的变更处理程序来处理所有表单域的变更是很常见的。它可能看起来像这样:

```
updateState(event) {
 const {name, value} = event.target;
 let user = this.state.user; // this is a reference, not a copy...
 user[name] = value; // so this mutates state ?
 return this.setState({user});
}
```

问题在第 4 行。第 4 行实际上改变了 state，因为用户变量是 state 的引用。反应状态应该被视为不可变的。

从[反应文件](https://facebook.github.io/react/docs/react-component.html#state):

> 千万不要直接变异`this.state`，因为之后调用`setState()`可能会替换你所做的变异。视`this.state`为不可改变的事物。

为什么？

1.  setState 批处理在后台工作。这意味着在处理 setState 时，手动状态突变可能会被覆盖。
2.  如果你声明一个 shouldComponentUpdate 方法，你不能在里面使用===相等检查，因为*对象引用不会改变*。所以上面的方法也有潜在的性能影响。

**底线**:上面的例子通常工作正常，但是为了避免边缘情况，将状态视为不可变的。

以下是将状态视为不可变的四种方式:

### 方法 1: Object.assign

[Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 创建一个对象的副本。第一个参数是目标，然后为想要附加的属性指定一个或多个参数。因此，修改上面的例子只需要对第 3 行进行简单的修改:

```
updateState(event) {
 const {name, value} = event.target;
 let user = Object.assign({}, this.state.user);
 user[name] = value;
 return this.setState({user});
}
```

在第 3 行，我说“创建一个新的空对象，并将 this.state.user 上的所有属性添加到其中。”这将创建存储在 state 中的用户对象的单独副本。现在我可以安全地改变第 4 行中的用户对象了——它是一个完全独立于状态对象的对象。

确保 polyfill Object.assign，因为它是 IE 中不支持的[，并且](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)[不是由巴别塔](https://babeljs.io/docs/usage/polyfill/)传送的。要考虑的四个选项:

1.  [对象分配](https://www.npmjs.com/package/object-assign)
2.  [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
3.  [巴别塔聚宝盆](https://babeljs.io/docs/usage/polyfill/)
4.  [Polyfill.io](https://polyfill.io/v2/docs/features/#Object_assign)

### 方法 2:对象传播

物体传播目前是第三阶段的一个特征，可以被通天塔传送到 T2。这种方法更简洁:

```
updateState(event) {
 const {name, value} = event.target;
 let user = {...this.state.user, [name]: value};
 this.setState({user});
}
```

在第 3 行，我说“使用 this.state.user 上的所有属性创建一个新对象，然后将[name]表示的属性设置为 event.target.value 上传递的新值”。因此，这种方法的工作方式与 Object.assign 方法类似，但有两个好处:

1.  不需要多填料，因为巴别塔可以传输
2.  更简洁

您甚至可以使用析构和内联将它变成一行代码:

```
updateState({target}) {
 this.setState({user: {...this.state.user, [target.name]: target.value}});
}
```

我在方法签名中析构 event 以获得对 event.target 的引用。然后我声明 state 应该设置为 this.state.user 的副本，相关属性设置为新值。我喜欢这种简洁。这是目前我最喜欢的编写变更处理程序的方法。？

上面这两种方法是处理不可变状态的最常见和最直接的方法。想要更多的权力？看看下面的另外两个选项。

### 方法 3:不变性助手

不变性助手是一个方便的库，可以在不改变数据源的情况下改变数据的副本。这个库是在 [React 的文档](https://facebook.github.io/react/docs/update.html)中建议的。

```
// Import at the top:
import update from 'immutability-helper';

updateState({target}) {
 let user = update(this.state.user, {$merge: {[target.name]: target.value}});
 this.setState({user});
}
```

在第 5 行，我调用了 merge，这是 immutuability-helper 提供的众多命令中的[之一。与 Object.assign 非常相似，我将目标对象作为第一个参数传递给它，然后指定我想要合并的属性。](https://github.com/kolodny/immutability-helper#available-commands)

不变性助手远不止这些。它使用了受 MongoDB 查询语言启发的语法，并提供了多种处理不可变数据的强大方法。

### 方法 4:不可变的. js

想通过编程来增强不变性吗？考虑[不可变. js](https://facebook.github.io/immutable-js/) 。这个库提供了不可变的数据结构。

下面是一个使用不可变映射的示例:

```
 // At top, import immutable
import { Map } from 'immutable';

// Later, in constructor...
this.state = {
  // Create an immutable map in state using immutable.js
  user: Map({ firstName: 'Cory', lastName: 'House'})
};

updateState({target}) {
 // this line returns a new user object assuming an immutable map is stored in state.
 let user = this.state.user.set(target.name, target.value);
 this.setState({user});
}
```

上面有三个基本步骤:

1.  导入不可变。
2.  在构造函数中将状态设置为不可变映射
3.  使用更改处理程序中的 set 方法创建 user 的新副本。

immutable.js 的妙处:**如果直接尝试变异状态，会失败**。使用上面的其他方法，很容易忘记，并且当您直接改变状态时，React 不会警告您。

不可变的缺点？

1.  **膨胀**。不可变的. js 将 57K minified 添加到您的包中。考虑到像 [Preact 这样的库只能在 3K](https://preactjs.com) 取代 react，这很难接受。
2.  **语法**。你必须通过字符串和方法调用来引用对象属性，而不是直接引用。比起 user.get('name ')，我更喜欢 user.name。
3.  **YATTL** (又一件需要学习的事情)——任何加入您团队的人都需要学习另一种获取和设置数据的 API，以及一组新的数据类型。

其他几个有趣的选择值得考虑:

*   [无缝-不可变](https://github.com/rtfeldman/seamless-immutable)
*   [反应-复制-写入](https://github.com/aweary/react-copy-write)

### 警告:小心嵌套对象！

上面的选项#1 和# 2(Object . assign 和 Object spread)只做了一个*浅层*克隆。所以如果你的对象包含嵌套对象，**那些嵌套对象将通过引用而不是通过值**被复制。因此，如果您更改嵌套对象，您将改变原始对象。？

对你要克隆的东西要谨慎。不要克隆所有的东西。克隆已更改的对象。不变性助手(如上所述)使这变得容易。像 [immer](https://github.com/mweststrate/immer) 、 [updeep](https://github.com/substantial/updeep) 或者[一长串备选方案](https://github.com/markerikson/redux-ecosystem-links/blob/master/immutable-data.md#immutable-update-utilities)也是如此。

你可能会尝试使用深度合并工具，比如 [clone-deep](https://www.npmjs.com/package/clone-deep) ，或者 [lodash.merge](https://lodash.com/docs/#merge) ，但是**要避免盲目的深度克隆**。

原因如下:

1.  深度克隆是昂贵的。
2.  深度克隆通常是浪费的(相反，只克隆实际发生变化的部分)
3.  深度克隆会导致不必要的渲染，因为 React 认为一切都发生了变化，而实际上可能只有一个特定的子对象发生了变化。

感谢丹·阿布拉莫夫给了我上面提到的建议:

> 我觉得 cloneDeep()不是一个好的推荐。它可能非常昂贵。只复制实际发生变化的部分。像 immutanbility-helper([https://t.co/YadMmpiOO8](https://t.co/YadMmpiOO8))、up deep([https://t.co/P0MzD19hcD](https://t.co/P0MzD19hcD))或者 immer([https://t.co/VyRa6Cd4IP](https://t.co/VyRa6Cd4IP))这样的库在这方面有所帮助。
> 
> — Dan Abramov (@dan_abramov) [April 23, 2018](https://twitter.com/dan_abramov/status/988546679115800577?ref_src=twsrc%5Etfw)