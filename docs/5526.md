# 函数式编程模式:食谱

> 原文：<https://www.freecodecamp.org/news/functional-programming-patterns-cookbook-3a0dfe2d7e0a/>

本文的目标读者是从像`ramda`这样的函数库毕业，开始使用代数数据类型的人。我们将优秀的`[crocks](https://evilsoft.github.io/crocks/?source=post_page---------------------------)`库用于我们的 ADT 和助手，尽管这些概念可能也适用于其他概念。我们将专注于展示实际的应用程序和模式，而不是钻研大量的理论。

#### 安全执行危险功能

假设我们有这样一种情况，我们想要使用第三方库中的一个名为`darken`的函数。`darken`取一个乘数，一个颜色，返回该颜色的较暗阴影。

```
// darken :: Number -> String -> String
darken(0.1)("gray")
//=> "#343434"
```

对于我们的 CSS 需求来说非常方便。但事实证明，函数并没有看起来那么无辜。收到意外参数时抛出错误！

```
darken(0.1)(null)
=> // Error: Passed an incorrect argument to a color function, please pass a string representation of a color. 
```

当然，这对于调试非常有帮助——但是我们不希望我们的应用程序仅仅因为我们不能获得颜色而崩溃。这里是`tryCatch`前来救援的地方。

```
import { darken } from "polished"
import { tryCatch, compose, either, constant, identity, curry } from "crocks"

// safeDarken :: Number -> String -> String
const safeDarken = curry(n =>
  compose(
    either(constant("inherit"), identity),
    tryCatch(darken(n))
  )
)
```

`tryCatch`执行 try-catch 块中提供的函数，并返回一个名为`Result`的 Sum 类型。本质上，Sum 类型基本上是“或”类型。这意味着如果操作成功**`Result`可能是一个`Ok`，或者如果操作失败** 可能是一个`Error`。Sum 类型的其他例子还有`Maybe`、`Either`、`Async`等等。`either` point-free helper 将值从`Result`框中取出，如果事情变糟，则返回 CSS 默认值`inherit`，如果一切顺利，则返回变暗的颜色。

```
safeDarken(0.5)(null)
//=> inherit

safeDarken(0.25)('green')
//=> '#004d00'
```

#### 使用可能的帮助器强制类型

使用 JavaScript，我们经常遇到函数爆炸的情况，因为我们期望一个特定的数据类型，但是我们收到的却是一个不同的类型。`crocks`提供了`safe`、`safeAfter`和`safeLift`函数，允许我们通过使用`Maybe`类型更可预测地执行代码。让我们来看一种将 camelCased 字符串转换成 Title Case 的方法。

```
import { safeAfter, safeLift, isArray, isString, map, compose, option } from "crocks"

// match :: Regex -> String -> Maybe [String]
const match = regex => safeAfter(isArray, str => str.match(regex))

// join :: String -> [String] -> String
const join = separator => array => array.join(separator)

// upperFirst :: String -> String
const upperFirst = x =>
  x.charAt(0)
    .toUpperCase()
    .concat(x.slice(1).toLowerCase())

// uncamelize :: String -> Maybe String
const uncamelize = safeLift(isString, compose(
  option(""),
  map(compose(join(" "), map(upperFirst))),
  match(/(((^[a-z]|[A-Z])[a-z]*)|[0-9]+)/g),
))

uncamelize("rockTheCamel")
//=> Just "Rock The Camel"

uncamelize({})
//=> Nothing
```

我们已经创建了一个助手函数`match`，它使用`safeAfter`来消除`String.prototype.match`在没有匹配时返回`undefined`的行为。`isArray`谓词确保如果没有找到匹配，我们会收到一个`Nothing`，如果匹配，我们会收到一个`Just [String]`。`safeAfter`非常适合以可靠安全的方式执行现有或第三方功能。

(提示:`safeAfter`与返回`a | undefined`的`ramda`函数配合得非常好。)

