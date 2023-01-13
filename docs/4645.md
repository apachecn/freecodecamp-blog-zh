# 什么会出错？如何处理角度误差

> 原文：<https://www.freecodecamp.org/news/global-error-handling-in-angular-with-the-help-of-the-cdk/>

大约一年前，我在一个项目上实现了第一个 e2e 测试。这是一个相当大的应用程序，在后端使用 JAVA SpringBoot，在前端使用 Angular。我们使用量角器作为测试工具，它使用硒。在前端代码中有一个服务，它有一个错误处理方法。调用该方法时，会弹出一个模态对话框，用户可以看到错误的详细信息和堆栈跟踪。

问题是，虽然它跟踪了后端发生的每一个错误，但前端却默默无闻地失败了。*类型错误*、*引用错误*和其他未捕获的异常仅记录到控制台。当 e2e 测试运行期间出现问题时，测试步骤失败时拍摄的屏幕截图完全没有显示任何内容。祝调试愉快！

幸运的是 Angular 有一个内置的处理错误的方法，并且非常容易使用。我们只需要创建自己的服务，它实现 Angular 的 *ErrorHandler* 接口:

```
import { ErrorHandler, Injectable } from '@angular/core';

@Injectable({
    providedIn: 'root'
})
export class ErrorHandlerService implements ErrorHandler{
    constructor() {}

    handleError(error: any) {
        // Implement your own way of handling errors
    }
} 
```

虽然我们可以很容易地在我们的 *AppModule* 中提供我们的服务，但是在一个单独的模块中提供这个服务可能是一个好主意。这样，我们可以创建自己的库，并在未来的项目中使用它:

```
// ERROR HANDLER MODULE
import {ErrorHandler, ModuleWithProviders, NgModule} from '@angular/core';
import {ErrorHandlerComponent} from './components/error-handler.component';
import {FullscreenOverlayContainer, OverlayContainer, OverlayModule} from '@angular/cdk/overlay';
import {ErrorHandlerService} from './error-handler.service';
import {A11yModule} from '@angular/cdk/a11y';

@NgModule({
  declarations: [ErrorHandlerComponent],
  imports: [CommonModule, OverlayModule, A11yModule],
  entryComponents: [ErrorHandlerComponent]
})
export class ErrorHandlerModule {
  public static forRoot(): ModuleWithProviders {
    return {
      ngModule: ErrorHandlerModule,
      providers: [
        {provide: ErrorHandler, useClass: ErrorHandlerService},
        {provide: OverlayContainer, useClass: FullscreenOverlayContainer},
      ]
    };
  }
} 
```

我们使用 *Angular CLI* 来生成 *ErrorHandlerModule* ，所以我们已经生成了一个组件，它可以是我们的模态对话框的内容。为了让我们能够将它放入一个有角度的 CDK 覆盖图中，它需要是一个 entryComponent。这就是为什么我们把它放到了 *ErrorHandlerModule* 的 entryComponents 数组中。

我们还增加了一些进口货。*重叠模块*和*a11 模块*来自 CDK 模块。当我们的错误对话框打开时，需要它们来创建我们的覆盖图和捕捉焦点。如你所见，我们使用*FullscreenOverlayContainer*类来提供 *OverlayContainer* ，因为如果发生错误，我们希望将用户的交互限制在我们的错误模型内。如果我们没有全屏背景，用户可能会与应用程序进行交互，并导致进一步的错误。让我们将新创建的模块添加到我们的 *AppModule* 中:

```
// APP MODULE
import {BrowserModule} from '@angular/platform-browser';
import {NgModule} from '@angular/core';

import {AppRoutingModule} from './app-routing.module';
import {AppComponent} from './app.component';
import {MainComponent} from './main/main.component';
import {ErrorHandlerModule} from '@btapai/ng-error-handler';
import {HttpClientModule} from '@angular/common/http';

@NgModule({
  declarations: [ AppComponent, MainComponent ],
  imports: [
    BrowserModule,
    HttpClientModule,
    ErrorHandlerModule.forRoot(),
    AppRoutingModule,
  ],
  bootstrap: [AppComponent]
})
export class AppModule {
} 
```

现在我们已经有了“ErrorHandlerService ”,我们可以开始实现逻辑了。我们将创建一个模态对话框，以一种清晰易读的方式显示错误。这个对话框将有一个覆盖/背景，它将在角度 CDK 的帮助下被动态放置到 DOM 中。让我们安装它:

```
npm install @angular/cdk --save 
```

