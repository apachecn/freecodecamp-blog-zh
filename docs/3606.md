# 举例说明角度依赖注入

> 原文：<https://www.freecodecamp.org/news/angular-dependency-injection/>

# **什么是依赖注入？**

#### **动机**

依赖注入通常简称为 DI。这个范例存在于整个 Angular。它保持代码的灵活性、可测试性和可变性。类可以继承外部逻辑，而不知道如何创建它。那些类的任何消费者也不需要知道任何事情。

DI 避免了类和消费者知道不必要的东西。然而，由于 Angular 中支持 DI 的机制，代码像以前一样是模块化的。

服务是直接投资的主要受益者。他们依赖于将*注入*各种消费者的范式。然后，这些消费者可以利用所提供的服务和/或将其转发到其他地方。

服务并不孤单。指令、管道、组件等等:Angular 中的每个原理图都以某种方式受益于 DI。

## 注射器

注入器是存储指令的数据结构，这些指令详细说明了服务在哪里以及如何形成。它们在角 DI 系统中充当中介。

模块、指令和组件类包含特定于注射器的元数据。每一个类都有一个新的注入器实例。通过这种方式，应用程序树反映了其注射器层次结构。

`providers: []`元数据接受服务，然后向类的注入器注册。此 provider 字段添加了注射器运行所需的说明。一个类(假设它有依赖关系)通过把它的类作为它的数据类型来实例化一个服务。注入器将这种类型对齐，代表类创建该服务的实例。

当然，这个类只能实例化注入器的指令。如果类自己的注入器没有注册服务，那么它查询它的父类。如此等等，直到到达具有服务或应用程序根的注入器。

服务可以在应用程序中的任何注入器上注册。服务进入类模块、指令或组件的`providers: []`元数据字段。类的子类可以实例化在类的注入器中注册的服务。子注入器最终会退回到父注入器。

## 依赖注入

看看每个类的框架:服务、模块、指令和组件。

```
// service

import { Injectable } from '@angular/core';

@Injectable({
  providedIn: /* injector goes here */
})
export class TemplateService {
  constructor() { }
}
```

```
// module

import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

@NgModule({
  imports: [
    CommonModule
  ],
  declarations: [],
  providers: [ /* services go here */ ]
})
export class TemplateModule { }
```

```
// directive

import { Directive } from '@angular/core';

@Directive({
  selector: '[appTemplate]',
  providers: [ /* services go here */ ]
})
export class TemplateDirective {
  constructor() { }
}
```

```
//component

import { Component } from '@angular/core';

@Component({
  selector: 'app-template',
  templateUrl: './template.component.html',
  styleUrls: ['./template.component.css'],
  providers: [ /* services go here */ ]
})
export class TemplateComponent {
  // class logic ...
}
```

每个框架都可以向注入器注册服务。事实上，TemplateService *是*的一个服务。从 Angular 6 开始，服务现在可以使用`@Injectable`元数据向注入器注册。

##### **在任何情况下**

请注意`providedIn: string` ( `@Injectable`)和`providers: []` ( `@Directive`、`@Componet`和`@Module`)元数据。它们告诉注入者在哪里以及如何创建服务。否则，注入器不知道如何实例化。

如果一个服务有依赖怎么办？结果会如何？提供者回答这些问题，以便注入器可以正确地实例化。

注射器构成了 DI 框架的主干。它们存储实例化服务的指令，这样消费者就不必这么做了。他们接收服务实例，而不需要知道任何关于源依赖的信息！

我还应该注意，没有注入器的其他原理图仍然可以利用依赖注入。他们不能注册额外的服务，但是他们仍然可以从注入器实例化。

## 服务

`@Injectable`的`providedIn: string`元数据指定向哪个注射器注册。使用这种方法，并且根据服务是否被使用，服务可能会也可能不会向注入器注册。角称这个*为摇树*。

默认情况下，该值设置为`‘root’`。这转化为应用程序的根注入器。基本上，将字段设置为`‘root’`会使服务在任何地方都可用。

##### **快速注释**

