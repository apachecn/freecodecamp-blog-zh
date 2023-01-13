# 权威打字手册——初学者学习打字

> 原文：<https://www.freecodecamp.org/news/the-definitive-typescript-handbook/>

根据对 90，000 名开发者的栈溢出调查，TypeScript 是人们最想学习的工具之一。

在过去的几年里，TypeScript 在受欢迎程度、社区规模和采用率方面都有了爆炸性的增长。今天，甚至脸书在脸书的 Jest 项目也转向了打字稿。

# 什么是 TypeScript？

TypeScript 是 javascript 的静态类型超集，旨在简化大型 JavaScript 应用程序的开发。它也被称为 **JavaScript，缩放** 。

## 为什么要使用 TypeScript？

JavaScript 在过去的几年里有了很大的发展。它是用于客户端和服务器端的最通用的跨平台语言。

但是 JavaScript 从来不是为这种大规模的应用程序开发而设计的。它是一种没有类型系统的动态语言，这意味着变量可以有任何类型的值，比如字符串或布尔值。

类型系统提高了代码质量和可读性，并使维护和重构代码库变得更加容易。更重要的是，错误可以在编译时捕获，而不是在运行时捕获。

如果没有类型系统，很难扩展 JavaScript 来构建复杂的应用程序，让大型团队处理相同的代码。

TypeScript 在编译时为代码的不同部分提供了保证。编译器错误通常会告诉您到底哪里出了问题，到底出了什么问题，而运行时错误则伴随着堆栈跟踪，这可能会产生误导，并导致在调试工作上花费大量时间。

## **打字稿优点**

1.  在开发周期的早期捕捉潜在的错误。
2.  管理大型代码库。
3.  更简单的重构。
4.  让团队工作更容易——当代码中的契约更强时，不同的开发人员更容易进出代码库，而不会无意中破坏代码。
5.  文档——类型提供了某种类型的文档，您自己和其他开发人员都可以遵循。

## 打字稿缺点

1.  这是需要额外学习的东西— **这是短期减速和长期提高效率和维护之间的权衡。**
2.  类型错误可能是不一致的。
3.  配置极大地改变了它的行为。

# 类型

## **布尔型**

```
const isLoading: boolean = false;
```

## **号**

```
const decimal: number = 8;
const binary: number = 0b110;
```

## **字符串**

```
const fruit: string = "orange";
```

## **数组**

数组类型可以用以下两种方式之一编写:

```
// Most common
let firstFivePrimes: number[] = [2, 3, 5, 7, 11];
// Less common. Uses generic types (more on that later)
let firstFivePrimes2: Array<number> = [2, 3, 5, 7, 11];
```

## **元组**

元组类型允许您表达一个有组织的数组，其中固定数量的元素的类型是已知的。这意味着你会得到一个错误

```
let contact: [string, number] = ['John', 954683];
contact = ['Ana', 842903, 'extra argument']  /* Error! 
Type '[string, number, string]' is not assignable to type '[string, number]'. */
```

## **任何**

`any`兼容类型系统中的任何和所有类型，意味着任何东西都可以赋给它，它也可以赋给任何东西。它给了你选择退出类型检查的权力。

```
let variable: any = 'a string';
variable = 5;
variable = false;
variable.someRandomMethod(); /* Okay, 
someRandomMethod might exist at runtime. */
```

## **作废**

根本没有任何类型。它通常用作不返回值的函数的返回类型。

```
function sayMyName(name: string): void {
  console.log(name);
}
sayMyName('Heisenberg');
```

## **从不**

`never`类型表示从不出现的值的类型。例如，`never`是一个函数的返回类型，它总是抛出一个异常或者没有到达它的终点。

```
// throws an exception
function error(message: string): never {
  throw new Error(message);
}

// unreachable end point
function continuousProcess(): never {
  while (true) {
      // ...
  }
}
```

## **空**和**未定义**

`undefined`和`null`实际上都有自己的类型，分别命名为`undefined`和`null`。与`void`非常相似，它们本身并不是非常有用，但是当在联合类型 **(稍后会详细介绍)** 中使用时，它们变得非常有用

```
type someProp = string | null | undefined;
```

## **未知**

