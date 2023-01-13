# 角度 RxJS 深度

> 原文：<https://www.freecodecamp.org/news/angular-rxjs-in-depth/>

在本教程中，我们将学习在 Angular 6 或 Angular 7 中使用 RxJS 6 库。我们将了解:

*   如何导入 Observable 类和其他操作符。
*   如何订阅和退订 Observables？
*   如何导入调用运算符并用`pipe()`函数进行链式连接？
*   我们还将看到如何使用异步管道从 Angular 模板订阅 Observables。
*   最后，我们将看到如何使用一些流行的可管道化操作符，如`tap()`、`map()`和`filter()`，以及它们在 RxJS 6 中的新导入路径。

**注意**:本教程适用于角度 6 和角度 7。

在本教程中，我们将开始了解什么是反应式编程、异步操作和数据流，以及它们与 RxJS 库的关系。然后，我们将通过示例了解 RxJS `Observable`的概念，以及各种类型的可观察值，例如:

*   `Subject`，
*   `BehaviorSubject`和`ReplaySubject`，
*   单播和多播可观测量，
*   冷观和热观等。

接下来，我们将了解什么是 RxJS 运算符，以及一些常见运算符的示例，如`tap()`、`map()`、`filter()`、`share()`等。最后，我们将看到 Angular 如何使用 RxJS Observable 来进行异步编程。

## 什么是反应式编程

![What is Reactive Programming](img/9bd5a9f69217901e131d45ef8cde36d0.png)

让我们从不同的来源来看看反应式编程的定义。

