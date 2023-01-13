# 解释了角度视图、布线和 NgModules

> 原文：<https://www.freecodecamp.org/news/angular-vs-angularjs/>

## 角度与角度

angular js(1 . x 版)是一个基于 JavaScript 的开源框架。它是跨平台的，用于开发单页 Web 应用程序(SPWA)。

AngularJS 实现了 MVC 模式来分离逻辑、表现和数据组件。它还使用依赖注入来利用客户端应用程序中的服务器端服务。

angular(2 . x 及以上版本)是一个基于类型脚本的开源框架，用于开发前端 web 应用程序。Angular 有如下特性，比如泛型、静态类型、动态加载，还有一些 ES6 特性。

## **版本历史**

谷歌在 2010 年 10 月 20 日发布了 AngularJS 的初始版本。AngularJS 第一个稳定发布是在 2017 年 12 月 18 日 1.6.8 版本。

Angular 2.0 于 2014 年 9 月 22 日在 ng-Europe 会议上发布。

经过一些修改，Angular 4.0 于 2016 年 12 月发布。角度 4 向后兼容角度 2.0。HttpClient 库是 Angular 4.0 的新特性之一。

Angular 5 发布是在 2017 年 11 月 1 日。对渐进式 web 应用程序(PWAs)的支持是 Angular 4.0 的改进之一。