TypeScript 3.0 引入了未知类型，它是`any`的类型安全对应类型。任何东西都可以赋给`unknown`，但是`unknown`除了自身之外不能赋给任何东西，并且`any.`在没有首先声明或缩小到更具体的类型之前，不允许对`unknown`进行任何操作。

```
type I1 = unknown & null;    // null
type I2 = unknown & string;  // string
type U1 = unknown | null;    // unknown
type U2 = unknown | string;  // unknown
```

## **类型别名**

类型别名为类型注释提供了名称，允许您在多个地方使用它。它们是使用以下语法创建的:

```
type Login = string;
```

## **联合类型**

TypeScript 允许我们对一个属性使用多种数据类型。这叫做联合型。

```
type Password = string | number;
```

## **路口类型**

交集类型是组合了所有成员类型属性的类型。

```
interface Person {
  name: string;
  age: number;
}

interface Worker {
  companyId: string;
}

type Employee = Person & Worker;

const bestOfTheMonth: Employee = {
  name: 'Peter'
  age: 39,
  companyId: '123456' 
```

# 连接

接口就像是你和编译器之间的一个契约，在这个契约中，你在一个单独的命名注释中准确地指定了它各自的类型注释所期望的属性。
**旁注:接口没有运行时 JS 影响，它只用于类型检查** ing。

*   你可以声明 ****可选**** ****属性**** 标记那些带有`?`的属性，这意味着接口的对象可以定义也可以不定义这些属性。
*   您可以将 ****声明为只读**** ****属性**** ，这意味着属性一旦被赋值，就不能更改。

```
interface ICircle {
  readonly id: string;
  center: {
    x: number;
    y: number;
  },
  radius: number;
  color?: string;  // Optional property
}

const circle1: ICircle = {
  id: '001',
  center: { x: 0 },
  radius: 8,
};  /* Error! Property 'y' is missing in type '{ x: number; }' 
but required in type '{ x: number; y: number; }'. */

const circle2: ICircle = {
  id: '002',
  center: { x: 0, y: 0 },
  radius: 8,
}  // Okay

circle2.color = '#666';  // Okay
circle2.id = '003';  /* Error! 
Cannot assign to 'id' because it is a read-only property. */
```

## **扩展接口**

接口可以扩展一个或多个接口。这使得编写界面变得灵活和可重用。

```
interface ICircleWithArea extends ICircle {
  getArea: () => number;
}

const circle3: ICircleWithArea = {
  id: '003',
  center: { x: 0, y: 0 },
  radius: 6,
  color: '#fff',
  getArea: function () {
    return (this.radius ** 2) * Math.PI;
  },
};
```

## 实现接口

实现接口的类需要严格符合接口的结构。

```
interface IClock {
  currentTime: Date;
  setTime(d: Date): void;
}

class Clock implements IClock {
  currentTime: Date = new Date();
  setTime(d: Date) {
    this.currentTime = d;
  }
  constructor(h: number, m: number) { }
}
```

# **枚举**

`enum`(或枚举)是一种组织相关值集合的方法，这些值可以是数字或字符串值。

```
enum CardSuit {
  Clubs,
  Diamonds,
  Hearts,
  Spades
}

let card = CardSuit.Clubs;

card = "not a card suit"; /* Error! Type '"not a card suit"' 
is not assignable to type 'CardSuit'. */
```

默认情况下，枚举是基于数字的。`enum`值从零开始，每个成员递增 1。

我们前面的例子生成的 JavaScript 代码:

```
var CardSuit;
(function (CardSuit) {
  CardSuit[CardSuit["Clubs"] = 0] = "Clubs";
  CardSuit[CardSuit["Diamonds"] = 1] = "Diamonds";
  CardSuit[CardSuit["Hearts"] = 2] = "Hearts";
  CardSuit[CardSuit["Spades"] = 3] = "Spades";
})(CardSuit || (CardSuit = {}));

/**
 * Which results in the following object:
 * {
 *   0: "Clubs",
 *   1: "Diamonds",
 *   2: "Hearts",
 *   3: "Spades",
 *   Clubs: 0,
 *   Diamonds: 1,
 *   Hearts: 2,
 *   Spades: 3
 * }
 */
```

或者，枚举可以用字符串值初始化，这是一种可读性更强的方法。

