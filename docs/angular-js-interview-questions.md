# 面试问题

> 原文：<https://www.freecodecamp.org/news/angular-js-interview-questions/>

以下是 AngularJS 访谈中经常被问到的一些概念。

*   AngularJS 是什么？
*   什么是模型视图控制器(MVC)？
*   双向数据绑定
*   什么是依赖注入，它是如何工作的？
*   AngularJS 中的$scope 是什么？
*   AngularJS 中的$rootScope 是什么？
*   如何在 Angular 中实现路由？
*   解释指令
*   如何在 Angular 中创建自定义指令？
*   解释服务和工厂之间的区别
*   解释$q 服务、延期和承诺

# **问答示例**

### 问:列出 AngularJS 中的指令？

Answer: ngBind, ngModel, ngClass, ngApp, ngInit, ngRepeat

### 问题:AngularJS 中的$scope 是什么？

答:AngularJS 中的$scope 是一个对象，它指的是一个应用模型。它是一个将视图(DOM 元素)与控制器绑定的对象。在控制器中，通过$scope 对象访问模型数据。我们知道，AngularJS 支持 MV*模式，$scope 对象成为 MV*的模型。

### 问题:AngularJS 中的 SPA(单页应用)是什么？

答:单页应用程序(SPAs)是加载单个 HTML 页面并在用户与应用程序交互时动态更新页面的 web 应用程序。

SPAs 使用 AJAX 和 HTML 来创建流畅且响应迅速的 web 应用程序，无需不断地重新加载页面。然而，这意味着大部分工作发生在客户端，在 JavaScript 中。

这里的单个 HTML 页面意味着来自服务器的 UI 响应页面。源码可以是 ASP，ASP.NET，ASP.NET MVC，JSP 等等。

然而，单页 web 应用程序是作为一个页面交付给浏览器的，并且通常不需要在用户导航到应用程序的各个部分时重新加载页面。这为最终用户带来了更快的导航、更高效的网络传输和更好的整体性能。

### 问题:AngularJS 中的路由是什么？

答:路由是 AngularJS 的核心特性。这个特性在构建具有多个视图的 spa(单页应用程序)时非常有用。在 SPAs 中，所有视图都是不同的 HTML 文件，我们使用路由来加载应用程序的不同部分。对应用程序进行逻辑划分并使其易于管理是很有帮助的。换句话说，路由帮助我们将应用程序划分成逻辑视图，并将它们与不同的控制器绑定在一起。

### 问:解释 ng-repeat 指令。

答:ng-repeat 指令是 AngularJS 指令中使用最多的特性。它遍历一组条目并创建 DOM 元素。它不断监视数据源，以重新呈现模板来响应变化。

### 问题:ng-If 和 ng-show/ng-hide 有什么区别？

答:ng-If 指令只在条件为真时才呈现 DOM 元素。而 ng-show/ng-hide 指令呈现 DOM 元素，但更改了 ng-hide/ng-show 的类以保持元素在页面上的可见性。

### 问题:如何用 AngularJs 取消超时？

答:$timeout 是 AngularJs 的 window.setTimeout 的包装器，您可以应用以下函数取消超时:

```
$timeout.cancel(function (){
  // write your code.
});
```

### 问:什么是依赖注入？

答:依赖注入(DI)是一种软件设计模式，处理组件如何获得它们的依赖关系。

AngularJS 注射器子系统负责创建组件，解析它们的依赖关系，并根据请求将它们提供给其他组件。

### 问:解释一下 ng-App 指令。

答:ng-app 指令启动一个 AngularJS 应用程序。它定义了根元素。当加载包含 AngularJS 应用程序的网页时，它会自动初始化或引导应用程序。它还用于在 AngularJS 应用程序中加载各种 AngularJS 模块。

### 问:解释 ng-init 指令

答:ng-init 指令初始化 AngularJS 应用程序的数据。它用于为应用程序中使用的变量赋值。

例如，在下面的代码中，我们使用 JSON 语法来定义国家数组，从而初始化了一个国家数组。

```
<div ng-app = "" ng-init = "countries = [{locale:'en-US',name:'United States'}, {locale:'en-GB',name:'United Kingdom'}, {locale:'en-FR',name:'France'}]">
   ...
</div>
```

### 问:如何在控制器之间共享数据？

答:创建一个 AngularJS 服务来保存数据并将其注入控制器内部。使用服务是最干净、最快和最简单的测试方式。

但是，有几种其他方法可以在控制器之间实现数据共享，例如:

*   使用事件
*   使用$parent、nextSibling、controllerAs 等直接访问控制器
*   使用$rootScope 添加数据(不是一个好的实践)

### 问:ng-if 和 ng-show/hide 指令之间有什么区别？

答:ng-if 只会在 DOM 元素的条件为真时创建和显示 DOM 元素。如果条件为假或变为假，它将不会创建或销毁已创建的条件。

ng-show/hide 将始终生成 DOM 元素，但它将基于条件的评估应用 CSS 显示属性。

## 更多关于 AngularJS 的信息:

*   [角度对角度](https://www.freecodecamp.org/news/angular-vs-angularjs/)
*   [最佳角度和角度教程](https://www.freecodecamp.org/news/best-angular-tutorial-angularjs/)