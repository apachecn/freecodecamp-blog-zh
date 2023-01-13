# JavaScript 中的闭包——用例子解释

> 原文：<https://www.freecodecamp.org/news/javascript-closures-explained-with-example/>

在本文中，我们将讨论 JavaScript 中的闭包。我将向您介绍闭包的定义、一个简单的日常 fetch 实用程序闭包示例，以及使用闭包的一些优点和缺点。

## 目录

*   [先决条件](#prerequisites)
*   [什么是闭包](#what-are-closures)？
*   [闭包的用例](#use-case-of-closure-creating-a-fetch-utility-with-closures)
*   [闭包的优点](#advantages-of-closures)
*   [关闭的缺点](#disadvantages-of-closures)
*   [总结](#summary)

事不宜迟，我们开始吧。

## 先决条件

要理解本文，您应该对以下主题有很好的理解:

*   JavaScript 的[执行上下文](https://www.freecodecamp.org/news/javascript-execution-context-and-hoisting/)如何工作
*   什么是[获取 API](https://www.freecodecamp.org/news/javascript-fetch-api-tutorial-with-js-fetch-post-and-header-examples/) 以及如何使用它

## 什么是闭包？

闭包是这样的函数，即使外部函数不再存在，它也可以访问存在于其作用域链中的变量。

为了更详细地理解这一点，让我们来理解什么是范围链。作用域链是指父作用域不能访问其子作用域中的变量，但是子作用域可以访问父作用域中的变量。

让我们通过下面的例子来更清楚地了解这一点:

```
let buttonProps = (borderRadius) => {
	const createVariantButtonProps = (variant, color) => {
		const newProps = {
			borderRadius,
			variant,
			color
		};
		return newProps;
	}
	return createVariantButtonProps;
}
```

如你所见，我们有一个名为`buttonProps`的函数。这个函数接受`borderRadius`作为参数。让我们考虑将`buttonProps`函数作为我们的父函数。

我们还有另一个已经定义在父函数内部的函数，那就是`createVariantButtonProps`。该函数将接受`variant`和`color`作为参数，并返回一个对象，该对象构成了一个存在于其范围之外的变量`borderRadius`。

但是出现了一个问题，内部函数如何解析出现在父作用域中的变量。

这可以通过词法范围来实现。使用词法作用域，JS 解析器知道如何解析当前作用域中的变量，或者实际上知道如何解析嵌套函数中的变量。

所以根据上面的解释，`createVariantButtonProps`将可以访问其外部函数`buttonProps`中的变量。

在上面的例子中，内部函数`createVariantButtonProps`是一个闭包。为了详细理解闭包，我们将首先了解闭包的特征，如下所示:

*   即使外部函数不再存在，闭包仍然可以访问它的父变量。
*   闭包不能访问它们外部函数的`args`参数。

让我们更详细地了解每一点。

### 即使外部函数不再存在，它仍然可以访问其父变量。

这是任何闭包的基本功能。这是他们的主要人生格言，也是他们的工作原则。

为了看到这一点，我们现在将执行上面的`buttonProps`函数。

```
let primaryButton = buttonProps("1rem"); 
```

调用`buttonProps`函数将返回另一个函数，也就是我们的闭包。

现在让我们执行这个闭包:

```
const primaryButtonProps = primaryButton("primary", "red");
```

一旦闭包被执行，它将返回以下对象:

```
{
   "borderRadius":"1rem",
   "variant":"primary",
   "color":"red"
}
```

这里又出现了一个问题:`primaryButton`函数如何访问它内部不存在的变量`borderRadius`？

如果我们仔细阅读闭包的定义，以及我们之前讨论过的作用域链，它非常适合这个实例。

让我们更深入地探究为什么闭包仍然可以访问在它们的作用域之外定义的变量，即使外部函数不再存在——例如，`borderRadius`？

答案很简单:闭包不存储静态值。相反，它们存储对作用域链中存在的变量的引用。这样，即使外部函数死了，内部函数，也就是闭包，仍然可以访问它的父变量。

## 闭包的用例:用闭包创建获取实用程序

既然我们已经知道了闭包是什么，我们将创建一个很好的通用效用函数。它将使用 REST APIs 处理不同的请求方法，如 GET 和 POST。

对于这个用例，

*   我们将使用 [JSON 占位符 API](https://jsonplaceholder.typicode.com/)。这为我们提供了一些假数据，我们可以使用 REST APIs 对其进行编辑。
*   我们将使用 JavaScript 的 [fetch](https://developer.mozilla.org/en-US/docs/Web/API/fetch) API。

让我们首先讨论为什么我们甚至需要设计这样一个实用程序。有几个原因:

*   对于每个 fetch 调用，我们不想一直定义基本 URL(或其他公共参数)。因此，我们将创建一个机制，将基本 URL/参数存储为一个状态。
*   删除多余的代码。
*   在代码库中提供模块化。

让我们深入了解这个实用程序的细节。我们的 fetch 实用程序将如下所示:

```
const fetchUtility = (baseURL, headers) => {
  const createFetchInstance = (route, requestMethod, data) => {
    const tempReq = new Request(`${baseURL}${route}`, {
      method: requestMethod,
      headers,
      data: data || null
    });
    return [fetch, tempReq];
  };

  return createFetchInstance;
};
```

*   `fetchUtility`接受两个参数，即`baseURL`和`headers`。这些将在随后的闭包中用来构造基本 URL 和头。
*   然后我们有`createFetchInstance`，它接受`route` `requestMethod`和`data`作为参数。
*   接下来，这个函数创建一个新的请求对象，它将使用代码:`${baseURL}${route}`构造我们的 URL。我们还传入一个对象，该对象由请求方法类型、头和数据(如果有的话)组成。
*   然后我们返回获取 API 的实例以及请求对象。
*   最后，我们返回`createFetchInstance`函数。

现在让我们看看这个函数的运行情况。调用我们的`fetchUtility`函数来初始化`baseURL`:

```
const fetchInstance = fetchUtility("https://jsonplaceholder.typicode.com");
```

Initializing the baseURL 

*   如果我们观察，`fetchInstance`现在有了函数`fetchUtility`的闭包的值。
*   接下来，我们将请求的路由和类型传递给闭包`fetchInstance`:

```
const [getFunc, getReq] = fetchInstance("/todos/1", "GET");
```

Executing the closure

正如您所看到的，这返回给我们一个获取 API 实例的数组和我们配置的请求体。

最后，我们可以利用`getFunc` fetch API 来调用请求`getReq`，如下所示:

```
getFunc(getReq)
  .then((resp) => resp.json())
  .then((data) => console.log(data));
```

我们还可以创建一个类似于上面 GET 请求的 POST 请求。我们只需要再次调用`fetchInstance`如下:

```
const [postFunc, postReq] = fetchInstance(
  "/posts",
  "POST",
  JSON.stringify({
    title: "foo",
    body: "bar",
    userId: 1
  })
);
```

为了执行这个 post 请求，我们可以执行与 GET 请求类似的操作:

```
postFunc(postReq)
  .then((resp) => resp.json())
  .then((data) => console.log(data));
```

如果我们仔细观察上面的例子，我们可以看到内部函数`createFetchInstance`可以访问其作用域链中的变量。在词法范围的帮助下，在定义`createFetchInstance`期间，它解析变量名。

这样，闭包在其定义期间引用变量`baseURL`和`headers`，即使在外部函数`fetchUtility`已经不存在之后。

如果我们从不同的角度考虑闭包，那么闭包可以帮助我们维护像`baseURL`和`headers`这样的状态，我们可以在函数调用中使用它们。

## 闭包的优点

以下是闭包的一些优点:

*   它们允许您将变量附加到执行上下文中。
*   闭包中的变量可以帮助您维护一种状态，供以后使用。
*   它们提供数据封装。
*   它们有助于删除冗余代码。
*   他们帮助维护模块化代码。

## 关闭的缺点

过度使用闭包有两个主要缺点:

*   闭包内声明的变量不会被垃圾收集。
*   过多的闭包会降低应用程序的速度。这实际上是由于内存中的代码重复造成的。

## 摘要

所以这样一来，如果你想处理或实现某些设计模式，闭包会非常有用。它们还可以帮助您编写简洁的模块化代码。

如果您喜欢闭包的概念，那么我建议您进一步阅读以下主题:

*   [设计模式](https://www.patterns.dev/posts/classic-design-patterns/)
*   [匿名闭包](https://stackoverflow.com/questions/16032840/javascript-anonymous-closure)

感谢您的阅读！

在 [Twitter](https://twitter.com/keurplkar) 、 [GitHub](https://github.com/keyurparalkar) 和 [LinkedIn](https://www.linkedin.com/in/keyur-paralkar-494415107/) 上关注我。