```
enum SocialMedia {
  Facebook = 'FACEBOOK',
  Twitter = 'TWITTER',
  Instagram = 'INSTAGRAM',
  LinkedIn = 'LINKEDIN'
}
```

## 反向映射

`enum`支持反向映射，这意味着我们可以访问成员的值，也可以从值中访问成员名。
回到我们的 CardSuit 示例:

```
const clubsAsNumber: number = CardSuit.Clubs; // 3
const clubsAsString: string = CardSuit[0];    // 'Clubs'
```

# **功能**

您可以向每个参数添加类型，然后向函数本身添加返回类型。

```
function add(x: number, y: number): number {
  return x + y;
}
```

## 功能覆盖

TypeScript 允许你声明 **函数重载** 。基本上，可以有多个同名但参数类型和返回类型不同的函数。考虑下面的例子:

```
function padding(a: number, b?: number, c?: number, d?: any) {
  if (b === undefined && c === undefined && d === undefined) {
    b = c = d = a;
  }
  else if (c === undefined && d === undefined) {
    c = a;
    d = b;
  }
  return {
    top: a,
    right: b,
    bottom: c,
    left: d
  };
}
```

每个参数的含义根据传递给函数的参数数量而变化。此外，该函数只需要一个、两个或四个参数。要创建函数重载，只需多次声明函数头。最后一个函数头是在 函数体内实际活动的 **，但对外界不可用。**

```
function padding(all: number);
function padding(topAndBottom: number, leftAndRight: number);
function padding(top: number, right: number, bottom: number, left: number);
function padding(a: number, b?: number, c?: number, d?: number) {
  if (b === undefined && c === undefined && d === undefined) {
    b = c = d = a;
  }
  else if (c === undefined && d === undefined) {
    c = a;
    d = b;
  }
  return {
    top: a,
    right: b,
    bottom: c,
    left: d
  };
}

padding(1);       // Okay
padding(1,1);     // Okay
padding(1,1,1,1); // Okay
padding(1,1,1);   /* Error! No overload expects 3 arguments, but
overloads do exist that expect either 2 or 4 arguments. */
```

# **类**

您可以向属性和方法的参数添加类型

```
class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }
  greet(name: string) {
    return `Hi ${name}, ${this.greeting}`;
  }
}
```

## **访问修饰符**

Typescript 支持`public`，`private`，`protected`修饰符，这些修饰符决定了类成员的可访问性。

*   `public`成员的工作方式与普通 JavaScript 成员相同，并且是默认的修饰符。
*   不能从其包含类的外部访问`private`成员。
*   成员不同于私有成员，因为它也可以在派生类中被访问。

```
| Accessible on  | public | protected | private |
| :------------- | :----: | :-------: | :-----: |
| class          |   yes  |    yes    |   yes   |
| class children |   yes  |    yes    |    no   |
| class instance |   yes  |     no    |    no   |
```

## **只读修饰符**

属性必须在声明时或者在构造函数中初始化。

```
class Spider {
  readonly name: string;
  readonly numberOfLegs: number = 8;
  constructor (theName: string) {
    this.name = theName;
  }
}
```

## **参数属性**

**参数属性** 让您在一个地方创建和初始化成员。它们是通过在构造函数参数前面加上修饰符来声明的。

```
class Spider {
  readonly numberOfLegs: number = 8;
  constructor(readonly name: string) {
  }
}
```

## **摘要**

abstract 关键字既可以用于类，也可以用于抽象类方法。

*   ****抽象类**** 不能直接实例化。它们主要用于继承，其中扩展抽象类的类必须定义所有抽象方法。
*   ****抽象成员**** 不包含实现，因此不能被直接访问。这些成员必须在子类中实现 **(有点像接口)**

# **类型断言**

TypeScript 允许您以任何方式重写其推断类型。当您对变量类型的理解比编译器本身更好时，可以使用这种方法。

```
const friend = {};
friend.name = 'John';  // Error! Property 'name' does not exist on type '{}'

interface Person {
  name: string;
  age: number;
}

const person = {} as Person;
person.name = 'John';  // Okay
```

最初，类型断言的语法是

```
let person = <Person> {};
```

但这在 JSX 使用时产生了歧义。因此，建议使用`as`来代替。

