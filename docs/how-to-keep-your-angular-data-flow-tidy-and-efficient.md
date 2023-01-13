# 如何保持你的角度数据流整洁高效

> 原文：<https://www.freecodecamp.org/news/how-to-keep-your-angular-data-flow-tidy-and-efficient/>

开发大规模的角度应用程序感觉就像闭着眼睛双手绑在背后骑自行车。

Angular 应用程序中的数据流可能非常复杂，尤其是在处理双向数据绑定、异步更新、表单和路由时。

管理这种复杂性的关键是编写清晰简洁、易于阅读和理解的代码。无论您是构建企业应用程序还是简单的 web 应用程序，这都是至关重要的。

在本教程中，我们将回顾一些最佳实践，以帮助您确保您的应用程序遵循 Angular 方式，并且在处理视图更改时不会泄漏组件之间的状态或做不必要的工作。

这将帮助您创建复杂的 Angular 应用程序，团队中的其他开发人员可以轻松理解和维护这些应用程序。我们开始吧！

## 你会学到什么

在本帖中，我们将探索一些在 Angular 应用程序中保持数据流整洁和高效的最佳实践。我们将讨论组件通信、数据绑定等主题。

在这篇文章结束时，你会更好地理解如何在 Angular 应用程序中保持数据流畅。

## 尽可能使用内置组件

Angular 提供了一系列内置组件，您可以在应用程序中使用它们。这些组件被设计为高效且易于使用，所以尽可能地利用它们。

内置组件可以帮助减少您必须编写的代码量，使您的应用程序整体上更加高效。此外，使用内置组件可以减少出错的可能性，从而有助于保持数据流的整洁。

开发人员犯的一个常见错误是，在用模型更新更新了输入控件的值之后，忘记在输入控件上添加事件侦听器。

例如，内置的表单验证组件会自动为您实现这种检查，从而大大减少了这类错误。

最后，通过内置组件，可以更容易地在应用程序的不同部分之间共享更新，因为它们都使用相同的 API。

## 使用管道进行过滤，而不仅仅是转换

管道是过滤角度数据的好方法。通过使用管道，您可以声明性地指定希望如何转换数据，而不必在组件类中编写代码。

你可以通过玩下面的 GitHub [代码](https://github.com/gatwirival/filter-pipe)来学习如何使用管道进行过滤。在下面的代码中，我们创建了一个自定义过滤管道:

```
import { Pipe, PipeTransform } from '@angular/core';
import { User } from './model';

@Pipe({
  name: 'filter',
  pure: false,
})
export class FilterPipe implements PipeTransform {
  transform(value: User[], filterString: string, property: string): User[] {
    console.log('pipe run');
    if (value.length === 0 || !filterString) {
      return value;
    }
    let filteredUsers: User[] = [];
    for (let user of value) {
      if (user[property].toLowerCase().includes(filterString.toLowerCase())) {
        filteredUsers.push(user);
      }
    }
    return filteredUsers;
  }
}
```

filter-pipe.ts

这使得您的代码可读性更好，也更容易维护。另外，很容易对管道进行单元测试，因此您可以确信它们正在做您所期望的事情。

## 考虑使用 Redux

Redux 是在 Angular 应用程序中管理数据的一种很好的方式。它有助于保持您的代码组织和整洁，并可以使调试和测试更加容易。

Redux 还易于与其他库和框架一起使用，因此您可以快速入门。

如果你想学习 Redux 的基础知识，这里有一个对初学者很有帮助的指南。

## 利用依赖注入

Angular 的依赖注入系统是其最大的特点之一，使其易于模块化和重用代码。

当使用角度服务时，最好让他们专注于一项任务。这将有助于保持一个整洁的数据流，并避免代码重复。

要创建一个`injectable service`，使用以下命令:

```
ng generate service workers/worker
```

generating workers/worker service

```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class WorkerService {

  constructor() { }
}
```

generated code worker.service.ts

`@Injectable()` decorator 告诉 Angular 这个类可以用在 DI 系统中。

## 双向绑定损害了可读性和可维护性

虽然双向绑定非常方便，但它们会很快使您的代码库变得一团糟。当你有太多的双向绑定时，追踪通过你的应用的数据流就变得很困难。这可能会导致意外的错误和难以发现的错误。

### 双向绑定的示例

```
<input [(ngModel)]="data"  type="text">

<hr>

<h3> Learn coding from {{data}}</h3>
```

src/app/app.component.html

```
import { Component } from "@angular/core";

@Component({
  selector: "my-app",
  templateUrl: "./app.component.html",
})
export class AppComponent {
  data = "FreeCodeCamp";
}
```

src/app/app.component.ts

您将从上面的代码中获得下面的[结果](https://angular-ivy-dmmxji.stackblitz.io)

[双向绑定](https://angular.io/guide/two-way-binding#adding-two-way-data-binding)是方括号和圆括号的组合。此语法将属性绑定括号[]与事件绑定括号()结合在一起。

### 请改用单向绑定

为了保持数据流的整洁和高效，尽可能使用单向绑定。单向绑定明确了数据的来源和去向，这使得推理代码变得更加容易。

### 单向绑定的示例

```
<button (click)="myFunction()">Alert</button>
```

src/app/app.component.ts

```
import { Component } from '@angular/core';

@Component({
  selector: 'my-app',

  template: `<button (click)='myFunction()' >alert me</button>`,
})
export class AppComponent {
  myFunction(): void {
    alert('Show alert!');
  }
} 
```

src/app/app.component.ts

当您运行上面的代码时，您会看到一个`alert`按钮。当您单击那个按钮时，它将调用组件的`myFunction()`方法。这将执行`alert()`方法，显示一个文本为`Show alert`的警告框，如上面代码中的[示例](https://angular-ivy-ptdtve.stackblitz.io)所示。

## 编写可测试的代码

当你写代码的时候，永远记住测试的难易程度。

总的来说，尽可能使你的代码模块化，这样你就可以很容易地隔离和测试每一部分。

为尽可能多的代码编写单元测试。这将有助于确保您的代码按预期工作，并在早期捕捉任何错误。

## 结论

在 Angular 应用程序中，有许多方法可以组织数据流。关键是找到最适合你和你的团队的方法，并保持一致。

通过遵循这些最佳实践，您可以确保您的应用程序平稳高效地运行。