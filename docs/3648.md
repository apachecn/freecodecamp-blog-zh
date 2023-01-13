# 用示例解释角度动画

> 原文：<https://www.freecodecamp.org/news/angular-animations-explained-with-examples/>

# 为什么要使用动画？

现代 web 组件经常使用动画。级联样式表(CSS)为开发人员提供了创建令人印象深刻的动画的工具。CSS 可以实现属性转换、唯一命名的动画、多部分关键帧。多亏了 CSS，动画的可能性是无穷无尽的。

在现代 web 应用程序中，动画会吸引用户的注意力。好的动画试图以一种令人满意的、富有成效的方式引导用户的注意力。动画不应该让用户感到厌烦。

动画以运动的形式提供反馈。它们向用户显示应用程序正在主动处理他们的请求。当应用程序必须加载时，一些简单的事情，比如一个可见的按钮或者一个加载器，就能吸引用户的注意力。

动画在 Angular 的案例中变得越来越重要。谷歌在推广材料设计理念的同时发展 Angular。它鼓励简洁的用户界面(UI ),并辅以生动的用户反馈。它让 web 应用程序感觉有点生气，使用起来很有趣。

Angular 社区开发了一个名为 [Material2](https://github.com/angular/material2) 的核心小部件库。这个项目向 Angular 添加了各种小部件模块。大部分都是动画。为了理解它们是如何工作的，本文建议在继续阅读之前先学习 CSS 动画。

Angular animations 是 CSS 本身提供的框架的简化版本。CSS 是 web 浏览器中角度动画的核心技术。但是 CSS 超出了本文的范围。是时候正面处理棱角分明的动画了。

## 设置动画

在制作动画之前，`BrowserAnimationsModule`必须包含到根模块的导入数组中。从`@angular/platform-browser/animations`开始提供。这个 NgModule 确保动画在给定的平台上工作。本文假设每个示例都使用标准的 web 浏览器。

角度动画在`@Component`元数据中声明。`@Component`装饰一个类来区分它是一个有角的组件。它的元数据包含组件配置，包括`animations: []`字段。该字段中的每个数组元素代表一个动画触发器(`AnimationTriggerMetadata`)。

通过装饰者的元数据，动画只属于它们的宿主组件。动画只能在宿主组件的模板中使用。动画不会继承到组件的子组件。对此有一个简单的解决方法。

您总是可以创建一个单独的文件来导出数组。任何组件类都可以从其宿主文件的顶部导入该数组。然后，导入的数组令牌进入组件的动画元数据。对动画元数据中需要相同数组的任何其他组件重复此过程。

内容投影允许您将动画应用到组件 A 的内容 DOM(文档对象模型)。包装这个内容 DOM 的组件 B 可以将内容投影到自己的模板中。一旦这样，组件 A 的动画就不会否定。组件 B 通过内容投影合并了 A 的动画。

好的。您知道如何设置动画以及在哪里声明它们。下一步是实施。

## 动画方法

角度动画使用一系列可从`@angular/animations`导入的方法调用。`@Component`动画数组的每个元素都以单个方法开始。它的参数分解为一系列高阶方法调用。以下列表显示了一些用于构建角度动画的方法。

*   `trigger(selector: string, AnimationMetadata[])`

返回`AnimationTriggerMetadata`

*   `state(data: string, AnimationStyleMetadata, options?: object)`

返回`AnimationStateMetadata`

*   `style(CSSKeyValues: object)`

返回`AnimationStyleMetadata`

*   `animate(timing: string|number, AnimationStyleMetadata|KeyframesMetadata)`

返回`AnimationAnimateMetadata`

*   `transition(stateChange: string, AnimationMetadata|AnimationMetadata[], options?: object)`

返回`AnimationTransitionMetadata`

虽然肯定有更多的方法可以选择，但是这五个方法可以处理基本问题。试图将这些核心方法理解为一个列表并没有太大帮助。一点一点的解释加上一个例子会让你对它有更好的理解。

### 触发器(选择器:string，AnimationMetadata[])

方法封装了动画数组中的一个动画元素。

该方法的第一个参数`selector: string`匹配`[@selector]`成员属性。它的作用类似于组件模板中的属性指令。它本质上是通过属性选择器将动画元素连接到模板。

第二个参数是包含适用动画方法列表的数组。`trigger(...)`将它保存在一个数组中。

### state(data: string，AnimationStyleMetadata，options？:对象)

方法定义了动画的最终状态。动画结束后，它将 CSS 属性列表应用于目标元素。这使得动画元素的 CSS 与动画的分辨率相匹配。

第一个参数匹配绑定到动画绑定的数据的值。也就是说，模板中绑定到`[@selector]`的值与`state(...)`的第一个参数匹配。数据的值决定了最终状态。数值的变化决定了动画的方式(见`transition(...)`)。

第二个参数承载应用于元素后期动画的 CSS 样式。通过调用`style(...)`并将所需的样式作为对象传递给它的参数，可以传递样式。

选项列表可选地占据第三个参数。默认的`state(...)`选项应该保持不变，除非另有理由。

### 样式(CSSKeyValues: object)

你可能已经注意到`AnimationStyleMetadata`在之前的列表中出现过几次。`style(...)`组件返回这种类型的元数据。只要 CSS 样式适用，就必须调用`style(...)`方法。包含 CSS 样式的对象代表它的参数。

当然，CSS 中可动画化的样式被带入 Angular `style(...)`方法。当然，对于 CSS 来说没有什么不可能的事情会因为有角度的动画而突然变得可能。

### animate(计时:字符串|数字，动画样式元数据|动画关键帧元数据)

`animate(...)`函数接受一个计时表达式作为它的第一个参数。该参数对方法的动画进行计时、调步和/或延迟。该参数接受数字或字符串表达式。这里的[解释了](https://angular.io/api/animations/animate#usage)的格式。

`animate(...)`的第二个参数是保证动画的 CSS 属性。这采用了返回`AnimationStyleMetadata`的`style(...)`方法的形式。将`animate(...)`视为启动动画的方法。

一系列关键帧也可以应用于第二个参数。关键帧是一个更高级的选项，本文稍后会解释。关键帧区分动画的各个部分。

`animate(...)`可能收不到第二个自变量。在这种情况下，方法的动画计时只适用于在`state(...)`方法中反映的 CSS。触发器的`state(...)`方法中的属性变化将被激活。

### transition(changExpr: string，animation metadata | animation metadata[]，options？:对象)

`animate(...)`启动动画，而`transition(...)`决定启动哪个动画。

第一个参数由一种独特形式的微语法组成。它表示状态发生了变化(或数据发生了变化)。绑定到模板动画绑定(`[selector]="value"`)的数据决定了这个表达式。接下来的标题为“动画状态”的部分将进一步解释这个概念。

`transition(...)`的第二个自变量包含`AnimationMetadata`(由`animate(...)`返回)。该参数接受一个`AnimationMetadata`数组或一个实例。

第一个参数的值与模板中绑定的数据的值相匹配(`[selector]="value"`)。如果出现完全匹配，则参数评估成功。然后，第二个参数启动一个动画来响应第一个参数的成功。

选项列表可选地占据第三个参数。默认的`transition(...)`选项应该保持不变，除非另有理由。

## 动画示例

```
import { Component, OnInit } from '@angular/core';
import { trigger, state, style, animate, transition } from '@angular/animations';

@Component({
  selector: 'app-example',
  template: `
  <h3>Click the button to change its color!</h3>
  <button (click)="toggleIsCorrect()"     // event binding
    [@toggleClick]="isGreen">Toggle Me!</button>  // animation binding
    `,
    animations: [       // metadata array
      trigger('toggleClick', [     // trigger block
      state('true', style({      // final CSS following animation
        backgroundColor: 'green'
      })),
      state('false', style({
        backgroundColor: 'red'
      })),
      transition('true => false', animate('1000ms linear')),  // animation timing
      transition('false => true', animate('1000ms linear'))
    ])
  ]        // end of trigger block
})
export class ExampleComponent {
  isGreen: string = 'true';

  toggleIsCorrect() {
    this.isGreen = this.isGreen === 'true' ? 'false' : 'true'; // change in data-bound value
  }
}
```

上面的例子执行了一个非常简单的颜色交换。当然，根据`animate('1000ms linear')`，颜色以线性淡入淡出的方式快速过渡。通过将第一个参数`trigger(...)`匹配到`[@toggleClick]`动画绑定，动画绑定到按钮。

绑定绑定到组件类中的值`isGreen`。该值决定了由`trigger(...)`块中的两个`style(...)`方法设置的结果颜色。动画绑定是单向的，因此组件类中`isGreen`的变化会通知模板绑定。也就是动画绑定`[@toggleClick]`。

模板中的按钮元素也绑定了一个`click`事件。点击按钮使`isGreen`切换数值。这将更改组件类数据。动画绑定发现了这一点，并调用其匹配的`trigger(...)`方法。`trigger(...)`位于组件元数据的动画数组中。触发器被调用时会发生两件事。

第一个事件涉及两个`state(...)`方法。`isGreen`的新值与`state(...)`方法的第一个参数匹配。一旦匹配，`style(...)`的 CSS 样式将应用于动画绑定的主机元素的最终状态。`最终状态在所有动画之后生效。

现在是第二次。调用动画绑定的数据更改在两个`transition(...)`方法之间进行比较。其中一个将数据的变化与他们的第一个参数相匹配。第一次点击按钮导致`isGreen`从“真”变为“假”(“真= >假”)。这意味着第一个`transition(...)`方法激活了它的第二个参数。

对应于成功评估的`transition(...)`方法的`animate(...)`函数启动。此方法设置动画颜色淡入淡出的持续时间以及淡入淡出的速度。动画开始播放，按钮变为红色。

这个过程可以在点击一个按钮后发生任何次数。按钮的`backgroundColor`将以线性渐变方式在绿色和红色之间循环。

## 动画状态

`transition(...)`微语法值得详细讨论。Angular 通过评估该语法来确定动画及其计时。存在以下状态转换。他们模拟绑定到动画绑定的数据的变化。

*   `‘someValue’ => ‘anotherValue’`

绑定数据从“someValue”变为“anotherValue”的动画触发器。

*   `‘anotherValue’ => ‘someValue’`

绑定数据从“另一个值”变为“某个值”的动画触发器。

*   `‘someValue’ <=> ‘anotherValue’`

数据从“某个值”变为“另一个值”，反之亦然。

还存在`void`和`*`状态。`void`表示组件正在进入或离开 DOM。这对于进入和退出动画来说是完美的。

*   `‘someValue’ => void`:绑定数据的主机组件是*，离开*DOM
*   `void => ‘someValue’`:绑定数据的主机组件是进入 DOM 的

*`*`表示通配符状态。通配符状态可以解释为“任何状态”。这包括`void`以及对绑定数据的任何其他更改。*

## *关键帧*

*本文介绍了制作角度应用动画的基础知识。先进的动画技术与这些基础技术并存。将关键帧组合在一起就是这样一种技术。其灵感来自于`@keyframes` CSS 规则。如果你使用过 CSS `@keyframes`，你应该已经明白角度关键帧是如何工作的。这就变成了一个语法问题*

*`keyframes(...)`方法从`@angular/animations`导入。它进入了`animate(...)`的第二个论点，而不是典型的`AnimationStyleMetadata`。`keyframes(...)`方法接受一个参数作为数组`AnimationStyleMetadata`。这也可以称为`style(...)`方法的数组。*

*动画的每个关键帧都在`keyframes(...)`数组中。这些关键帧元素是支持`offset`属性的`style(...)`方法。`offset`表示动画持续时间中的一个点，其伴随的样式属性应在该点应用。其值从 0(动画开始)到 1(动画结束)。*

```
*`import { Component } from '@angular/core';
import { trigger, state, style, animate, transition, keyframes } from '@angular/animations';

@Component({
  selector: 'app-example',
  styles: [
    `.ball {
      position: relative;
      background-color: black;
      border-radius: 50%;
      top: 200px;
      height: 25px;
      width: 25px;
    }`
  ],
  template: `
  <h3>Arcing Ball Animation</h3>
  <button (click)="toggleBounce()">Arc the Ball!</button>
  <div [@animateArc]="arc" class="ball"></div>
  `,
  animations: [
    trigger('animateArc', [
      state('true', style({
        left: '400px',
        top: '200px'
      })),
      state('false', style({
        left: '0',
        top: '200px'
      })),
      transition('false => true', animate('1000ms linear', keyframes([
        style({ left: '0',     top: '200px', offset: 0 }),
        style({ left: '200px', top: '100px', offset: 0.50 }),
        style({ left: '400px', top: '200px', offset: 1 })
      ]))),
      transition('true => false', animate('1000ms linear', keyframes([
        style({ left: '400px', top: '200px', offset: 0 }),
        style({ left: '200px', top: '100px', offset: 0.50 }),
        style({ left: '0',     top: '200px', offset: 1 })
      ])))
    ])
  ]
})
export class ExampleComponent {
  arc: string = 'false';

  toggleBounce(){
    this.arc = this.arc === 'false' ? 'true' : 'false';
  }
}`*
```

*上述示例与其他示例的主要区别在于`animate(...)`的第二个参数。它现在包含一个`keyframes(...)`方法，托管一组动画关键帧。虽然动画本身也不同，但制作动画的技术是相似的。*

*单击按钮会使按钮在屏幕上成弧形。弧线按照`keyframes(...)`方法的数组元素(关键帧)移动。在动画的中点(`offset: 0.50`)，球会改变轨迹。当它继续在屏幕上移动时，它会下降到原来的高度。再次单击该按钮会反转动画。*

*`left`和`top`是为元素设置`position: relative;`后的动画属性。`transform`属性可以执行类似的基于运动的动画。`transform`是一个广阔但完全可动画的财产。*

*偏移量 0 和 1 之间可以存在任意数量的关键帧。复杂的动画序列采用关键帧的形式。它们是角度动画中的许多高级技术之一。*

## *带有宿主绑定的动画*

*毫无疑问，您会遇到这样的情况:您希望将动画附加到组件本身的 HTML 元素，而不是组件模板中的元素。这需要更多的努力，因为你不能直接进入模板 HTML 并在那里附加动画。相反，您必须导入`HostBinding`并利用它。*

*这个场景的最小代码如下所示。为了保持一致，我将对上面的代码重复使用相同的动画条件，并且我没有展示任何实际的动画代码，因为你可以很容易地在上面找到。*

```
*`import { Component, HostBinding } from '@angular/core';

@Component({
...
})
export class ExampleComponent {
  @HostBinding('@animateArc') get arcAnimation() {
    return this.arc;
  }
}`*
```

*制作主机组件动画的想法与制作模板中的元素动画非常相似，唯一的区别是您无法访问正在制作动画的元素。在声明`HostBinding`时，仍然需要传递动画的名称(`@animateArc`)，并且仍然需要返回动画的当前状态(`this.arc`)。函数的名字实际上并不重要，所以`arcAnimation`可以被改成任何名字，只要它不与组件上现有的属性名冲突，并且它会工作得非常好。*

## *结论*

*这涵盖了使用角度动画的基础。Angular 使得使用 Angular CLI 设置动画变得非常容易。开始制作第一个动画只需要一个组件类。记住，动画的作用范围是组件的模板。如果您计划在多个组件中使用变换数组，请从单独的文件中导出它。*

*每个动画工具/方法都从`@angular/animations`导出。他们一起工作来提供一个从 CSS 中得到灵感的健壮的动画系统。还有更多方法超出了本文的范围。*

*现在你已经知道了基础知识，可以随意浏览下面的链接来获得更多关于角度动画的信息。*

## *关于角度动画的更多信息:*

*   *[角度文件](https://angular.io/guide)*
*   *[如何使用带角度的动画 6](https://www.freecodecamp.org/news/how-to-use-animation-with-angular-6-675b19bc3496/)*