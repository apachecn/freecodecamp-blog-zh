# 角度生命周期挂钩:ngOnChanges、ngOnInit 等等

> 原文：<https://www.freecodecamp.org/news/angular-lifecycle-hooks/>

### 为什么我们需要生命周期挂钩？

现代前端框架将应用程序从一个状态转移到另一个状态。数据推动了这些更新。这些技术与数据交互，进而转换状态。随着每个状态的改变，有许多特定的时刻某些资产变得可用。

在一个实例中，模板可能已经准备好，在另一个实例中，数据将已经完成上传。每个实例的编码都需要一种检测手段。生命周期挂钩满足了这一需求。现代前端框架用各种生命周期挂钩包装自己。Angular 也不例外

## 生命周期挂钩解释

生命周期挂钩是定时方法。它们的不同之处在于执行的时间和原因。变化检测触发这些方法。它们的执行取决于当前周期的条件。角度运行不断改变对其数据的检测。生命周期挂钩有助于管理其影响。

这些钩子的一个重要方面是它们的执行顺序。它从不偏离。它们基于检测周期产生的一系列可预测的负载事件来执行。这使得它们是可预测的。

有些资产只有在某个钩子执行后才可用。当然，一个钩子只在当前变化检测周期中设定的特定条件下执行。

本文按照执行的顺序展示了生命周期挂钩(如果它们都执行的话)。某些情况下需要激活钩子。有一些只在组件初始化后执行一次。

从`@angular/core`开始，所有生命周期方法都可用。尽管不是必需的，Angular [建议实现每一个钩子](https://angular.io/guide/lifecycle-hooks#interfaces-are-optional-technically)。这种做法可以得到关于组件的更好的错误消息。

## 生命周期挂钩的执行顺序

### 恩贡昌斯

`ngOnChanges`绑定类成员`@Input`修改后触发。由`@Input()`装饰器绑定的数据来自外部来源。当外部源以可检测的方式改变数据时，它再次通过`@Input`属性。

有了这个更新，`ngOnChanges`立刻火了。它也在输入数据初始化时触发。钩子接收一个类型为`SimpleChanges`的可选参数。该值包含有关已更改的输入绑定属性的信息。

```
import { Component, Input, OnChanges } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
  <h3>Child Component</h3>
  <p>TICKS: {{ lifecycleTicks }}</p>
  <p>DATA: {{ data }}</p>
  `
})
export class ChildComponent implements OnChanges {
  @Input() data: string;
  lifecycleTicks: number = 0;

  ngOnChanges() {
    this.lifecycleTicks++;
  }
}

@Component({
  selector: 'app-parent',
  template: `
  <h1>ngOnChanges Example</h1>
  <app-child [data]="arbitraryData"></app-child>
  `
})
export class ParentComponent {
  arbitraryData: string = 'initial';

  constructor() {
    setTimeout(() => {
      this.arbitraryData = 'final';
    }, 5000);
  }
}
```

****概要:**** ParentComponent 将输入数据绑定到 ChildComponent。组件通过它的`@Input`属性接收这个数据。`ngOnChanges`火灾。五秒钟后，`setTimeout`回调触发。ParentComponent 改变 ChildComponent 的输入绑定属性的数据源。新数据流经输入属性。`ngOnChanges`火灾再次发生。

### 恩戈尼特

在组件的输入绑定(`@Input`)属性初始化时触发一次。下一个例子看起来与上一个相似。当 ChildComponent 接收输入数据时，挂钩不会触发。相反，它会在数据呈现到 ChildComponent 模板后立即触发。

```
import { Component, Input, OnInit } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
  <h3>Child Component</h3>
  <p>TICKS: {{ lifecycleTicks }}</p>
  <p>DATA: {{ data }}</p>
  `
})
export class ChildComponent implements OnInit {
  @Input() data: string;
  lifecycleTicks: number = 0;

  ngOnInit() {
    this.lifecycleTicks++;
  }
}

@Component({
  selector: 'app-parent',
  template: `
  <h1>ngOnInit Example</h1>
  <app-child [data]="arbitraryData"></app-child>
  `
})
export class ParentComponent {
  arbitraryData: string = 'initial';

  constructor() {
    setTimeout(() => {
      this.arbitraryData = 'final';
    }, 5000);
  }
}
```

****总结:**** ParentComponent 将输入数据绑定到 ChildComponent。ChildComponent 通过其`@Input`属性接收该数据。数据呈现到模板中。`ngOnInit`火灾。五秒钟后，`setTimeout`回调触发。ParentComponent 改变 ChildComponent 的输入绑定属性的数据源。ngOnInit**不火**。

