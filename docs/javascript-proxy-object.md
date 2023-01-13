# JavaScript 的代理对象是如何工作的——用用例解释

> 原文：<https://www.freecodecamp.org/news/javascript-proxy-object/>

在本教程中，您将学习什么是代理对象，以及它的局限性。

我们还将研究一些用例，展示如何使用代理对象来解决各种问题。

事不宜迟，我们开始吧。

## 目录

*   [先决条件](#prerequisites)
*   什么是代理？
*   [如何限制对象具有特定属性](#howtorestrictanobjecttohaveaspecificproperty)
*   [一个小弯路——什么是反射 API](#asmalldetourwhatisthereflectapi) ？
*   [像 Python 一样的数组切片](#arrayslicinglikepython)
*   [使用代理的缺点](#disadvantagesofusingproxies)
*   [总结](#summary)

## 先决条件

我强烈建议您在本教程中阅读以下主题:

*   [JavaScript 基础知识](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/JavaScript_basics)。
*   [对象实例方法。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object#instance_methods)
*   如何在 JavaScript 中使用 [apply 函数？](https://www.freecodecamp.org/news/understand-call-apply-and-bind-in-javascript-with-examples/#how-to-use-the-apply-function-in-javascript)

## 什么是代理？

来自[韦氏词典](https://www.merriam-webster.com/dictionary/proxy):

> 代理人可以指被授权代表他人行事的人，也可以指定代替他人服务的职能或权限。

因此，代理人只不过是代表给定一方说话或操作的中间人。

就编程而言，代理这个词也意味着代表一个对象或系统的实体。既然我们已经理清了这个术语，让我们来理解它在 JavaScript 中的含义。

在 JavaScript 中，有一个特殊的对象叫做代理。它帮助您代表原始对象创建另一个对象。

这个新的代理对象将充当真实世界和原始对象之间的中介。这样，我们将对与原始对象的交互拥有更多的控制权。

使用代理是一种与对象交互而不是直接与对象交互的强大方式。

下面是声明代理对象的语法:

```
new Proxy(<object>, <handler>) 
```

`Proxy`有两个参数:

*   `<object>`:需要代理的对象。
*   `<handler>`:定义可以被拦截的方法列表的对象。这些也被称为陷阱。

让我们看一个简单的例子:

```
const books = {
	"Deep work": "Cal Newport",
	"Atomic Habits": "James Clear"
}
const proxyBooksObj = new Proxy(books, {
	get: (target, key) => {
		console.log(`Fetching book ${key} by ${target[key]}`);
		return target[key];
	}
}) 
```

这里我们想要拦截对象`books`的`get`功能，这样我们就可以将书名和作者记录到控制台。

为此，我们创建了一个名为`proxyBooksObj`的新代理对象。这个对象是使用我们前面看到的`Proxy`函数构建的。它以`books`作为被代理的对象，一个由需要被捕获的处理函数组成的对象。

因为我们需要拦截`get`功能，所以我们添加了一个接受函数的`get`属性。这个处理函数将接受一个接受`target`T3 的函数。

这是 JavaScript 中一个非常简单的代理示例。代理有各种各样的使用案例——例如，它可以帮助验证、格式化、通知和调试。

要了解更多关于可用处理程序/陷阱的信息，这里是[列表](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy#a_complete_traps_list_example)。

现在让我们看一个例子，它将为我们提供代理如何工作的更清晰的说明。

## 如何限制对象具有特定的属性

有时候，限制用户使他们只拥有特定的属性是很有用的，就像`useRef`在 React 中所做的那样。它只允许编辑由`useRef`返回的对象的`current`属性。

让我们创建一个代理函数，它只允许用户更新`current`属性，而不允许他们创建任何其他属性。

```
const data = {};

const newProxy = new Proxy(data, {
  set: function (target, key, value) {
    if (key === "current") {
      Reflect.set(target, key, value);
      return true;
    }
    return false;
  }
});

newProxy.current = 1;
newProxy.point = 1; // Throws error 
```

在这里，因为我们正在处理给一个现有的属性赋值或者创建一个新的属性，所以我们使用了`set` trap 函数。我们需要挖掘对象集合的内部行为。

`set`处理函数将接受`target`、`key`和`value`，其中目标将是目标对象，键和值是属性和实际值。

我们确保密钥是`current`。当它存在时，我们将值设置为该属性并返回 true。

我们返回`true`很重要，因为这个处理函数期望遵循一些[不变量](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/set#invariants)。如果我们不想设置这个值，那么我们应该返回`false`。

关于代理中每个不变的陷阱函数，还有一些需要注意的地方。不变量只不过是每个陷阱函数都需要遵循的一个条件，这样基本功能就不会改变。

引用 MDN 的元编程指南:

> [实现自定义操作时保持不变的语义称为*不变量*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Meta_programming#handlers_and_traps)

### 绕了一个小圈子——什么是反射 API？

我们还可以在这里使用 Reflect API。JavaScript 提供了一个内置对象，该对象具有一组可以帮助拦截 JavaScript 操作的函数。可以是`set`、`get`、`apply`等操作。

要了解更多关于 Reflect API 提供的方法，您可以浏览下面的[链接](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect#static_methods)。

有了这个 API，操纵代理中的目标对象变得非常简单。关于反射 API 一个有趣的事实是，它不是一个可以实例化的对象。它提供的所有方法都是静态的。

在前面的例子中，我们试图通过像这样访问原始对象来直接返回对象属性中的值:`target[key]`。

相反，在这里我们可以利用`get`方法进行反思。所以现在我们上面的例子看起来像这样:

```
const books = {
	"Deep work": "Cal Newport",
	"Atomic Habits": "James Clear"
}
const proxyBooksObj = new Proxy(books, {
	get: (target, key) => {
		console.log(`Fetching book ${key} by ${target[key]}`);
		return Reflect.get(target, key);
	}
}) 
```

现在我们知道我们可以使用 Reflect，但是一个问题出现了——为什么要使用 Reflect？

我们使用 Reflect 是因为它允许我们实现原始对象上存在的函数的默认行为。用技术术语来说，这允许我们在陷阱函数中实现默认的转发行为。

同样，使用 Reflect，我们可以访问内部函数，并将它们应用于代理包装的对象。

所以在代理中使用反射 API 对我们来说是有益的。

## 像 Python 一样的数组切片

Python 是一门非常酷的语言。它提供的一个非常棒的功能是可以对阵列进行切片。甚至 Python 的 [NumPy](https://www.w3schools.com/python/numpy/numpy_array_slicing.asp) 库也提供了这个切片数组的特性。

您可以简单地提供由冒号分隔的开始索引(可选)和结束索引(可选)。以下是 Python 中对数组进行切片的语法:

```
arr[<start>:<end>] 
```

从上面的语法可以清楚地看出，`start`和`end`表示用于切片的`start`和`end`(独占)索引位置。

下面是一个如何在 Python 中分割数组的示例:

```
data = [1,2,3,4]
print(data[1:3]) # Output: [2, 3] 
```

下面是如何用 JavaScript 实现同样的功能:

```
const data = [1,2,3,4]
console.log(data.slice(1,3)) // Output: [2, 3] 
```

你需要使用`slice`函数将`data`数组从索引位置`1`切割到索引位置`3`。

注意，对数组进行切片不包括 slice 函数或 Python 切片机制中提供的结束索引。

如果我们也能在 JavaScript 中实现 Python 的切片机制岂不是很棒？

我们再次用 JavaScript 中的代理对象来解决这个问题。正如我们在前面一节中所建立的，代理是原始对象的包装。它们帮助您管理与原始对象的交互。

让我们再次创建这个代理对象——但是这一次它有切片机制。下面是用 JavaScript 实现切片机制的代码:

```
const arr = [1,2,3,4];

const arrProxy = new Proxy(arr, {
  get: function (target, key) {
    if (typeof key === "string" && key.includes(":")) {
      let [start, end] = key.split(":");
      if (start === "") {
        start = 0;
      } else if (end === "") {
        end = target.length;
      } else {
        start = parseInt(start, 10);
        end = parseInt(end, 10);
      }

      return Reflect.apply(Array.prototype.slice, target, [start, end]);
    }
    return Reflect.get(target, key);
  }
});

console.log(arrProxy["1:3"]) // [2,3] 
```

好了，这里有很多代码，但是让我们后退一步，理解发生了什么:

*   我们在数组`arr`的顶部创建了一个新的`proxy`对象。因为我们计划通过提供由冒号分隔的开始和结束位置来实现切片，所以我们需要对如何实现数组的 get 功能进行一些修改。
*   为此，我们将利用`get` trap 函数来修改数组的 get 机制。我们确保将这个陷阱函数作为一个属性添加到`Proxy`的`handler`对象中。
*   一个 [get](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/get) 陷阱函数接受`target`和`key`作为它的参数。我们利用这一点来编写我们的逻辑。
*   因为我们正在做类似于`arrProxy["1:3"]`的事情，在这种情况下，键将是 string 类型的，并且它将在其中包含`:`。我们的主要条件将是在同样的条件下进行区分。

```
if (typeof key === "string" && key.includes(":")) 
```

接下来，我们编写一些条件，这些条件是 Python 中切片机制的要求。以下是要求:

*   每当没有提供`start`索引时，我们应该将`start`设置为 0。
*   每当没有提供索引`end`时，我们应该将`end`设置为原始数组的长度。
*   如果两者都提供了，那么我们就把它们转换成整数。

为此，我们需要利用 JavaScript 中的 [slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) 函数。

我们将使用[反射 API 的](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect) [应用](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/apply)方法来执行函数`slice`，其中上下文(`target`)为原始数组，`slice`函数的参数为`[start, end]`。

最后，如果`key`参数是除字符串之外的任何其他数据类型，那么我们在`Reflect.get`的帮助下直接返回数组。

## 使用代理的缺点

我们现在知道了代理是如何与其用例一起工作的。但是了解使用代理的一些缺点也很重要。

首先，虽然使用代理很酷，但在性能是关键因素的场景中，这是一个非常糟糕的选择。

第二，无操作代理转发是使用代理转发目标的所有功能的机制。代理转发看起来是这样的:

```
const data = {};
const newData = new Proxy(data, {}); // No trap function.

data.point = { x: 1, y: 2};

console.log(data); 
```

这在常规对象上工作得很好，但对于 DOM 节点或有内部插槽的对象就不行了。

例如，执行如下操作将会产生错误:

```
const x = document.createElement("div")
x.className = "hello"

const domProxy = new Proxy(x, {});

console.log(domProxy.getAttribute("class")); // Throws typeError 
```

发生这种情况是因为`this`值引用代理对象，而不是引用原始对象。我们可以通过 get trap 函数来解决这个问题，它有助于引用原始对象。

```
const y = new Proxy(x, {
  get(target, key) {
    const value = Reflect.get(target, key);
    if(typeof value === "function"){
      return value.bind(target)
    }
    return value;
  }
});

console.log(y.getAttribute("class")); // Output: hello 
```

## 摘要

在本文中，我们学习了 JavaScript 中的代理及其用例。我们还研究了代理的一些缺点。

如果你喜欢使用代理的想法，那么你可以更进一步，尝试在各种验证场景中使用它们，或者复制一些库函数，比如 [lodash 的 get](https://lodash.com/docs/4.17.15#get) 和 [set](https://lodash.com/docs/4.17.15#set) 函数。

仅此而已。感谢阅读。

在 [Twitter](https://twitter.com/keurplkar) 、 [GitHub](https://github.com/keyurparalkar) 和 [LinkedIn](https://www.linkedin.com/in/keyur-paralkar-494415107/) 上关注我。