# JavaScript 代理快速介绍

> 原文：<https://www.freecodecamp.org/news/a-quick-intro-to-javascript-proxies-55695ddc4f98/>

什么是 JavaScript 代理？你可能会问。这是 ES6 附带的特性之一。可悲的是，它似乎没有被广泛使用。

根据 [MDN Web 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy):

> **代理**对象用于定义基本操作的自定义行为(如属性查找、赋值、枚举、函数调用等)。

简单来说，代理人就是拥有大量财富的**获取者**和**设定者**。代理对象位于对象和外部世界之间。它们拦截对对象的属性和方法的调用，即使这些属性和方法不存在。

为了理解代理是如何工作的，我们需要定义代理使用的三个术语:

1.  **处理程序**:包含陷阱的占位符对象(它们是拦截器)。
2.  **陷阱**:提供属性访问的方法(它们存在于处理程序中)。
3.  **目标**:代理虚拟的对象。

#### 句法

```
let myProxy = new Proxy(target, handler);
```

### 为什么是代理？

既然代理类似于**getter**和**setter**，我们为什么要使用它们呢？让我们看看为什么:

```
const staff = {
  _name: "Jane Doe",
  _age: 25,
  get name() {
    console.log(this._name);
  },
  get age() {
    console.log(this._age);
  },
  set age(newAge) {
    this._age = newAge;
    console.log(this._age)
  }
};
staff.name // => "Jane Doe"
staff.age // => 25
staff.age = 30
staff.age // => 30
staff.position // => undefined
```

让我们用代理编写相同的代码:

```
const staff = {
  name: "Jane Doe",
  age: 25
}
const handler = {
  get: (target, name) => {
    name in target ? console.log(target[name]) : console.log('404 not found');
  },
  set: (target, name, value) => {
    target[name] = value;
  }
}
const staffProxy = new Proxy(staff, handler);
staffProxy.name // => "Jane Doe"
staffProxy.age // => 25
staffProxy.age = 30
staffProxy.age // => 30
staffProxy.position // => '404 not found'
```

在上面使用**getter**和**setter**的例子中，我们必须为`staff`对象中的每个属性定义一个 **getter** 和 **setter** 。当我们试图访问一个不存在的属性时，我们得到了`undefined`。

有了代理，我们只需要一个`get`和`set`陷阱来管理与`staff`对象中每个属性的交互。每当我们试图访问一个不存在的属性时，都会得到一个自定义的错误消息。

代理还有许多其他的用例。让我们探索一些:

### 代理验证

通过代理，我们可以在 JavaScript 对象中实施值验证。假设我们有一个`staff`模式，并希望在保存人员之前执行一些验证:

```
const validator = {
  set: (target, key, value) => {
    const allowedProperties = ['name', 'age', 'position'];
    if (!allowedProperties.includes(key)) {
      throw new Error(`${key} is not a valid property`)
    }

    if (key === 'age') {
      if (typeof value !== 'number' || Number.isNaN(value) || value <= 0) {
        throw new TypeError('Age must be a positive number')
      }
    }
    if (key === 'name' || key === 'position') {
      if (typeof value !== 'string' || value.length <= 0) {
        throw new TypeError(`${key} must be a valid string`)
      }
    }
   target[key] = value; // save the value
   return true; // indicate success
  }
}
const staff = new Proxy({}, validator);
staff.stats = "malicious code" //=> Uncaught Error: stats is not a valid property
staff.age = 0 //=> Uncaught TypeError: Age must be a positive number
staff.age = 10
staff.age //=> 10
staff.name = '' //=> Uncaught TypeError: name must be a valid string
```

在上面的代码片段中，我们声明了一个`validator`处理程序，其中有一个`allowedProperties`数组。在`set`陷阱中，我们检查被设置的键是否是我们`allowedProperties`的一部分。如果不是，我们抛出一个错误。在保存值之前，我们还要检查设置的值是否属于特定的数据类型。

### 可撤销的代理

如果我们想要撤销对一个对象的访问，会怎么样呢？JavaScript 代理有一个`Proxy.revocable()`方法来创建一个可撤销的代理。这使我们能够撤销对代理的访问。让我们看看它是如何工作的:

```
const handler = {
  get: (target, name) => {
    name in target ? console.log(target[name]) : console.log('404 not found');
    console.log(target)
  },

  set: (target, name, value) => {
    target[name] = value;
  }
}
const staff = {
  name: "Jane Doe",
  age: 25
}
let { proxy, revoke } = Proxy.revocable(staff, handler);
proxy.age // => 25
proxy.name // => "Jane Doe"
proxy.age = 30
proxy.age // => 30
revoke() // revoke access to the proxy
proxy.age // => Uncaught TypeError: Cannot perform 'get' on a proxy that has been revoked
proxy.age = 30 // => Uncaught TypeError: Cannot perform 'set' on a proxy that has been revoked
```

在上面的例子中，我们使用析构来访问由`Proxy.revocable()`返回的对象的`proxy`和`revoke`属性。

在我们调用`revoke`函数后，任何应用于`proxy`的操作都会导致一个`TypeError`。在我们的代码中使用这一点，我们可以防止用户对某些对象执行某些操作。

JavaScript 代理是创建和管理对象间交互的强大方法。代理的其他实际应用包括:

*   扩展构造函数
*   操作 DOM 节点
*   值修正和额外属性
*   跟踪属性访问
*   捕获函数调用

这样的例子不胜枚举。

代理比我们在这里讨论的要多。您可以检查[代理 MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)以找出所有可用的陷阱以及如何使用它们。

我希望这个教程对你有用。请做并分享，这样其他人就可以找到这篇文章。如有问题或需要聊天，请通过 Twitter @d [evelopia_](https://twitter.com/developia_) 联系我。