# 如何使用部分应用程序来改进您的 JavaScript 代码

> 原文：<https://www.freecodecamp.org/news/how-to-use-partial-application-to-improve-your-javascript-code-5af9ad877833/>

里克·亨利

# 如何使用部分应用程序来改进您的 JavaScript 代码

#### 利用这种函数技术可以使您的代码更加优雅

![YlwtX1HWYUmiNRvHck44VFmc7Ho86aQTIG54](img/9a50b62f5b9040c34f1a3c3468e18916.png)

函数式编程为我们提供了解决代码问题的技术。其中之一，部分应用，理解起来有点棘手，但它可以让我们写得更少(听起来很有趣，对吧？).

#### 这是什么？

部分应用从函数开始。我们用这个函数创建一个新函数，它的一个或多个参数已经“设置”或部分应用了。这听起来很奇怪，但是它将减少我们的函数所需的参数数量。

让我们就何时可以使用部分应用给出一些背景:

```
const list = (lastJoin, ...items) => {  const commaSeparated = items.slice(0,-1).join(", ");  const lastItem = items.pop();  return `${commaSeparated} ${lastJoin} ${lastItem}`;}
```

这个小函数接受一个单词`lastJoin`和任意数量的`items`。最初，`list`声明了一个`commaSeparated`变量。此变量存储除最后一个元素之外的所有元素的逗号分隔的联合数组。在下一行，我们将`items`中的最后一项存储在一个`lastItem`变量中。然后，该函数使用字符串模板返回。

然后，该函数以列表格式返回字符串形式的`items`。例如:

```
list("and", "red", "green", "blue");     // "red, green and blue"list("with", "red", "green", "blue");    // "red, green with blue"list("or", "red", "green", "blue");      // "red, green or blue"
```

我们的`list`函数允许我们在需要的时候创建列表。我们创建的每种类型的列表，“and”、“with”、“or”都是一般`list`函数的专门化。如果它们可以是自己的函数，那不是很好吗？！

#### 如何使用部分应用程序

这就是局部应用可以有所帮助的地方。例如，要创建一个`listAnd`函数，我们将`lastJoin`参数“设置”(或*部分应用*)为“and”。这样做的结果意味着我们可以像这样调用我们部分应用的函数:

```
listAnd("red", "green", "blue");    // "red, green and blue"
```

也不需要就此打住。我们可以通过对列表函数部分应用参数来创建许多特殊的函数:

```
listOr("red", "green", "blue");      // "red, green or blue"listWith("red", "green", "blue");    // "red, green with blue"
```

为此，我们需要创建一个`partial`实用函数:

```
const partial = (fn, firstArg) => {  return (...lastArgs) => {    return fn(firstArg, ...lastArgs);  }}
```

该函数将函数`fn`作为其第一个参数，将`firstArg`作为其第二个参数。它返回一个只有一个参数的全新函数，`lastArgs`。这将收集传入的参数。

现在，为了创建我们的`listAnd`函数，我们调用`partial`,传入我们的`list`函数和最后一个连接词:

```
const listAnd = partial(list, "and");
```

我们的`listAnd`函数现在只接受一个任意的条目列表。这个函数在被调用时将依次调用传入的`list`函数。我们可以看到，它将通过“和”作为第一个论点和其后聚集的`lastArgs`。

我们现在已经创建了一个部分应用的函数。我们可以在程序中反复使用这个特殊的函数:

```
listAnd("red", "green", "blue");    // "red, green and blue"
```

#### **更进一步**

我们创建的`partial`函数是为了说明部分应用程序是如何工作的。有一些很棒的功能性 JavaScript 库内置了这个工具，比如 [Ramda JS](https://ramdajs.com/docs/#partial) 。

值得注意的是，即使你是部分应用这个概念的新手，你也很有可能一直在使用它。如果你曾经在一个函数上使用过`.bind()`方法，这是一个局部应用的例子。通常的做法是将`this`传递给 bind 来设置它的上下文。在引擎盖下，它部分应用了`this`，并返回了一个新功能。