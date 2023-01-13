# 您已经使用了类型——所以这就是为什么您应该使用类型系统

> 原文：<https://www.freecodecamp.org/news/you-already-use-types/>

这篇文章是为类型系统的怀疑者和新手写的，目的是清晰地表达而不是强行推销。

1.  首先，我们将看看静态类型约定如何出现在动态类型编码中。
2.  然后，我们将后退一步，试着思考这个现象告诉我们，我们应该如何编码。
3.  最后，我们会问一些(领导！)这些见解应该产生的问题。

## 1A:输入姓名

不管什么语言，你学习编码的时候就开始了你的类型之旅。基本列表数据结构邀请相应的复数:

```
var dog = 'Fido'
var dogs = ['Fido', 'Sudo', 'Woof'] 
```

当您处理越来越多的代码时，您开始形成您可能要求您的团队或风格指南的观点:

*   总是使用特定的名字，比如`dogID` vs `dogName` vs `dogBreed`或者一个名称空间/类/对象，比如`dog.name`或者`dog.id`或者`dog.breed`
*   单数不应该是复数的子串，例如坏:`blog`和`blogs`，好:`blogPost`对`blogList`
*   布尔型[应该有一个布尔型前缀](https://github.com/typescript-eslint/typescript-eslint/issues/515)，比如`isLoading`、`hasProperty`、`didChange`
*   有副作用的函数应该有动词
*   内部变量应该有一个`_prefix`

这可能看起来微不足道，因为我们在谈论变量名，但是这条脉络非常深。我们编码中的名称反映了我们对代码的概念和约束，以使代码在规模上更易于维护:

*   [表示组件与有状态/连接容器](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)
*   [原子、分子、有机体、模板、页面](http://bradfrost.com/blog/post/atomic-web-design/)
*   [概念、动作、操作数](https://reactjs.org/blog/2016/09/28/our-first-50000-stars.html#api-churn)(有史以来最成功的名字语法之一)
*   [Block__Element - Modifier](http://getbem.com/naming/)
*   [高阶组件](https://reactjs.org/docs/higher-order-components.html)

这些都相应地渗入到你的代码中:`*Container`、`*Component`、`*Reducer`、`*Template`、`*Page`、`with*`。

一旦你开始跨越执行范例，你就开始摸索到一元类型提示。

Node.js 很早就感受到了这一点:

```
fs.readFile(myfile, callback)
fs.readFileSync(myfile) // introduced when people realized callback hell might not be worth non-blocking 
```

React 引入了`use`前缀来表示挂钩到必须遵守[某些规则](https://reactjs.org/docs/hooks-rules.html)的运行时:

```
function Component() {
  const [bool, setBool] = React.useState(true)
  React.useEffect(callback)
  const foo = useCustomHook()
  // ...
} 
```

我个人喜欢可空性的提醒:

```
const maybeResult = await fetchAPI()
if (maybeResult) {
  const result = maybeResult
  // do things with result
} else {
  // maybeResult is falsy, dont assume it is there
} 
```

在你命名的几乎所有事物中，你已经在使用类型了。

你会问，那又怎样？

继续读下去，我会越读越好的。

## 1B:数据结构中的类型

在名称中对类型进行编码的问题是，这种语言可能不关心您精心命名的变量(事实上，在 JavaScript 中，它可能会被无情地缩小到无法识别)。它将愉快地运行您的代码，如果您忘记遵守自己的 nametypehints，它将抛出一个运行时错误。如果我们通过数据结构使类型在形式上可检查会怎么样？

最基本的是常数。在 Redux 中，显式地(并且冗余地)设置 spinning _ CASE _ CONSTANTS 是很常见的:

```
const ADD_TODO = 'slice/ADD_TODO'

// later in redux code:
import { ADD_TODO } from './redux/types'
switch (action.type) {
  case ADD_TODO:
  // do stuff based on the action
  // ...
} 
```

这主要是因为你不能相信你的开发伙伴不会输入他们的字符串。

然而，即使这些字符串也提供了太多的信任，我们发现添加一个新的语言特性来保证唯一性是非常重要的:

```
const ADD_TODO = Symbol('slice/ADD_TODO') 
```

我们也以这种方式伪造我们的枚举方式:

```
const colors = {
  BLUE: Symbol(1),
  GREEN: Symbol(2),
  RED: Symbol(3),
} 
```

但是简单的值(字符串、数字、布尔值)实际上很容易比较和相应处理。

更紧迫的是在复数值中编码类型。

当您拥有对象数组，并且这些对象在某些方面不同而在其他方面相似时，通常会发生这种情况:

```
const animals = [{ name: 'Fido', legs: 4, says: 'woof' }, { name: 'Kermit', legs: 2, marriedTo: 'Piggy' }]
// will have bugs if an animal with both `says` and `marriedTo` exists
animals.forEach((animal) => {
  if (animal.says) {
    // i guess it's a dog?
  }
  if (animal.marriedTo) {
    // i guess it's a frog?
  }
}) 
```

错误的检查和隐式假定的类型通常是造成痛苦的原因。最好显式键入:

```
const animals = [
  {
    type: 'dog', // new!
    name: 'Fido',
    legs: 4,
    says: 'woof',
  },
  {
    type: 'frog', // new!
    name: 'Kermit',
    legs: 2,
    marriedTo: 'Piggy',
  },
]
animals.forEach((animal) => {
  if (animal.type === 'dog') {
    // must be a dog!
  }
  if (animal.type === 'frog') {
    // must be a frog!
  }
}) 
```

这实际上是 Redux 发生的事情(有趣的是，这对于其他事情也很方便，如[歧视工会](https://basarat.gitbooks.io/typescript/docs/types/discriminated-unions.html))，但你会在[盖茨比](https://github.com/sw-yx/overreacted.io/blob/master/gatsby-config.js#L25-L50)和[中到处看到这种**巴贝尔**](https://babeljs.io/docs/en/plugins/#plugin-options)和[反应](https://reactjs.org/docs/react-without-jsx.html)，我相信你知道我不知道的情况。

类型甚至存在于 HTML 中:`<input type="file">`和`<input type="checkbox">`的行为如此不同！(我已经用 [Block__Element - Modifier](http://getbem.com/naming/) 提到了 CSS 中的类型)

甚至在 HTML/CSS 中，你已经在使用类型了。

## 1C:API 中的类型

我快好了。即使在你的编程语言之外，机器之间的接口也涉及类型。

REST 的重大创新基本上是客户机-服务器请求的原始类型:`GET`、`PUT`、`POST`、`DELETE`。Web 约定在请求中引入了其他类型的字段，比如`accept-encoding`头，你必须遵守这些字段才能得到你想要的。然而，RESTfulness 基本上是不强制的，并且因为它不提供保证，下游工具不能假定行为正确的端点。

GraphQL 采纳了这一想法，并将其扩展到 11:类型是查询、突变和片段的关键，但也是每个字段和每个输入变量的关键，由规范在客户端和服务器端进行验证。有了更强有力的保证，它就能够将更好的工具作为社区规范发布。

我不知道 SOAP、XML、gRPC 和其他机器间通信协议的历史，但我敢打赌有很强的相似性。

## 第 2 部分:这告诉我们什么？

这是一个非常长的，但不详尽的类型检查渗透到你做的每一件事。现在你已经看到了这些模式，你可能会想到更多我现在忘记的例子。但是在每一个转折点上，似乎通向更易维护的代码和更好的工具的方法是以某种方式添加类型。

我在[如何命名事物](https://www.swyx.io/writing/how-to-name-things)中提到了这篇论文的部分内容，但基本上所有的命名模式都属于匈牙利符号的开明形式，正如乔尔·斯波尔斯基的[让错误的代码看起来错误](https://www.joelonsoftware.com/2005/05/11/making-wrong-code-look-wrong/)中所描述的。

如果我所描述的都不能引起你的共鸣，并且不是你已经在做的事情，那么类型可能不适合你。

但是如果是这样的话，而且您一直在以一种草率的方式做这件事，那么您可能会对如何在代码中使用类型的更多结构感兴趣，并且会对使用更好的工具感兴趣，这些工具可以利用您已经为类型付出的所有努力。

你可能正在朝着一个类型系统前进，甚至不知道这一点。

## 第三部分:引导性问题

所以知道了我们现在所知道的在没有类型系统的情况下在代码中使用类型。我会问一些很难的问题。

**问题 1:在没有类型系统的情况下，您目前如何实施类型？**

在个人层面，你从事防御性编码和手工验证。基本上是手动目测自己的代码，条件反射地添加检查和保护，而不知道它们是否真的需要(或者，更糟糕的是，不这样做，在看到运行时异常后才弄清楚)。

在团队层面上，你在代码审查上花费了开发人员数小时的时间，邀请自行车脱落在名字上，我们都知道这很有趣。

这两个过程都是手动方法，并且非常浪费开发人员的时间。不要唱黑脸，这会破坏团队的活力。在规模上，你在数学上肯定会有代码质量的失误(因此导致产品缺陷)，要么是因为每个人都错过了一些东西，要么是因为没有足够的时间，你必须发布一些东西，要么是还没有足够好的政策。

当然，解决方案是使其自动化。正如尼克·施勒克所说，[只要有可能就委托给工装](https://medium.com/@schrockn/on-code-reviews-b1c7c94d868c)。更漂亮和 ESLint 有助于保持你的代码质量——只到程序能根据 AST 理解你的程度。它没有提供任何跨越函数和文件边界的帮助——如果函数`Foo`需要 4 个参数，而你只传递了 3 个，没有 linter 会对你大喊大叫，你必须在`Foo`内部编写防御性代码。

所以用棉绒机只能自动化这么多。剩下的不能自动化的怎么办？

这是最后一个选择:什么都不做。

大多数人不去执行他们非正式设计的类型系统。

问题 2:你自己写了多少这样的类型？

不言而喻，如果你所有的类型策略都是你创建的，那么它们必须由你编写，由你执行。

这与我们今天编写代码的方式完全不同。我们非常依赖开源- [97%的现代 web 应用程序代码来自 npm](https://mobile.twitter.com/housecor/status/1078634947831914496) 。我们导入共享代码，然后编写让我们的应用程序与众不同的最后一英里部分(也就是业务逻辑)。

有分享类型的方法吗？

([是](https://github.com/DefinitelyTyped/DefinitelyTyped/)

问题 3:如果你的类型被标准化了会怎样？

研究表明，程序员采用一种语言的首要原因是现有的可供他们使用的能力和功能。我会学习 Python 使用 TensorFlow。我将学习目标 C 来创建原生 iOS 体验。相应地，JS 之所以如此成功，是因为它可以在任何地方运行，再加上其他人编写的免费开源软件的广泛可用性。有了一些标准化的类型系统，我们可以[导入类型，就像我们导入其他人写的开源软件](https://github.com/DefinitelyTyped/DefinitelyTyped/)一样容易。

就像 GraphQL 和 REST 一样，语言中的标准化类型提供了更好的工具。我将举 4 个例子:

**示例 1:更快的反馈**

我们可能需要几个月甚至几天的时间从运行时错误中学习，而这些错误是暴露给用户的，所以这是最糟糕的结果。

我们编写测试并应用 lint 规则和其他检查将这些错误转移到**构建时错误**，这将反馈周期缩短到几分钟和几小时。(正如我最近写的:[类型不能代替测试！](https://css-tricks.com/types-or-tests-why-not-both/))

类型系统可以将这种反馈缩短另一个数量级，缩短到秒，在**写时间**进行检查。(棉绒也可以这样。两者都需要有一个支持性的 IDE，比如 VS Code)作为副作用，您可以免费获得自动完成功能，因为自动完成和写时间验证是一个硬币的两面。

**示例 2:更好的错误消息**

```
const Foo = {
  getData() {
    return 'data'
  },
}
Foo['getdata']() // Error: undefined is not a function 
```

JavaScript 是有意设计的懒惰评估。我们可以把它移到写时间，而不是在运行时使用可怕的、难以描述的`undefined is not a function`。下面是完全相同的代码的写时错误消息:

```
const Foo = {
  getData() {
    return 'data'
  },
}
Foo['getdata']() // Property 'getdata' does not exist on type '{ getData(): string; }'. Did you mean 'getData'? 
```

为什么是的，打字稿，我做到了。

**例 3:边缘案例穷尽**

```
let fruit: string | undefined
fruit.toLowerCase() // Error: Object is possibly 'undefined'. 
```

除了内置的可空检查(当函数需要 4 个参数时，它负责传递 3 个参数之类的问题)，类型系统可以充分利用枚举(也称为联合类型)。我努力想出一个好例子，但这里有一个:

```
type Fruit = 'banana' | 'orange' | 'apple'
function makeDessert(fruit: Fruit) {
  // Error: Not all code paths return a value.
  switch (fruit) {
    case 'banana':
      return 'Banana Shake'
    case 'orange':
      return 'Orange Juice'
  }
} 
```

**例子 4:无畏的重构**

许多人提到了这一点，老实说，我花了很长时间才明白这一点。人们的想法是:“那又怎样？我不怎么重构。所以这意味着 TypeScript 对我的好处比你小，因为我比你强。”

这是错误的。

当我们开始探索一个问题时，我们开始对解决方案有一个模糊的想法。随着我们的进步，我们对问题了解得更多，或者优先级改变，除非我们已经做了一百万次，否则我们可能会在这个过程中挑出一些错误，无论是函数 API、数据结构还是更大规模的东西。

![refact8](img/e9f66c36debe0fa31cda52f4401ce695.png)

接下来的问题是要么坚持下去直到它崩溃，要么在你感觉到你将不再拥有你曾经拥有的东西的时候进行重构。我假设你承认重构通常有好处。那么我们为什么要避免重构呢？

你推迟重构的原因是它代价高昂，而不是因为它对你没有好处。然而推迟只会增加未来的成本。

类型系统工具有助于显著降低重构的成本，因此您可以更早地体验到好处。它通过更快的反馈、详尽的检查和更好的错误消息来降低成本。

## 广告中的真实

学习不是你写的类型系统是有代价的。这种成本可能会抵消自动化类型检查带来的任何想象中的好处。这就是为什么我花了很大力气来帮助降低学习曲线。但是，请注意，这是一门新语言，会涉及到不熟悉的概念，而且即使是工具也是一项不完善的工作。

但它对 AirBnb、T2、谷歌、亚特兰大、Lyft、Priceline、Slack 和 T11 来说已经足够好了，而且可能适合你。