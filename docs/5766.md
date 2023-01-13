# Javascript 中的函数式编程原则

> 原文：<https://www.freecodecamp.org/news/functional-programming-principles-in-javascript-1b8fc6c3563f/>

在长时间学习和使用面向对象编程之后，我退一步思考系统复杂性。

> `“Complexity is anything that makes software hard to understand or to modify.`——约翰·奥特罗德

在做一些研究时，我发现了像不变性和纯函数这样的函数式编程概念。这些概念使您能够构建无副作用的功能，因此更容易维护系统——还有一些其他的[好处](https://hackernoon.com/why-functional-programming-matters-c647f56a7691)。

在这篇文章中，我将告诉你更多关于函数式编程的知识，以及一些重要的概念，并附有大量的 JavaScript 代码示例。

### 什么是函数式编程？

> 函数式编程是一种编程范式——一种构建计算机程序的结构和元素的风格——将计算视为数学函数的评估，并避免改变状态和可变数据— [维基百科](https://en.wikipedia.org/wiki/Functional_programming)

#### 纯函数

![0*FMur6URY7yAVjeuP](img/d51f30593314192a197f5e39814604fd.png)

“water drop” by [Mohan Murugesan](https://unsplash.com/@martinmohan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当我们想要理解函数式编程时，我们学习的第一个基本概念是**纯函数**。但是这到底意味着什么呢？是什么使得一个函数是纯的？

那么我们如何知道一个函数是否是`pure`？这里有一个非常严格的纯度定义:

*   如果给定相同的参数，它将返回相同的结果(也称为`deterministic`)
*   它不会引起任何明显的副作用

#### 如果给定相同的参数，它将返回相同的结果

假设我们想要实现一个计算圆的面积的函数。一个不纯的函数将接收`radius`作为参数，然后计算`radius * radius * PI`:

```
let PI = 3.14;

const calculateArea = (radius) => radius * radius * PI;

calculateArea(10); // returns 314.0
```

为什么这是一个不纯函数？因为它使用了一个没有作为参数传递给函数的全局对象。

现在想象一些数学家争论说`PI`的值实际上是`[42](https://en.wikipedia.org/wiki/Phrases_from_The_Hitchhiker%27s_Guide_to_the_Galaxy#Answer_to_the_Ultimate_Question_of_Life,_the_Universe,_and_Everything_(42))`并且改变了全局对象的值。

我们的不纯函数现在将导致`10 * 10 * 42` = `4200`。对于同一个参数(`radius = 10`，我们有不同的结果。

让我们修理它！

```
let PI = 3.14;

const calculateArea = (radius, pi) => radius * radius * pi;

calculateArea(10, PI); // returns 314.0
```

现在我们总是将`PI`的值作为参数传递给函数。所以现在我们只是访问传递给函数的参数。没有`external object`。

*   对于参数`radius = 10`和`PI = 3.14`，我们将总是得到相同的结果:`314.0`
*   对于参数`radius = 10`和`PI = 42`，我们将总是得到相同的结果:`4200`

#### 读取文件

如果我们的函数读取外部文件，它就不是一个纯粹的函数——文件的内容可以改变。

```
const charactersCounter = (text) => `Character count: ${text.length}`;

function analyzeFile(filename) {
  let fileContent = open(filename);
  return charactersCounter(fileContent);
}
```

#### 随机数生成

任何依赖于随机数生成器的函数都不可能是纯函数。

```
function yearEndEvaluation() {
  if (Math.random() > 0.5) {
    return "You get a raise!";
  } else {
    return "Better luck next year!";
  }
}
```

#### 它不会引起任何明显的副作用

可观察到的副作用的例子包括修改全局对象或通过引用传递的参数。

现在我们想实现一个函数来接收一个整数值并返回增加了 1 的值。

```
let counter = 1;

function increaseCounter(value) {
  counter = value + 1;
}

increaseCounter(counter);
console.log(counter); // 2
```

我们有了`counter`值。我们的非纯函数接收该值，并将该值加 1 重新分配给计数器。

```
let counter = 1;

const increaseCounter = (value) => value + 1;

increaseCounter(counter); // 2
console.log(counter); // 1
```

**观察**:函数式编程不鼓励可变性。

我们正在修改全局对象。但是我们怎么才能做到呢？只需返回增加 1 的值。

看到我们的纯函数`increaseCounter`返回 2，但是`counter`值还是一样的。该函数返回增加的值，而不改变变量的值。

如果我们遵循这两条简单的规则，理解我们的程序就变得更容易了。现在，每个功能都是孤立的，无法影响我们系统的其他部分。

纯函数是稳定的、一致的和可预测的。给定相同的参数，纯函数将总是返回相同的结果。我们不需要考虑相同参数有不同结果的情况，因为这永远不会发生。

#### 纯功能优势

代码肯定更容易测试。我们不需要嘲笑任何东西。所以我们可以在不同的环境下对纯函数进行单元测试:

*   给定参数`A` →期望函数返回值`B`
*   给定参数`C` →期望函数返回值`D`

一个简单的例子是一个接收数字集合的函数，并期望它递增这个集合的每个元素。

```
let list = [1, 2, 3, 4, 5];

const incrementNumbers = (list) => list.map(number => number + 1);
```

我们接收`numbers`数组，使用`map`递增每个数字，并返回一个新的递增数字列表。

```
incrementNumbers(list); // [2, 3, 4, 5, 6]
```

对于`input` `[1, 2, 3, 4, 5]`，预期的`output`将是`[2, 3, 4, 5, 6]`。

#### 不变

> 不随时间变化或者不能被改变。

![0*MGlzHgISuw0dXwsf](img/985158161d0ef04bc9819edf76221e9d.png)

“Change neon light signage” by [Ross Findon](https://unsplash.com/@rossf?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当数据不可变时，其状态在创建后不能改变。如果你想改变一个不可变的对象，你不能。相反，你用新值创建一个新对象。

在 JavaScript 中，我们通常使用`for`循环。下一个`for`语句有一些可变变量。

```
var values = [1, 2, 3, 4, 5];
var sumOfValues = 0;

for (var i = 0; i < values.length; i++) {
  sumOfValues += values[i];
}

sumOfValues // 15
```

对于每次迭代，我们都在改变`i`和`sumOfValue`状态。但是我们如何处理迭代中的可变性呢？递归。

```
 let list = [1, 2, 3, 4, 5];
let accumulator = 0;

function sum(list, accumulator) {
  if (list.length == 0) {
    return accumulator;
  }

  return sum(list.slice(1), accumulator + list[0]);
}

sum(list, accumulator); // 15
list; // [1, 2, 3, 4, 5]
accumulator; // 0
```

所以这里我们有接收数值向量的`sum`函数。该函数调用自己，直到我们得到空列表([我们的递归`base case`](https://en.wikipedia.org/wiki/Recursion_(computer_science)#Recursive_functions_and_algorithms) )。对于每一次“迭代”,我们将把值添加到`total`累加器中。

通过递归，我们保持我们的变量不变。`list`和`accumulator`变量不变。它保持相同的值。

**观察**:我们可以用`reduce`来实现这个功能。我们将在高阶函数主题中讨论这一点。

构建对象的最终状态也很常见。假设我们有一个字符串，我们想把这个字符串转换成一个`url slug`。

在 Ruby 的面向对象编程中，我们会创建一个类，比如说，`UrlSlugify`。这个类将有一个`slugify`方法将字符串输入转换成一个`url slug`。

```
class UrlSlugify
  attr_reader :text

  def initialize(text)
    @text = text
  end

  def slugify!
    text.downcase!
    text.strip!
    text.gsub!(' ', '-')
  end
end

UrlSlugify.new(' I will be a url slug   ').slugify! # "i-will-be-a-url-slug"
```

已经实施了！

这里我们有命令式编程，确切地说明我们在每个`slugify`进程中想要做什么——首先是小写，然后删除无用的空格，最后用连字符替换剩余的空格。

但是我们在这个过程中改变了输入状态。

我们可以通过函数组合或函数链接来处理这种变异。换句话说，一个函数的结果将被用作下一个函数的输入，而不修改原始的输入字符串。

```
const string = " I will be a url slug   ";

const slugify = string =>
  string
    .toLowerCase()
    .trim()
    .split(" ")
    .join("-");

slugify(string); // i-will-be-a-url-slug
```

这里我们有:

*   `toLowerCase`:将字符串全部转换成小写
*   `trim`:删除字符串两端的空白
*   `split`和`join`:用给定字符串中的替换来替换 match 的所有实例

我们把这 4 个函数组合起来，就可以`"slugify"`我们的字符串。

#### 对透明性有关的

![0*K0VAbQjAwmKZb1at](img/94f19116081163147f3dd783fe787a1d.png)

“person holding eyeglasses” by [Josh Calabrese](https://unsplash.com/@joshcala?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

让我们实现一个`square function`:

```
const square = (n) => n * n;
```

给定相同的输入，这个纯函数将总是有相同的输出。

```
square(2); // 4
square(2); // 4
square(2); // 4
// ...
```

将`2`作为`square function`的参数传递将总是返回 4。所以现在我们可以用 4 代替`square(2)`。我们的功能是`referentially transparent`。

基本上，如果一个函数对于相同的输入始终产生相同的结果，那么它就是参照透明的。

纯函数+不可变数据=引用透明性

有了这个概念，我们可以做的一件很酷的事情就是[记忆](https://en.wikipedia.org/wiki/Memoization)这个函数。想象我们有这个函数:

```
const sum = (a, b) => a + b;
```

我们用这些参数来称呼它:

```
sum(3, sum(5, 8));
```

`sum(5, 8)`等于`13`。这个函数将总是导致`13`。所以我们可以这样做:

```
sum(3, 13);
```

而这个表达式总会产生`16`。我们可以用一个数字常数替换整个表达式，然后用[来记忆](https://en.wikipedia.org/wiki/Memoization)它。

#### 作为一级实体发挥作用

![0*K6m1Ftw54Wm6tfFB](img/9696911ee9931c7e6e2e7f3735cb2429.png)

“first-class” by [Andrew Neel](https://unsplash.com/@andrewtneel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

函数作为一级实体的想法是，函数也被视为值，并被用作数据。

作为一级实体的功能可以:

*   从常量和变量中引用它
*   将其作为参数传递给其他函数
*   从其他函数返回结果

其思想是将函数视为值，并像传递数据一样传递函数。通过这种方式，我们可以组合不同的功能来创建具有新行为的新功能。

假设我们有一个函数，它将两个值相加，然后将值加倍。大概是这样的:

```
const doubleSum = (a, b) => (a + b) * 2;
```

现在是一个减去数值并返回双精度值的函数:

```
const doubleSubtraction = (a, b) => (a - b) * 2;
```

这些函数具有相似的逻辑，但不同之处在于运算符函数。如果我们可以将函数视为值，并将它们作为参数传递，我们就可以构建一个接收操作符函数的函数，并在函数内部使用它。

```
const sum = (a, b) => a + b;
const subtraction = (a, b) => a - b;

const doubleOperator = (f, a, b) => f(a, b) * 2;

doubleOperator(sum, 3, 1); // 8
doubleOperator(subtraction, 3, 1); // 4
```

现在我们有了一个`f`参数，并用它来处理`a`和`b`。我们传递了`sum`和`subtraction`函数来与`doubleOperator`函数组合并创建一个新的行为。

#### 高阶函数

当我们谈到高阶函数时，我们指的是这样的函数:

*   接受一个或多个函数作为参数，或者
*   返回一个函数作为结果

我们上面实现的`doubleOperator`函数是一个高阶函数，因为它将一个操作符函数作为参数并使用它。

你可能已经听说过`filter`、`map`和`reduce`。让我们看看这些。

#### 过滤器

给定一个集合，我们希望按属性过滤。filter 函数需要一个`true`或`false`值来确定元素是否应该包含在结果集合中。基本上，如果回调表达式是`true`，过滤函数会将元素包含在结果集合中。否则，不会。

一个简单的例子是，当我们有一个整数集合，我们只想要偶数。

#### **命令式方法**

用 JavaScript 实现这一点的一个必要方法是:

*   创建一个空数组`evenNumbers`
*   迭代`numbers`数组
*   将偶数压入`evenNumbers`数组

```
var numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
var evenNumbers = [];

for (var i = 0; i < numbers.length; i++) {
  if (numbers[i] % 2 == 0) {
    evenNumbers.push(numbers[i]);
  }
}

console.log(evenNumbers); // (6) [0, 2, 4, 6, 8, 10]
```

我们还可以使用`filter`高阶函数来接收`even`函数，并返回一个偶数列表:

```
const even = n => n % 2 == 0;
const listOfNumbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
listOfNumbers.filter(even); // [0, 2, 4, 6, 8, 10]
```

我在[黑客等级 FP](https://www.hackerrank.com/domains/fp) 路径上解决的一个有趣的问题是[过滤器阵列问题](https://www.hackerrank.com/challenges/fp-filter-array/problem)。问题的想法是过滤一个给定的整数数组，只输出那些小于指定值`X`的值。

这个问题的一个强制性 JavaScript 解决方案是这样的:

```
var filterArray = function(x, coll) {
  var resultArray = [];

  for (var i = 0; i < coll.length; i++) {
    if (coll[i] < x) {
      resultArray.push(coll[i]);
    }
  }

  return resultArray;
}

console.log(filterArray(3, [10, 9, 8, 2, 7, 5, 1, 3, 0])); // (3) [2, 1, 0]
```

我们确切地说了我们的函数需要做什么——迭代集合，将集合当前项与`x`进行比较，如果它通过了条件，就将这个元素推送到`resultArray`。

#### **声明式方法**

但是我们想要一个更具声明性的方法来解决这个问题，并且使用`filter`高阶函数。

一个声明性的 JavaScript 解决方案应该是这样的:

```
function smaller(number) {
  return number < this;
}

function filterArray(x, listOfNumbers) {
  return listOfNumbers.filter(smaller, x);
}

let numbers = [10, 9, 8, 2, 7, 5, 1, 3, 0];

filterArray(3, numbers); // [2, 1, 0]
```

首先，在`smaller`函数中使用`this`似乎有点奇怪，但是很容易理解。

`this`将是`filter`函数中的第二个参数。在这种情况下，`3`(`x`)由`this`代表。就是这样。

我们也可以用地图来做这件事。假设我们有一张地图，上面有人们的`name`和`age`。

```
let people = [
  { name: "TK", age: 26 },
  { name: "Kaio", age: 10 },
  { name: "Kazumi", age: 30 }
];
```

我们希望只过滤超过指定年龄值的人，在本例中是 21 岁以上的人。

```
const olderThan21 = person => person.age > 21;
const overAge = people => people.filter(olderThan21);
overAge(people); // [{ name: 'TK', age: 26 }, { name: 'Kazumi', age: 30 }]
```

代码摘要:

*   我们有一份人员名单(有`name`和`age`)。
*   我们有一个函数`olderThan21`。在本例中，对于 people 数组中的每个人，我们想要访问`age`并查看它是否大于 21 岁。
*   我们根据这个函数过滤所有人。

#### 地图

映射的思想是转换一个集合。

> `map`方法通过对集合的所有元素应用一个函数并从返回值构建一个新的集合来转换集合。

让我们得到上面相同的`people`集合。我们现在不想按“超龄”来过滤。我们只是想要一个字符串列表，类似于`TK is 26 years old`。所以最后一个字符串可能是`:name is :age years old`，其中`:name`和`:age`是来自`people`集合中每个元素的属性。

按照强制的 JavaScript 方式，应该是:

```
var people = [
  { name: "TK", age: 26 },
  { name: "Kaio", age: 10 },
  { name: "Kazumi", age: 30 }
];

var peopleSentences = [];

for (var i = 0; i < people.length; i++) {
  var sentence = people[i].name + " is " + people[i].age + " years old";
  peopleSentences.push(sentence);
}

console.log(peopleSentences); // ['TK is 26 years old', 'Kaio is 10 years old', 'Kazumi is 30 years old'] 
```

以声明性的 JavaScript 方式，它应该是:

```
const makeSentence = (person) => `${person.name} is ${person.age} years old`;

const peopleSentences = (people) => people.map(makeSentence);

peopleSentences(people);
// ['TK is 26 years old', 'Kaio is 10 years old', 'Kazumi is 30 years old']
```

整个想法是把一个给定的数组转换成一个新的数组。

另一个有趣的黑客排名问题是[更新列表问题](https://www.hackerrank.com/challenges/fp-update-list/problem)。我们只想用给定数组的绝对值来更新它们的值。

比如输入`[1, 2, 3, -4, 5]`需要输出为`[1, 2, 3, 4, 5]`。`-4`的绝对值是`4`。

一个简单的解决方案是对每个集合值进行就地更新。

```
var values = [1, 2, 3, -4, 5];

for (var i = 0; i < values.length; i++) {
  values[i] = Math.abs(values[i]);
}

console.log(values); // [1, 2, 3, 4, 5]
```

我们使用`Math.abs`函数将值转换成绝对值，并进行就地更新。

这是**而不是**实现该解决方案的功能性方式。

首先，我们学习了不变性。我们知道不变性对于使我们的函数更加一致和可预测是多么重要。这个想法是建立一个包含所有绝对值的新集合。

第二，这里为什么不用`map`来“转换”所有数据？

我的第一个想法是测试`Math.abs`函数只处理一个值。

```
Math.abs(-1); // 1
Math.abs(1); // 1
Math.abs(-2); // 2
Math.abs(2); // 2
```

我们希望将每个值转换为正值(绝对值)。

现在我们知道了如何对一个值执行`absolute`操作，我们可以使用这个函数作为参数传递给`map`函数。你还记得一个`higher order function`可以接收一个函数作为参数并使用它吗？是的，地图可以做到！

```
let values = [1, 2, 3, -4, 5];

const updateListMap = (values) => values.map(Math.abs);

updateListMap(values); // [1, 2, 3, 4, 5]
```

哇哦。太美了！

#### 减少

reduce 的思想是接收一个函数和一个集合，并返回通过组合这些项而创建的值。

人们谈论的一个常见例子是获取订单的总金额。想象你在一个购物网站上。您已经将`Product 1`、`Product 2`、`Product 3`和`Product 4`添加到您的购物车(订单)中。现在我们要计算购物车的总金额。

在命令式方法中，我们将迭代订单列表，并将每个产品的金额加到总金额中。

```
var orders = [
  { productTitle: "Product 1", amount: 10 },
  { productTitle: "Product 2", amount: 30 },
  { productTitle: "Product 3", amount: 20 },
  { productTitle: "Product 4", amount: 60 }
];

var totalAmount = 0;

for (var i = 0; i < orders.length; i++) {
  totalAmount += orders[i].amount;
}

console.log(totalAmount); // 120
```

使用`reduce`，我们可以构建一个函数来处理`amount sum`，并将其作为参数传递给`reduce`函数。

```
let shoppingCart = [
  { productTitle: "Product 1", amount: 10 },
  { productTitle: "Product 2", amount: 30 },
  { productTitle: "Product 3", amount: 20 },
  { productTitle: "Product 4", amount: 60 }
];

const sumAmount = (currentTotalAmount, order) => currentTotalAmount + order.amount;

const getTotalAmount = (shoppingCart) => shoppingCart.reduce(sumAmount, 0);

getTotalAmount(shoppingCart); // 120
```

这里我们有`shoppingCart`，接收当前`currentTotalAmount`的函数`sumAmount`，以及`order`对象`sum`它们。

`getTotalAmount`功能用于使用`sumAmount`从`0`开始`reduce`T2。

另一种得到总量的方法是将`map`和`reduce`合成。我这么说是什么意思？我们可以使用`map`将`shoppingCart`转换成`amount`值的集合，然后只需将`reduce`函数与`sumAmount`函数一起使用。

```
const getAmount = (order) => order.amount;
const sumAmount = (acc, amount) => acc + amount;

function getTotalAmount(shoppingCart) {
  return shoppingCart
    .map(getAmount)
    .reduce(sumAmount, 0);
}

getTotalAmount(shoppingCart); // 120
```

`getAmount`接收产品对象并只返回`amount`值。所以我们这里有`[10, 30, 20, 60]`。然后`reduce`将所有项目相加。漂亮！

我们看了一下每个高阶函数是如何工作的。我想向你们展示一个例子，我们如何在一个简单的例子中组合所有三个函数。

谈到`shopping cart`，假设我们的订单中有以下产品列表:

```
let shoppingCart = [
  { productTitle: "Functional Programming", type: "books", amount: 10 },
  { productTitle: "Kindle", type: "eletronics", amount: 30 },
  { productTitle: "Shoes", type: "fashion", amount: 20 },
  { productTitle: "Clean Code", type: "books", amount: 60 }
]
```

我们需要购物车中所有书籍的总数。就这么简单。算法？

*   按图书类型过滤
*   使用 map 将购物车转换为金额的集合
*   通过用 reduce 将所有项目相加来组合它们

```
let shoppingCart = [
  { productTitle: "Functional Programming", type: "books", amount: 10 },
  { productTitle: "Kindle", type: "eletronics", amount: 30 },
  { productTitle: "Shoes", type: "fashion", amount: 20 },
  { productTitle: "Clean Code", type: "books", amount: 60 }
]

const byBooks = (order) => order.type == "books";
const getAmount = (order) => order.amount;
const sumAmount = (acc, amount) => acc + amount;

function getTotalAmount(shoppingCart) {
  return shoppingCart
    .filter(byBooks)
    .map(getAmount)
    .reduce(sumAmount, 0);
}

getTotalAmount(shoppingCart); // 70
```

搞定了。

#### 资源

我整理了一些我阅读和研究的资源。我正在分享一些我觉得非常有趣的照片。更多资源，请访问我的[函数式编程 Github 库](https://github.com/LeandroTk/learning-functional-programming)

*   [Wes Bos 的 EcmaScript 6 课程](https://ES6.io/friend/LEANDRO)
*   [一个月的 JavaScript](https://mbsy.co/lFtbC)
*   [Ruby 特定资源](https://github.com/LeandroTk/learning-functional-programming/tree/master/ruby)
*   [Javascript 特定资源](https://github.com/LeandroTk/learning-functional-programming/tree/master/javascript)
*   [Clojure 特定资源](https://github.com/LeandroTk/learning-functional-programming/tree/master/clojure)
*   [通过构建 App 学习 React](https://alterclass.io/?ref=5ec57f513c1321001703dcd2)

#### 介绍

*   [在 JS 中学习 FP](https://www.youtube.com/watch?v=e-5obm1G_FY)
*   [用 Python 介绍 do FP](https://codewords.recurse.com/issues/one/an-introduction-to-functional-programming)
*   [FP 概述](https://blog.codeship.com/overview-of-functional-programming)
*   [功能 JS 快速介绍](https://hackernoon.com/a-quick-introduction-to-functional-javascript-7e6fe520e7fa)
*   [什么是 FP？](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0)
*   [函数式编程行话](https://github.com/hemanth/functional-programming-jargon)

#### 纯函数

*   [什么是纯函数？](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)
*   [纯函数编程 1](https://www.fpcomplete.com/blog/2017/04/pure-functional-programming)
*   [纯函数编程 2](https://www.fpcomplete.com/blog/2017/05/pure-functional-programming-part-2)

#### 不可变数据

*   [用于函数式编程的不可变 DS](https://www.youtube.com/watch?v=Wo0qiGPSV-s)
*   [为什么共享可变状态是万恶之源](http://henrikeichenhardt.blogspot.com/2013/06/why-shared-mutable-state-is-root-of-all.html)

#### 高阶函数

*   [雄辩的 JS:高阶函数](https://eloquentjavascript.net/05_higher_order.html)
*   [趣味趣味功能滤镜](https://www.youtube.com/watch?v=BMUiFMZr7vk&t=0s&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84&index=2&ab_channel=FunFunFunction)
*   [好玩好玩的功能图](https://www.youtube.com/watch?v=bCqtb-Z5YGQ&index=2&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84&ab_channel=FunFunFunction)
*   [趣味趣味功能基本减少](https://www.youtube.com/watch?v=Wl98eZpkp-c&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84&index=3&frags=wn&ab_channel=FunFunFunction)
*   [趣味趣味功能高级减少](https://www.youtube.com/watch?v=1DMolJ2FrNY&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84&index=4&ab_channel=FunFunFunction)
*   [Clojure 高阶函数](https://clojure.org/guides/higher_order_functions)
*   [纯函数滤波器](https://purelyfunctional.tv/lesson/filter/)
*   [纯功能图](https://purelyfunctional.tv/lesson/map/)
*   [纯功能还原](https://purelyfunctional.tv/lesson/reduce/)

#### 声明式编程

*   [声明式编程 vs 命令式编程](https://tylermcginnis.com/imperative-vs-declarative-programming/)

#### 就是这样！

嘿，朋友们，我希望你们在阅读这篇文章的时候有乐趣，并且我希望你们在这里学到了很多！这是我尝试分享我所学到的东西。

这里是本文中所有代码的存储库。

来跟我学吧。我在这个[学习函数式编程库](https://github.com/LeandroTk/learning-functional-programming)中共享资源和我的代码。

我也写了一篇 [FP 帖子，但是主要使用 Clojure](https://medium.freecodecamp.org/an-introduction-to-the-basic-principles-of-functional-programming-a2c2a15c84)

我希望你在这里看到了对你有用的东西。下次再见！:)

我的[Twitter](https://twitter.com/LeandroTk_)&[Github](https://github.com/LeandroTk)。

TK。