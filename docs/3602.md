# 角管讲解:如何直接转换模板数据

> 原文：<https://www.freecodecamp.org/news/angular-pipes-explained-how-to-transform-template-data-directly/>

# **管道**

#### **动机**

输出数据转换。它们确保数据在加载到用户屏幕上时是理想的格式。通常数据在幕后转换。使用管道，可以在模板 HTML 中转换数据。管道直接转换模板数据。

管道看起来很漂亮，也很方便。它们有助于保持组件的类没有基本转换。从技术上来说，管道封装了数据转换逻辑。

#### **输出转换**

如前一节所述，管道内联转换数据。管道的语法与外壳脚本相关联。在许多脚本中，命令一部分的输出通过管道*作为输出传递到下一部分作为输入。同样的趋势也体现在弯管上。*

在 Angular 中，有许多方法可以与模板 HTML 中的数据进行交互。管道可以应用于将数据解析成模板 HTML 的任何地方。它们可以出现在 microsyntax 逻辑和 innerHTML 变量插值中。管道考虑了所有的转换，而没有添加到组件类中。

管道也是*可链接的*。您可以一个接一个地集成管道来执行日益复杂的转换。作为专门的数据转换器，管道并不简单。

#### **用例**

Angular 预装了一套基本的管道。与他们中的几个人一起工作将会培养出处理其余问题的直觉。下面的列表提供了两个例子。

*   异步管道
*   数据管道

这两个执行简单的任务。它们的简单性非常有益。

##### **异步管道**

这部分需要对承诺或可观察到的事物有一个基本的理解，才能完全领会。AsyncPipe 在这两者中的任何一个上运行。AsyncPipe 从承诺/可观察值中提取数据，作为下一步的输出。

对于 Obervables，AsyncPipe 自动订阅数据源。不管数据来自哪里，AsyncPipe 都订阅源可观察对象。`async`是 AsyncPipe 的语法名称，如下所示。

```
<ul *ngFor=“let potato of (potatoSack$ | async); let i=index”>
  <li>Potatoe {{i + 1}}: {{potato}}</li>
</ul>
```

在本例中，`potatoSack$`是一个等待上传土豆的可观察值。一旦土豆同步或异步到达，AsyncPipe 就以一个*可迭代*数组的形式接收它们。然后列表元素被土豆填满。

##### **日期管**

用 JavaScript `Date`对象格式化日期字符串需要相当多的时间。假设给定的输入是有效的时间格式，DatePipe 提供了一种强大的方法来格式化日期。

```
// example.component.ts

@Component({
  templateUrl: './example.component.html'
})
export class ExampleComponent {
  timestamp:string = ‘2018-05-24T19:38:11.103Z’;
}
```

```
<!-- example.component.html -->

<div>Current Time: {{timestamp | date:‘short’}}</div>
```

上面`timestamp`的格式是[ISO 8601¹](https://en.wikipedia.org/wiki/ISO_8601)——不是最容易读懂的。DatePipe ( `date`)将 ISO 日期格式转换成更传统的`mm/dd/yy, hh:mm AM|PM`。还有许多其他格式选项。所有这些选项都在[官方文档](https://angular.io/api/common/DatePipe)中。

#### **创建管道**

虽然 Angular 只有固定数量的管道，但`@Pipe` decorator 允许开发人员创建自己的管道。这个过程从`ng generate pipe [name-of-pipe]`开始，用一个更好的文件名替换`[name-of-pipe]`。这是一个[角度 CLI](https://cli.angular.io/) 命令。它产生以下结果。

```
import { Pipe, PipeTransform } from ‘@angular/core’;

@Pipe({
  name: 'example'
})
export class ExamplePipe implements PipeTransform {
  transform(value: any, args?: any): any {
      return null;
  }
}
```

该管道样板简化了自定义管道的创建。装饰者告诉 Angular 这个类是一个管道。`name: ‘example’`的值，在这里是`example`，是 Angular 在扫描定制管道的模板 HTML 时识别的值。

关于类逻辑。`PipeTransform`实现为`transform`函数提供了指令。这个函数在`@Pipe`装饰器的上下文中有特殊的含义。默认情况下，它接收两个参数。

`value: any`是管道接收的输出。想到`<div>{{ someValue | example }}</div>`。someValue 的值被传递给`transform`函数的`value: any`参数。这是在 ExamplePipe 类中定义的同一个`transform`函数。

`args?: any`是管道可选接收的任何参数。想到`<div>{{ someValue | example:[some-argument] }}</div>`。`[some-argument]`可以用任何一个值代替。这个值被传递给`transform`函数的`args?: any`参数。也就是 ExamplePipe 的类中定义的`transform`函数。

无论函数返回什么(`return null;`)都会成为管道操作的输出。请看下一个例子，以了解 ExamplePipe 的完整示例。根据管道接收的变量，它会将输入大写或小写，作为新的输出。无效或不存在的参数将导致管道返回与输出相同的输入。

```
// example.pipe.ts

@Pipe({
  name: 'example'
})
export class ExamplePipe implements PipeTransform {
  transform(value:string, args?:string): any {
    switch(args || null) {
      case 'uppercase':
        return value.toUpperCase();
      case 'lowercase':
        return value.toLowerCase();
      default:
        return value;
    }
  }
}
```

```
// app.component.ts

@Component({
  templateUrl: 'app.component.html'
})
export class AppComponent {
  someValue:string = "HeLlO WoRlD!";
}
```

```
<!-- app.component.html -->

<!-- Outputs “HeLlO WoRlD!” -->
<h6>{{ someValue | example }}</h6>

<!-- Outputs “HELLO WORLD!” -->
<h6>{{ someValue | example:‘uppercase’ }}</h6>

<!-- Outputs “hello world!” -->
<h6>{{ someValue | example:‘lowercase’ }}</h6>
```

理解了上面的例子，就意味着你理解了角管。只剩下一个话题要讨论了。

#### **纯净和不纯净管道**

到目前为止，你所看到的一切都是纯粹的*烟斗。默认情况下，`pure: true`在`@Pipe`装饰器的元数据中设置。两者之差构成了 Angular 的变化检测。*

每当其派生输入的值发生变化时，纯管道就会自动更新。当输入值发生可检测的变化时，管道将重新执行以产生新的输出。可检测的变化由 Angular 的变化检测回路决定。

每当发生变化检测周期时，不纯管道会自动更新。Angular 的变化检测经常发生。如果组件类的成员数据发生变化，它会发出信号。如果是这样，模板 HTML 将使用更新后的数据进行更新。否则，什么都不会发生。

在不纯管道的情况下，无论是否有可检测的变化，它都将更新。每当变化检测循环时，不纯管道更新。不纯的管道通常会消耗大量资源，并且通常是不明智的。也就是说，它们更像是一个逃生出口。如果你需要一个对检测敏感的管道，在`@Pipe`装饰器的元数据中切换`pure: false`。

#### **结论**

那包括管道。管道还有很多潜力，超出了本文的范围。管道为组件的模板 HTML 提供了简洁的数据转换。在数据必须经历小的转换的情况下，它们是很好的实践。