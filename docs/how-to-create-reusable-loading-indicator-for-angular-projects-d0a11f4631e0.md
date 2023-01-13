# 如何为角度项目创建可重复使用的加载指示器

> 原文：<https://www.freecodecamp.org/news/how-to-create-reusable-loading-indicator-for-angular-projects-d0a11f4631e0/>

**复用性**。最近在做一个 Angular 项目的时候，一个词在我脑海里闪过好几次。我已经决定创建我自己的角度再利用和博客体验。

![1*hEbJvltnslRrdEzjWQ7Img](img/d5b45a517a28f3518a794172796c1e11.png)

Photo by [Luca Bravo](https://unsplash.com/photos/XJXWbfSo2f0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/front-end?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

那么，什么是负载指示器呢？通常，它是某种带有覆盖层的旋转器，这阻止了用户交互。用户界面不可点击，焦点被捕获。因此，用户不会因为与覆盖层后面的输入交互而意外改变数据或应用程序状态。

加载停止后，带有微调器的覆盖图将从 DOM 中移除，先前聚焦的元素将再次聚焦。

我从触发旋转器的逻辑开始。为此，我使用了一个简单的 BehaviorSubject 和两个装饰函数:

```
import {BehaviorSubject} from 'rxjs';
import {distinctUntilChanged} from 'rxjs/operators';

const indicatorSubject = new BehaviorSubject<boolean>(false);

export const isLoading$ = indicatorSubject.asObservable().pipe(distinctUntilChanged());

export function startLoadingIndicator(target: any, propertyKey: string | symbol, propertyDescriptor: PropertyDescriptor): any {
  const original = propertyDescriptor.value;
  propertyDescriptor.value = (...args) => {
    indicatorSubject.next(true);
    const result = original.call(target, ...args);
    return result;
  };
  return propertyDescriptor;
}

export function stopLoadingIndicator(target: any, propertyKey: string, propertyDescriptor: PropertyDescriptor): any {
  const original = propertyDescriptor.value;
  propertyDescriptor.value = (...args) => {
    indicatorSubject.next(false);
    const result = original.call(target, ...args);
    return result;
  };
  return propertyDescriptor;
} 
```

这样，我们不需要一个可注入的服务来触发或停止旋转器。这两个简单的装饰方法只是调用。下一个()是我们的行为主题。isLoading$变量作为可观察值导出。

让我们在加载指示器组件中使用它。

```
get isLoading$(): Observable<boolean> {
  return isLoading$;
}
```

现在在你的模板中，你可以使用你的 isLoading$ getter 和异步管道来显示/隐藏整个覆盖图。

```
<div class="btp-overlay" *ngIf="isLoading$ | async">
  <div class="btp-loading-indicator__container" [style.width]="indicatorSize" [style.height]="indicatorSize">
    <btp-spinner></btp-spinner>
  </div>
</div>
```

正如你所看到的，我将 spinner 提取到它自己的组件中，并且我还做了一些其他的事情。我添加了一些焦点捕捉的逻辑，以及使用 InjectionToken 配置微调器的大小和颜色的能力。

```
import {LoadingIndicatorConfig} from './interfaces/loading-indicator.interfaces';
import {InjectionToken} from '@angular/core';

export const DEFAULT_CONFIG: LoadingIndicatorConfig = {
  size: 160,
  color: '#7B1FA2'
};

export const LOADING_INDICATOR_CONFIG: InjectionToken<string> = new InjectionToken('btp-li-conf'); 
```

使用 InjectionToken 提供配置对象是在构造函数中提供可配置属性的好方法。

```
 constructor(@Inject(LOADING_INDICATOR_CONFIG)
              private config: LoadingIndicatorConfig) {
  }
```

现在，我们必须将所有内容打包成一个 NgModule:

```
import {ModuleWithProviders, NgModule} from '@angular/core';
import {LoadingIndicatorComponent} from './loading-indicator/loading-indicator.component';
import {CommonModule} from '@angular/common';
import {SpinnerComponent} from './spinner/spinner.component';
import {DEFAULT_CONFIG, LOADING_INDICATOR_CONFIG} from './loading-indicator.config';

@NgModule({
  declarations: [LoadingIndicatorComponent, SpinnerComponent],
  imports: [
    CommonModule
  ],
  exports: [LoadingIndicatorComponent]
})
export class LoadingIndicatorModule {
  static forRoot(): ModuleWithProviders {
    return {
      ngModule: LoadingIndicatorModule,
      providers: [{provide: LOADING_INDICATOR_CONFIG, useValue: DEFAULT_CONFIG}]
    };
  }
}
```

在构建了这个库并将其安装到 Angular 应用程序中之后，使用两个 decorator 方法触发微调器变得非常容易。

首先，我们需要将组件添加到 DOM 中的适当位置。我通常把它放在应用入口组件中，放在模板的底部。

```
<h1>Loading indicator</h1>

<button data-test-id="cy-trigger-indicator" (click)="triggerLoadingIndicator()">START LOADING</button>

<btp-loading-indicator></btp-loading-indicator> 
```

如您所见，当单击按钮时，triggerLoadingIndicator 方法被调用。该方法是一个修饰方法:

```
 @startLoadingIndicator
  triggerLoadingIndicator() {
    setTimeout(this.triggerLoadingIndicatorStop.bind(this), 500);
  }

  @stopLoadingIndicator
  triggerLoadingIndicatorStop() {
    console.log('stopped');
  }
```

就是这样。当然，在实际的应用程序中，可以用它来修饰请求和它们各自的响应处理程序。一个小提示:也装饰你的错误处理器。:)

非常感谢你阅读这篇博文。如果你想试试上面提到的 lib，你可以在这里找到安装包和说明[。](https://www.npmjs.com/package/@btapai/ng-loading-indicator)

你也可以在 [Twitter](https://twitter.com/TapaiBalazs) 或 [GitHub](https://github.com/TapaiBalazs) 上关注我。