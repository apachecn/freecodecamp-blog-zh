# 如何单元测试你的第一个 Vue.js 组件

> 原文：<https://www.freecodecamp.org/news/how-to-unit-test-your-first-vue-js-component-14db6e1e360d/>

莎拉·达扬

在 [**构建你的第一个 Vue.js 组件**](https://frontstuff.io/build-your-first-vue-js-component) 中我们做了一个星级组件。我们已经介绍了许多基本概念，以帮助您创建更复杂的 Vue.js 组件。

然而，有一个关键点是你需要构建可以在生产中使用的防弹组件:**单元测试**。

### 为什么要对组件进行单元测试？

单元测试是持续集成的关键部分。它们通过关注小的、孤立的实体，并确保这些实体总是按预期运行，使您的代码更加可靠。您可以自信地迭代您的项目，而不用担心破坏东西。

单元测试并不局限于脚本。只要你遵循一些好的实践，我们可以单独测试的任何东西都是单元可测试的。这些实践包括单一责任、可预测性和松散耦合。

作为我们应用程序的可重用实体， **Vue.js 组件是单元测试的绝佳候选对象**。我们将使用各种输入和用户交互来测试我们作为一个单元制作的程序，并确保它总是按照我们的预期运行。

### 开始之前

从最初的教程开始，一些事情发生了变化。 [Vue CLI 3](https://cli.vuejs.org/) 发布。[Vue Test Utils](https://vue-test-utils.vuejs.org/)——官方的 Vue.js 单元测试实用程序库——已经成熟到 beta 版本。在第一个教程中，我们使用了 [webpack-simple](https://github.com/vuejs-templates/webpack-simple) ，一个不包含测试特性的原型模板。出于这些原因，最简单的做法是从头开始，将项目从教程迁移到更新的 Vue.js 安装中。

我重新创建了第一个教程中的项目，所以你可以直接从 [GitHub](https://github.com/sarahdayan/star-rating-vue-js-tutorial) 下载。然后，导航到解压缩的目录并安装依赖项。

**注意:**在继续下一步之前，请确保您安装了 [Node.js](https://nodejs.org/) :

```
cd path/to/my/project npm install
```

然后，运行项目:

```
npm run serve
```

### 测试工具和笑话

对于本教程，我们将使用官方的 Vue.js 测试工具包 [Vue Test Utils](https://vue-test-utils.vuejs.org/) ，以及由脸书支持的 JavaScript 测试运行器 [Jest](https://jestjs.io/) 。

Vue Test Utils 允许您独立安装 Vue 组件并模拟用户交互。它有测试单文件组件的所有必要工具，包括那些使用 Vue 路由器或 Vuex 的工具。

Jest 是一个全功能的测试运行程序，几乎不需要任何配置。它还提供了一个内置的断言库。

Vue CLI 3(我用来生成[样板文件](https://github.com/sarahdayan/star-rating-vue-js-tutorial))允许您选择您最喜欢的测试运行程序，并为您设置它。如果你想使用另一个测试运行程序(比如[摩卡](https://mochajs.org/)，安装 [Vue CLI 3](https://cli.vuejs.org/) 并生成你自己的启动项目。然后你可以从我的样板文件中移植源文件。

### 我们应该测试什么？

一种常见的单元测试方法是**只关注公共 API** (也就是黑盒测试)。通过忽略实现细节，您可以在不调整测试的情况下改变内部结构。毕竟，你想要做的是**确保你的公共 API 不会破坏**。幕后发生的事情是间接测试的，但重要的是公共 API 保持可靠。

这也是来自 [Vue 测试工具指南](https://vue-test-utils.vuejs.org/guides/#common-tips)的官方建议。因此，我们将只测试我们可以从组件外部访问的内容:

*   用户交互
*   道具变化

我们不会直接测试计算属性、方法或钩子。这些将通过测试公共接口进行隐式测试。

### 设置等级库文件

像常规测试一样，每个组件都有一个描述我们想要运行的所有测试的规格文件。

规格是 JavaScript 文件。按照惯例，它们与正在测试的组件同名，外加一个`.spec`后缀。

继续创建一个`test/unit/Rating.spec.js`文件:

```
// Rating.spec.js
```

```
import { shallowMount } from '@vue/test-utils'import Rating from '@/components/Rating'
```

```
describe('Rating', () => {  // your tests go here})
```

我们已经导入了我们的`Rating`组件和`shallowMount`。后者是一个 Vue Test Utils 函数，它允许我们挂载我们的组件而不挂载它的子组件。

`describe`函数调用包装了我们将要编写的所有测试——它描述了我们的**测试套件**。它有自己的作用域，可以自己包装其他嵌套的套件。

说够了，**让我们开始写测试**。

### 识别测试场景

当我们从外面看`Rating`时，我们可以看到它做了以下事情:

*   它呈现一个星星列表，这个列表等于用户传递的`maxStars`属性的值
*   它向索引低于或等于用户传递的`stars`道具的每个星星添加一个`active`类
*   当用户点击它时，它在一颗星星上切换`active`类，并在下一颗星星上移除它
*   当用户点击一颗星星时，它会切换图标`star`和`star-o`
*   如果用户将`hasCounter`道具设置为`true`，它会显示一个计数器，如果用户将其设置为`false`，它会隐藏计数器，并显示文本，说明当前最大数量的星星中有多少颗是活动的。

请注意，我们只是从外部来看组件做了什么。我们不关心单击一个星号会执行`rate`方法，或者内部的`stars`数据属性会改变。我们可以重命名这些，但这不应该破坏我们的测试。

### 我们的第一个测试

让我们编写第一个测试。我们首先需要用`shallowMount`手动安装我们的组件，并将其存储在一个变量中，我们将对该变量执行断言。我们也可以通过`propsData`属性传递道具，作为一个对象。

安装的组件是一个对象，它附带了一些有用的实用方法:

```
describe('Rating', () => {  const wrapper = shallowMount(Rating, {    propsData: {      maxStars: 6,      grade: 3    }  })  it('renders a list of stars with class `active` equal to prop.grade', () => {    // our assertion goes here  })})
```

然后，我们可以写出我们的第一个断言:

```
it('renders a list of stars with class `active` equal to prop.grade', () => {  expect(wrapper.findAll('.active').length).toEqual(3)})
```

我们来分析一下这里发生了什么。首先，我们使用 Jest 的`[expect](https://jestjs.io/docs/en/expect#expectvalue)`函数，它将我们想要测试的值作为参数。在我们的例子中，我们调用我们的`wrapper`上的`[findAll](https://vue-test-utils.vuejs.org/api/wrapper/#findall-selector)`方法来获取带有`active`类的所有元素。这将返回一个`[WrapperArray](https://vue-test-utils.vuejs.org/api/wrapper-array/)`，它是一个包含数组`[Wrappers](https://vue-test-utils.vuejs.org/api/wrapper/)`的对象。

一个`WrapperArray`有两个属性:`wrappers`(包含的`Wrappers`)和`length`(`Wrappers`的个数)。后者是我们需要拥有预期数量的恒星。

`expect`函数也返回一个对象，我们可以在其上调用方法来测试传递的值。这些方法被称为**匹配器**。这里，我们使用了`toEqual`匹配器，并像在参数中一样传递给它期望值。该方法返回一个布尔值，这是测试期望通过或失败的结果。

总而言之，这里我们说我们期望我们在包装器中找到的带有类`active`的元素的总数等于 3(我们分配给`grade`属性的值)。

在您的终端中，运行您的测试:

```
npm run test:unit
```

你应该看到它通过？

是时候再写一些了。

### 模拟用户输入

Vue Test Utils 使得模拟真实用户在生产中的行为变得容易。在我们的例子中，用户可以点击星星来切换它们。我们可以在测试中用`trigger`方法伪造这一点，并分派各种事件。

```
it('adds `active` class on an inactive star when the user clicks it', () => {  const fourthStar = wrapper.findAll('.star').at(3)  fourthStar.trigger('click')  expect(fourthStar.classes()).toContain('active')})
```

这里，我们首先用`findAll`和`[at](https://vue-test-utils.vuejs.org/api/wrapper-array/#at-index)`得到第四个星号，它从传递的索引处的`WrapperArray`返回一个`Wrapper`(从零开始计数)。然后，我们在上面模拟`click`事件——我们模仿用户点击第四颗星的动作。

由于我们将`grade`属性设置为 3，第四颗星星在我们点击之前应该是不活动的，因此点击事件应该使它活动。在我们的代码中，这是由一个类`active`表示的，只有当它们被激活时，我们才会将它附加到星星上。我们通过调用 star 上的`[classes](https://vue-test-utils.vuejs.org/api/wrapper/#classes-classname)`方法来测试它，该方法以字符串数组的形式返回类名。然后，我们使用`[toContain](https://jestjs.io/docs/en/expect#tocontainitem)`匹配器来确保`active`类在这里。

#### 安装和拆卸

因为我们触发了对组件的点击，所以我们改变了它的状态。问题是我们在所有的测试中都使用了相同的组件。如果我们改变测试的顺序，把这个移到第一个位置，会发生什么？那么第二次测试就会失败。

当涉及到测试时，你不希望依赖于脆弱的东西，比如秩序。一个测试套件应该是健壮的，现有的测试最好不要改变，除非你破坏了 API。

我们希望确保我们总是有一个可预测的包装器来执行断言。我们可以通过安装和拆卸功能来实现这一点。这些是帮助器，让我们在运行测试之前初始化，然后清理。

在我们的例子中，一种方法是在每次测试之前创建我们的包装器，然后销毁它。

```
let wrapper = null
```

```
beforeEach(() => {  wrapper = shallowMount(Rating, {    propsData: {      maxStars: 6,      grade: 3    }  })})
```

```
afterEach(() => {  wrapper.destroy()})
```

```
describe('Rating', () => {  // we remove the `const wrapper = …` expression  // …}
```

顾名思义，`[beforeEach](https://jestjs.io/docs/en/api#beforeeachfn-timeout)`和`[afterEach](https://jestjs.io/docs/en/api#aftereachfn-timeout)`分别在每次测试之前和之后运行。这样我们就可以 100%确定我们在运行新的测试时使用了新的包装。

### 测试的特殊标识符

出于样式和其他目的(比如测试挂钩)混合使用选择器从来都不是一个好主意。

如果您更改了标记名或类会怎样？

如果您在想要测试的元素(比如，在我们的例子中，计数器)上没有特定的标识符，该怎么办呢？

你不希望用无用的类污染你的产品代码。如果有专门的测试挂钩，比如专门的数据属性**，那会好得多，但是只能在测试**期间使用。这样就不会在最终版本中留下混乱。

处理这个问题的一种方法是创建一个[自定义 Vue 指令](https://vuejs.org/v2/guide/custom-directive.html)。

Vue 实例有一个带两个参数的`directive`方法——一个**名称**,一个**函数对象**,用于组件生命周期的每个[钩子，当注入 DOM 时。如果不在乎具体的钩子，也可以传递单个函数。](https://vuejs.org/v2/guide/custom-directive.html#Hook-Functions)

让我们在`src/`中创建一个名为`directives`的新目录，并添加一个`test.js`文件。我们将导出我们希望在指令中传递的函数。

```
// test.js
```

```
export default (el, binding) => {  // do stuff}
```

一个指令钩子可以接受[几个参数](https://vuejs.org/v2/guide/custom-directive.html#Directive-Hook-Arguments)，在我们的例子中，我们只需要前两个参数:`el`和`binding`。`el`参数指的是指令绑定到的元素。`binding`参数是一个包含我们在指令中传递的数据的对象。这样我们可以随心所欲地操纵元素。

```
export default (el, binding) => {  Object.keys(binding.value).forEach(value => {    el.setAttribute(`data-test-${value}`, binding.value[value])  })}
```

我们将一个对象传递给我们的指令，因此我们可以生成以`data-test-`开始的数据属性。在处理函数中，我们遍历了`binding`的每个属性，并在元素上设置了一个数据属性——基于名称和值。

现在我们需要注册我们的指令，以便我们可以使用它。我们可以在全球范围内这样做，但是在我们的例子中，我们只打算在本地注册它——就在我们的`Rating.vue`组件中。

```
<script>import Test from '@/directives/test.js'
```

```
export default {  // …  directives: { Test },  // …}</script>
```

我们的指令现在可以用`v-test`的名字访问。尝试在计数器上设置以下指令:

```
<span v-test="{ id: 'counter' }" v-if="hasCounter">  {{ stars }} of {{ maxStars }}</span>
```

现在用开发者工具在你的浏览器中检查 HTML。您的计数器应该是这样的:

```
<span data-test-id="counter">2 of 5</span>
```

太好了，成功了！现在，无论是在开发模式还是在构建项目时，我们都不需要它。这个数据属性的唯一目的是能够在测试期间定位元素，所以我们只想在运行它们时设置它。为此，我们可以使用 Webpack 提供的`NODE_ENV`环境变量，它是为我们的项目提供动力的模块捆绑器。

当我们运行测试时，`NODE_ENV`被设置为`'test'`。因此，我们可以用它来决定什么时候设置测试属性。

```
export default (el, binding) => {  if (process.env.NODE_ENV === 'test') {    Object.keys(binding.value).forEach(value => {      el.setAttribute(`data-test-${value}`, binding.value[value])    })  }}
```

在浏览器中刷新你的 app，再次检查计数器:**数据属性没了**。

现在我们可以对所有需要定位的元素使用`v-test`指令。让我们来看看之前的测试:

```
it('adds `active` class on an inactive star when the user clicks it', () => {  const fourthStar = wrapper.findAll('[data-test-id="star"]').at(3)  fourthStar.trigger('click')  expect(fourthStar.classes()).toContain('active')})
```

我们已经用`[data-test-id="star"]`替换了`.star`选择器，这允许我们在不破坏测试的情况下为了表示的目的而改变类。我们得到了[单一责任原则](https://en.wikipedia.org/wiki/Single_responsibility_principle)和松散[耦合](https://en.wikipedia.org/wiki/Coupling_(computer_programming))的好处之一——当你的抽象只有一个改变的理由时，你就避免了各种讨厌的副作用。

### 我们是否也应该为我们测试的类使用这些钩子？

在将这个指令设置为要测试的目标元素之后，您可能想知道是否也应该使用它们来替换我们主动寻找的类。让我们看看第一次测试中的断言:

```
expect(wrapper.findAll('.active').length).toEqual(3)
```

我们应该在带有`active`类的元素上使用`v-test`，并替换断言中的选择器吗？**大问题**。

单元测试就是一次测试一件事情。`it`函数的第一个参数是一个字符串，我们用它从消费者的角度描述我们正在做的事情**。**

包装我们断言的测试表明`renders a list of stars with class active equal to prop.grade.`这是消费者所期望的。当他们将一个数字传递给`grade`属性时，他们期望检索一个**相等数量的**活跃的或被选择的恒星。然而，在我们组件的逻辑中，`active`类正是我们用来定义这个特征的。我们根据特定的条件来分配它，所以我们可以从视觉上区分活跃的恒星。这里，这个特定类的存在正是我们想要测试的。

因此，当决定是否应该使用已经拥有的选择器或者设置一个`v-test`指令时，问自己这样一个问题:**我在测试什么，从业务逻辑的角度来看，使用这个选择器有意义吗？**

### 它与功能测试或端到端测试有什么不同？

起初，单元测试组件可能看起来很奇怪。为什么要对 UI 和用户交互进行单元测试？这不就是功能测试的目的吗？

在测试一个组件的公共 API——也就是从**消费者**的角度——和从**用户**的角度测试一个组件之间有一个基本而微妙的区别。首先，让我们强调一些重要的事情:**我们正在测试定义良好的 JavaScript 函数，而不是 UI 片段**。

当你看到一个单文件组件时，很容易忘记这个组件会编译成一个 JavaScript 函数。我们没有测试底层的 Vue 机制，该机制会导致面向 UI 的副作用，比如在 DOM 中注入 HTML。这就是 Vue 自己的测试已经解决的问题。在我们的例子中，我们的组件与任何其他函数没有什么不同:**它接受输入并返回输出**。这些原因和结果是我们正在测试的，而不是别的。

令人困惑的是，我们的测试看起来与常规的单元测试有点不同。通常，我们会这样写:

```
expect(add(3)(4)).toEqual(7)
```

这里没有争论。数据的输入和输出，这就是我们所关心的。对于组件，我们希望事情能够可视化。我们正在遍历一个虚拟 DOM 并测试节点的存在。这也是你在功能测试或端到端测试中所做的，使用像 [Selenium](https://www.seleniumhq.org/) 或 [Cypress.io](https://www.cypress.io/) 这样的工具。那这有什么不同呢？

你不需要混淆**我们正在做什么**来获取我们想要测试的数据和测试的实际**目的**。**通过单元测试，我们测试的是孤立的行为。通过功能测试或端到端测试，我们正在测试场景**。

单元测试确保程序的**单元**按预期运行。它面向组件的**消费者**——在他们的软件中使用组件的程序员。从**用户**的角度来看，功能测试确保**功能**或**工作流**的行为符合预期——最终用户使用完整的软件。

### 更进一步

我不会深入每个测试的细节，因为它们都有相似的结构。你可以在 GitHub 上找到[的完整规范文件，我强烈建议你先自己尝试实现它们。软件测试既是一门科学，也是一门艺术，它需要两倍于理论的实践。](https://github.com/sarahdayan/star-rating-vue-js-tutorial/blob/tests/tests/unit/Rating.spec.js)

不要担心你没有得到所有的东西，或者你正在努力编写你的第一个测试:**测试是出了名的难**。此外，如果你有问题，不要犹豫，打我的[推特](https://twitter.com/frontstuff_io)！

最初发布于 [frontstuff.io](https://frontstuff.io/unit-test-your-first-vuejs-component) 。