类型断言通常在从 JavaScript 移植代码时使用，您可能知道比当前赋值的变量更准确的变量类型。但是断言可以被 ****认为有害。****

让我们看一下上一个例子中的 Person 接口。你注意到有什么不对吗？如果你注意到了丢失的财产**，恭喜你！编译器可能会帮助您为 Person 的属性提供自动完成功能，但是如果您遗漏了任何属性，它不会报错。**

# ****类型推断****

**当没有类型批注形式的显式信息时，TypeScript 会推断变量的类型。**

```
`/**
 * Variable definitinon
 */
let a = "some string";
let b = 1;
a = b;  // Error! Type 'number' is not assignable to type 'string'.

// In case of complex objects, TypeScript looks for the most common type
// to infer the type of the object.
const arr = [0, 1, false, true];  // (number | boolean)[]

/**
 * Function return types
 */
function sum(x: number, y: number) {
  return x + y;  // infer to return a number
}`
```

# ****类型兼容性****

**类型兼容性基于结构类型，结构类型仅基于类型的成员来关联类型。**

**结构类型的基本规则是，如果`y`的成员至少与`x`相同，那么`x`与`y`兼容。**

```
`interface Person {
  name: string;
}

let x: Person;  // Okay, despite not being an implementation of the Person interface
let y = { name: 'John', age: 20 };  // type { name: string; age: number }
x = y;

