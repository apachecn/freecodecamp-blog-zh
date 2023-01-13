# 我如何建立一个带有角度动态组件的可定制负载指示器

> 原文：<https://www.freecodecamp.org/news/how-i-built-a-customizable-loading-indicator-with-angular-dynamic-components-a291310f01d/>

最近，我写了一篇关于为 Angular 项目创建一个可重用的加载指示器组件的博文。下一步是使指示器部件可定制。但是，如何将您的组件插入到覆盖层中呢？这就是动态组件可以帮助我们的地方。

***注:*** *自从我上一篇博文以来，我对库的一些部分进行了重构。请随意查看 [git 库](https://github.com/TapaiBalazs/angular-reusables)。*

用例是我们有一个非常容易使用的负载指示器。默认情况下，它有一个微调器，可以使用库的 decorator 方法来触发。然而，我们的最终用户只希望在覆盖图上显示“正在加载…”。我们可以复制逻辑，然后用文本本身替换微调器，但这是相当多余的。

为了能够使用动态组件，首先，我们需要实现一个简单的装饰器。这个装饰器使得将我们自己的组件注入模板成为可能。

```
import { Directive, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[btpIndicatorHost]',
})
export class IndicatorHostDirective {
  constructor(public viewContainerRef: ViewContainerRef) { }
}
```

我们必须将这个指令添加到我们库的 NgModule 中。然后，用以下内容替换加载指示器模板中的微调器组件:

```
<btp-overlay>
  <div class="btp-loading-indicator__container" [style.width]="indicatorSize" [style.height]="indicatorSize">
    <ng-template btpIndicatorHost></ng-template>
  </div>
</btp-overlay>
```

现在我们有了这个模板，我们需要在 loading-indicator 组件中做 3 件事。

1.  将 ComponentFactoryResolver 注入到组件中。
2.  使用@ViewChild 装饰器来获取我们的指示器主机。
3.  加载提供的组件。

```
import {Component, ComponentFactoryResolver, ComponentRef, Inject, OnDestroy, OnInit, ViewChild} from '@angular/core';
import {LOADING_INDICATOR_CONFIG} from '../loading-indicator.config';
import {LoadingIndicatorConfig} from '../interfaces/loading-indicator.interfaces';
import {IndicatorHostDirective} from '../directives/indicator-host.directive';
import {SpinnerComponent} from '../spinner/spinner.component';
import {DEFAULT_SIZE, INDICATOR_COLOR} from '../constants/indicator.constants';

@Component({
  selector: 'btp-loading-indicator',
  templateUrl: './loading-indicator.component.html',
  styleUrls: ['./loading-indicator.component.css']
})
export class LoadingIndicatorComponent implements OnInit, OnDestroy {
  @ViewChild(IndicatorHostDirective)
  host: IndicatorHostDirective;

  constructor(@Inject(LOADING_INDICATOR_CONFIG)
              private config: LoadingIndicatorConfig,
              private componentFactoryResolver: ComponentFactoryResolver) {
  }

  get indicatorSize(): string {
    return `${this.config.size}px`;
  }

  ngOnInit(): void {
    this.loadComponent();
  }

  ngOnDestroy(): void {
    this.host.viewContainerRef.clear();
  }

  private loadComponent() {
    const component = this.config.indicatorComponent || SpinnerComponent;
    const componentFactory = this.componentFactoryResolver.resolveComponentFactory(component as any);
    const viewContainerRef = this.host.viewContainerRef;
    viewContainerRef.clear();
    const componentRef: ComponentRef<any> = viewContainerRef.createComponent(componentFactory);
    componentRef.instance.color = this.config.color || INDICATOR_COLOR;
    componentRef.instance.size = this.config.size || DEFAULT_SIZE;
  }
}
```

我们需要在 OnInit 生命周期挂钩中加载组件。OnInit 钩子在第一个 ngOnChanges()之后运行，它只被调用一次。这是将组件动态加载到 DOM 的理想位置。我们还需要在组件销毁期间清除 viewContainer 引用。

```
 ngOnInit(): void {
    this.loadComponent();
  }

  ngOnDestroy(): void {
    this.host.viewContainerRef.clear();
  }
```

让我们进一步检查一下我们的“loadComponent”方法。我们希望使用我们的配置逻辑来提供我们的定制组件。当配置中没有提供自定义组件时，我们的指示器将是默认的微调组件。

```
 private loadComponent() {
    const component = this.config.indicatorComponent || SpinnerComponent;
    const componentFactory = this.componentFactoryResolver.resolveComponentFactory(component as any);
    const viewContainerRef = this.host.viewContainerRef;
    viewContainerRef.clear();
    const componentRef: ComponentRef<any> = viewContainerRef.createComponent(componentFactory);
    componentRef.instance.color = this.config.color || INDICATOR_COLOR;
    componentRef.instance.size = this.config.size || DEFAULT_SIZE;
  }
```

然后我们使用 componentFactoryResolver 来获取组件的工厂。为了安全起见，我们首先清除我们的视图 ContainerRef。然后，我们使用解析的工厂创建组件，并在创建的实例上设置配置值。

我们的最终用户只想要一个小文本，而不是一个花哨的旋转。一个相当简单的组件如下所示:

```
import {Component} from '@angular/core';

@Component({
  selector: 'app-loading-message',
  template: `<h1>Loading...</h1>`,
  styles: [``]
})
export class LoadingMessageComponent {
}
```

我们在应用程序的主模块中提供它，在那里我们设置和配置我们的库。将组件添加到“entryComponents”数组中可以确保在加载过程中可以解析其工厂。

从现在开始，我们可以在我们的任何 Angular 项目中替换指示器组件，而不需要一遍又一遍地重新实现大部分逻辑。

```
@NgModule({
  declarations: [AppComponent, LoadingMessageComponent],
  imports: [
    CommonModule,
    AppRoutingModule,
    LoadingIndicatorModule.forRoot(),
  ],
  providers: [
    {
      provide: LOADING_INDICATOR_CONFIG,
      useValue: {
        indicatorComponent: LoadingMessageComponent
      }
    }
  ],
  entryComponents: [LoadingMessageComponent]
})
export class AppModule {
}
```

*如果你想了解更多关于动态组件的知识，我推荐你阅读:[这里是你需要了解的关于动态组件的知识](https://blog.angularindepth.com/here-is-what-you-need-to-know-about-dynamic-components-in-angular-ac1e96167f9e)作者 [**Max Koretskyi**](https://twitter.com/maxkoretskyi)*

非常感谢你阅读这篇博文。如果你想试试上面提到的 lib，你可以在这里找到安装包和说明[。](https://www.npmjs.com/package/@btapai/ng-loading-indicator)

你也可以在 [Twitter](https://twitter.com/TapaiBalazs) 或 [GitHub](https://github.com/TapaiBalazs) 上关注我。