我们的`uncamelize ?`函数是用`safeLift(isString)`执行的，这意味着只有当输入为`isString`谓词返回 true 时，它才会执行。

除此之外，crocks 还提供了`prop`和`propPath`助手，允许你从`Object`和`Array`中选择属性

```
import { prop, propPath, map, compose } from "crocks"

const goodObject = {
  name: "Bob",
  bankBalance: 7999,
  address: {
    city: "Auckland",
    country: "New Zealand",
  },
}

prop("name")(goodObject)
//=> Just "Bob"
propPath(["address", "city"])(goodObject)
//=> Just "Auckland"

// getBankBalance :: Object -> Maybe String
const getBankBalance = compose(
  map(balance => balance.toFixed(2)),
  prop("bankBalance")
)

getBankBalance(goodObject)
//=> Just '7999.00'
getBankBalance({})
//=> Nothing
```

这很好，特别是如果我们处理的数据来自于不在我们控制之下的副作用，比如 API 响应。但是，如果 API 开发人员突然决定在他们的终端处理格式化，会发生什么呢？

```
const badObject = { 
  name: "Rambo",
  bankBalance: "100.00",
  address: {
    city: "Hope",
    country: "USA"
  }
}

getBankBalance(badObject) // TypeError: balance.toFixed is not a function :-(
```

运行时错误！我们试图在一个字符串上调用`toFixed`方法，这个方法实际上并不存在。在我们对它调用`toFixed`之前，我们需要确保`bankBalance`确实是一个`Number`。让我们试着和我们的`safe`助手一起解决这个问题。

```
import { prop, propPath, compose, map, chain, safe, isNumber } from "crocks"

// getBankBalance :: Object -> Maybe String
const getBankBalance = compose(
  map(balance => balance.toFixed(2)),
  chain(safe(isNumber)),
  prop("bankBalance")
)

getBankBalance(badObject) //=> Nothing
getBankBalance(goodObject) //=> Just '7999.00'
```

我们将`prop`函数的结果通过管道传递给我们的`safe(isNumber)`函数，后者也返回一个`Maybe`，这取决于`prop`的结果是否满足谓词。上面的管道保证只有当`bankBalance`是`Number`时，包含`toFixed`的最后一个`map`才会被调用。

如果您将处理大量类似的案例，提取此模式作为助手是有意义的:

```
import { Maybe, ifElse, prop, chain, curry, compose, isNumber } from "crocks"

const { of, zero } = Maybe

// propIf :: (a -> Boolean) -> [String | Number] -> Maybe a
const propIf = curry((fn, path) =>
  compose(
    chain(ifElse(fn, of, zero)),
    prop(path)
  )
)

propIf(isNumber, "age", goodObject) 
//=> Just 7999
propIf(isNumber, "age", badObject) 
//=> Nothing
```

#### 使用应用程序保持函数的整洁

很多时候，我们发现自己处于这样的情况:我们想要使用一个现有的函数，它的值被包装在一个容器中。让我们使用上一节的概念，尝试设计一个只允许数字的安全`add`函数。这是我们的第一次尝试。

```
import { Maybe, safe, isNumber } from "crocks"

// safeNumber :: a -> Maybe a
const safeNumber = safe(isNumber)

// add :: a -> b -> Maybe Number
const add = (a, b) => {
  const maybeA = safeNumber(a)
  const maybeB = safeNumber(b)

  return maybeA.chain(
    valA => maybeB.map(valB => valA + valB)
  )
}

add(1, 2)
//=> Just 3

add(1, {})
//=> Nothing
```

这正是我们所需要的，但是我们的`add`函数不再是简单的`a + b`。它必须首先将我们的值提升到`Maybe`中，然后进入其中访问这些值，然后返回结果。我们需要找到一种方法来保留我们的`add`函数的核心功能，同时允许它处理 ADT 中包含的值！这就是应用函子派上用场的地方。