如前所述，子注入器依靠它们的父注入器。这种后备策略确保父母不必为每个注射器重新注册。参考这篇关于[服务和注入器](https://guide.freecodecamp.org/angular/services-and-injectors)的文章，了解这个概念的说明。

注册的服务是*单例的*。也就是说，实例化服务的指令只存在于一个注入器上。这假设它没有在其他地方明确注册。

## 模块、指令和组件

模块和组件都有自己的注入器实例。考虑到`providers: []`元数据字段，这一点很明显。该字段接受一组服务，并将它们注册到模块或组件类的注入器中。这种方法发生在`@NgModule`、`@Directive`或`@Component`装饰者身上。

该策略省略了*树摇动*，或者从注入器中可选地移除未使用的服务。服务实例在模块或组件的整个生命周期中都存在于它们的注入器中。

## 实例化引用

对 DOM 的引用可以从任何类中实例化。请记住，引用仍然是服务。它们与传统服务的不同之处在于表示其他事物的状态。这些服务包括与其引用进行交互的功能。

指令经常需要 DOM 引用。指令通过这些引用对它们的宿主元素执行变异。请参见下面的示例。指令的注入器将宿主元素的引用实例化到类的构造函数中。

```
// directives/highlight.directive.ts

import { Directive, ElementRef, Renderer2, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(
    private renderer: Renderer2,
    private host: ElementRef
  ) { }

  @Input() set appHighlight (color: string) {
    this.renderer.setStyle(this.host.nativeElement, 'background-color', color);
  }
}
```

```
// app.component.html

<p [appHighlight]="'yellow'">Highlighted Text!</p>
```

`Renderer2`也被实例化。这些服务来自哪个注射器？嗯，每个服务的源代码都来自`@angular/core`。然后，这些服务必须向应用程序的根注入器注册。

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';
import { HighlightDirective } from './directives/highlight.directive';

@NgModule({
  declarations: [
    AppComponent,
    HighlightDirective
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [
    AppComponent
  ]
})
export class AppModule { }
```

一个空的 providers 数组！？不要害怕。Angular 自动向根注入器注册许多服务。这包括`ElementRef`和`Renderer2`。在这个例子中，我们通过源于`ElementRef`实例化的接口来管理主机元素。`Renderer2`让我们通过 Angular 的视图模型更新 DOM。

你可以从[这篇文章](https://guide.freecodecamp.org/angular/views)中了解更多观点。它们是 Angular 应用程序中 DOM/view 更新的首选方法。

认识到注射器在上述示例中的作用很重要。通过在构造函数中声明变量类型，类获得了有价值的服务。每个参数的数据类型映射到注射器内的一组指令。如果注射器具有该类型，则它返回所述类型的实例。

## 实例化服务

[服务和注入器](https://guide.freecodecamp.org/angular/services-and-injectors)文章在一定程度上解释了这一部分。尽管如此，这一节还是重复了前一节或大部分内容。服务通常会提供对其他内容的引用。它们也可以提供一个接口来扩展一个类的能力。

下一个例子将定义一个日志服务，它通过组件的`providers: []`元数据被添加到组件的注入器中。

```
// services/logger.service.ts

import { Injectable } from '@angular/core';

@Injectable()
export class LoggerService {
  callStack: string[] = [];

  addLog(message: string): void {
    this.callStack = [message].concat(this.callStack);
    this.printHead();
  }

  clear(): void {
    this.printLog();
    this.callStack = [];
    console.log(“DELETED LOG”);
  }

  private printHead(): void {
    console.log(this.callStack[0] || null);
  }

  private printLog(): void {
    this.callStack.reverse().forEach((log) => console.log(message));
  }
}
```

```
// app.component.ts

import { Component } from '@angular/core';
import { LoggerService } from './services/logger.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  providers: [LoggerService]
})
export class AppComponent {
  constructor(private logger: LoggerService) { }

  logMessage(event: any, message: string): void {
    event.preventDefault();
    this.logger.addLog(`Message: ${message}`);
  }

  clearLog(): void {
    this.logger.clear();
  }
}
```

```
// app.component.html

<h1>Log Example</h1>
<form (submit)="logMessage($event, userInput.value)">
  <input #userInput placeholder="Type a message...">
  <button type="submit">SUBMIT</button>
</form>

<h3>Delete Logged Messages</h3>
<button type="button" (click)="clearLog()">CLEAR</button>
```

重点关注 AppComponent 构造函数和元数据。组件注入器从包含 LoggerService 的提供者元数据字段接收指令。然后，注入器知道从构造函数中请求的实例化 LoggerService 的内容。

构造函数参数`loggerService`具有注入器识别的类型`LoggerService`。注入器如前所述完成实例化。

## 结论

依赖注入(DI)是一个范例。它在 Angular 中的工作方式是通过注射器的层次结构。一个类接收它的资源，而不必创建或了解它们。注入器接收指令，并根据所请求的服务实例化服务。

DI 经常出现在 Angular。官方的 Angular 文档解释了为什么这个范例如此流行。除了本文所讨论的，他们还继续以角度的方式描述了 DI 的许多用例。点击下面查看！

## 关于依赖注入的更多信息:

*   [角度依赖注入简介](https://www.freecodecamp.org/news/angular-dependency-injection-in-detail-8b6822d6457c/)
*   [依赖注入快速介绍](https://www.freecodecamp.org/news/angular-dependency-injection/freecodecamp.org/news/a-quick-intro-to-dependency-injection-what-it-is-and-when-to-use-it-7578c84fa88f/)