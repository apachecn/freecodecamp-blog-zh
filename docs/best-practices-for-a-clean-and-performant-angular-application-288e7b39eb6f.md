# 清洁高效的角度应用的最佳实践

> 原文：<https://www.freecodecamp.org/news/best-practices-for-a-clean-and-performant-angular-application-288e7b39eb6f/>

由 Vamsi 查看

# 清洁高效的角度应用的最佳实践

![SDOlp6vTNKPYse3Oqe0CiNTSvEt3sQhpHQPq](img/791bd9ed28cff7dade4d59e5212c9988.png)

几年来，我一直在新西兰的 [Trade Me](https://trademe.nz) 从事大规模角度应用研究。在过去的几年中，我们的团队已经在编码标准和性能方面改进了我们的应用程序，使其处于最佳状态。

本文概述了我们在应用程序中使用的实践，并与 Angular、Typescript、RxJs 和@ngrx/store 相关。我们还将浏览一些通用的编码指南，以帮助使应用程序更加整洁。

#### **1)轨迹线**

当使用`ngFor`遍历模板中的一个数组时，使用一个`trackBy`函数，它将为每一项返回一个唯一的标识符。

**为什么？**

当数组改变时，Angular 重新渲染整个 DOM 树。但是如果你使用`trackBy`，Angular 会知道哪个元素发生了改变，并且只会对那个特定的元素进行 DOM 改变。

关于这方面的详细解释，请参考[本文](https://netbasal.com/angular-2-improve-performance-with-trackby-cc147b5104e5)作者 [Netanel Basal](https://www.freecodecamp.org/news/best-practices-for-a-clean-and-performant-angular-application-288e7b39eb6f/undefined) 。

**在**之前

```
<li *ngFor="let item of items;">{{ item }}</li>
```

之后

```
`// in the template

<li *ngFor="let item of items; trackBy: trackByFn">{{ item }}</li>

// in the component

trackByFn(index, item) {    
   return item.id; // unique id corresponding to the item
}`
```

#### ****2) const vs let****

**声明变量时，如果值不会被重新分配，请使用 const。**

****为什么？****

**在适当的地方使用`let`和`const`可以使声明的意图更加清晰。当一个值由于抛出编译时错误而被意外地重新分配给一个常量时，它也将有助于识别问题。它还有助于提高代码的可读性。**

****在**之前**

```
`let car = 'ludicrous car';

let myCar = `My ${car}`;
let yourCar = `Your ${car};

if (iHaveMoreThanOneCar) {
   myCar = `${myCar}s`;
}

if (youHaveMoreThanOneCar) {
   yourCar = `${youCar}s`;
}`
```

**之后**

```
**`// the value of car is not reassigned, so we can make it a const
const car = 'ludicrous car';

let myCar = `My ${car}`;
let yourCar = `Your ${car};

if (iHaveMoreThanOneCar) {
   myCar = `${myCar}s`;
}

if (youHaveMoreThanOneCar) {
   yourCar = `${youCar}s`;
}`**
```

#### ****3)管道操作员****

****使用 RxJs 运算符时，请使用可管道化运算符。****

******为什么？******

****可管道操作符是可树摇动的，这意味着只有我们需要执行的代码才会在导入时被包含在内。****

****这也使得识别文件中未使用的操作符变得容易。****

*****注:*这个需要 Angular 版本 5.5+。****

******在**之前****

```
**`import 'rxjs/add/operator/map';
import 'rxjs/add/operator/take';

iAmAnObservable
    .map(value => value.item)
    .take(1);`**
```

****之后****

```
****`import { map, take } from 'rxjs/operators';

iAmAnObservable
    .pipe(
       map(value => value.item),
       take(1)
     );`****
```

#### ******4)隔离 API 黑客******

******并非所有的 API 都是防弹的——有时我们需要在代码中添加一些逻辑来弥补 API 中的错误。与其在组件中需要的地方进行黑客攻击，不如将它们隔离在一个地方——比如在服务中，并从组件中使用服务。******

********为什么？********

****这有助于让黑客“更接近 API”，从而尽可能地接近发出网络请求的地方。这样，处理未受攻击代码的代码就更少了。此外，这是所有黑客居住的地方，更容易找到他们。当修复 API 中的 bug 时，在一个文件中查找它们要比在代码库中查找黑客更容易。****

****您还可以创建类似于 TODO 的 API_FIX 这样的自定义标记，并用它来标记修复，以便更容易找到。****

#### ****5)在模板中订阅****

****避免从组件订阅观察值，而是从模板订阅观察值。****

******为什么？******

****管道会自动取消订阅，并且通过消除手动管理订阅的需要，使代码更加简单。它还降低了意外忘记取消订阅组件中的订阅的风险，这将导致内存泄漏。这种风险也可以通过使用 lint 规则来检测未订阅的可观测量来降低。****

****这也阻止了组件成为有状态的，并防止引入数据在订阅之外发生变异的错误。****

******在**之前****

```
**`// // template

<p>{{ textToDisplay }}</p>

// component

iAmAnObservable
    .pipe(
       map(value => value.item),
       takeUntil(this._destroyed$)
     )
    .subscribe(item => this.textToDisplay = item);`**
```

****之后****

```
****`// template

<p>{{ textToDisplay$ | async }}</p>

// component

this.textToDisplay$ = iAmAnObservable
    .pipe(
       map(value => value.item)
     );`****
```

#### ********6)清理订阅********

****当订阅可观察对象时，一定要确保通过使用像`take`、`takeUntil`等操作符来适当地取消订阅。****

******为什么？******

****未能取消订阅 observables 将导致不必要的内存泄漏，因为 observable 流保持打开，甚至可能在组件被破坏/用户导航到另一个页面之后。****

****更好的是，制定一个 lint 规则来检测未被订阅的可观察对象。****

******在**之前****

```
**`iAmAnObservable
    .pipe(
       map(value => value.item)     
     )
    .subscribe(item => this.textToDisplay = item);`**
```

****之后****

******当您想要监听变化直到另一个可观察值发出一个值时，使用`takeUntil`:******

```
****`private _destroyed$ = new Subject();

public ngOnInit (): void {
    iAmAnObservable
    .pipe(
       map(value => value.item)
      // We want to listen to iAmAnObservable until the component is destroyed,
       takeUntil(this._destroyed$)
     )
    .subscribe(item => this.textToDisplay = item);
}

public ngOnDestroy (): void {
    this._destroyed$.next();
    this._destroyed$.complete();
}`****
```

******像这样使用私有主题是一种管理取消订阅组件中许多可观察对象的模式。******

******使用`take`当你只想要第一个被观察对象发出的值时:******

```
****`iAmAnObservable
    .pipe(
       map(value => value.item),
       take(1),
       takeUntil(this._destroyed$)
    )
    .subscribe(item => this.textToDisplay = item);`****
```

******这里注意`takeUntil`和`take`的用法。这是为了避免组件销毁前订阅没有收到值时导致的内存泄漏。如果这里没有`takeUntil`,订阅仍然会一直存在，直到它获得第一个值，但是由于组件已经被破坏，它将永远不会获得值——导致内存泄漏。******

#### ******7)使用适当的运算符******

******对观察值使用展平运算符时，请根据具体情况使用适当的运算符。******

*******switchMap:* 当你想忽略以前的排放时会有新的排放******

*******mergeMap:* 当您想要同时处理所有排放时******

*******concatMap:* 当您想要一个接一个地处理发射时******

*******排放图:*当您想要在处理之前的排放时取消所有新的排放时******

******关于这个更详细的解释，请参考 [Nicholas Jamieson](https://www.freecodecamp.org/news/best-practices-for-a-clean-and-performant-angular-application-288e7b39eb6f/undefined) 的[这篇](https://blog.angularindepth.com/switchmap-bugs-b6de69155524)文章。******

********为什么？********

****尽可能使用单个操作符，而不是将多个其他操作符链接在一起以达到相同的效果，可以减少交付给用户的代码。使用错误的运算符会导致不必要的行为，因为不同的运算符以不同的方式处理可观察到的内容。****

#### ****8)懒人加载****

****如果可能的话，尝试延迟加载 Angular 应用程序中的模块。延迟加载是指只在需要使用的时候才加载，例如，只在组件要被看到的时候才加载。****

******为什么？******

****这将减少要加载的应用程序的大小，并通过不加载不使用的模块来缩短应用程序的启动时间。****

******在**之前****

```
**`// app.routing.ts

{ path: 'not-lazy-loaded', component: NotLazyLoadedComponent }`**
```

****之后****

```
****`// app.routing.ts

{ 
  path: 'lazy-load',
  loadChildren: 'lazy-load.module#LazyLoadModule' 
}

// lazy-load.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterModule } from '@angular/router';
import { LazyLoadComponent }   from './lazy-load.component';

@NgModule({
  imports: [
    CommonModule,
    RouterModule.forChild([
         { 
             path: '',
             component: LazyLoadComponent 
         }
    ])
  ],
  declarations: [
    LazyLoadComponent
  ]
})
export class LazyModule {}`****
```

#### ******9)避免在订阅中包含订阅******

******有时，您可能需要来自多个可观察对象的值来执行某个操作。在这种情况下，避免在一个观察对象的订阅块中订阅另一个观察对象。相反，使用适当的链接操作符。链接操作符运行在它们之前的操作符的可观察对象上。一些链接操作符有:`withLatestFrom`、`combineLatest`等。******

********在**之前******

```
**`firstObservable$.pipe(
   take(1)
)
.subscribe(firstValue => {
    secondObservable$.pipe(
        take(1)
    )
    .subscribe(secondValue => {
        console.log(`Combined values are: ${firstValue} & ${secondValue}`);
    });
});`**
```

****之后****

```
****`firstObservable$.pipe(
    withLatestFrom(secondObservable$),
    first()
)
.subscribe(([firstValue, secondValue]) => {
    console.log(`Combined values are: ${firstValue} & ${secondValue}`);
});`****
```

********为什么？********

*****代码味道/可读性/复杂性*:没有充分使用 RxJs，表明开发人员不熟悉 RxJs API 表面区域。****

*****性能*:如果可观测量冷了，它会订阅 firstObservable，等待它完成，然后开始第二个可观测量的工作。如果这些是网络请求，它将显示为同步/瀑布。****

#### ****10)避免任何；键入所有内容；****

****总是用不同于`any`的类型声明变量或常量。****

******为什么？******

****当在 Typescript 中声明没有类型的变量或常数时，变量/常数的类型将由赋给它的值来推断。这将导致意想不到的问题。一个经典的例子是:****

```
**`const x = 1;
const y = 'a';
const z = x + y;

console.log(`Value of z is: ${z}`

// Output
Value of z is 1a`**
```

****当您希望 y 也是一个数字时，这会导致不必要的问题。这些问题可以通过适当地输入变量来避免。****

```
**`const x: number = 1;
const y: number = 'a';
const z: number = x + y;

// This will give a compile error saying:

Type '"a"' is not assignable to type 'number'.

const y:number`**
```

****这样，我们可以避免因缺少类型而导致的错误。****

****在你的应用程序中拥有好的类型的另一个好处是它使得重构更加容易和安全。****

****考虑这个例子:****

```
**`public ngOnInit (): void {
    let myFlashObject = {
        name: 'My cool name',
        age: 'My cool age',
        loc: 'My cool location'
    }
    this.processObject(myFlashObject);
}

public processObject(myObject: any): void {
    console.log(`Name: ${myObject.name}`);
    console.log(`Age: ${myObject.age}`);
    console.log(`Location: ${myObject.loc}`);
}

// Output
Name: My cool name
Age: My cool age
Location: My cool location`**
```

****比方说，我们想将`myFlashObject`中的属性`loc`重命名为`location`:****

```
**`public ngOnInit (): void {
    let myFlashObject = {
        name: 'My cool name',
        age: 'My cool age',
        location: 'My cool location'
    }
    this.processObject(myFlashObject);
}

public processObject(myObject: any): void {
    console.log(`Name: ${myObject.name}`);
    console.log(`Age: ${myObject.age}`);
    console.log(`Location: ${myObject.loc}`);
}

// Output
Name: My cool name
Age: My cool age
Location: undefined`**
```

****如果我们没有对`myFlashObject`进行类型化，它会认为`myFlashObject`上的属性`loc`是未定义的，而不是它不是一个有效的属性。****

****如果我们输入`myFlashObject`，我们将得到一个如下所示的编译时错误:****

```
**`type FlashObject = {
    name: string,
    age: string,
    location: string
}

public ngOnInit (): void {
    let myFlashObject: FlashObject = {
        name: 'My cool name',
        age: 'My cool age',
        // Compilation error
        Type '{ name: string; age: string; loc: string; }' is not assignable to type 'FlashObjectType'.
        Object literal may only specify known properties, and 'loc' does not exist in type 'FlashObjectType'.
        loc: 'My cool location'
    }
    this.processObject(myFlashObject);
}

public processObject(myObject: FlashObject): void {
    console.log(`Name: ${myObject.name}`);
    console.log(`Age: ${myObject.age}`)
    // Compilation error
    Property 'loc' does not exist on type 'FlashObjectType'.
    console.log(`Location: ${myObject.loc}`);
}`**
```

****如果您正在开始一个新项目，在`tsconfig.json`文件中设置`strict:true`来启用所有严格的类型检查选项是值得的。****

#### ****11)利用 lint 规则****

****`[tslint](https://palantir.github.io/tslint/)`已经内置了各种选项，如`[no-any](https://palantir.github.io/tslint/rules/no-any)`、`[no-magic-numbers](https://palantir.github.io/tslint/rules/no-magic-numbers)`、`[no-console](https://palantir.github.io/tslint/rules/no-console)`等，您可以在您的`tslint.json`中配置这些选项，以在您的代码库中实施某些规则。****

******为什么？******

****有了 lint 规则，意味着当你做一些不应该做的事情时，你会得到一个好的错误。这将增强应用程序的一致性和可读性。请参考[此处的](https://palantir.github.io/tslint/rules/)了解您可以配置的更多规则。****

****一些 lint 规则甚至附带了解决 lint 错误的修复。如果您想配置自己的自定义 lint 规则，也可以这样做。关于如何使用 [TSQuery](https://github.com/phenomnomnominal/tsquery) 编写自己的自定义 lint 规则，请参考 [Craig Spence](https://www.freecodecamp.org/news/best-practices-for-a-clean-and-performant-angular-application-288e7b39eb6f/undefined) 的[这篇文章](https://medium.com/@phenomnominal/custom-typescript-lint-rules-with-tsquery-and-tslint-144184b6ff2d)。****

******在**之前****

```
**`public ngOnInit (): void {
    console.log('I am a naughty console log message');
    console.warn('I am a naughty console warning message');
    console.error('I am a naughty console error message');
}

// Output
No errors, prints the below on console window:
I am a naughty console message
I am a naughty console warning message
I am a naughty console error message`**
```

****之后****

```
****`// tslint.json
{
    "rules": {
        .......
        "no-console": [
             true,
             "log",    // no console.log allowed
             "warn"    // no console.warn allowed
        ]
   }
}

// ..component.ts

public ngOnInit (): void {
    console.log('I am a naughty console log message');
    console.warn('I am a naughty console warning message');
    console.error('I am a naughty console error message');
}

// Output
Lint errors for console.log and console.warn statements and no error for console.error as it is not mentioned in the config

Calls to 'console.log' are not allowed.
Calls to 'console.warn' are not allowed.`****
```

#### ******12)小型可重复使用的组件******

******提取组件中可以重用的部分，并将其作为一个新组件。让组件尽可能的简单，因为这将使它在更多的场景中工作。使一个组件哑意味着该组件中没有任何特殊的逻辑，并且完全基于提供给它的输入和输出来操作。******

******一般来说，组件树中的最后一个孩子是最笨的。******

********为什么？********

****可重用组件减少了代码的重复，因此更容易维护和更改。****

****哑组件更简单，所以它们不太可能有 bug。哑组件让你更加努力地思考公共组件 API，并帮助你嗅出混合的关注点。****

#### ****13)组件应该只处理显示逻辑****

****尽可能避免在组件中包含除显示逻辑之外的任何逻辑，让组件只处理显示逻辑。****

******为什么？******

****组件是为表示的目的而设计的，并控制视图应该做什么。在适当的情况下，任何业务逻辑都应该提取到它自己的方法/服务中，将业务逻辑与视图逻辑分开。****

****当提取到服务中时，业务逻辑通常更容易进行单元测试，并且可以被任何其他需要应用相同业务逻辑的组件重用。****

#### ****14)避免冗长的方法****

****长方法一般表示他们做的事情太多了。尽量使用单一责任原则。方法本身作为一个整体可能在做一件事，但是在它内部，可能会发生一些其他的操作。我们可以将这些方法提取到它们自己的方法中，让它们各自做一件事，然后使用它们。****

******为什么？******

****冗长的方法很难阅读、理解和维护。它们也容易出现错误，因为改变一件事可能会影响该方法中的许多其他事情。它们也使得重构(这在任何应用程序中都是一件关键的事情)变得困难。****

****这有时被度量为“[圈复杂度](https://en.wikipedia.org/wiki/Cyclomatic_complexity)”。还有一些 [TSLint 规则](https://www.npmjs.com/package/tslint-sonarts)来检测圈/认知复杂性，您可以在您的项目中使用它们来避免 bug 并检测代码气味和可维护性问题。****

#### ****干燥****

****不要重复你自己。确保您没有将相同的代码复制到代码库中的不同位置。提取重复的代码，用它代替重复的代码。****

******为什么？******

****在多个地方有相同的代码意味着如果我们想对代码中的逻辑进行修改，我们必须在多个地方进行。这使得它很难维护，也容易出现错误，我们可能会错过更新它。对逻辑进行修改需要更长的时间，测试也是一个漫长的过程。****

****在这些情况下，提取重复的代码并使用它。这意味着只有一个地方要改变，一件事要测试。向用户交付较少的重复代码意味着应用程序会更快。****

#### ****16)添加缓存机制****

****当进行 API 调用时，其中一些 API 的响应不会经常改变。在这些情况下，您可以添加一个缓存机制并存储来自 API 的值。当对同一个 API 发出另一个请求时，检查缓存中是否有它的值，如果有，就使用它。否则，进行 API 调用并缓存结果。****

****如果值变化不频繁，您可以引入一个缓存时间来检查它最后一次被缓存的时间，并决定是否调用 API。****

******为什么？******

****拥有缓存机制意味着避免不必要的 API 调用。通过只在需要时调用 API 并避免重复，应用程序的速度得到了提高，因为我们不必等待网络。这也意味着我们不会一遍又一遍地下载相同的信息。****

#### ****17)避免模板中的逻辑****

****如果你的模板中有任何类型的逻辑，即使它是一个简单的`&&`子句，最好将其提取到它的组件中。****

******为什么？******

****模板中包含逻辑意味着不可能对其进行单元测试，因此在更改模板代码时更容易出现错误。****

******在**之前****

```
**`// template
<p *ngIf="role==='developer'"> Status: Developer </p>

// component
public ngOnInit (): void {
    this.role = 'developer';
}`**
```

****之后****

```
****`// template
<p *ngIf="showDeveloperStatus"> Status: Developer </p>

// component
public ngOnInit (): void {
    this.role = 'developer';
    this.showDeveloperStatus = true;
}`****
```

#### ******18)琴弦应该是安全的******

******如果您有一个 string 类型的变量，它只能有一组值，您可以将可能值的列表声明为类型，而不是将其声明为 string 类型。******

********为什么？********

****通过适当地声明变量的类型，我们可以避免在编译时而不是运行时编写代码时出现错误。****

******在**之前****

```
**`private myStringValue: string;

if (itShouldHaveFirstValue) {
   myStringValue = 'First';
} else {
   myStringValue = 'Second'
}`**
```

****之后****

```
****`private myStringValue: 'First' | 'Second';

if (itShouldHaveFirstValue) {
   myStringValue = 'First';
} else {
   myStringValue = 'Other'
}

// This will give the below error
Type '"Other"' is not assignable to type '"First" | "Second"'
(property) AppComponent.myValue: "First" | "Second"`****
```

### ********大图********

#### ****状态管理****

****考虑使用 [@ngrx/store](https://github.com/ngrx/platform) 来维护应用程序的状态，使用 [@ngrx/effects](https://github.com/ngrx/effects) 作为 store 的副作用模型。状态变化由动作描述，而变化是由称为 reducers 的纯函数完成的。****

******为什么？******

*****@ngrx/store* 将所有与状态相关的逻辑隔离在一个地方，并使其在整个应用程序中保持一致。当访问存储中的信息时，它还具有适当的记忆机制，从而导致更高性能的应用程序。 *@ngrx/store* 结合 Angular 的变化检测策略导致更快的应用。****

#### ****不可变状态****

****使用 *@ngrx/store* 时，考虑使用 [ngrx-store-freeze](https://github.com/brandonroberts/ngrx-store-freeze) 使状态不可变。 *ngrx-store-freeze* 通过抛出异常来防止状态突变。这避免了状态的意外突变导致不希望的后果。****

******为什么？******

****组件状态的变化会导致应用程序的行为不一致，这取决于组件的加载顺序。它打破了重复模式的思维模式。如果存储状态更改并重新发出，更改可能会被覆盖。关注点分离——组件是视图层，它们不应该知道如何改变状态。****

#### ****玩笑****

****Jest 是脸书的 JavaScript 单元测试框架。通过在代码库中并行化测试运行，它使得单元测试更快。在 watch 模式下，只运行与所做更改相关的测试，这使得测试的反馈循环更短。Jest 也提供了测试的代码覆盖，并在 VS 代码和 Webstorm 上得到支持。****

****你可以为 Jest 使用一个[预置](https://github.com/thymikee/jest-preset-angular)，当你在你的项目中设置 Jest 时，它将为你完成大部分繁重的工作。****

#### ****因果报应****

****[Karma](https://karma-runner.github.io/2.0/index.html) 是 AngularJS 团队研发的一款测试跑者。它需要一个真正的浏览器/DOM 来运行测试。它也可以在不同的浏览器上运行。Jest 不需要 chrome headless/phantomjs 来运行测试，它在 pure Node 中运行。****

#### ****普遍的****

****如果你还没有让你的应用成为通用的应用，现在是时候了。 [Angular Universal](https://angular.io/guide/universal) 让你在服务器上运行 Angular 应用程序，并进行服务器端渲染(SSR ),提供静态预渲染 html 页面。这使得应用程序非常快，因为它几乎可以立即在屏幕上显示内容，而不必等待 JS 包的加载和解析，或者 Angular 的引导。****

****它也是 SEO 友好的，因为 Angular Universal 生成静态内容，使网络爬虫更容易索引应用程序，使其无需执行 JavaScript 即可搜索。****

******为什么？******

****通用极大地提高了应用程序的性能。我们最近更新了我们的应用程序来做服务器端渲染，网站加载时间从几秒钟变成了几十毫秒！！****

****它还允许你的网站正确地显示在社交媒体预览片段中。第一个有意义的绘图非常快，让用户可以看到内容，没有任何不必要的延迟。****

### ****结论****

****构建应用程序是一个持续的旅程，并且总有改进的空间。这个优化列表是一个很好的起点，持续地应用这些模式会让你的团队开心。你的用户也会因为你的少错误和高性能的应用程序的良好体验而喜欢你。****

*****感谢您的阅读！如果您喜欢这篇文章，请随时*？*帮助别人找到它。请不要犹豫，在下面的评论区分享你的想法。关注我的[媒体](https://medium.com/@vamsivempati)或[推特](https://twitter.com/_VamsiVempati_)获取更多文章。快乐的编码人！！？* ☕️****