应用函子就像常规函子一样，但是除了`map`，它还实现了两个额外的方法:

```
of :: Applicative f => a -> f a
```

`of`是一个完全愚蠢的构造函数，它将你给它的任何值提升到我们的数据类型中。在其他语言中也被称为`pure`。

```
Maybe.of(null)
//=> Just null

Const.of(42)
//=> Const 42
```

所有的钱都在这里，即`ap`方法:

```
ap :: Apply f => f a ~> f (a -> b) -> f b
```

签名看起来与`map`非常相似，唯一的区别是我们的`a -> b`函数也被包装在一个`f`中。让我们来看看实际情况。

```
import { Maybe, safe, isNumber } from "crocks"

// safeNumber :: a -> Maybe a
const safeNumber = safe(isNumber)

// add :: a -> b -> c
const add = a => b => a + b 

// add :: a -> b -> Maybe Number
const safeAdd = (a, b) => Maybe.of(add)
  .ap(safeNumber(a))
  .ap(safeNumber(b))

safeAdd(1, 2)
//=> Just 3

safeAdd(1, "danger")
//=> Nothing
```

我们首先将我们的 curried `add`函数提升为一个`Maybe`，然后对其应用`Maybe a`和`Maybe b`。到目前为止，我们一直使用`map`来访问容器内部的值，`ap`也不例外。在内部，`map`开启`safeNumber(a)`以访问`a`并将其应用于`add`。这导致`Maybe`包含部分应用的`add`。我们对`safeNumber(b)`重复相同的过程来执行我们的`add`函数，如果`a`和`b`都有效，则结果为`Just`，否则为`Nothing`。

Crocks 还为我们提供了`liftA2`和`liftN`助手，以一种无指针的方式表达相同的概念。下面是一个简单的例子:

```
liftA2(add)(Maybe(1))(Maybe(2))
//=> Just 3
```

我们将在`Expressing Parallelism`一节中广泛使用这个助手。

提示:既然我们已经观察到`ap`使用`map`来访问值，我们可以做一些很酷的事情，比如在给定两个列表的情况下生成一个笛卡尔乘积。

```
import { List, Maybe, Pair, liftA2 } from "crocks"

const names = List(["Henry", "George", "Bono"])
const hobbies = List(["Music", "Football"])

List(name => hobby => Pair(name, hobby))
  .ap(names)
  .ap(hobbies)
// => List [ Pair( "Henry", "Music" ), Pair( "Henry", "Football" ), 
// Pair( "George", "Music" ), Pair( "George", "Football" ), 
// Pair( "Bono", "Music" ), Pair( "Bono", "Football" ) ]
```

#### 使用异步进行可预测的错误处理