这是 Andre Staltz,[cycle . js](https://cycle.js.org/)(一个可预测代码的功能性和反应性 JavaScript 框架)的创建者对它的定义:

反应式编程是用异步数据流编程

这意味着当您编写处理异步操作和数据流的代码时，您正在进行反应式编程。

现在，这是来自[维基百科](https://en.wikipedia.org/wiki/Reactive_programming)的更深入的定义:

在计算中，反应式编程是一种与数据流和变化传播有关的声明式编程范例。

这意味着反应式编程是一种声明式(相对于过程式)的编程风格，适用于数据流。

关于反应式编程和数据流的详细指南，请查看:[你错过的反应式编程介绍](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)。

**什么是流**

流是反应式编程中的一个基本概念，所以在我们进一步讨论之前，有必要看一下它的定义。

![What is a stream](img/d6d693a0139f5bb51a55b021681207a5.png)

在所有的定义中，我们都见过单词 **stream。**

那么什么是溪流呢？

简单来说:

流是指数据超时的值。

我们稍后会看到，可观测量和流是非常相关的概念。

## 什么是 RxJS

现在，我们已经看到了反应式编程和数据流的概念，让我们看看 RxJS 是什么。

![What is RxJS](img/c2cd5dac26eb44b815c4ff9558e98c09.png)

RxJS 是一个受 web 开发人员欢迎的库。它为处理事件和数据流提供了函数式和反应式编程模式，并且已经集成到许多 web 开发库和框架中，比如 Angular。

RxJS 使得 JavaScript 开发人员使用可组合的可观察对象而不是回调和承诺来编写异步代码变得容易。

RxJS 代表 JavaScript 的反应式扩展，它实际上在 Java、Python、Ruby 和 PHP 等其他编程语言中都有实现。它也适用于 Android 等平台。查看[支持的语言和平台的完整列表](http://reactivex.io/languages.html)。

RxJS v6 是目前 RxJS 的稳定版本，它与 RxJS v5 有许多突破性的变化。你可以从这个官方的[迁移指南](https://github.com/ReactiveX/rxjs/blob/master/docs_app/content/guide/v6/migration.md)中查看更多关于变化以及如何从旧版本迁移的信息。

与之前的 RxJS 5 版本相比，RxJS 6 有许多优势，例如:

*   库的包大小更小，
*   最新版本的性能更好，
*   RxJS 6 Observable 遵循 [Observable Spec 提案](https://github.com/zenparsing/es-observable)，
*   最新版本提供了更好的可调试性，
*   更好的模块化架构，
*   是向后兼容的。

## 如何安装和使用 RxJS

RxJS 是一个 JavaScript 库，这意味着您可以像安装其他库一样安装它:

**通过 npm 使用 RxJS 和 ES6**

在您的项目中，您可以运行以下命令来安装 RxJS:

```
$ npm install rxjs 
```

然后，您可以从`rxjs`包或子包(如`rxjs/operators`)中导入您想要使用的符号:

```
import { Observable, Subscriber } from 'rxjs';
import { tap, map, filter } from 'rxjs/operators'; 
```

我们从`rxjs`导入了`Observable`和`Subscriber`符号，从`rxjs/operators`导入了`tap`、`map`和`filter`运算符。

我们稍后会看到这些符号是什么，以及如何在您的角度应用中使用它们。

**使用来自 CDN 的 RxJS】**

您还可以在 HTML 文档中使用`<script>`来使用来自 [CDN](https://unpkg.com/rxjs/bundles/rxjs.umd.min.js) 的 RxJS:

```
<script src="https://unpkg.com/rxjs/bundles/rxjs.umd.min.js"></script> 
```

**注意**:请注意，在 Angular 6 & 7 中，RxJS 6 已经包含在你的项目中，所以不需要手动安装。

## RxJS 6 中什么是可观察的，观察者和描述

RxJS 使用可观察的概念来处理和使用异步的和基于事件的代码。

异步一词来源于异步。在计算机编程中，下面是来自[维基百科](https://en.wikipedia.org/wiki/Asynchrony_(computer_programming))的异步的定义:

异步，在计算机程序设计中，是指独立于主程序流程的事件的发生以及处理这类事件的方式。这些可能是“外部”事件，例如信号的到达，或者由程序发起的与程序执行同时发生的动作，而不需要程序阻塞来等待结果。

读完这个定义后，您可能已经得出了异步对于计算机和编程有多重要的结论！

让我们简单点！

**异步**代码与**同步**代码相反，后者是你第一次接触编程时思考代码的原始方式。

当你的代码按顺序运行时，也就是按源代码中出现的顺序一条指令接一条指令地运行时，它就是同步的。

例如，让我们考虑这个简单的 JavaScript 代码:

```
const foo = "foo" //1
const bar = "bar" //2
const foobar = foo  +  bar //3
console.log(foobar) //4 
```

浏览器将从第 1 行到第 4 行逐行运行这个同步代码，首先分配`foo`和`bar`变量，连接它们并在控制台中显示`foobar`变量。

JavaScript 还支持编写代码的**异步**方法，这是有意义的，因为你需要在浏览器中响应用户事件，但是当你编写代码时，你实际上并不知道用户何时与你的应用程序交互(以及以什么顺序交互)。

这最初是使用回调来实现的，您需要在代码中定义回调，并指定何时调用它们。

比如下面的异步代码会显示**你点击了按钮！**当用户点击由`mybutton`标识符标识的按钮时:

```
document.getElementById('mybutton').addEventListener('click', () => {
  console.log("You clicked the button!")
}) 
```

`addEventListener()`方法的第二个参数是回调。

您还可以使用回调来处理不涉及 DOM 的异步操作。例如，以下代码可用于向 web 服务器发送 HTTP POST 请求:

```
const xhr = new XMLHttpRequest()
xhr.onreadystatechange = () => {
  if (xhr.readyState === 4) {
    xhr.status === 200 ? console.log(xhr.responseText) : console.error('error')
  }
}
xhr.open('POST', 'your.server.com')
xhr.send() 
```

这就是在 JavaScript 中执行著名的 Ajax 调用的方式。

其实， [Ajax](https://en.wikipedia.org/wiki/Ajax_(programming)) 本身就代表了 **A** 同步**J**avaScript**A**nd**X**ML。

**注意**:发送 HTTP 请求(这是 web 应用程序中的一个常见操作)本质上是一个异步操作，因为请求到达服务器需要时间，然后服务器会向您的客户端应用程序发回一个响应。与此同时，应用程序需要响应其他操作，执行其他任务，并且只在收到服务器响应时才进行处理。

如果你曾经广泛地使用过回调，你会注意到它们的一个问题。他们很难追踪！

当你编写复杂的应用程序时，你通常会以编写多层嵌套的回调函数(回调函数中的回调函数)而告终。这就是所谓的[回调地狱](https://stackoverflow.com/questions/25098066/what-is-callback-hell-and-how-and-why-rx-solves-it)。

现代 JavaScript 引入了其他方法或抽象来处理异步操作(不使用太多回调)，如承诺和 Async/Await。

承诺已在 [ES6](https://www.ecma-international.org/ecma-262/6.0/) (JS 2015)中介绍。

Async/await 已经在 ES8 (JS 2017)中引入，它实际上是承诺之上的语法糖，帮助开发人员以看起来同步的方式编写带有承诺的异步代码。

但是承诺实际上类似于回调，在某种程度上有相同的嵌套问题。

由于开发人员总是在寻找更好的解决方案，我们现在有了使用[观察者](https://en.wikipedia.org/wiki/Observer_pattern)软件模式的可观察对象。

观察者模式是一种软件设计模式，在这种模式中，一个名为 subject 的对象维护一个名为 observer 的依赖者列表，并自动通知它们任何状态变化，通常是通过调用它们的方法之一。[观察者模式](https://en.wikipedia.org/wiki/Observer_pattern)。

Observables 在[react vex](http://reactivex.io/)项目中实现，该项目有多种语言的实现。RxJS 是 JavaScript 实现。

**注意** : Observables 在许多其他库中实现，例如 [zen-observable](https://github.com/zenparsing/zen-observable) 和 [xstream](https://github.com/staltz/xstream) ，但是 RxJS Observables 在 JavaScript 中最受欢迎。

Observables 还不是 JavaScript 的内置特性，但是有一个提议[将它们添加到 EcmaScript 中。](https://tc39.github.io/proposal-observable/)

现在，什么是 RxJS 可观测值？

可观察对象是一个实体，它随着时间的推移异步地发出(或发布)多个数据值(数据流)。

这是来自 [RxJS 文档](https://rxjs.dev/guide/overview)的可观察的定义

Observable 表示未来值或事件的可调用集合的概念。

**观察者和订阅者**

在使用 Observables 时，还会用到一些相关的概念，即**观察者**和**订阅**。

观察者也被称为监听器(或消费者)，因为他们可以监听或订阅来获取观察到的数据。

从 RxJS 文档中:

Observer 是一个回调集合，它知道如何监听被观察对象传递的值。

[订阅](http://reactivex.io/rxjs/class/es6/Subscription.js~Subscription.html)是当你订阅一个可观察对象时返回的对象。它们包含许多方法，比如`unsubscribe()`方法，您可以调用该方法来取消接收来自可观察对象的已发布值。

来自官方文件:

订阅表示可观察的执行，主要用于取消执行。

## RxJS 中的主题是什么

一个[主题](https://rxjs-dev.firebaseapp.com/guide/subject)是一种特殊类型的可观察对象，观察者也可以订阅它来接收发布的值，但是有一个不同:**这些值被多播给许多观察者**。

**注意**:默认情况下，RxJS 可观测是单播。

单播仅仅意味着每个订阅的观察器具有可观察的独立执行，而多播意味着可观察的执行由多个观察器共享。

**注**:被摄体类似于角度事件发射器。

因此，当使用主题而不是普通的观察对象时，所有订阅的观察对象将获得相同的发射数据值。

**注**:受试者也是观察者，也就是说，他们也可以订阅其他可观察数据，并收听已发布的数据。

**冷热可观测量**

与常规的可观测物体不同，这些物体被称为**热点**。一个热的可观察对象甚至在任何观察者订阅它之前就开始发出事件，这意味着如果观察者没有在正确的时间订阅，他们可能会丢失以前发出的值，而**冷的**可观察对象* * * *在至少有一个观察者订阅时开始发出值。

**注意**:你可以用`asObservable()`的方法把一个主体转换成一个可观察的。

## RxJS' `BehaviorSubject`和`ReplaySubject`

RxJS 提供了另外两种类型的科目:`BehaviorSubject`和`ReplaySubject`。

对于普通主题，稍后订阅的观察器将不会收到在订阅之前发出的数据值。在许多情况下，这不是我们想要实现的行为。这可以用`BehaviorSubject`和`ReplaySubject`来解决。

`ReplaySubject`使用一个缓冲区来保存发出的值，并在订阅新的观察器时重新发出。

`BehaviorSubject`的工作方式与`ReplaySubject`相似，但只是重新发出最后一次发出的值。

## 如何创建 RxJS 可观察值

您可以使用`Observable.create()`方法创建一个 RxJS 可观察对象，该方法采用一个带有`observer`参数的函数。然后，您可以订阅返回的可观察实例。

除了静态`create()`方法之外，还有许多其他方法可以创建可观察值:

*   从被调用的实例(源)创建新的可观察对象的实例方法，
*   创建单值可观察值的`of([])`运算符。接下来我们会看到一个例子，
*   `interval(interval)`操作符，它创建一个可观察对象，发出一个无限的数字序列。每个数字以恒定的时间间隔(以秒为单位)发出，
*   [timer()](http://reactivex.io/documentation/operators/timer.html) 运算符，它返回一个可观察值，在指定的时间后，在每个指定的持续时间内按顺序发出数字，
*   从一个承诺或一组值中创建一个可观察值的方法，
*   从 DOM 事件创建可观察对象的`fromEvent()`方法，
*   `ajax()`方法创建了一个发送 Ajax 请求的 Observable。

稍后我们将通过示例来了解这些创建方法。

### 如何订阅 RxJS 观察

在创建了一个`Observable`之后，您可以在返回`Subscription`实例的实例上使用`subscribe()`方法来订阅它。

### RxJS 可观察值的简单示例

现在让我们看一个创建和使用可观察对象的简单例子。

首先让我们创建一个可观察的:

```
let ob$ = Observable.create((observer) => {
    observer.next("A new value!");
}); 
```

我们创建一个`ob$`可观察对象，并定义我们的可观察对象应该在传入方法的主体中执行的逻辑。

在本例中，可观察对象将简单地向**发送一个新值！**对订阅观察者的价值。

**注意**:美元符号只是一种命名变量的惯例，这些变量包含了可观察变量的实例。

我们调用 observer 对象的`next()`方法来通知它可用的值。

**注意**:所有观察者对象都必须有`next()`、`complete()`、`error()`等方法的集合。这使得可观测量可以与它们交流。

被观察对象使用`next()`方法将值(发布值)传递给订阅的观察对象。

接下来，让我们创建一个观察者对象:

```
let observer = {
    next: data => console.log( 'Data received: ', data),
    complete: data => console.log('Completed'),
}; 
```

观察者是一个普通的 JavaScript 对象，它包含诸如`next()`、`complete()`和`error()`之类的方法。这意味着它知道如何从可观察对象那里得到通知。

**注意**:除了`next()`、`complete()`、`error()`之外，您还可以为观察者对象添加其他自定义属性和方法。

最后，让我们订阅我们的`ob$`可观察值并返回一个`Subscription`:

```
let subscription = ob$.subscribe(observer); 
```

一旦您订阅了`ob$` Observable，您将在控制台中获得以下输出:

```
Data received: A new value! 
```

## RxJS 运算符

RxJS 提供了可观察概念的实现，还提供了各种允许您组合可观察对象的操作符。

操作符提供了一种声明性的方式来执行复杂的异步操作。

操作符通过观察发出的值并对它们应用预期的转换来处理源可观察对象，然后返回带有修改值的新可观察对象。

有许多 RxJS 运算符，例如:

*   `tap()`，
*   `map()`，
*   `filter()`，
*   `concat()`，
*   `share()`，
*   `retry()`，
*   `catchError()`，
*   `switchMap()`，
*   以及`flatMap()`等。

## 管道:组合多个运算符

RxJS 提供了两个版本的`pipe()`函数:一个独立的函数和一个`Observable`接口上的方法。

您可以使用`pipe()`函数/方法来组合多个运算符。例如:

```
import { filter, map } from 'rxjs/operators';
const squareOf2 = of(1, 2, 3, 4, 5,6)
  .pipe(
    filter(num => num % 2 === 0),
    map(num => num * num)
  );
squareOf2.subscribe( (num) => console.log(num)); 
```

`of()`方法将从`1, 2, 3, 4, 5,6`数字中创建并返回一个可观察值，而`pipe()`方法将对每个发出的值应用`filter()`和`map()`操作符。

## 如何在角度上使用可观测量

Angular 使用 RxJS Observable 作为其许多 API 的内置类型，例如:

*   `HttpClient`方法返回可观察对象，只有当您订阅返回的可观察对象时，才会发送实际请求。
*   路由器在多个地方使用可观测数据，例如:
*   路由器实例的`[events](https://angular.io/api/router/Router#events)`是监听路由器上事件的可观察对象。
*   此外，`ActivatedRoute`(包含与路由器出口上当前加载的组件相关联的路由信息)具有许多可观察的属性，例如路由参数的`params`和`paramMap`。

让我们假设，您有一个 Angular 组件和作为`router`注入的路由器服务。这个来自 [StackOverflow](https://stackoverflow.com/questions/33520043/how-to-detect-a-route-change-in-angular) 的例子向您展示了如何订阅路由器事件来检测路由变化:

```
import { Component } from '@angular/core'; 
import { Router, Event, NavigationStart, NavigationEnd, NavigationError } from '@angular/router';
@Component({
    selector: 'app-root',
    template: `<router-outlet></router-outlet>`
})
export class AppComponent {
    constructor(private router: Router) {
        this.router.events.subscribe((event: Event) => {
            if (event instanceof NavigationStart) {
                console.log("Navigation start");
            }
            if (event instanceof NavigationEnd) {
                console.log("Navigation end");
            }
            if (event instanceof NavigationError) {

                console.log(event.error);
            }
        });
   }
} 
```

*   反应式表单模块使用反应式编程和观察值来监听用户输入。
*   组件中的`@output()`装饰器接受一个`EventEmitter`实例。`EventEmitter`是 RxJS 可观测的子类。

## 如何在你的角码中使用 RxJS 6 可观测

Angular 对所有异步事件使用 Observables(用 RxJS 库实现)。如果您使用 Angular CLI 6|7，RxJS 6 将默认安装在您的项目中。

否则，您可以使用以下命令通过 npm 安装它:

```
$ npm install rxjs --save 
```

为了能够在代码中使用可观察符号，首先需要导入它:

```
import { Observable } from 'rxjs'; 
```

这是 RxJS 6 中不同于 RxJS 5 的新导入路径。

## 使用 HttpClient 模块和 Observables

默认情况下，新的 Angular `HttpClient`适用于可观测量。`get()`、`post()`、`put()`、`delete()`等方法返回一个可观察接口的实例。

HTTP 请求仅在我们订阅可观察对象时发送。

这是一个发出 HTTP 请求的示例:

```
getItems(): Observable<Item[]> {
   return this.httpClient.get<Item[]>(this.itemUrl);
} 
```

我们假设您已经将`HttpClient`服务注入为 *httpClient* 。

## 使用`Observable`和`AsyncPipe`

Angular `AsyncPipe`订阅 Observable 并返回发射的数据。比如说。假设我们有这个方法:

```
getItems(): Observable {
  this.items$ = this.httpClient.get(this.itemUrl);
} 
```

`items$`变量属于可观察类型<项[] >`。

在组件上调用`getItems()`方法后，我们可以使用组件模板中的`async`管道来订阅返回的可观察对象:

## 订阅 Observables

Observables 用于更好地支持事件处理、异步编程和处理多个值。当您定义一个可观察对象来为消费者发布一些值时，这些值直到您实际订阅该可观察对象时才会发出。

订阅可观察对象的消费者不断接收值，直到可观察对象完成或消费者取消订阅可观察对象。

让我们从定义一个提供更新流的可观察对象开始

## 使用`map()`操作符

`map()`操作符类似于`Array.map()`方法。它让您将可观察到的响应映射到其他值。例如:

```
import { Observable} from 'rxjs';
import { map } from 'rxjs/operators';
getItems(): Observable> {
  return this.aService.getItems().pipe(map(response => response.data));
} 
```

`getItems()`方法返回一个可观察值。我们使用`map()`操作符返回响应对象的`data`属性。

运算符使我们能够将可观察流的响应映射到`data`值。

我们从`rxjs/operators`包中导入可管道化的操作符`map()`,并使用`pipe()`方法(采用可变数量的可管道化操作符)包装该操作符。

## 使用`filter()`操作符

`filter()`操作符类似于`Array.filter()`方法。它让你过滤可观察的流并返回另一个可观察的。例如:

```
import { Observable} from 'rxjs';
import { filter } from 'rxjs/operators';

filter(): Observable<Array<any>> {

  return this.aService.getItems()
    .pipe(
      filter(response => response.code === 200));
} 
```

当 HTTP 响应的状态代码是 200 时，我们使用`filter()`操作符只向可观察流的观察者发出通知。

## 结论

在本教程中，您已经了解了反应式编程、数据流和 RxJS 6。

您已经了解了反应式编程是关于用异步数据流编码的，RxJS 是实现 Observables 和 observer 模式的最流行的实现。

您已经了解了什么是可观察对象——随着时间的推移异步发出或发布值的对象。

您已经了解了观察器和订阅等观察器的相关概念——观察器是监听和使用观察器发布的值的对象，订阅是从`subscribe()`方法返回的对象(它们通常用于取消观察器对观察器的订阅)。

您还学习了特殊类型的观察对象，如主题、行为主题(`BehaviorSubject`)和重放主题(`ReplaySubject`)，以及单播和多播观察对象之间的区别。提醒一下，多播可观察对象在其所有观察者之间共享其执行。

您了解了冷观察值和热观察值——热观察值是指观察值在获得任何订阅之前就开始发布值。

您了解了 RxJS 操作符，这些操作符是用于组合观察值并处理其数据流的方法。

最后，您了解了 Angular 6 & 7 使用 RxJS v6 在许多常用模块(如`HttpClient`、`Router`和`ReactiveForms`)中处理异步操作和 API(而不是回调或承诺)。

这篇[文章](https://www.techiediaries.com/angular-rxjs-tutorial/)最初发表在[技术刊物](https://www.techiediaries.com/)上。