# 创建可重用 UI 组件的提示和技巧

> 原文：<https://www.freecodecamp.org/news/tips-tricks-for-creating-reusable-ui-components-2b1452147bda/>

加布里埃尔·科伦坡

# 创建可重用 UI 组件的提示和技巧

![MtpKyjS5FI6RzBjGfV7kmEGuDdfyGLCd5Gks](img/463b48c24d026027ba6beb67d5aebc0b.png)

Photo by [Farzard Nazifi](https://unsplash.com/photos/p-xSl33Wxyc)

在这篇文章中，我想分享一些我在使用 Ember.js 构建我们的核心前端库时使用的技巧和诀窍。我希望你们喜欢它！请注意，用于举例说明文章中观点的代码包含的信息足以让读者理解这一点。它还使用了一些 Ember.js 术语，但是这些概念是与框架无关的。

### 目标

简单地说，构建库的要求如下:

1.  它必须是多产的。
2.  它必须是可维护的。
3.  必须是一致的。

### 方法

#### 最小化业务逻辑

我在项目中遇到的最常见的问题之一是组件中包含了太多的逻辑。因此，执行理论上超出其范围的任务。

在实现任何功能之前，最好先概述一下组件负责的一些职责。

假设我们正在构建一个按钮组件。

我希望能够:

*   告知按钮的类型——主按钮还是常规按钮
*   告知按钮内显示的内容(图标和文本)
*   禁用或启用按钮
*   单击时执行一些操作

有了这个小小的轮廓，把构建这个组件的过程中涉及到的不同部分分开。试着确定东西可以放在哪里。

1-类型和内容是特定于组件的，因此它们可以放在组件文件中。

因为类型在某种程度上是必需的，所以让我们添加一个验证，以防没有提供值。

```
const type = get(this, 'type');
```

```
const types = {  primary: 'btn--primary',  regular: 'btn--regular',}
```

```
return (type) ? types[type] : types.regular;
```

我喜欢将属性映射到一个对象中，因为它允许事情不需要太多的努力就可以扩展——以防我们需要一个危险按钮或类似的东西。

2 —禁用状态可以在不同的组件上找到，如输入。为了避免重复，这种行为可以被移到一个模块或任何共享结构中——人们称之为 *mixin* 。

3 —单击动作可以在不同的组件中找到。所以它也可以被移动到另一个文件中，并且不应该包含任何逻辑——只需调用开发人员提供的回调函数。

这样，我们可以知道我们的组件需要处理什么情况，同时帮助勾勒出支持扩展的基础架构。

#### 独立的可重用 UI 状态

某些 UI 交互在不同的组件中是通用的，例如:

*   启用/禁用— *例如按钮、输入*
*   扩展/收缩— *例如，折叠下拉列表*
*   显示/隐藏— *几乎所有内容*

这些属性通常只是用来控制视觉状态——希望如此。

在不同的组件中保持一致的命名。所有与视觉状态相关的动作都可以移动到 mixin 中。

```
/* UIStateMixin */
```

```
disable() {  set(this, ‘disabled’, true);
```

```
 return this;},
```

```
enable() {  set(this, 'disabled', false');
```

```
 return this;},
```

每个方法只负责切换特定的变量，并返回链接的当前上下文，例如:

```
button  .disable()  .showLoadingIndicator();
```

这种方法可以扩展。它可以接受不同的上下文并控制外部变量，而不是使用内部变量。例如:

```
_getCurrentDisabledAttr() {  return (isPresent(get(this, 'disabled')))    ? 'disabled'            /*  External parameter  */    : 'isDisabled';         /*  Internal variable   */},
```

```
enable(context) {  set(context || this, this._getCurrentDisabledAttr(), false);
```

```
 return this;}
```

#### 抽象基本功能

每个组件都包含特定的例程。无论组件的用途是什么，都必须执行这些例程。例如，在触发回调之前验证它。

这些默认方法也可以移动到它们自己的 mixins 中，就像这样:

```
/* BaseComponentMixin */
```

```
_isCallbackValid(callbackName) {  const callback = get(this, callbackName);    return !!(isPresent(callback) && typeof callback === 'function');},
```

```
_handleCallback(callback, params) {  if (!this._isCallbackValid(callback)) {    throw new Error(/* message */);  }
```

```
 this.sendAction(callback, params);},
```

然后包含在组件中。

```
/* Component */
```

```
onClick(params) {  this._handleCallback('onClick', params);}
```

这使您的基础架构保持一致。它还允许扩展，甚至与第三方软件集成。但是请不要做 [*哲思抽象者*](https://www.quora.com/What-are-the-growth-stages-of-a-programmer/answer/Andreas-Blixt) 。

#### 构成组件

尽可能避免重写功能。可以做到专业化。可以通过构图和分组来完成。以及将较小的组件组合在一起以创建新的组件。

例如:

```
Base components: Button, dropdown, input.
```

```
Dropdown button => button + dropdownAutocomplete => input + dropdownSelect => input (readonly) + dropdown
```

这样，每个组件都有自己的职责。每个组件处理自己的状态和参数，而包装器组件处理其特定的逻辑。

*关注点的最佳分离。*

#### 分裂的关注

当组合更复杂的组件时，有可能会拆分关注点。您可以在组件的不同部分之间划分关注点

假设我们正在构建一个精选组件。

```
{{form-select binding=productId items=items}}
```

```
items = [  { description: 'Product #1', value: 1 },  { description: 'Product #2', value: 2 }]
```

在内部，我们有一个简单的输入组件和一个下拉菜单。

```
{{form-input binding=_description}}
```

```
{{ui-dropdown items=items onSelect=(action 'selectItem')}}
```

我们的主要任务是向用户呈现描述，但是它对我们的应用程序没有意义——值有意义。

当选择一个选项时，您拆分对象，通过一个内部变量将描述向下发送到我们的输入，同时将值向上推送到控制器，更新绑定变量。

这个概念可以应用于必须转换绑定值的组件，如数字、自动完成或选择字段。日期选择器也可以实现此行为。他们可以在更新绑定变量之前取消日期屏蔽，同时向用户显示屏蔽的值。

随着转换复杂性的增加，风险也越来越高。由于过多的逻辑或者必须支持事件——所以在实现这种方法之前要想清楚。

#### 预设与新组件

有时为了促进开发，有必要优化组件和服务。这些以预设或新组件的形式提供。

预设是参数。当被通知时，它们在组件上设置预定义的值，简化了它的声明。然而，新组件通常是基础组件的更专门化的版本。

难的是要知道什么时候实现预置或者创建新的组件。在做这个决定时，我遵循以下指导原则:

**何时创建预设**

1 —重复使用模式

有时候，某个特定的组件会在不同的地方以相同的参数被重用。在这些情况下，我更喜欢预置而不是新组件，尤其是当基本组件有过多的参数时。

```
/* Regular implementation */
```

```
{{form-autocomplete    binding=productId    url="products"            /*   URL to be fetched         */    labelAttr="description"   /*   Attribute used as label   */    valueAttr="id"            /*   Attribute used as value   */    apiAttr="product"         /*   Param sent on request     */}}
```

```
/* Presets */
```

```
{{form-autocomplete    preset="product"    binding=productId}}
```

只有在没有通知参数的情况下，才设置预置值，以保持其灵活性。

```
/* Naive implementation of the presets module */
```

```
const presets = {  product: {    url: ‘products’,    labelAttr: ‘description’,    valueAttr: ‘id’,    apiAttr: ‘product’,  }, }
```

```
const attrs = presets[get(this, ‘preset’)];
```

```
Object.keys(attrs).forEach((prop) =&gt; {  if (!get(this, prop)) {    set(this, prop, attrs[prop]);  }});
```

这种方法减少了定制组件所需的知识。同时，它允许您在一个地方更新默认值，从而方便了维护。

2-基础组件太复杂

当您用来创建更具体组件的基本组件接受太多参数时。因此，创建它会产生一些问题。例如:

*   您必须将新组件的大部分(如果不是全部)参数注入到基本组件中。随着越来越多的组件从它派生出来，对基础组件的任何更新都会反映出大量的变化。因此，导致更高的 bug 发生率。
*   随着创建的组件越来越多，记录和记忆不同的细微差别就越来越难。对于新开发者来说更是如此。

**何时创建新组件**

1-扩展功能

当从一个简单的组件扩展功能时，创建一个新的组件是可行的。它有助于防止特定于组件的逻辑泄露给另一个组件。这在实现额外行为时特别有用。

```
/* Declaration */
```

```
{{ui-button-dropdown items=items}}
```

```
/* Under the hood */
```

```
{{#ui-button onClick=(action 'toggleDropdown')}}  {{label}} <i class="fa fa-chevron-down"></i>  {{/ui-button}}
```

```
{{#if isExpanded}}  {{ui-dropdown items=items}}{{/if}}
```

上面的例子使用了按钮组件。这扩展了它的布局以支持固定图标，同时包括一个下拉组件及其可见性状态。

2 —装饰参数

创建新组件还有另一个可能的原因。这是需要控制参数可用性或修饰默认值的时候。

```
/* Declaration */
```

```
{{form-datepicker onFocus=(action 'doSomething')}}
```

```
/* Under the hood */
```

```
{{form-input onFocus=(action '_onFocus')}}
```

```
_onFocus() {  $(this.element)    .find('input')    .select();                 /* Select field value on focus */
```

```
 this._handleCallback('onFocus'); /* Triggers param callback */}
```

在这个例子中，它被提供给组件一个当字段被聚焦时要调用的函数。

在内部，它不是将回调直接传递给基本组件，而是传递一个内部函数。这将执行特定的任务(选择字段值)，然后调用提供的回调。

它没有重定向基本输入组件接受的所有参数。这有助于控制某些功能的范围。它还避免了不必要的验证。

在我的例子中，onBlur 事件被另一个事件——onChange 取代。当用户填写字段或选择日历上的日期时，就会触发此操作。

### 结论

当构建你的组件时，考虑你的立场以及在日常生活中使用该组件的人。这样，大家都赢了。

> 最好的结果来自于团队中的每个人做对自己和团队最有利的事情——约翰·纳西

此外，不要羞于寻求反馈。你总能找到可以改进的地方。

为了进一步提高你的软件工程技能，我推荐跟随 [Eric Elliott](https://www.freecodecamp.org/news/tips-tricks-for-creating-reusable-ui-components-2b1452147bda/undefined) 的系列“*编写软件”。*太牛逼了！

好吧，我希望你喜欢这篇文章。请把这些概念，变成自己的想法，分享给我们！

还有，随时在 twitter 上联系我 [@gcolombo_](https://twitter.com/gcolombo_) ！我很想听听你的意见，甚至一起工作。

谢谢！