`ngOnInit`是一劳永逸的挂钩。初始化是它唯一关心的问题。

### ngDoCheck

`ngDoCheck`在每个变更检测周期触发。角行程经常改变检测。执行任何动作都会导致它循环。`ngDoCheck`用这些周期点火。慎用。如果实施不当，可能会造成性能问题。

让开发人员手动检查他们的数据。他们可以有条件地触发新的申请日期。结合`ChangeDetectorRef`，开发人员可以为变更检测创建他们自己的检查。

```
import { Component, DoCheck, ChangeDetectorRef } from '@angular/core';

@Component({
  selector: 'app-example',
  template: `
  <h1>ngDoCheck Example</h1>
  <p>DATA: {{ data[data.length - 1] }}</p>
  `
})
export class ExampleComponent implements DoCheck {
  lifecycleTicks: number = 0;
  oldTheData: string;
  data: string[] = ['initial'];

  constructor(private changeDetector: ChangeDetectorRef) {
    this.changeDetector.detach(); // lets the class perform its own change detection

    setTimeout(() => {
      this.oldTheData = 'final'; // intentional error
      this.data.push('intermediate');
    }, 3000);

    setTimeout(() => {
      this.data.push('final');
      this.changeDetector.markForCheck();
    }, 6000);
  }

  ngDoCheck() {
    console.log(++this.lifecycleTicks);

    if (this.data[this.data.length - 1] !== this.oldTheData) {
      this.changeDetector.detectChanges();
    }
  }
}
```

注意控制台和显示器。冻结前，数据进展到“中间”。如控制台所示，在此期间会发生三轮更改检测。当‘final’被推到`this.data`的末尾时，发生又一轮的变化检测。然后进行最后一轮变更检测。if 语句的评估确定不需要对视图进行更新。

****总结:**** 类经过两轮变更检测后实例化。类构造器初始化了两次`setTimeout`。三秒钟后，第一个`setTimeout`触发变化检测。`ngDoCheck`标记显示器进行更新。三秒钟后，第二个`setTimeout`触发变化检测。根据`ngDoCheck`的评估，不需要更新视图。

### 警告

在继续之前，了解一下内容 DOM 和视图 DOM 之间的区别(DOM 代表文档对象模型)。

