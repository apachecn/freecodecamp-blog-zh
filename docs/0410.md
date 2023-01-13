# 角度结构指示模式——它们是什么以及如何使用它们

> 原文：<https://www.freecodecamp.org/news/angular-structural-directive-patterns-what-they-are-and-how-to-use-them/>

Angular 中有两种类型的指令。属性指令修改 DOM 元素的外观或行为。**结构指令**在 DOM 中添加或删除元素。

结构指令是 Angular 最强大的特性之一，但是它们经常被误解。

如果您有兴趣了解更多关于结构指令的知识，请继续阅读，了解它们是什么，为什么它们很有帮助，以及如何在您的项目中有效地使用它们。

## 你会学到什么

在本教程中，您将学习角度结构方向模式。你将了解它们是什么以及如何使用它们。

在本文结束时，您将对这些指令以及如何在您自己的项目中使用它们有更好的理解。

## 什么是角度结构指令？

角度结构指令是改变 DOM 结构的指令。他们可以添加、删除或替换元素。结构指令的名字前面有一个`*`。

在 Angular 中，有三个标准的结构指令:

*   `*ngIf`–根据布尔值返回的表达式值，有条件地包含一个模板。
*   `*ngFor`–简化数组的迭代。
*   `*ngSwitch`–渲染每个匹配的视图。

这是一个结构指令的例子。语法如下所示:

```
 <element ng-if="Condition"></element> 
```

条件必须分析为真或假。

```
<div *ngIf="worker" class="name">{{worker.name}}</div>
```

src/app/app.component.html (asterisk)

Angular 生成一个`<ng-template>`元素，并对其应用`*ngIf`指令。这将它转换成方括号中的属性绑定，例如`[ngIf]`。然后将`<div>`的剩余部分，包括它的类属性，插入到`<ng-template>`中:

这里有一个例子:

```
<ng-template [ngIf]="worker">
  <div class="name">{{worker.name}}</div>
</ng-template>
```

src/app/app.component.html (ngif-template)

## 角度结构指令是如何工作的？

若要使用结构化指令，请在 HTML 模板中添加带有指令的元素。然后，将根据您在指令中设置的条件或表达式添加、移除或替换该元素。

### 结构指令的例子

让我们从添加一些简单的 HTML 代码开始。

您的`app.component.html`文件应该包含以下代码:

```
<div style="text-align:center">
  <h1>
    Welcome 
  </h1>
</div>
<h2> <app-illustrations></app-illustrations></h2>
```

### 如何使用`*ngIf`指令

您可以使用`*ngIf`根据给定的条件显示或删除一个元素。`ngIf`与其他编程语言中使用的`if-else`非常相似。

如果表达式的计算结果为 false，`*ngIf`指令将删除 HTML 元素。如果`if`语句返回 true，元素的副本将被添加到 DOM 中。

**`*ngIf`的例子:**

首先在 Angular starter 应用程序中创建组件插图。然后打开您的插图组件`HTML`文件，并添加以下代码:

```
<h1>
	<button (click)="toggleOn =!toggleOn">ng-if illustration</button>
  </h1>
  <div *ngIf="!toggleOn">
  <h2>Hello </h2>
  <p>Good morning to you,click the button to view</p>
  </div>
  <hr>
  <p>Today is Monday and this is a dummy text element to make you feel better</p>
  <p>Understanding the ngIf directive with the else clause</p>
```

illustration.component.html

然后在`.ts`文件中添加以下布尔变量:

```
toggleOn: boolean=true;
```

illustration.component.ts

我们有一个保存问候语的 div 标签。如果 toggleOn 的值为 false，则显示 ngIf 指令。之后，我们添加了一些虚拟段落。

该开关允许您通过隐藏和显示`divs`来演示`*ngif`。

插图的完整代码可以在[这里](https://github.com/gatwirival/Structural-directives/tree/main/ngif-illustration)找到。

### 如何使用`*ngFor`指令

我们使用`*ngfor`指令来遍历数组的值。这类似于其他编程语言或框架中的 for 循环。

**`*ngFor`的例子:**

使用`ng new myapp`生成一个角度应用程序，然后创建组件插图。打开`HTML`文件并添加以下代码:

```
<ul>

    <li *ngFor="let wok of workers">{{ wok }}</li>

</ul>
```

illustration.component.html

然后将下面的代码添加到您的插图组件类型脚本文件中:

```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-illustrations',
  templateUrl: './illustrations.component.html',
  styleUrls: ['./illustrations.component.css']
})
export class IllustrationsComponent implements OnInit {
  workers: any = [

    'worker 1',

    'worker 2',

    'worker 3',

    'worker 4',

    'worker 5',

  ]

  constructor() { }

  ngOnInit(): void {
  }

}
```

illustration.component.ts

我们有一个无序列表，其中我们使用一个 for 循环来遍历元素数组。所以元素使用了附加属性`*ngFor`。它接受一个数组并呈现每个元素，直到到达集合中的最后一个值。

插图的完整代码可以在[这里](https://github.com/gatwirival/Structural-directives/tree/main/ngfor-illustration)找到。

### 如何使用`*ngSwitch`指令

我们使用`ngSwitch`根据单个条件以及不同的 case 语句来呈现元素。`*ngSwitch`指令类似于开关情况。

*** ng switch 示例:**

```
<div [ngSwitch]="Myshopping">
  <p *ngSwitchCase="'cups'">cups</p>
  <p *ngSwitchCase="'veg'">Vegetables</p>
  <p *ngSwitchCase="'clothes'">Trousers</p>
  <p *ngSwitchDefault>My Shopping</p>
</div>
```

illustration.component.html

然后在`.ts`文件中添加以下字符串变量:

```
 Myshopping: string = ''; 
```

我们有一个名为`Myshopping`的变量，它有一个默认值，在模板中用来呈现满足条件的某些元素。

当条件保持为真时，相关元素在 DOM 中呈现，其余元素被忽略。

插图的完整代码可以在[这里](https://github.com/gatwirival/Structural-directives/tree/main/ngfor-illustration)找到。

如果没有匹配项，带有 NgSwitchDefault 指令的视图将按照我们代码的输出所示进行呈现。

## 什么时候应该使用角度结构指令？

如果想在 DOM 中添加或删除元素，可以使用结构化指令。您还可以使用它们来更改元素的 CSS 样式，或者添加事件侦听器。您甚至可以使用它们来创建以前没有的新元素！

最好的规则是:*如果你正在考虑操纵 DOM，那么是时候使用结构化指令了。*

## 结论

结构指令是 Angular 的一个重要部分，您可以以多种方式使用它们。

希望本教程能让您更好地理解如何使用这些指令以及何时使用每种模式。