根据[文档](https://material.angular.io/cdk/overlay/overview),*叠加*组件需要一些预建的 css 文件。现在，如果我们在我们的项目中使用棱角分明的材料，那就没有必要了，但情况并非总是如此。让我们在 *styles.css* 文件中导入覆盖 css。注意，如果你已经在你的应用程序中使用了角度材质，你不需要导入这个 css。

```
@import '~@angular/cdk/overlay-prebuilt.css'; 
```

让我们使用我们的 *handleError* 方法来创建我们的模态对话框。重要的是要知道，*错误处理器*服务是 Angular 的应用程序初始化阶段的一部分。为了避免令人讨厌的[循环依赖错误](https://stackoverflow.com/a/39767492)，我们使用注入器作为它唯一的构造函数参数。当实际的方法被调用时，我们使用 Angular 的依赖注入系统。让我们从 CDK 导入覆盖图，并将我们的 *ErrorHandlerComponent* 附加到 DOM 中:

```
// ... imports

@Injectable({
   providedIn: 'root'
})
export class ErrorHandlerService implements ErrorHandler {
   constructor(private injector: Injector) {}

   handleError(error: any) {
       const overlay: Overlay = this.injector.get(Overlay);
       const overlayRef: OverlayRef = overlay.create();
       const ErrorHandlerPortal: ComponentPortal<ErrorHandlerComponent> = new ComponentPortal(ErrorHandlerComponent);
       const compRef: ComponentRef<ErrorHandlerComponent> = overlayRef.attach(ErrorHandlerPortal);
   }
} 
```

让我们把注意力转向我们的错误处理程序模型。一个非常简单的解决方案是显示错误消息和堆栈跟踪。让我们也在底部添加一个“解散”按钮。

```
// imports
export const ERROR_INJECTOR_TOKEN: InjectionToken<any> = new InjectionToken('ErrorInjectorToken');

@Component({
  selector: 'btp-error-handler',
  // TODO: template will be implemented later
  template: `${error.message}<br><button (click)="dismiss()">DISMISS</button>`
  styleUrls: ['./error-handler.component.css'],
})
export class ErrorHandlerComponent {
  private isVisible = new Subject();
  dismiss$: Observable<{}> = this.isVisible.asObservable();

  constructor(@Inject(ERROR_INJECTOR_TOKEN) public error) {
  }

  dismiss() {
    this.isVisible.next();
    this.isVisible.complete();
  }
} 
```

如您所见，组件本身非常简单。我们将在模板中使用两个相当重要的指令，以使对话框可访问。第一个是 *cdkTrapFocus* ，它将在呈现对话框时捕获焦点。这意味着用户不能聚焦模式对话框后面的元素。第二个指令是 *cdkTrapFocusAutoCapture* ，它将自动聚焦焦点陷阱中的第一个可聚焦元素。此外，当对话框关闭时，它会自动将焦点恢复到先前聚焦的元素。

为了能够显示错误的属性，我们需要使用构造函数注入它。为此，我们需要自己的*注射。我们还创建了一个相当简单的逻辑，使用 subject 和*dissolve $*属性发出 dissolve 事件。让我们将它与我们服务中的 *handleError* 方法连接起来，并做一些重构。*

```
// imports
export const DEFAULT_OVERLAY_CONFIG: OverlayConfig = {
  hasBackdrop: true,
};

@Injectable({
  providedIn: 'root'
})
export class ErrorHandlerService implements ErrorHandler {

  private overlay: Overlay;

  constructor(private injector: Injector) {
    this.overlay = this.injector.get(Overlay);
  }

  handleError(error: any): void {
    const overlayRef = this.overlay.create(DEFAULT_OVERLAY_CONFIG);
    this.attachPortal(overlayRef, error).subscribe(() => {
      overlayRef.dispose();
    });
  }

  private attachPortal(overlayRef: OverlayRef, error: any): Observable<{}> {
    const ErrorHandlerPortal: ComponentPortal<ErrorHandlerComponent> = new ComponentPortal(
      ErrorHandlerComponent,
      null,
      this.createInjector(error)
    );
    const compRef: ComponentRef<ErrorHandlerComponent> = overlayRef.attach(ErrorHandlerPortal);
    return compRef.instance.dismiss$;
  }

  private createInjector(error: any): PortalInjector {
    const injectorTokens = new WeakMap<any, any>([
      [ERROR_INJECTOR_TOKEN, error]
    ]);

    return new PortalInjector(this.injector, injectorTokens);
  }
} 
```

让我们首先关注作为注入参数提供的误差。正如您所看到的， *ComponentPortal* 类需要一个必需的参数，这就是组件本身。第二个参数是一个 *ViewContainerRef* ，它将影响组件在组件树中的逻辑位置。第三个参数是我们的 *createInejctor* 方法。如您所见，它返回了一个新的 *PortalInjector* 实例。让我们快速看一下它的底层实现:

```
export class PortalInjector implements Injector {
 constructor(
   private _parentInjector: Injector,
   private _customTokens: WeakMap<any, any>) { }

 get(token: any, notFoundValue?: any): any {
   const value = this._customTokens.get(token);

   if (typeof value !== 'undefined') {
     return value;
   }

   return this._parentInjector.get<any>(token, notFoundValue);
 }
} 
```

如您所见，它期望一个*注入器*作为第一个参数，以及一个定制令牌的 WeakMap。我们正是使用与我们的错误本身相关联的*错误注入器令牌*来做到这一点的。创建的 *PortalInjector* 用于正确实例化我们的 *ErrorHandlerComponent* ，它将确保错误本身会出现在组件中。

最后，我们的 *attachPortal* 方法返回最近实例化的组件的*disasset $*属性。我们订阅它，当它发生变化时，我们称之为*。在我们的 *overlayRef* 上 dispose()* 。我们的错误模式对话框被关闭。请注意，我们还在组件内调用了我们主题的 complete，因此，我们不需要取消订阅。

* * *

现在，这对于 clinet 端代码出现问题时抛出的错误非常有用。但是我们正在创建 web 应用程序，并且使用 API 端点。那么当 REST 端点返回一个错误时会发生什么呢？

我们可以处理自己服务中的每个错误，但是我们真的想这样做吗？如果一切正常，就不会抛出错误。如果有特定的需求，例如用一个飞行独角兽来处理 [418 状态码](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/418) ，你可以在它的服务中实现它的处理程序。但是，当我们面对相当常见的错误时，比如 404 或 503，我们可能希望在同一个错误对话框中显示这些错误。

让我们快速收集一下抛出 *HttpErrorResponse* 时会发生什么。它将异步发生，所以我们可能会面临一些变更检测问题。这种错误类型具有与简单错误不同的属性，因此，我们可能需要一个 sanitiser 方法。现在让我们通过为*杀毒错误*创建一个相当简单的接口来深入了解它:

```
export interface SanitizedError {
  message: string;
  details: string[];
} 
```

让我们为我们的 *ErrorHandlerComponent* 创建一个模板:

```
// Imports

@Component({
  selector: 'btp-error-handler',
  template: `
    <section cdkTrapFocus [cdkTrapFocusAutoCapture]="true" class="btp-error-handler__container">
      <h2>Error</h2>
      <p>{{error.message}}</p>
      <div class="btp-error-handler__scrollable">
        <ng-container *ngFor="let detail of error.details">
          <div>{{detail}}</div>
        </ng-container>
      </div>
      <button class="btp-error-handler__dismiss button red" (click)="dismiss()">DISMISS</button>
    </section>`,
  styleUrls: ['./error-handler.component.css'],
})
export class ErrorHandlerComponent implements OnInit {
 // ...
} 
```

我们将整个模态打包到一个 *<部分>* 中，并向其中添加了 *cdkTrapFocus* 指令。这个指令将阻止用户在我们的 overlay/modal 后面的 DOM 中导航。*[cdkTrapFocusAutoCapture]= " true "*确保解散按钮被立即聚焦。当模式关闭时，先前获得焦点的元素将重新获得焦点。我们简单地使用 **ngFor* 显示错误消息和细节。让我们跳回我们的*错误处理服务*:

```
// Imports

@Injectable({
  providedIn: 'root'
})
export class ErrorHandlerService implements ErrorHandler {
  // Constructor

  handleError(error: any): void {
    const sanitised = this.sanitiseError(error);
    const ngZone = this.injector.get(NgZone);
    const overlayRef = this.overlay.create(DEFAULT_OVERLAY_CONFIG);

    ngZone.run(() => {
      this.attachPortal(overlayRef, sanitised).subscribe(() => {
        overlayRef.dispose();
      });
    });
  }

  // ...

  private sanitiseError(error: Error | HttpErrorResponse): SanitizedError {
    const sanitisedError: SanitizedError = {
      message: error.message,
      details: []
    };
    if (error instanceof Error) {
      sanitisedError.details.push(error.stack);
    } else if (error instanceof HttpErrorResponse) {
      sanitisedError.details = Object.keys(error)
        .map((key: string) => `${key}: ${error[key]}`);
    } else {
      sanitisedError.details.push(JSON.stringify(error));
    }
    return sanitisedError;
  }
  // ...
} 
```

通过一个相当简单的 *sanitiseError* 方法，我们创建了一个基于我们之前定义的接口的对象。我们检查错误类型并相应地填充数据。更有趣的部分是使用注射器得到 *ngZone* 。当错误异步发生时，它通常发生在变更检测之外。我们用 *ngZone.run(/*包装我们的 *attachPortal* ...*/)* ，所以当一个 *HttpErrorResponse* 被捕获时，它会在我们的模态中正确地呈现。

尽管目前的状态运行良好，但仍缺乏个性化。我们使用来自 CDK 模块的覆盖，因此为定制配置公开一个注入令牌会很好。这个模块的另一个重要缺点是，当这个模块被使用时，另一个模块不能用于错误处理。例如，集成 Sentry 需要您实现一个类似的轻量级 ErrorHandler 模块。为了能够使用这两者，我们应该在错误处理程序中实现使用钩子的可能性。首先，让我们创建我们的 *InjectionToken* 和默认配置:

```
import {InjectionToken} from '@angular/core';
import {DEFAULT_OVERLAY_CONFIG} from './constants/error-handler.constants';
import {ErrorHandlerConfig} from './interfaces/error-handler.interfaces';

export const DEFAULT_ERROR_HANDLER_CONFIG: ErrorHandlerConfig = {
  overlayConfig: DEFAULT_OVERLAY_CONFIG,
  errorHandlerHooks: []
};

export const ERROR_HANDLER_CONFIG: InjectionToken<ErrorHandlerConfig> = new InjectionToken('btp-eh-conf'); 
```

然后用我们的模块提供它，使用我们现有的 *forRoot* 方法:

```
@NgModule({
  declarations: [ErrorHandlerComponent],
  imports: [CommonModule, OverlayModule, A11yModule],
  entryComponents: [ErrorHandlerComponent]
})
export class ErrorHandlerModule {

  public static forRoot(): ModuleWithProviders {
    return {
      ngModule: ErrorHandlerModule,
      providers: [
        {provide: ErrorHandler, useClass: ErrorHandlerService},
        {provide: OverlayContainer, useClass: FullscreenOverlayContainer},
        {provide: ERROR_HANDLER_CONFIG, useValue: DEFAULT_ERROR_HANDLER_CONFIG}
      ]
    };
  }
} 
```

然后将这个配置处理也集成到我们的 *ErrorHandlerService* 中:

```
// Imports
@Injectable({
  providedIn: 'root'
})
export class ErrorHandlerService implements ErrorHandler {
  // ...

  handleError(error: any): void {
    const sanitised = this.sanitiseError(error);
    const {overlayConfig, errorHandlerHooks} = this.injector.get(ERROR_HANDLER_CONFIG);
    const ngZone = this.injector.get(NgZone);

    this.runHooks(errorHandlerHooks, error);
    const overlayRef = this.createOverlayReference(overlayConfig);
    ngZone.run(() => {
      this.attachPortal(overlayRef, sanitised).subscribe(() => {
        overlayRef.dispose();
      });
    });
  }
  // ...
  private runHooks(errorHandlerHooks: Array<(error: any) => void> = [], error): void {
    errorHandlerHooks.forEach((hook) => hook(error));
  }

  private createOverlayReference(overlayConfig: OverlayConfig): OverlayRef {
    const overlaySettings: OverlayConfig = {...DEFAULT_OVERLAY_CONFIG, ...overlayConfig};
    return this.overlay.create(overlaySettings);
  }
  // ...
} 
```

我们差不多准备好了。让我们将第三方错误处理程序挂钩集成到我们的应用程序中:

```
// Imports
const CustomErrorHandlerConfig: ErrorHandlerConfig = {
  errorHandlerHooks: [
    ThirdPartyErrorLogger.logErrorMessage,
    LoadingIndicatorControl.stopLoadingIndicator,
  ]
};

@NgModule({
  declarations: [
    AppComponent,
    MainComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule,
    ErrorHandlerModule.forRoot(),
    AppRoutingModule,
  ],
  providers: [
    {provide: ERROR_HANDLER_CONFIG, useValue: CustomErrorHandlerConfig}
  ],
  bootstrap: [AppComponent]
})
export class AppModule {
} 
```

正如您所看到的，处理错误是软件开发中极其重要的一部分，但也很有趣。

非常感谢你阅读这篇博文。如果你喜欢阅读代码，请查看我的 [ng-reusables git 库](https://github.com/TapaiBalazs/angular-reusables)。您也可以使用这个 [npm 包](https://www.npmjs.com/package/@btapai/ng-error-handler)来尝试实现。

你也可以在 [Twitter](https://twitter.com/TapaiBalazs) 或者 [GitHub](https://github.com/TapaiBalazs) 上关注我。