内容 DOM 定义了指令元素的 innerHTML。相反，视图 DOM 是组件的模板，不包括任何嵌套在指令中的 HTML 模板。为了更好的理解，参考[这篇博文](http://blog.mgechev.com/2016/01/23/angular2-viewchildren-contentchildren-difference-viewproviders)。

### ngAfterContentInit

在组件的内容 DOM 初始化(首次加载)后触发。等待`@ContentChild(ren)`查询是钩子的主要用例。

查询产生内容 DOM 的元素引用。因此，在加载内容 DOM 之前，它们是不可用的。因此使用了`ngAfterContentInit`和它的对应词`ngAfterContentChecked`。

```
import { Component, ContentChild, AfterContentInit, ElementRef, Renderer2 } from '@angular/core';

@Component({
  selector: 'app-c',
  template: `
  <p>I am C.</p>
  <p>Hello World!</p>
  `
})
export class CComponent { }

@Component({
  selector: 'app-b',
  template: `
  <p>I am B.</p>
  <ng-content></ng-content>
  `
})
export class BComponent implements AfterContentInit {
  @ContentChild("BHeader", { read: ElementRef }) hRef: ElementRef;
  @ContentChild(CComponent, { read: ElementRef }) cRef: ElementRef;

  constructor(private renderer: Renderer2) { }

  ngAfterContentInit() {
    this.renderer.setStyle(this.hRef.nativeElement, 'background-color', 'yellow')

    this.renderer.setStyle(this.cRef.nativeElement.children.item(0), 'background-color', 'pink');
    this.renderer.setStyle(this.cRef.nativeElement.children.item(1), 'background-color', 'red');
  }
}

@Component({
  selector: 'app-a',
  template: `
  <h1>ngAfterContentInit Example</h1>
  <p>I am A.</p>
  <app-b>
    <h3 #BHeader>BComponent Content DOM</h3>
    <app-c></app-c>
  </app-b>
  `
})
export class AComponent { }
```

`@ContentChild`查询结果从`ngAfterContentInit`开始。`Renderer2`更新包含`h3`标签和 CComponent 的 BComponent 的内容 DOM。这是一个常见的[内容投影](https://alligator.io/angular/content-projection-angular)的例子。

****概要:**** 渲染从一个组件开始。若要完成，组件必须呈现 BComponent。BComponent 通过`<ng-content></ng-content>`元素投影嵌套在其元素中的内容。CComponent 是投影内容的一部分。投影内容完成渲染。`ngAfterContentInit`火灾。BComponent 完成渲染。组件完成渲染。`ngAfterContentInit`不会再火了。

### ngAfterContentChecked

`ngAfterContentChecked`在针对内容 DOM 的每个更改检测周期后触发。这让开发人员可以更方便地处理内容 DOM 对变更检测的反应。`ngAfterContentChecked`如果实施不当，可能会频繁触发并导致性能问题。

在组件的初始化阶段也会触发。就在`ngAfterContentInit`之后。

```
import { Component, ContentChild, AfterContentChecked, ElementRef, Renderer2 } from '@angular/core';

@Component({
  selector: 'app-c',
  template: `
  <p>I am C.</p>
  <p>Hello World!</p>
  `
})
export class CComponent { }

@Component({
  selector: 'app-b',
  template: `
  <p>I am B.</p>
  <button (click)="$event">CLICK</button>
  <ng-content></ng-content>
  `
})
export class BComponent implements AfterContentChecked {
  @ContentChild("BHeader", { read: ElementRef }) hRef: ElementRef;
  @ContentChild(CComponent, { read: ElementRef }) cRef: ElementRef;

  constructor(private renderer: Renderer2) { }

  randomRGB(): string {
    return `rgb(${Math.floor(Math.random() * 256)},
    ${Math.floor(Math.random() * 256)},
    ${Math.floor(Math.random() * 256)})`;
  }

  ngAfterContentChecked() {
    this.renderer.setStyle(this.hRef.nativeElement, 'background-color', this.randomRGB());
    this.renderer.setStyle(this.cRef.nativeElement.children.item(0), 'background-color', this.randomRGB());
    this.renderer.setStyle(this.cRef.nativeElement.children.item(1), 'background-color', this.randomRGB());
  }
}

@Component({
  selector: 'app-a',
  template: `
  <h1>ngAfterContentChecked Example</h1>
  <p>I am A.</p>
  <app-b>
    <h3 #BHeader>BComponent Content DOM</h3>
    <app-c></app-c>
  </app-b>
  `
})
export class AComponent { }
```

这和`ngAfterContentInit`几乎没有区别。BComponent 中只添加了一个`<button></button>`。单击它会导致变化检测循环。这将激活由`background-color`随机化指示的挂钩。

****概要:**** 渲染从一个组件开始。若要完成，组件必须呈现 BComponent。BComponent 通过`<ng-content></ng-content>`元素投影嵌套在其元素中的内容。CComponent 是投影内容的一部分。投影内容完成渲染。`ngAfterContentChecked`火灾。BComponent 完成渲染。组件完成渲染。`ngAfterContentChecked`可能通过变化探测再次开火。

### ngAfterViewInit

视图 DOM 完成初始化后触发一次。视图总是在内容之后加载。`ngAfterViewInit`等待`@ViewChild(ren)`的查询解决。这些元素是从组件的同一个视图中查询的。

在下面的示例中，查询了 BComponent 的`h3`头。`ngAfterViewInit`一旦查询结果可用就执行。

```
import { Component, ViewChild, AfterViewInit, ElementRef, Renderer2 } from '@angular/core';

@Component({
  selector: 'app-c',
  template: `
  <p>I am C.</p>
  <p>Hello World!</p>
  `
})
export class CComponent { }

@Component({
  selector: 'app-b',
  template: `
  <p #BStatement>I am B.</p>
  <ng-content></ng-content>
  `
})
export class BComponent implements AfterViewInit {
  @ViewChild("BStatement", { read: ElementRef }) pStmt: ElementRef;

  constructor(private renderer: Renderer2) { }

  ngAfterViewInit() {
    this.renderer.setStyle(this.pStmt.nativeElement, 'background-color', 'yellow');
  }
}

@Component({
  selector: 'app-a',
  template: `
  <h1>ngAfterViewInit Example</h1>
  <p>I am A.</p>
  <app-b>
    <h3>BComponent Content DOM</h3>
    <app-c></app-c>
  </app-b>
  `
})
export class AComponent { }
```

`Renderer2`改变 BComponent 标题的背景颜色。这表明视图元素由于`ngAfterViewInit`而被成功查询。

****概要:**** 渲染从一个组件开始。若要完成，组件必须呈现 BComponent。BComponent 通过`<ng-content></ng-content>`元素投影嵌套在其元素中的内容。CComponent 是投影内容的一部分。投影内容完成渲染。BComponent 完成渲染。`ngAfterViewInit`火灾。组件完成渲染。`ngAfterViewInit`不会再火了。

### ngAfterViewChecked

`ngAfterViewChecked`在针对组件视图的任何更改检测周期后触发。`ngAfterViewChecked`钩子让开发人员更容易理解变化检测如何影响视图 DOM。

```
import { Component, ViewChild, AfterViewChecked, ElementRef, Renderer2 } from '@angular/core';

@Component({
  selector: 'app-c',
  template: `
  <p>I am C.</p>
  <p>Hello World!</p>
  `
})
export class CComponent { }

@Component({
  selector: 'app-b',
  template: `
  <p #BStatement>I am B.</p>
  <button (click)="$event">CLICK</button>
  <ng-content></ng-content>
  `
})
export class BComponent implements AfterViewChecked {
  @ViewChild("BStatement", { read: ElementRef }) pStmt: ElementRef;

  constructor(private renderer: Renderer2) { }

  randomRGB(): string {
    return `rgb(${Math.floor(Math.random() * 256)},
    ${Math.floor(Math.random() * 256)},
    ${Math.floor(Math.random() * 256)})`;
  }

  ngAfterViewChecked() {
    this.renderer.setStyle(this.pStmt.nativeElement, 'background-color', this.randomRGB());
  }
}

@Component({
  selector: 'app-a',
  template: `
  <h1>ngAfterViewChecked Example</h1>
  <p>I am A.</p>
  <app-b>
    <h3>BComponent Content DOM</h3>
    <app-c></app-c>
  </app-b>
  `
})
export class AComponent { }
```

****概要:**** 渲染从一个组件开始。若要完成，组件必须呈现 BComponent。BComponent 通过`<ng-content></ng-content>`元素投影嵌套在其元素中的内容。CComponent 是投影内容的一部分。投影内容完成渲染。BComponent 完成渲染。`ngAfterViewChecked`火灾。组件完成渲染。`ngAfterViewChecked`可能通过变化探测再次开火。

点击`<button></button>`元素启动一轮变化检测。`ngAfterContentChecked`每点击一次按钮，激发并随机化被查询元素的`background-color`。

### 恩贡德斯特罗伊

从视图和后续 DOM 中移除组件时触发。这个钩子提供了一个机会，可以在删除一个组件之前清理任何遗留问题。

```
import { Directive, Component, OnDestroy } from '@angular/core';

@Directive({
  selector: '[appDestroyListener]'
})
export class DestroyListenerDirective implements OnDestroy {
  ngOnDestroy() {
    console.log("Goodbye World!");
  }
}

@Component({
  selector: 'app-example',
  template: `
  <h1>ngOnDestroy Example</h1>
  <button (click)="toggleDestroy()">TOGGLE DESTROY</button>
  <p appDestroyListener *ngIf="destroy">I can be destroyed!</p>
  `
})
export class ExampleComponent {
  destroy: boolean = true;

  toggleDestroy() {
    this.destroy = !this.destroy;
  }
}
```

****总结:**** 按钮被点击。示例组件的`destroy`成员切换为假。结构指令`*ngIf`的计算结果为假。`ngOnDestroy`火灾。`*ngIf`移除其宿主`<p></p>`。这个过程重复任意次，点击按钮将`destroy`切换到假。

## 结论

请记住，每个挂钩都必须满足某些条件。不管怎样，它们总是按照彼此的顺序执行。这使得钩子具有足够的可预测性，即使有些钩子不执行。

有了生命周期挂钩，确定类的执行时间就变得很容易了。它们让开发人员跟踪变更检测在哪里发生，以及应用程序应该如何反应。对于需要基于负载的依赖项的代码，它们会延迟一段时间才可用。

组件生命周期是现代前端框架的特征。Angular 通过提供前面提到的钩子来规划它的生命周期。

## **来源**

*   [棱角分明的队伍。“生命周期挂钩”。*谷歌*。2018 年 6 月 2 日访问](https://angular.io/guide/lifecycle-hooks)
*   [格切夫·明科。“查看儿童和内容儿童角度”。2018 年 6 月 2 日访问](http://blog.mgechev.com/2016/01/23/angular2-viewchildren-contentchildren-difference-viewproviders)

## **资源**

*   [角度文件](https://angular.io/docs)
*   [Angular GitHub 知识库](https://github.com/angular/angular)
*   [生命周期深度挂钩](https://angular.io/guide/lifecycle-hooks)