// Please note that x is still of type Person. 
// In the following example, the compiler will show an error message as it does not
// expect the property age in Person but the result will be as expected:
console.log(x.age); // 20`
```

**由于`y`有一个成员`name: string`，它匹配 Person 接口所需的属性，这意味着`x`是`y`的一个子类型。因此，转让是允许的。**

## ***功能***

******参数个数****
在一个函数调用中你需要传入至少足够多的参数，也就是说多余的参数不会导致任何错误。**

```
`function consoleName(person: Person) {
  console.log(person.name);
}
consoleName({ name: 'John' });           // Okay
consoleName({ name: 'John', age: 20 });  // Extra argument still Okay`
```

******返回类型****
返回类型至少要包含足够的数据。**

```
`let x = () => ({name: 'John'});
let y = () => ({name: 'John', age: 20 });
x = y;  // OK
y = x;  /* Error! Property 'age' is missing in type '{ name: string; }'
but required in type '{ name: string; age: number; }' */`
```

# ****型防护罩****

**类型保护允许您缩小条件块中对象的类型。**

## ****类型****

**在条件块中使用 typeof，编译器将知道变量的类型是不同的。在下面的示例中，TypeScript 理解在条件块之外，`x`可能是一个布尔值，并且不能对它调用函数`toFixed`。**

```
`function example(x: number | boolean) {
  if (typeof x === 'number') {
    return x.toFixed(2);
  }
  return x.toFixed(2); // Error! Property 'toFixed' does not exist on type 'boolean'.
}`
```

## ****的实例****

```
`class MyResponse {
  header = 'header example';
  result = 'result example';
  // ...
}
class MyError {
  header = 'header example';
  message = 'message example';
  // ...
}
function example(x: MyResponse | MyError) {
  if (x instanceof MyResponse) {
    console.log(x.message); // Error! Property 'message' does not exist on type 'MyResponse'.
    console.log(x.result);  // Okay
  } else {
    // TypeScript knows this must be MyError

    console.log(x.message); // Okay
    console.log(x.result);  // Error! Property 'result' does not exist on type 'MyError'.
  }
}`
```

## **中的**

****操作符检查一个对象的属性是否存在。****

```
**`interface Person {
  name: string;
  age: number;
}

const person: Person = {
  name: 'John',
  age: 28,
};

const checkForName = 'name' in person; // true`**
```

# ******文字类型******

****文字是 JavaScript 原语的 **确切的** 值。它们可以在类型联合中组合起来，以创建有用的抽象。****

```
**`type Orientation = 'landscape' | 'portrait';
function changeOrientation(x: Orientation) {
  // ...
}
changeOrientation('portrait'); // Okay
changeOrientation('vertical'); /* Error! Argument of type '"vertical"' is not 
assignable to parameter of type 'Orientation'. */`**
```

## ******条件类型******

****条件类型描述了一个类型关系测试，并根据该测试的结果从两个可能的类型中选择一个。****

```
**`type X = A extends B ? C : D;`**
```

****这意味着如果类型`A`可分配给类型`B`，那么`X`与`C`是相同的类型。否则`X`与类型`D;`相同****

# ******通用类型******

****泛型类型是一种必须包含或引用另一种类型才能完整的类型。它在各种变量之间实施有意义约束。在下面的例子中，一个函数返回你传入的任何类型的数组。****

```
**`function reverse<T>(items: T[]): T[] {
  return items.reverse();
}
reverse([1, 2, 3]); // number[]
reverse([0, true]); // (number | boolean)[]`**
```

## ******keyof******

****`keyof`操作符查询给定类型的键集。****

```
**`interface Person {
  name: string;
  age: number;
}
type PersonKeys = keyof Person; // 'name' | 'age'`**
```

## ******映射类型******

****映射类型允许您通过映射属性类型从现有类型创建新类型。现有类型的每个属性都根据您指定的规则进行转换。****

## ******部分******

```
**`type Partial<T> = {
  [P in keyof T]?: T[P];
}`**
```

*   ****通用分部类型由一个类型参数`T`定义。****
*   ****`keyof T` 表示字符串类型的`T`的所有属性名的并集。****
*   ****`[P in keyof T]?: T[P]`表示`T`类型的每个属性`P`的类型应该是可选的，并转换为`T[P]`。****
*   ****`T[P]`表示类型`T`的属性`P`的类型。****

## ******只读******

****正如我们在接口一节中所介绍的，TypeScript 允许您创建只读属性。有一个`Readonly`类型接受类型`T`并将其所有属性设置为 readonly。****

```
**`type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};`**
```

## ******排除******

****`Exclude`允许您从另一种类型中删除某些类型。`Exclude`从`T`可分配给`T`的任何东西。****

```
**`/**
 * type Exclude<T, U> = T extends U ? never : T;
 */
type User = {
  _id: number;
  name: string;
  email: string;
  created: number;
};

type UserNoMeta = Exclude<keyof User, '_id' | 'created'>`**
```

## ******挑选******

****`Pick`允许您从另一种类型中选择某些类型。`Pick`从`T`可分配给`T`的任何东西。****

```
**`/**
 * type Pick<T, K extends keyof T> = {
 *   [P in K]: T[P];
 *  };
 */
type UserNoMeta = Pick<User, 'name' | 'email'>`**
```

## *****推断*****

****您可以使用`infer`关键字来推断条件类型的`extends`子句中的类型变量。这种推断类型变量只能在条件类型的 true 分支中使用。****

## ******返回类型******

****获取函数的返回类型。****

```
**`/**
 * Original TypeScript's ReturnType
 * type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;
 */
type MyReturnType<T> = T extends (...args: any) => infer R ? R : any;

type TypeFromInfer = MyReturnType<() => number>;  // number
type TypeFromFallback = MyReturnType<string>;     // any`**
```

****让我们来分解一下`MyReturnType`:****

*   ****`T`的返回类型是…****
*   ****首先，`T`是函数吗？****
*   ****如果是，那么该类型解析为推断的返回类型`R`。****
*   ****否则类型解析为`any`。****

# ****参考和有用的链接****

****[https://basarat.gitbooks.io/typescript/](https://basarat.gitbooks.io/typescript/)****

****[https://www.typescriptlang.org/docs/home.html](https://www.typescriptlang.org/docs/home.html)****

****[https://www.tutorialsteacher.com/typescript](https://www.tutorialsteacher.com/typescript)****

****[https://github.com/dzharii/awesome-typescript](https://github.com/dzharii/awesome-typescript)****

****[https://github . com/typescript-cheat sheets/react-typescript-cheat sheet](https://github.com/typescript-cheatsheets/react-typescript-cheatsheet)****

* * *

****为了学习和尝试 TypeScript，我用 TS 和 React-Native 构建了一个简单的 CurrencyConverter 应用程序。你可以在这里查看这个项目[。](https://github.com/gustavoaz7/React-Native_Learning/tree/master/CurrencyConverter)****

****感谢并祝贺你读到这里！如果你对此有任何想法，欢迎发表评论。****

****你可以在推特上找到我。****