并且最终在 2018 年 5 月发布了 Angular 6。最新的稳定版本是 [6.1.9](https://blog.angular.io/angular-v6-1-now-available-typescript-2-9-scroll-positioning-and-more-9f1c03007bb6)

## 如何安装

我们可以通过参考可用的资源或下载框架来添加 Angular。

### 链接到源

AngularJS:我们可以参考 Google 的内容交付网络来添加 AngularJS (Angular 1.x 版本)。

```
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script> 
```

下载/安装:我们可以用 npm、Bower 或 composer 下载框架

****棱角分明**JS**1 . x**:**

npm

```
npm install angular
```

然后在你的`index.html`上加一个`<script>`:

```
<script src="/node_modules/angular/angular.js"></script>
```

凉亭

```
bower install angular
```

然后在你的`index.html`上加一个`<script>`:

```
<script src="/bower_components/angular/angular.js"></script>
```

**棱角分明:**

有关文档的更多信息，请参考 [AngularJS](https://docs.angularjs.org/api) 的官方网站。

按照 [Angular](https://angular.io/guide/quickstart) 官方文档中的步骤，可以安装 ****Angular 2.x**** 等版本。

现在让我们多学一点关于角的知识，好吗？

# **简介**

视图提供了必要的抽象层。它们保持角度独立于平台特定的实用程序。作为一种跨平台技术，Angular 使用它的视图与平台连接。

对于 Angular 的模板 HTML 中的每个元素，都有一个对应的视图。Angular 建议通过这些视图与平台进行交互。虽然直接操纵仍然是可能的，Angular 警告不要这样做。Angular 提供了自己的应用编程接口(API)来代替本地操作。

回避特定于平台的 API 的视图有其后果。在 web 浏览器中开发 Angular 时，元素存在于两个地方:DOM 和视图。只处理 DOM 不会影响视图。

由于角度不与平台接触，这就产生了不连续性。视图应该与平台一一对应。否则 Angular 会浪费资源来管理与之不匹配的元素。这在删除元素的情况下是很可怕的。

这些差异使得视图显得没有必要。不要忘记 Angular 首先是一个通用的开发平台。为此，视图是必要的抽象。

通过坚持视图，Angular 应用程序将在所有支持的开发平台上运行。平台包括 Web、Android 和苹果 iOS。

#### **注**

从现在开始，本文假设一个 web 浏览器环境。请随意用更适合您的首选平台的东西来替换 DOM。

## 什么是视图？

视图几乎就像它们自己的虚拟 DOM。每个视图都包含对 DOM 中相应部分的引用。视图中的节点反映了本部分中的内容。Angular 为每个 DOM 元素分配一个视图节点。每个节点都有一个对匹配元素的引用。

当 Angular 检查更改时，它会检查视图。Angular 避免引擎盖下的 DOM。视图代表它引用 DOM。还有其他机制可以确保视图变化呈现到 DOM 中。相反，对 DOM 的更改不会影响视图。

同样，视图在除 DOM 之外的所有开发平台上都是通用的。即使是为一个平台开发，视图仍然被认为是最佳实践。他们保证 Angular 对 DOM 有正确的解释。

第三方库中可能不存在视图。直接 DOM 操作是这种场景的一个出口。当然，不要期望应用程序能够跨平台运行。

### 视图类型

有两种主要类型的视图:嵌入视图和宿主视图。

还存在视图容器。它们包含嵌入视图和宿主视图，通常被称为简单的“视图”。

每个`@Component`类用 Angular 注册一个视图容器(view)。新组件生成一个针对特定 DOM 元素的定制选择器。视图会附加到该元素出现的任何位置。Angular 现在知道查看视图模型时零部件的存在。

主体视图附加到使用工厂动态创建的组件。工厂为视图实例化提供了蓝图。这样，应用程序可以在运行时实例化组件的主机视图。根据组件的实例化，主机视图附加到组件的包装器。该视图存储描述传统组件能力的数据。

`<ng-template></ng-template>`是一个类似于 HTML5 `<template></template>`的元素。Angular 的`ng-template`使用嵌入式视图。与宿主视图不同，这些视图不附加到 DOM 元素。它们与宿主视图相同，因为它们都存在于视图容器中。

请记住，`ng-template`不是一个 DOM 元素。它稍后被注释掉，只留下嵌入的视图节点。

差异取决于输入数据；嵌入式视图不存储组件数据。它们将一系列元素存储为构成其模板的节点。模板组成了`ng-template`的所有 innerHTML。嵌入式视图中的每个元素都是它自己独立的视图节点。

### 主体视图和容器

主机视图*主机动态组件*。视图容器(视图)会自动附加到模板中已有的元素。视图可以附加到组件类特有的元素之外的任何元素。

想想传统的组件生成方法。首先创建一个类，用`@Component`修饰它，并填充元数据。这种方法适用于模板的任何预定义组件元素。

尝试使用 Angular 命令行界面(CLI)命令:`ng generate component [name-of-component]`。它产生以下结果。

```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css']
})
export class ExampleComponent implements OnInit {
  constructor() { }

  ngOnInit() { }
}
```

这就创建了带有选择器`app-example`的组件。这将视图容器附加到模板中的`<app-example></app-example>`。如果这是应用程序的根，它的视图将封装所有其他视图。从 Angular 的角度来看，根视图标志着应用程序的开始。

动态创建组件并在角度视图模型中注册它们需要一些额外的步骤。结构化指令帮助管理动态内容(`*ngIf, *ngFor, and *ngSwitch…`)。然而，指令不能扩展到更大的应用程序。太多的结构指令会使模板变得复杂。

这就是从现有的类逻辑实例化组件派上用场的地方。这些组件需要创建一个可以插入视图模型的主体视图。主体视图保存构件的数据，以便 Angular 识别它们的结构用途。

### 主机视图续

每个组件都有一个类定义。然而 JavaScript 不支持类。类是语法糖。相反，它们产生包含组件工厂的函数。

工厂充当宿主视图的蓝图。他们构建视图来代表他们的组件与 Angular 接口。宿主视图附加到 DOM 元素。从技术上讲，任何元素都可以，但最常见的目标是`<ng-component></ng-component>`。

用于保存视图的视图容器(视图)必须首先存在。`<ng-container></ng-container>`是一个连接视图容器的好地方。视图容器是同样类型的视图，也适用于模板类元素。

来自`@angular/core`的一些助手和参考资料提供了其他需要的工具。下面的例子将所有这些放在一起。

```
// another.component.ts

import { Component } from '@angular/core';

@Component({
  template: `
  <h1>Another Component Content</h1>
  <h3>Dynamically Generated!</h3>
  `
})
export class AnotherComponent { }
```

```
// example.component.ts

import { AfterViewInit, Component, ViewChild,
ViewContainerRef, ComponentFactoryResolver } from '@angular/core';
import { AnotherComponent } from './another.component';

@Component({
  selector: 'app-example',
  template: `
  <h1>Application Content</h1>
  <ng-container #container></ng-container>
  <h3>End of Application</h3>
  `,
  entryComponents: [ AnotherComponent ]
})
export class ExampleComponent implements AfterViewInit {
  @ViewChild("container", { read: ViewContainerRef }) ctr: ViewContainerRef;

  constructor(private resolve: ComponentFactoryResolver) { }

  ngAfterViewInit() {
    const factory = this.resolve.resolveComponentFactory(AnotherComponent);
    this.ctr.createComponent(factory);
  }
}
```

假设 AnotherComponent 和 ExampleComponent 都在同一个模块下声明。AnotherComponent 是一个简单的类组件，动态添加到 ExampleComponent 的视图中。示例组件的`entryComponents`元数据必须包含用于[引导](https://angular.io/guide/bootstrapping)的另一个组件。

当 ExampleComponent 是模板的一部分时，AnotherComponent 保持分离。它从 ExampleComponent 动态呈现到模板中。

这里有两个视图容器:`<app-example></app-example>`和`<ng-container></ng-container>`。本例的主机视图将插入到`ng-container`中。

在`@ViewChild`查询完成后，`AfterViewInit`生命周期挂钩触发。使用*模板引用变量* `#container`，`@ViewChild`引用`ng-container`为`ctr`。

`ViewContainerRef`是视图容器(视图)的引用类型。`ViewContainerRef`引用一个支持插入其他视图的视图。`ViewContainerRef`包含更多的方法来管理它所包含的视图。

通过依赖注入，构造器实例化 Angular 的`ComponentFactoryResolver`服务的一个实例。该服务提取另一个组件的工厂功能(主机视图蓝图)。

`createComponent`的单参数需要一个工厂。`createComponent`函数源于`ViewContainerRef`。它在从组件工厂派生的主机视图下实例化另一个组件。

然后，主体视图插入到视图容器中。`<ng-component></ng-component>`将组件包装在视图容器中。它附加了前面提到的主机视图。`ng-component`是主机视图与 DOM 的连接。

从组件动态创建宿主视图还有其他方法。其他方式往往[侧重于优化](https://blog.angularindepth.com/working-with-dom-in-angular-unexpected-consequences-and-optimization-techniques-682ac09f6866)。

`ViewContainerRef`拥有强大的 API。它可以管理任意数量的视图，无论是宿主视图还是嵌入视图。API 包括视图操作，如插入、移动和删除。这让您可以通过 Angular 的视图模型操作 DOM。这是最佳实践，以便 Angular 和 DOM 相互匹配。

### 嵌入式视图

注意:嵌入视图附加到其他视图，没有添加输入。宿主视图附加到 DOM 元素，来自宿主视图的输入数据将其描述为一个组件。

结构化指令在一大块 HTML 内容周围创建一个 [`ng-template`。指令的 host 元素附加了一个视图容器。这使得内容可以有条件地呈现到其预期的布局中。](https://angular.io/guide/structural-directives#the-asterisk--prefix)

`ng-template`包含表示 innerHTML 中每个元素的嵌入式视图节点。`ng-template`绝不是 DOM 元素。它会将自己注释掉。标签定义了其嵌入视图的范围。

### 嵌入式视图(续)

实例化一个嵌入式视图除了它自己的引用之外不需要任何外部资源。`@ViewChild`查询可以获取这些信息。

有了模板引用，从它调用`createEmbeddedView`就可以了。引用的 innerHTML 成为它自己的嵌入视图实例。

在下一个例子中，`<ng-container></ng-container>`是一个视图容器。`ng-container`在编译时被注释掉，就像`ng-template`一样。因此，它为插入嵌入式视图提供了一个出口，同时保持 DOM 精简。

嵌入式视图模板插入到`ng-container`的布局位置。除了视图容器之外，这个新插入的视图没有额外的视图封装。请记住它与主机视图的不同之处(主机视图附加到它们的`ng-component`元素包装器)。

```
import { Component, AfterViewInit, ViewChild,
ViewContainerRef, TemplateRef } from '@angular/core';

@Component({
  selector: 'app-example',
  template: `
  <h1>Application Content</h1>
  <ng-container #container></ng-container> <!-- embed view here -->
  <h3>End of Application</h3>

  <ng-template #template>
    <h1>Template Content</h1>
    <h3>Dynamically Generated!</h3>
  </ng-template>
  `
})
export class ExampleComponent implements AfterViewInit {
  @ViewChild("template", { read: TemplateRef }) tpl: TemplateRef<any>;
  @ViewChild("container", { read: ViewContainerRef }) ctr: ViewContainerRef;

  constructor() { }

  ngAfterViewInit() {
    const view =  this.tpl.createEmbeddedView(null);
    this.ctr.insert(view);
  }
}
```

`@ViewChild`查询*模板参考变量*T1。这提供了一个`TemplateRef`类型的模板引用。`TemplateRef`持有`createEmbeddedView`功能。它将模板实例化为嵌入式视图。

`createEmbeddedView`的单个参数用于上下文。如果你想传入额外的元数据，你可以在这里作为一个对象。这些字段应该与`ng-template`属性(`let-[context-field-key-name]=“value”`)匹配。通过`null`表示不需要额外的元数据。

第二个`@ViewChild`查询提供了对`ng-container`的引用作为`ViewContainerRef`。嵌入式视图只附加到其他视图，而不是 DOM。`ViewContainerRef`引用嵌入视图中的视图。

嵌入式视图也可以插入到`<app-example></app-example>`的组件视图中。这种方法将视图定位在 ExampleComponent 视图的最末端。然而，在这个例子中，我们希望内容显示在`ng-container`所在的正中间。

`ViewContainerRef` `insert`函数*将*嵌入的视图插入`ng-container`。视图内容将 ups 显示在 ExampleComponent 视图中间的预定位置。

## 结论

不建议用特定于平台的方法操作 DOM。创建和管理一组紧凑的视图可以使 Angular 和 DOM 保持在同一页面上。更新视图通知 Angular DOM 的当前状态。对视图的更新也会延续到 DOM 显示的内容中。

Angular 为视图交互提供了一个灵活的 API。由于这个抽象层次，开发独立于平台的应用程序成为可能。当然，依赖平台策略的诱惑依然存在。除非你有很好的理由不这么做，否则尽量坚持 API Angular 提供的观点。这将在所有平台上产生可预测的结果。

# **角度路由**

路由至关重要。许多现代 web 应用程序在一个页面上承载了太多的信息。用户也不必滚动浏览整个应用程序的内容。一个应用程序需要把自己分成不同的部分。

用户优先考虑必要的信息。路由帮助他们找到具有这些信息的应用部分。对其他用户有用的任何其他信息可能存在于完全独立的路径上。通过路由，两个用户都可以快速找到他们需要的东西。无关的细节隐藏在无关的路线后面。

路由擅长排序和限制对应用程序数据的访问。敏感数据不应向未经授权的用户显示。在每条路线之间，应用程序可能会介入。它可以检查用户的会话以进行身份验证。这种检查决定了该路线应该渲染什么。路由为开发人员提供了在继续之前验证用户的绝佳机会。

创建路线列表也有助于组织。就开发而言，它让开发人员在不同的部分进行思考。用户也从中受益，但更多的是开发者在浏览应用程序代码时受益。编程路由器列表描绘了应用程序前端的精确模型。

至于 Angular，routing 在框架内占据了自己的整个库。所有现代前端框架都支持路由，Angular 也不例外。路由发生在客户端，使用哈希或位置路由。两种风格都允许客户端管理自己的路由。在初始请求之后，不需要来自服务器的额外帮助。

web 浏览器很少使用客户端路由刷新。尽管没有刷新，但书签、历史记录和地址栏等 Web 浏览器实用程序仍然可以工作。这带来了流畅的路由体验，不会搞乱浏览器。当路由到不同的页面时，不再有跳动的页面重新加载。

Angular 在用于路由的核心技术上增加了一个抽象层。本文旨在解释这种抽象。Angular 中存在两种路由策略:路径定位和哈希。本文主要关注路径定位策略，因为它是默认选项。

此外，在全面发布 [Angular Universal](https://universal.angular.io/) 之后，路径定位可能会弃用哈希路由。不管怎样，这两种策略在实现上非常相似。学一个学另一个。是时候开始了！

## 路由器模块设置

从`@angular/router`可用的`RouterModule`导出路由实用程序。它不是核心库的一部分，因为不是所有的应用程序都需要路由。引入路由的最传统方式是作为它自己的[功能模块](https://angular.io/guide/feature-modules)。

随着路由复杂性的增加，将它作为自己的模块将促进根模块的简单性。在不牺牲功能的情况下保持简单就是好的模块设计。

```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

import { AComponent } from '../../components/a/a.component';
import { BComponent } from '../../components/b/b.component';

// an array of soon-to-be routes!
const routes: Routes = [];

@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ]
})
export class AppRoutingModule { }
```

`.forRoot(...)`是 RouterModule 类中可用的类函数。该函数接受一个由`Route`对象组成的数组作为`Routes`。`.forRoot(...)`为快速加载配置路由，而其替代`.forChild(...)`为慢速加载配置路由。

急切加载意味着路由从一开始就将其内容加载到应用程序中。延迟加载按需发生。本文的重点是急切加载。这是加载应用程序的默认方法。RouterModule 类定义类似于下一个代码块。

```
@NgModule({
  // … lots of metadata ...
})
export class RouterModule {
  forRoot(routes: Routes) {
    // … configuration for eagerly loaded routes …
  }

  forChild(routes: Routes) {
    // … configuration for lazily loaded routes …
  }
}
```

不要担心示例用注释省略的配置细节。现在有个大概的了解就行了。

注意 AppRoutingModule 是如何在导出 RouterModule 的同时导入它的。假定 AppRoutingModule 是一个特征模块，这是有意义的。它作为特征模块导入到根模块中。它向根组件树公开 RouterModule 指令、接口和服务。

这解释了为什么 AppRoutingModule 必须导出 RouterModule。这样做是为了根模块的底层组件树。它需要访问那些路由实用程序！

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { AComponent } from './components/a/a.component';
import { BComponent } from './components/b/b.component';
import { AppRoutingModule } from './modules/app-routing/app-routing.module';

@NgModule({
  declarations: [
    AppComponent,
    AComponent,
    BComponent
  ],
  imports: [
    AppRoutingModule, // routing feature module
    BrowserModule
  ],
  providers: [],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

适当的模块令牌从最顶端导入。它的令牌插入到根模块的导入数组中。根组件树现在可以利用 RouterModule 库。这包括已经提到的指令、接口和服务。非常感谢 AppRoutingModule 导出 RouterModule！

RouterModule 实用程序对于根的组件来说很方便。AppComponent 的基本 HTML 使用了一个指令:`router-outlet`。

```
<!-- app.component.html -->

<ul>
  <!-- routerLink(s) here -->
</ul>
<router-outlet></router-outlet>
<!-- routed content appends here (AFTER THE ELEMENT, NOT IN IT!) -->
```

`routerLink`是 RouterModule 的属性指令。一旦路线建立，它将连接到`<ul></ul>`的每个元素。`router-outlet`是一个行为有趣的组件指令。它更多地充当显示路由内容的标记。路由内容是导航到特定路线的结果。通常这意味着在适当的模块中配置一个单独的组件

路由的内容在`<router-outlet></router-outlet>`之后立即呈现。没有任何东西在里面渲染。这并没有造成太大的差别。也就是说，不要期望`router-outlet`表现得像一个路由内容的容器。它只是一个标记，用于将路由的内容附加到文档对象模型(DOM)中。

## 基本路由

上一节建立了路由的基本设置。在实际的路由发生之前，还有一些事情必须解决

要解决的第一个问题是这个应用程序将使用什么路由？有两个组件:a 组件和 b 组件。每个人都应该有自己的路线。它们可以根据当前的路线位置从 AppComponent 的`router-outlet`进行渲染。

路由位置(或路径)通过一系列斜杠(`/`)定义了什么附加到一个[网站的起点](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Origin)(例如 [http://localhost:4200](http://localhost:4200/) )。

```
// … same imports from before …

const routes: Routes = [
  {
    path: 'A',
    component: AComponent
  },
  {
    path: 'B',
    component: BComponent
  }
];

@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ]
})
export class AppRoutingModule { }
```

`http://localhost:4200/A`从 AppComponent 的`router-outlet`呈现一个组件。`http://localhost:4200/B`渲染 b 组件。您需要一种不使用地址栏就能到达这些位置的方法。应用程序不应该依赖网页浏览器的地址栏来导航。

全局 CSS(层叠样式表)补充了它下面的 HTML。应用程序的路由器链接应该有一个令人愉快的外观。这个 CSS 也适用于所有其他的例子。

```
/* global styles.css */

ul li {
  cursor: pointer;
  display: inline-block;
  padding: 20px;
  margin: 5px;
  background-color: whitesmoke;
  border-radius: 5px;
  border: 1px solid black;
}

ul li:hover {
  background-color: lightgrey;
}
```

```
<!-- app.component.html -->

<ul>
  <li routerLink="/A">Go to A!</li>
  <li routerLink="/B">Go to B!</li>
</ul>
<router-outlet></router-outlet>
```

这是基本路由！单击任何一个路由器链接元素都会路由网址。它会重新分配它，而不刷新 web 浏览器。Angular 的`Router`将路由地址映射到 AppRoutingModule 中配置的`Routes`。它将地址匹配到数组中单个`Route`对象的`path`属性。第一个匹配总是赢，所以匹配所有路线应该位于`Routes`数组的最末端。

匹配-所有路由防止应用程序在无法匹配当前路由时崩溃。这可以从地址栏中发生，用户可以在地址栏中键入任何路线。为此，Angular 提供了一个接受所有路由的通配符路径值`**`。此路径通常会呈现一个 PageNotFoundComponent 组件，显示“错误 404:找不到页面”。

```
// … PageNotFoundComponent imported along with everything else …

const routes: Routes = [
  {
    path: 'A',
    component: AComponent
  },
  {
    path: 'B',
    component: BComponent
  },
  {
    path: '',
    redirectTo: 'A',
    pathMatch: 'full'
  },
  {
    path: '**',
    component: PageNotFoundComponent
  }
];
```

包含`redirectTo`的`Route`对象阻止 PageNotFoundComponent 作为`http://localhost:4200`的结果进行渲染。这是应用程序的归途。为了解决这个问题，`redirectTo`将回家的路线改为`http://localhost:4200/A`。`http://localhost:4200/A`间接成为应用程序的新 home route。

`pathMatch: 'full'`告诉`Route`对象匹配归途(`http://localhost:4200`)。它与空路径相匹配。

这两个新的`Route`对象位于数组的末尾，因为第一次匹配获胜。最后一个数组元素(`path: '**'`)总是匹配的，所以它排在最后。

在继续之前，还有最后一件事值得一提。用户如何知道他或她在应用程序中相对于当前路线的位置？当然，可能有特定于路线的内容，但是用户应该如何建立这种联系呢？应该有某种形式的突出应用于路由器链接。这样，用户将知道哪个路由对于给定的网页是活动的。

这很容易解决。当你点击一个`routerLink`元素时，Angular 的`Router`将*焦点*分配给它。这种聚焦可以触发某些向用户提供有用反馈的样式。`routerLinkActive`指令可以为开发者追踪这个焦点。

```
<!-- app.component.html -->

<ul>
  <li routerLink="/A" routerLinkActive="active">Go to A!</li>
  <li routerLink="/B" routerLinkActive="active">Go to B!</li>
</ul>
<router-outlet></router-outlet>
```

`routerLinkActive`的右赋值表示一串类。这个例子只描述了一个类(`.active`)，但是可以应用任意数量的空格分隔的类。当`Router`将*焦点*分配给 routerLink 时，空格分隔的类应用于主机元素。当焦点转移时，这些类会自动移除。

```
/* global styles.css */

.active {
  background-color: lightgrey !important;
}
```

用户现在可以很容易地识别当前路线和页面内容是如何重合的。`lightgrey`高亮显示适用于匹配当前路线的路线链接。`!important`确保突出显示覆盖内嵌样式。

## 参数化路线

路由不必完全硬编码。它们可以包含从对应于`Route`对象的组件引用的动态变量。在编写路线路径时，这些变量被声明为参数。

为了匹配特定的`Route`，路由参数可以是可选的，也可以是强制的。这取决于路由如何写它的参数。存在两种策略:矩阵和传统参数化。

传统的参数化从 AppRoutingModule 中配置的`Routes`数组开始。

```
const routes: Routes = [
  // … other routes …
  {
    path: 'B',
    component: BComponent
  },
  {
    path: 'B/:parameter',
    component: BComponent
  },
  // … other routes …
];
```

关注两条 b 组分路线。参数化最终将出现在两条路线中。

传统的参数化发生在第二个 b 组件`Route`中。`B/:parameter`包含`parameter`参数，如`:`所示。冒号后面的内容标记了参数的名称。第二个 b 组件`Route`匹配需要`parameter`参数。

`parameter`读入任何传入路由的值。路由到`http://localhost:4200/B/randomValue`将为`parameter`分配`randomValue`的值。该值可以包括除另一个`/`之外的任何内容。例如，`http://localhost:4200/B/randomValue/blahBlah`不会触发第二个 b 组件`Route`。而是呈现 PageNotFoundComponent。

BComponent 可以从其组件类中引用路由参数。两种参数化方法(矩阵和传统)在 BComponent 中产生相同的结果。在查看 BComponent 之前，请检查下面参数化的矩阵形式。

```
// app.component.ts

import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html'
})
export class AppComponent {
  constructor(private router: Router) { }

  routeMatrixParam(value: string) {
    if (value)
      this.router.navigate(['B', { parameter: value }]); // matrix parameter
    else
      this.router.navigate(['B']);
  }

  routeAddressParam(value: string) {
    this.router.navigate(['B', value]);
  }
}
```

Angular 的依赖注入系统提供了`Router`的实例化。这允许组件以编程方式进行路由。`.navigate(...)`函数接受一个值数组，该数组解析为一个*可路由的*路径。类似`.navigate(['path', 'to', 'something'])`的东西解析为`http://localhost:4200/path/to/something`。`.navigate(...)`在将数组规范化为*可路由*路径时添加路径定界`/`标记。

第二种形式的参数化出现在`routeMatrixParam(...)`中。看到这行代码:`this.router.navigate(['B', { parameter: value }])`。这种形式的`parameter`是一个矩阵参数。它的值对于要匹配的第一个 b 组件`Route`(`/B`)是可选的。不管路径中是否存在参数，`Route`都会匹配。

`routeAddressParam(...)`解析与`http://localhost:4200/B/randomValue`参数化方法相匹配的路线。这种传统策略需要一个参数来匹配第二个 b 组件路线(`B/:parameter`)。

矩阵策略涉及`routeMatrixParam(...)`。无论路径中是否有矩阵参数，第一个 b 组件路径仍然匹配。与传统方法一样，`parameter`参数传递给 BComponent。

为了充分理解上面的代码，下面是相应的模板 HTML。

```
// app.component.html

<ul>
  <li routerLink="/A">Go to A!</li>
  <li>
    <input #matrixInput>
    <button (click)="routeMatrixParam(matrixInput.value)">Matrix!</button>
  </li>
  <li>
    <input #paramInput>
    <button (click)="routeAddressParam(paramInput.value)">Param!</button>
  </li>
</ul>
<router-outlet></router-outlet>
```

在模板中，值被接受为文本输入。输入将其作为参数注入到路由路径中。每个参数化策略(传统和矩阵)都有两组独立的框。所有的部分都准备好了，是时候检查 BComponent 组件类了。

```
// b.component.ts

import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, ParamMap } from '@angular/router';

@Component({
  selector: 'app-b',
  template: `
  <p>Route param: {{ currParam }}</p>
  `
})
export class BComponent implements OnInit {
  currParam: string = "";

  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
    this.route.params.subscribe((param: ParamMap) => {
      this.currParam = param['parameter'];
    });
  }
}
```

b 组件由两个 b 组件路径中的任一个在适当的模块中产生。`ActivatedRoute`实例化为一组关于当前路线的有用信息。也就是导致 BComponent 呈现的路由。`ActivatedRoute`通过针对类构造函数的依赖注入进行实例化。

`ActivatedRoute.params`的`.params`字段返回一个发出路线参数的`Observable`。注意这两种不同的参数化方法是如何产生`parameter`参数的。返回的`Observable`在`ParamMap`对象中将其作为键值对发出。

在这两种参数化方法中，`parameter`参数的解析结果相同。尽管采用了参数化方法，该值还是从`ActivatedRoute.params`发出。

地址栏区分每种方法的最终结果。矩阵参数化(可选用于`Route`匹配)产生地址:`http://localhost:4200/B;parameter=randomValue`。传统参数化(`Route`匹配所需)产生:`http://localhost:4200/B/randomValue`。

无论哪种方式，都会产生相同的 b 组件。实际差异:不同的 b 组件`Route`匹配。这完全取决于参数化策略。矩阵方法确保参数对于`Route`匹配是可选的。传统的方法需要它们。

## 嵌套路由

`Routes`可能形成等级制度。在 DOM 中，这涉及到一个父`router-outlet`呈现至少一个子`router-outlet`。在地址栏里是这样的:`http://localhost/parentRoutes/childRoutes`。在`Routes`配置中，`children: []`属性将`Route`对象表示为具有嵌套(子)路由。

```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

import { NestComponent } from '../../components/nest/nest.component';
import { AComponent } from '../../components/nest/a/a.component';
import { BComponent } from '../../components/nest/b/b.component';

const routes: Routes = [
  {
    path: 'nest',
    component: NestComponent,
    children: [
      { path: 'A', component: AComponent },
      { path: 'B', component: BComponent }
    ]
  }
];

@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ]
})
export class AppRoutingModule { }
```

```
// nest.component.ts

import { Component } from '@angular/core';

@Component({
  selector: 'app-nest',
  template: `
  <ul>
    <li routerLink="./A">Go to A!</li>
    <li routerLink="./B">Go to B!</li>
  </ul>
  <router-outlet></router-outlet>
  `
})
export class NestComponent { }
```

NestComponent 在从 AppComponent 中的另一个根级别`router-outlet`呈现自身之后呈现一个`router-outlet`。NestComponent 模板的`router-outlet`可以呈现 AComponent ( `/nest/A`)或 BComponent ( `/nest/B`)。

AppRoutingModule 反映了 NestComponent 的`Route`对象中的这种嵌套。字段保存了一个由`Route`对象组成的数组。这些`Route`对象也可以在它们的`children: []`字段中嵌套路由。无论嵌套的路线有多少层，这种情况都可以继续。上面的例子显示了两层嵌套。

与`/`相比，每个`routerLink`包含一个`./`。`.`确保 routerLink 附加到路由路径。否则，路由器链接会完全替换该路径。路由到`/nest`后，`.`扩展成`/nest`。

这对于从`.nest`路由到`/nest/A`或`/nest/B`很有用。`A`和`B`构成了`/nest`的嵌套路线。路由到`/A`或`/B`返回 PageNotFound。`/nest`必须预先计划好两条路线。

看一看在模板中包含根级别`router-outlet`的 AppComponent。AppComponent 是嵌套的第一层，而 NestComponent 是第二层。

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
  <ul>
    <li routerLink="/nest">Go to nested routes!</li>
    <li routerLink="/">Back out of the nested routes!</li>
  </ul>
  <router-outlet></router-outlet>
  `
})
export class AppComponent { }
```

在嵌套`Route`对象中，`children: []`包含两个嵌套的路径。如前所述，当从`/nest`路由时，它们产生 a 组件和 b 组件。为了便于演示，这些组件非常简单。`<li routerLink="/">...</li>`允许您导航出嵌套路线，通过导航至本地路线来重置示例。

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-a',
  template: `
  <p>a works!</p>
  `
})
export class AComponent { }
```

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-b',
  template: `
  <p>b works!</p>
  `
})
export class BComponent { }
```

数组接受对象作为元素。`children: []`同样可以适用于这些要素中的任何一个。这些元素的子元素可以继续嵌套。无论嵌套多少层，这种模式都可以继续。为每一层嵌套布线在模板中插入一个`router-outlet`。

不管`Route`对象的嵌套级别如何，路由技术都适用。参数化技术只有一个方面不同。子路由只能通过`ActivatedRoute.parent.params`访问其父路由的参数。`ActivatedRoute.params`目标是同一级别的嵌套路由。这不包括父级路由及其参数。

守卫特别适合嵌套路由。一个`Route`对象可以限制对其所有嵌套(子)路由的访问。

## 守卫路线

Web 应用程序通常由公共和私有数据组成。这两种类型的数据都有自己的页面，有*保护的*路由。这些路由根据用户的权限允许/限制访问。未经授权的用户可能会与受保护的路由进行交互。如果用户试图访问路由内容，路由应该阻止他或她。

Angular 提供了一个可以附加到任何路由的身份验证保护包。这些方法根据用户与受保护路线的交互方式自动触发。

*   `canActivate(...)` -当用户试图访问路线时触发
*   `canActivateChild(...)` -当用户试图访问路由的嵌套(子)路由时触发
*   `canDeactivate(...)` -当用户试图离开路线时触发

Angular 的守卫方法从`@angular/router`开始可用。为了帮助他们进行身份验证，他们可以选择接收一些参数。这些参数不会通过依赖注入来注入。在幕后，每个值都作为参数传递给被调用的 guard 方法。

*   `ActivatedRouteSnapshot` -对三者都可用
*   `RouterStateSnapshot` -对三者都可用
*   `Component` -适用于`canDeactivate(...)`

`ActivatedRouteSnapshot`提供对受保护路线的路线参数的访问。`RouterStateSnapshot`暴露匹配路由的 URL(统一资源定位符)网址。`Component`引用由路线渲染的组件。

为了保护路由，实现保护方法的类首先需要作为服务存在。服务可以注入适当的模块来保护它的`Routes`。服务的令牌值可以注入到任何一个`Route`对象中。

```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

import { AuthService } from '../../services/auth.service';
import { UserService } from '../../services/user.service';

import { PrivateNestComponent } from '../../components/private-nest/private-nest.component';
import { PrivateAComponent } from '../../components/private-nest/private-a/private-a.component';
import { PrivateBComponent } from '../../components/private-nest/private-b/private-b.component';

const routes: Routes = [
  {
    path: 'private-nest',
    component: PrivateNestComponent,
    canActivate: [ AuthService ], // !!!
    canActivateChild: [ AuthService ], // !!!
    canDeactivate: [ AuthService ], // !!!
    children: [
      { path: 'private-A', component: PrivateAComponent },
      { path: 'private-B', component: PrivateBComponent }
    ]
  }
];

@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ],
  providers: [
    AuthService,
    UserService
  ]
})
export class AppRoutingModule { }
```

`canActivate`、`canActivateChild`、`canDeactivate`从 AuthService 实现。服务实现将与用户服务实现一起显示。

UserService 提供认证用户所需的信息。AuthService guard 方法实现执行身份验证。AppRoutingModule 必须将这两个服务包含到其 providers 数组中。这是为了让模块的注入器知道如何实例化它们。

嵌套路线存在于`/private-nest`路径之外。`/private-nest`的`Route`对象包含更多的新字段。它们的名字应该看起来很熟悉，因为它们反映了相应的保护方法。

当被触发时，每个字段在服务内部触发与其同名的方法实现。任何数量的服务也可以填充这个数组。测试每个服务的方法实现。它们必须返回一个布尔值或一个发出布尔值的`Observable`。

请参见下面的 AuthService 和 UserService 实现。

```
// user.service.ts

import { Injectable } from '@angular/core';
import { Router } from '@angular/router';

class TheUser {
  constructor(public isLoggedIn: boolean = false) { }

  toggleLogin() {
    this.isLoggedIn = true;
  }

  toggleLogout() {
    this.isLoggedIn = false;
  }
}

const globalUser = new TheUser();

@Injectable({
  providedIn: 'root'
})
export class UserService {
  theUser: TheUser = globalUser;

  constructor(private router: Router) { }

  get isLoggedIn() {
    return this.theUser.isLoggedIn;
  }

  login() {
    this.theUser.toggleLogin();
  }

  logout() {
    this.theUser.toggleLogout();
    this.router.navigate(['/']);
  }
}
```

UserService 的每个实例化都会传递同一个实例`TheUser`。`TheUser`提供对决定用户登录状态的`isLoggedIn`的访问。另外两个公共方法让 UserService 切换`isLoggedIn`的值。这是为了让用户可以登录和注销。

您可以将`TheUser`视为一个全局实例。`UserService`是配置这个全局的可实例化接口。一个`UserService`实例化对`TheUser`的更改会应用到所有其他的`UserService`实例。`UserService`实现到 AuthService 中，提供对`TheUser`的`isLoggedIn`的访问以进行认证。

```
import { Component, Injectable } from '@angular/core';
import { CanActivate, CanActivateChild, CanDeactivate, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';

import { UserService } from './user.service';

@Injectable({
  providedIn: 'root'
})
export class AuthService implements CanActivate, CanActivateChild, CanDeactivate<Component> {
  constructor(private user: UserService) {}

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
    if (this.user.isLoggedIn)
      return true;
    else
      return false;
  }

  canActivateChild(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
    return this.canActivate(route, state);
  }

  canDeactivate(component: Component, route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
    if (!this.user.isLoggedIn || window.confirm('Leave the nest?'))
      return true;
    else
      return false;
  }
}
```

AuthService 实现了从`@angular/router`导入的每一个 guard 方法。每个保护方法都映射到 PrivateNestComponent 的`Route`对象中的相应字段。UserService 的实例从 AuthService 构造函数中实例化。AuthService 确定用户是否可以使用 UserService 公开的`isLoggedIn`继续。

从守卫返回`false`指示路由阻止用户路由。返回值`true`让用户前进到他的路线目的地。如果有多个服务通过身份验证，它们都必须返回 true 以允许访问。`canActivateChild`守卫 PrivateNestComponent 的子路由。这种保护方法解决了用户通过地址栏绕过 PrivateNestComponent 的问题。

调用时会自动传入保护方法参数。虽然示例中没有使用它们，但它们确实提供了来自路线的有用信息。开发人员可以使用这些信息来帮助验证用户。

AppComponent 还实例化了 UserService，以便在其模板中直接使用。AppComponent 和 AuthService 的 UserService 实例化引用同一个用户类(`TheUser`)。

```
import { Component } from '@angular/core';

import { UserService } from './services/user.service';

@Component({
  selector: 'app-root',
  template: `
  <ul>
    <li routerLink="/private-nest">Enter the secret nest!</li>
    <li routerLink="/">Leave the secret nest!</li>
    <li *ngIf="user.isLoggedIn"><button (click)="user.logout()">LOGOUT</button></li>
    <li *ngIf="!user.isLoggedIn"><button (click)="user.login()">LOGIN</button></li>
  </ul>
  <router-outlet></router-outlet>
  `
})
export class AppComponent {
  constructor(private user: UserService) { }
}
```

UserService 处理 AppComponent 的所有逻辑。AppComponent 主要关注它的模板。UserService 从类构造函数实例化为`user`。`user`数据决定模板的功能。

## 结论

路由在组织和限制应用程序部分之间取得了微妙的平衡。较小的应用程序(如博客或贡品页面)可能不需要任何路由。即便如此，包含一点散列路由也不会有什么坏处。用户可能只想引用页面的一部分。

Angular 应用了自己的路由库，构建在 HTML5 [历史 API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) 之上。这个 API 省略了散列路由，而是使用了`pushState(...)`和`replaceState(...)`方法。他们在不刷新页面的情况下更改网址 URL。Angular 中的默认路径位置路由策略就是这样工作的。如果愿意，设置`RouterModule.forRoot(routes, { useHash: true })`启用哈希路由。

本文主要关注默认路径定位策略。无论采用何种策略，都有许多路由实用程序可用于路由应用程序。`RouterModule`通过它的导出来公开这些工具。利用 RouterModule，基本路由、参数化路由、嵌套路由和保护路由都是可能的。

# **ng 模块**

角度应用从根部模块开始。Angular 通过由 NgModules 组成的模块系统来管理应用程序的依赖关系。除了普通的 JavaScript 模块，NgModules 还确保了代码的模块化和封装。

模块还提供了组织代码的最高层次。每个 NgModule 都从自己的代码块中分出一部分作为根。该模块为其代码提供了自顶向下的封装。然后，整个代码块可以导出到任何其他模块。从这个意义上说，NgModules 就像自己代码块的看门人。

Angular 的文档实用程序来自 Angular 编写的 NgModules。除非声明它的 NgModule 包含在根中，否则没有实用程序可用。这些实用程序还必须从它们的主机模块中导出，以便导入程序可以使用它们。这种封装形式使开发人员能够在同一个文件系统中生成自己的 NgModules。

另外，了解 Angular CLI(命令行界面)为什么从`@angular/core`导入`BrowserModule`是有意义的。每当使用 CLI 命令`ng new [name-of-app]`生成新的应用程序时，就会发生这种情况。

在大多数情况下，理解实现的要点就足够了。然而，理解实现如何将自己连接到根会更好。这一切都是通过将`BrowserModule`导入到根目录中自动发生的。

### NgModule 装饰器

Angular 通过修饰一个泛型类来定义它的模块。装饰者向 Angular 指出了类的模块化目的。NgModule 类合并了从模块范围可访问/可实例化的根依赖项。“范围”是指源于模块元数据的任何东西。

```
import { NgModule } from '@angular/core';

@NgModule({
  // … metadata …
})
export class AppModule { }
```

### ng 模块元数据

CLI 生成的根 NgModule 包括以下元数据字段。这些字段为 NgModule 管理的代码块提供配置。

*   `declarations: []`
*   `imports: []`
*   `providers: []`
*   `bootstrap: []`

### 声明

声明数组包括 NgModule 托管的所有组件、指令或管道。除非在模块的元数据中显式导出，否则它们是模块私有的。给定这个用例，组件、指令和管道被昵称为“可声明的”。NgModule 必须唯一地声明可声明项。可声明项不能在单独的 NgModules 中声明两次。否则会引发错误。参见下面的例子。

```
import { NgModule } from '@angular/core';
import { TwoComponent } from './components/two.component.ts';

@NgModule({
  declarations: [ TwoComponent ]
})
export class TwoModule { }

@NgModule({
  imports: [ TwoModule ],
  declarations: [ TwoComponent ]
})
export class OneModule { }
```

为了 NgModule 封装，Angular 抛出一个错误。默认情况下，可声明项对于声明它们的 NgModule 是私有的。如果多个 NgModule 需要某个可声明的，它们应该导入声明的 ng module。然后，这个 NgModule 必须导出所需的 declarable，以便其他 ng module 可以使用它。

```
import { NgModule } from '@angular/core';
import { TwoComponent } from './components/two.component.ts';

@NgModule({
  declarations: [ TwoComponent ],
  exports: [ TwoComponent ]
})
export class TwoModule { }

@NgModule({
  imports: [ TwoModule ] // this module can now use TwoComponent
})
export class OneModule { }
```

上面的例子不会抛出错误。TwoComponent 已在两个 NgModules 之间唯一声明。OneModule 还可以访问 TwoComponent，因为它导入 TwoModule。TwoModule 又导出 TwoComponent 供外部使用。

### 进口

imports 数组只接受 NgModules。除了其他 NgModules 之外，此数组不接受可声明性、服务或任何其他内容。导入一个模块提供了对该模块发布的可声明内容的访问。

这解释了为什么导入`BrowserModule`可以提供对其各种实用程序的访问。在`BrowserModule`中声明的每个可声明的实用程序都从它的元数据中导出。在导入`BrowserModule`时，那些导出的可申报单对导入 NgModule 可用。服务根本不会导出，因为它们缺乏相同的封装。

### 提供者

考虑到可声明性的封装，缺少服务封装可能看起来很奇怪。请记住，服务与声明或导出分开进入提供者数组。

Angular 编译时，它将根 NgModule 及其导入展平到一个模块中。服务组合在由合并的 NgModule 托管的单个 providers 数组中。可声明性通过一组编译时标志来维护它们的封装。

如果 NgModule 提供程序包含匹配的标记值，则导入根模块优先。在此之后，最后导入的 NgModule 优先。请看下一个例子。特别注意导入另外两个的 NgModule。认识到这是如何影响所提供服务的优先级的。

```
import { NgModule } from '@angular/core';

@NgModule({
  providers: [ AwesomeService ], // 1st precedence + importing module
  imports: [
    BModule,
    CModule
  ]
})
export class AModule { }

@NgModule({
  providers: [ AwesomeService ]  // 3rd precedence + first import
})
export class BModule { }

@NgModule({
  providers: [ AwesomeService ]  // 2nd precedence + last import
})
export class CModule { }
```

在模块范围内实例化 AwesomeService 会产生一个在模块元数据中提供的 AwesomeService 实例。如果一个模块的提供者忽略了这个服务，CModule 的 AwesomeService 将优先。如果 CModule 的提供者省略了 AwesomeService，则 BModule 以此类推。

## 引导程序

引导阵列接受组件。对于数组的每个组件，Angular 将组件作为其自己的根插入到`index.html`文件中。应用程序的 CLI 生成的根 NgModule 将始终具有此字段。

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [ AppComponent ],
  imports: [ BrowserModule ],
  providers: [],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

AppComponent 的元素会注入到 app 的底层 HTML 中(`index.html`)。组件树的其余部分从那里展开。总体 NgModule 的范围覆盖了整个树以及从 bootstrap 数组注入的任何其他内容。该数组通常只包含一个元素。这个组件将模块表示为单个元素及其底层树。

## NgModules vs JavaScript 模块

在前面的例子中，您已经看到了 Angular 和 JavaScript 模块一起工作。最顶层的`import..from`语句构成了 JavaScript 模块系统。每个语句目标的文件位置必须导出与请求匹配的类、变量或函数。`import { TARGET } from './path/to/exported/target'`。

在 JavaScript 中，模块是文件分隔的。如上所述，使用`import..from`关键字导入文件。另一方面，NgModules 是类分离的，用`@NgModule`修饰。因此，许多角度模块可以存在于单个文件中。JavaScript 不会发生这种情况，因为文件定义了一个模块。

当然，约定说每个`@NgModule`修饰类应该有自己的文件。即便如此，要知道在 Angular 中文件并不构成自己的模块。用`@NgModule`修饰的类创造了这种区别。

JavaScript 模块提供了对`@NgModule`元数据的令牌引用。这发生在承载 NgModule 类的文件的顶部。NgModule 在其元数据字段(声明者、导入者、提供者等)中使用这些标记。`@NgModule`能够首先修饰一个类的唯一原因是因为 JavaScript 从文件的顶部导入它。

```
// JavaScript module system provides tokens
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';
import { AppService } from './app.service';
// Javascript module system is strict about where it imports. It can only import at the top of files.

// Angular NgModule uses those tokens in its metadata settings
@NgModule({ // import { NgModule } from '@angular/core';
  declarations: [
    AppComponent // import { AppComponent } from './app.component';
  ],
  imports: [
    BrowserModule // import { BrowserModule } from '@angular/platform-browser';
  ],
  providers: [
    AppService // import { AppService } from './app.service';
  ],
  bootstrap: [
    AppComponent // import { AppComponent } from './app.component';
  ]
})
export class AppModule { }
// JavaScript module system exports the class. Other modules can now import AppModule.
```

上面的例子没有引入任何新的东西。这里的重点是解释这两个模块化系统如何一起工作的注释。JavaScript 提供令牌引用，而 NgModule 使用这些令牌来封装和配置其底层代码块。

### 功能模块

应用程序随着时间的推移而增长。适当地扩展它们需要应用程序组织。一个可靠的系统将使进一步的开发更加容易。

在 Angular 中，schematics 确保代码的目的驱动部分保持可区分性。除了子 NgModule 原理图，还有 ng module 本身。它们也是一种图式。除了应用本身之外，它们位于原理图列表中的其余部分之上。

一旦应用程序开始扩展，根模块就不应该是独立的。特征模块包括与根 NgModule 一起使用的任何 NgModule。您可以认为根模块有`bootstrap: []`元数据字段。功能应用程序确保根模块不会使其元数据过饱和。

功能模块代表任何导入模块隔离一段代码。他们可以独立处理整个应用程序部分。这意味着它可以用于其根模块导入特性模块的任何应用程序中。这种策略节省了开发人员在多个应用程序中的时间和精力！它还保持应用程序的根 NgModule 精简。

在一个应用程序的根 NgModule 中，将一个特性模块的令牌添加到根的`imports`数组中就可以做到这一点。无论功能模块输出或提供什么，根用户都可以使用。

```
// ./awesome.module.ts

import { NgModule } from '@angular/core';
import { AwesomePipe } from './awesome/pipes/awesome.pipe';
import { AwesomeComponent } from './awesome/components/awesome.component';
import { AwesomeDirective } from './awesome/directives/awesome.directive';

@NgModule({
  exports: [
    AwesomePipe,
    AwesomeComponent,
    AwesomeDirective
  ]
  declarations: [
    AwesomePipe,
    AwesomeComponent,
    AwesomeDirective
  ]
})
export class AwesomeModule { }
```

```
// ./app.module.ts

import { AwesomeModule } from './awesome.module';
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    AwesomeModule,
    BrowserModule
  ],
  providers: [],
  bootstrap: [
    AppComponent
  ]
})
export class AppModule { }
```

```
// ./app.component.ts

import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
  <!-- AwesomeDirective -->
  <h1 appAwesome>This element mutates as per the directive logic of appAwesome.</h1>

  <!-- AwesomePipe -->
  <p>Generic output: {{ componentData | awesome }}</p>

  <section>
    <!-- AwesomeComponent -->
    <app-awesome></app-awesome>
  </section>
  `
})
export class AppComponent {
  componentData: string = "Lots of transformable data!";
}
```

`<app-awesome></app-awesome>`(组件)`awesome`(管道)`appAwesome`(指令)是 AwesomeModule 专有的。如果它没有导出这些 declarables 或者 AppModule 没有将 AwesomeModule 添加到它的导入中，那么 AwesomeModule 的 declarables 就不能被 AppComponent 的模板使用。AwesomeModule 是根 NgModule AppModule 的功能模块。

Angular 提供了一些它自己的模块，在它们被导入时补充了 root。这是因为这些功能模块导出了它们所创建的内容。

### 静态模块方法

有时，模块提供了使用自定义配置对象进行配置的选项。这是通过利用模块类中的静态方法来实现的。

这种方法的一个例子是`RoutingModule`，它直接在模块上提供了一个`.forRoot(...)`方法。

要定义自己的静态模块方法，可以使用`static`关键字将其添加到模块类中。返回类型必须是`ModuleWithProviders`。

```
// configureable.module.ts

import { AwesomeModule } from './awesome.module';
import { ConfigureableService, CUSTOM_CONFIG_TOKEN, Config } from './configurable.service';
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

@NgModule({
  imports: [
    AwesomeModule,
    BrowserModule
  ],
  providers: [
    ConfigureableService
  ]
})
export class ConfigureableModule { 
  static forRoot(config: Config): ModuleWithProviders {
    return {
        ngModule: ConfigureableModule,
        providers: [
            ConfigureableService,
            {
                provide: CUSTOM_CONFIG_TOKEN,
                useValue: config
            }
        ]
    };
  }
}
```

```
// configureable.service.ts

import { Inject, Injectable, InjectionToken } from '@angular/core';

export const CUSTOM_CONFIG_TOKEN: InjectionToken<string> = new InjectionToken('customConfig');

export interface Config {
  url: string
}

@Injectable()
export class ConfigureableService {
  constructor(
    @Inject(CUSTOM_CONFIG_TOKEN) private config: Config
  )
}
```

注意，`forRoot(...)`方法返回的对象与`NgModule`配置几乎相同。

`forRoot(...)`方法接受用户在导入模块时提供的定制配置对象。

```
imports: [
  ...
  ConfigureableModule.forRoot({ url: 'http://localhost' }),
  ...
]
```

然后使用名为`CUSTOM_CONFIG_TOKEN`的自定义`InjectionToken`提供配置，并注入到`ConfigureableService`中。使用`forRoot(...)`方法只能导入`ConfigureableModule`一次。这为`CUSTOM_CONFIG_TOKEN`提供了自定义配置。所有其他模块应该导入没有`forRoot(...)`方法的`ConfigureableModule`。

## Angular 中的 NgModule 示例

Angular 提供了多种可从`@angular`导入的模块。最常见的两个导入模块是`CommonModule`和`HttpClientModule`。

`CommonModule`其实是`BrowserModule`的子集。两者都提供了对`*ngIf`和`*ngFor`结构指令的访问。`BrowserModule`包括网络浏览器的特定平台安装。`CommonModule`省略此安装。`BrowserModule`应该导入到 web 应用程序的根 NgModule 中。`CommonModule`为不需要平台安装的功能模块提供`*ngIf`和`*ngFor`。

`HttpClientModule`提供`HttpClient`服务。记住，服务放在`@NgModule`元数据的提供者数组中。它们是不可申报的。在编译期间，每个 NgModule 都被合并到一个单独的模块中。服务不像可声明者那样被封装。它们都可以通过位于合并 NgModule 旁边的根注入器来实例化。

回到正题。像任何其他服务一样，`HttpClient`通过依赖注入(DI)通过其构造函数实例化为一个类。使用 DI，根注入器将`HttpClient`的一个实例注入到构造函数中。该服务允许开发人员通过服务的实现发出 HTTP 请求。

`HttpClient`实现包含到`HttpClientModule`提供者数组中。只要根 NgModule 导入`HttpClientModule`，`HttpClient`就会像预期的那样从根的作用域内实例化。

## 结论

您可能已经利用了 Angular 的 NgModules。Angular 使得将模块放入根 NgModule 的 imports 数组变得非常容易。实用程序通常是从导入模块的元数据中导出的。因此，为什么它的实用程序在输入到根 NgModule 后突然变得可用。

NgModules 与普通的 JavaScript 模块紧密合作。一个提供令牌，一个使用它们进行配置。他们的团队合作产生了一个强健的模块化系统，这是 Angular framework 所独有的。它在除应用程序之外的所有其他原理图之上提供了一个新的组织层。

希望这篇文章能加深您对 NgModules 的理解。Angular 可以在一些更奇特的用例中进一步利用这个系统。