`crocks`提供了`Async`数据类型，允许我们构建惰性异步计算。要了解更多，你可以参考广泛的官方文件[这里](https://evilsoft.github.io/crocks/docs/crocks/Async.html?source=post_page---------------------------)。本节旨在提供一些例子，说明我们如何使用`Async`来提高错误报告的质量，并使我们的代码具有弹性。

通常，我们会遇到这样的情况，我们希望进行相互依赖的 API 调用。在这里,`getUser`端点从 GitHub 返回一个用户实体，响应包含许多用于存储库、明星、收藏夹等的嵌入式 URL。我们将看到如何使用`Async`来设计这个用例。

```
import { Async, prop, compose, chain,  safe, isString, maybeToAsync } from "crocks"

const { fromPromise } = Async

// userPromise :: String -> Promise User Error
const userPromise = user => fetch(`https://api.github.com/users/${user}`)
  .then(res => res.json())

// resourcePromise :: String -> Promise Resource Error
const resourcePromise = url => fetch(url)
  .then(res => res.json())

// getUser :: String -> Async User Error
const getUser = compose(
  chain(fromPromise(userPromise)),
  maybeToAsync('getUser expects a string'),
  safe(isString)
)

// getResource :: String -> Object -> Async Resource Error
const getResource = path => user => {
  if (!isString(path)) {
    return Async.Rejected("getResource expects a string")
  }
  return maybeToAsync("Error: Malformed user response received", prop(path, user))
    .chain(fromPromise(resourcePromise))
}

// logError :: (...a) -> IO()
const logError = (...args) => console.log("Error: ", ...args)

// logResponse :: (...a) -> IO()
const logSuccess = (...args) => console.log("Success: ", ...args)

getUser("octocat")
  .chain(getResource("repos_url"))
  .fork(logError, logSuccess)
//=> Success: { ...response }

getUser(null)
  .chain(getResource("repos_url"))
  .fork(logError, logSuccess)
//=> Error: The user must be as string

getUser("octocat")
  .chain(getResource(null))
  .fork(logError, logSuccess)
//=> Error: getResource expects a string

getUser("octocat")
  .chain(getResource("unknown_path_here"))
  .fork(logError, logSuccess)
//=> Error: Malformed user response received
```

使用`maybeToAsync`转换允许我们使用从使用`Maybe`中获得的所有安全特性，并将它们带到我们的`Async`流中。我们现在可以标记输入和其他错误，作为我们的`Async`流的一部分。

#### 有效利用幺半群

当我们在本地 JavaScript 中执行像`String` / `Array`连接和数字加法这样的操作时，我们已经在使用幺半群了。它只是一种为我们提供以下方法的数据类型。

```
concat :: Monoid m => m a -> m a -> m a
```

`concat`允许我们通过预先指定的操作将两个相同类型的幺半群组合在一起。

```
empty :: Monoid m => () => m a
```

`empty`方法为我们提供了一个 identity 元素，当`concat`与同类型的其他幺半群进行运算时，将返回相同的元素。这就是我要说的。

```
import { Sum } from "crocks"

Sum.empty()
//=> Sum 0

Sum(10)
  .concat(Sum.empty())
//=> Sum 10

Sum(10)
  .concat(Sum(32))
//=> Sum 42
```

这本身看起来没什么用，但是`crocks`提供了一些额外的幺半群以及助手`mconcat`、`mreduce`、`mconcatMap`和`mreduceMap`。

```
import { Sum, mconcat, mreduce, mconcatMap, mreduceMap } from "crocks"

const array = [1, 3, 5, 7, 9]

const inc = x => x + 1

mconcat(Sum, array)
//=> Sum 25

mreduce(Sum, array)
//=> 25

mconcatMap(Sum, inc, array)
//=> Sum 30

mreduceMap(Sum, inc, array)
//=> 30
```

`mconcat`和`mreduce`方法接受一个幺半群和一个元素列表，并将`concat`应用于它们的所有元素。它们之间唯一的区别是`mconcat`返回幺半群的一个实例，而`mreduce`返回原始值。`mconcatMap`和`mreduceMap`助手的工作方式相同，除了它们在调用`concat`之前接受一个用于映射每个元素的额外函数。

让我们看另一个来自`crocks`的幺半群的例子，`First`幺半群。串联时，`First`将总是返回第一个非空值。

```
import { First, Maybe } from "crocks"

First(Maybe.zero())
  .concat(First(Maybe.zero()))
  .concat(First(Maybe.of(5)))
//=> First (Just 5)

First(Maybe.of(5))
  .concat(First(Maybe.zero()))
  .concat(First(Maybe.of(10)))
//=> First (Just 5)
```

使用`First`的力量，让我们试着创建一个函数，尝试获取对象的第一个可用属性。

```
import { curry, First, mreduceMap, flip, prop, compose } from "crocks"

/** tryProps -> a -> [String] -> Object -> b */
const tryProps = flip(object => 
  mreduceMap(
    First, 
    flip(prop, object),
  )
)

const a = {
  x: 5,
  z: 10,
  m: 15,
  g: 12
}

tryProps(["a", "y", "b", "g"], a)
//=> Just 12

tryProps(["a", "b", "c"], a)
//=> Nothing

tryProps(["a", "z", "c"], a)
//=> Just 10
```

相当整洁！下面是另一个例子，当提供不同类型的值时，它试图创建一个尽力而为的格式化程序。

```
 import { 
  applyTo, mreduceMap, isString, isEmpty, mreduce, First, not, isNumber, chain
  compose, safe, and, constant, Maybe, map, equals, ifElse, isBoolean, option,
} from "crocks";

// isDate :: a -> Boolean
const isDate = x => x instanceof Date;

// lte :: Number -> Number -> Boolean
const lte = x => y => y <= x;

// formatBoolean :: a -> Maybe String
const formatBoolean = compose(
  map(ifElse(equals(true), constant("Yes"), constant("No"))),
  safe(isBoolean)
);

// formatNumber :: a -> Maybe String
const formatNumber = compose(
  map(n => n.toFixed(2)),
  safe(isNumber)
);

// formatPercentage :: a -> Maybe String
const formatPercentage = compose(
  map(n => n + "%"),
  safe(and(isNumber, lte(100)))
);

// formatDate :: a -> Maybe String
const formatDate = compose(
  map(d => d.toISOString().slice(0, 10)),
  safe(isDate)
);

// formatString :: a -> Maybe String
const formatString = safe(isString)

// autoFormat :: a -> Maybe String
const autoFormat = value =>
  mreduceMap(First, applyTo(value), [
    formatBoolean,
    formatPercentage,
    formatNumber,
    formatDate,
    formatString
  ]);

autoFormat(true)
//=> Just "Yes"

autoFormat(10.02)
//=> Just "10%"

autoFormat(255)
//=> Just "255.00"

autoFormat(new Date())
//=> Just "2019-01-14"

autoFormat("YOLO!")
//=> Just "YOLO!"

autoFormat(null)
//=> Nothing
```

#### 以无点方式表达并行性

我们可能会遇到这样的情况:希望对单个数据执行多个操作，并以某种方式组合结果。`crocks`为我们提供了两种方法来实现这一点。第一种模式利用产品类型`Pair`和`Tuple`。让我们看一个小例子，我们有一个看起来像这样的对象:

```
{ ids: [11233, 12351, 16312], rejections: [11233] }
```

我们想写一个函数，接受这个对象并返回一个`ids`的`Array`，排除被拒绝的对象。我们在原生 JavaScript 中的第一次尝试如下所示:

```
const getIds = (object) => object.ids.filter(x => object.rejections.includes(x))
```

这当然是可行的，但是如果其中一个属性格式错误或者没有定义，它就会爆炸。让我们让`getIds`返回一个`Maybe`来代替。我们使用接受两个函数的`fanout`助手，在相同的输入上运行它并返回结果的`Pair`。

```
import { prop, compose, equals, filter, fanout, merge, liftA2 } from "crocks"

/**
 * object :: Record
 * Record :: {
 *  ids: [Number]
 *  rejection: [Number]
 * }
 **/
const object = { ids: [11233, 12351, 16312], rejections: [11233] }

// excludes :: [a] -> [b] -> Boolean
const excludes = x => y => !x.includes(y)

// difference :: [a] -> [a] -> [a]
const difference = compose(filter, excludes)

// getIds :: Record -> Maybe [Number]
const getIds = compose(
  merge(liftA2(difference)),
  fanout(prop("rejections"), prop("ids"))
)

getIds(object)
//=> Just [ 12351, 16312 ]

getIds({ something: [], else: 5 })
//=> Nothing
```

使用无点方法的主要好处之一是，它鼓励我们将逻辑分成更小的部分。我们现在有了可重用的助手`difference`(和前面看到的`liftA2`)，我们可以用它来`merge`两个一起平分`Pair`。

第二种方法是使用`converge`组合器来获得类似的结果。`converge`接受三个函数和一个输入值。然后，它将输入应用于第二个和第三个函数，并将两个函数的结果通过管道传递给第一个函数。让我们用它来创建一个函数，根据对象的`id`规范化对象的`Array`，我们将使用允许我们将对象组合在一起的`Assign`幺半群。

```
import {
  mreduceMap, applyTo, option, identity, objOf, map,
  converge, compose, Assign, isString, constant
} from "crocks"
import propIf from "./propIf"

// normalize :: String -> [Object] -> Object
const normalize = mreduceMap(
  Assign,
  converge(
    applyTo,
    identity,
    compose(
      option(constant({})),
      map(objOf),
      propIf(isString, "id")
    )
  )
)

normalize([{ id: "1", name: "Kerninghan" }, { id: "2", name: "Stallman" }])
//=> { 1: { id: '1', name: 'Kerninghan' }, 2: { id: '2', name: 'Stallman' } }

normalize([{ id: null}, { id: "1", name: "Knuth" }, { totally: "unexpected" }])
//=> { 1: { id: '1', name: 'Knuth' } }
```

#### 使用遍历和序列来确保数据的完整性

我们已经看到了如何使用`Maybe`和朋友来确保我们总是与我们期望的类型一起工作。但是当我们处理一个包含其他值的类型时，比如一个`Array`或者一个`List`，会发生什么呢？让我们看一个简单的函数，它给出了包含在一个`Array`中的所有字符串的总长度。

```
import { compose, safe, isArray, reduce, map } from "crocks"

// sum :: [Number] -> Number
const sum = reduce((a, b) => a + b, 0)

// length :: [a] -> Number
const length = x => x.length;

// totalLength :: [String] -> Maybe Number 
const totalLength = compose(
  map(sum),
  map(map(length)),
  safe(isArray)
)

const goodInput = ["is", "this", "the", "real", "life?"]
totalLength(goodInput)
//=> Just 18

const badInput = { message: "muhuhahhahahaha!"}
totalLength(badInput)
//=> Nothing
```

太好了。我们已经确保，如果我们的函数没有收到`Array`，它总是返回一个`Nothing`。但是，这就足够了吗？

```
totalLength(["stairway", "to", undefined])
//=> TypeError: x is undefined
```

不完全是。我们的函数不能保证列表的内容不会有任何惊喜。解决这个问题的方法之一是定义一个只处理字符串的`safeLength`函数:

```
// safeLength :: a -> Maybe Number 
const safeLength = safeLift(isString, length)
```

如果我们使用`safeLength`而不是`length`作为我们的映射函数，我们将收到一个`[Maybe Number]`而不是一个`[Number]`，并且我们不能再使用我们的`sum`函数了。这就是`sequence`派上用场的地方。

```
import { sequence, Maybe, Identity } from "crocks"

sequence(Maybe, Identity(Maybe.of(1)))
//=> Just Identity 1

sequence(Array, Identity([1,2,3]))
//=> [ Identity 1, Identity 2, Identity 3 ]

sequence(Maybe, [Maybe.of(4), Maybe.of(2)])
//=> Just [ 4, 2 ]

sequence(Maybe, [Maybe.of(4), Maybe.zero()])
//=> Nothing
```

`sequence`在执行某个`effect`时，帮助用外部类型交换内部类型，假设内部类型是应用型的。`Identity`上的`sequence` 非常愚蠢——它只是`map`覆盖了内部类型，并返回包装在`Identity`容器中的内容。对于`List`和`Array`，`sequence`使用列表上的`reduce`通过`ap`和`concat`组合其内容。让我们在重构的`totalLength`实现中看看这一点。

```
// totalLength :: [String] -> Maybe Number 
const totalLength = compose(
  map(sum),
  chain(sequence(Maybe)),
  map(map(safeLength)),
  safe(isArray)
)

const goodString = ["is", "this", "the", "real", "life?"]
totalLength(goodString)
//=> Just 18

totalLength(["stairway", "to", undefined])
//=> Nothing
```

太好了！我们建造了一个完全防弹的。这种映射来自`a -> m b`的东西然后使用`sequence`的模式非常普遍，以至于我们有了另一个叫做`traverse`的助手来一起执行这两种操作。让我们看看如何在上面的例子中使用`traverse`来代替序列。

```
// totalLengthT :: [String] -> Maybe Number 
const totalLengthT = compose(
  map(sum),
  chain(traverse(Maybe, safeLength)),
  safe(isArray)
)
```

那里！它的工作方式完全一样。想想看，我们的`sequence`算子基本上就是`traverse`，用一个`identity`作为映射函数。

注意:由于我们不能使用 JavaScript 推断内部类型，我们必须显式地提供类型构造函数作为第一个参数给`traverse`和`sequence`。

很容易看出`sequence`和`traverse`对于验证数据的价值。让我们尝试创建一个通用的验证器，它接受一个模式并验证一个输入对象。我们将使用`Result`类型，它接受允许我们收集错误的左边的半群。半群类似于幺半群，它定义了一个`concat`方法——但与幺半群不同，它不需要`empty`方法的存在。我们还引入了下面的转换函数`maybeToResult`，这将帮助我们在`Maybe`和`Result`之间进行互操作。

```
 import {
  Result, isString, map, merge, constant, bimap, flip, propOr, identity, 
  toPairs, safe, maybeToResult, traverse, and, isNumber, compose
} from "crocks"

// length :: [a] -> Int
const length = x => x.length

// gte :: Number -> a -> Result String a
const gte = x => y => y >= x

// lte :: Number -> a -> Result String a
const lte = x => y => y <= x

// isValidName :: a -> Result String a
const isValidName = compose(
  maybeToResult("expected a string less than 20 characters"),
  safe(and(compose(lte(20), length), isString))
)

// isAdult :: a -> Result String a
const isAdult = compose(
  maybeToResult("expected a value greater than 18"),
  safe(and(isNumber, gte(18)))
)

/**
 *  schema :: Schema
 *  Schema :: {
 *    [string]: a -> Result String a
 *  }
 * */
const schema = {
  name: isValidName,
  age: isAdult,
}

// makeValidator :: Schema -> Object -> Result [String] Object
const makeValidator = flip(object =>
  compose(
    map(constant(object)),
    traverse(Result, merge((key, validator) =>
        compose(
          bimap(error => [`${key}: ${error}`], identity),
          validator,
          propOr(undefined, key)
        )(object)
      )
    ),
    toPairs
  )
)

// validate :: Object -> Result [String] Object
const validate = makeValidator(schema)

validate(({
  name: "Car",
  age: 21,
}))
//=> Ok { name: "Car", age: 21 }

validate(({
  name: 7,
  age: "Old",
}))
//=>  Err [ "name: expected a string less than 20 characters", "age: expected a value greater than 18" ]
```

因为我们翻转了`makeValidator`函数，使其更适合于 curry，所以我们的`compose`链接收我们需要首先验证的模式。我们首先将模式分解成键值`Pair`，并将每个属性的值传递给相应的验证函数。万一函数失败，我们使用`bimap`来映射错误，添加更多的信息，并作为单例`Array`返回。然后`traverse`将`concat`所有存在的错误，或者返回有效的原始对象。我们也可以返回一个`String`而不是一个`Array`，但是一个`Array`感觉更好。

*感谢[伊恩·霍夫曼-希克斯、](https://www.freecodecamp.org/news/functional-programming-patterns-cookbook-3a0dfe2d7e0a/undefined)、西尼萨·卢克和[戴尔·弗朗西斯](https://github.com/dalefrancis88)对这篇文章的投入。*