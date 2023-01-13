# 高级类型脚本类型备忘单(带示例)

> 原文：<https://www.freecodecamp.org/news/advanced-typescript-types-cheat-sheet-with-examples/>

TypeScript 是一种类型化语言，允许您指定变量、函数参数、返回值和对象属性的类型。

这里有一个高级的 TypeScript 类型的备忘单和例子。

让我们开始吧。

*   [路口类型](#intersection-types)
*   [工会类型](#union-types)
*   [通用类型](#generic-types)
*   [公用事业类型](#utility-types)
*   [部分](#partial)
*   [必需的](#required)
*   [只读](#readonly)
*   [挑选](#pick)
*   [省略](#omit)
*   [提取](#extract)
*   [排除](#exclude)
*   [记录](#record)
*   [不可空](#nonnullable)
*   [映射类型](#mapped-types)
*   [型防护装置](#type-guards)
*   [条件类型](#conditional-types)

## 交叉点类型

交集类型是一种将多种类型组合成一种类型的方式。这意味着您可以将给定的类型 A 与类型 B 或更多类型合并，并获得具有所有属性的单个类型。

```
type LeftType = {
  id: number
  left: string
}

type RightType = {
  id: number
  right: string
}

type IntersectionType = LeftType & RightType

function showType(args: IntersectionType) {
  console.log(args)
}

showType({ id: 1, left: "test", right: "test" })
// Output: {id: 1, left: "test", right: "test"} 
```

如您所见，`IntersectionType`结合了两种类型- `LeftType`和`RightType`，并使用`&`符号来构造交集类型。

## 工会类型

联合类型允许在一个给定的变量中有不同类型的注释。

```
type UnionType = string | number

function showType(arg: UnionType) {
  console.log(arg)
}

showType("test")
// Output: test

showType(7)
// Output: 7 
```

函数`showType`是一个联合类型，接受字符串和数字作为参数。

## 泛型类型

泛型是重用给定类型的一部分的一种方式。它有助于捕获作为参数传入的类型`T`。

```
function showType<T>(args: T) {
  console.log(args)
}

showType("test")
// Output: "test"

showType(1)
// Output: 1 
```

要构造一个泛型类型，你需要使用括号并把`T`作为参数传递。
在这里，我使用`T`(名字由你决定)，然后用不同的类型注释调用函数`showType`两次，因为它是通用的——可以重用。

```
interface GenericType<T> {
  id: number
  name: T
}

function showType(args: GenericType<string>) {
  console.log(args)
}

showType({ id: 1, name: "test" })
// Output: {id: 1, name: "test"}

function showTypeTwo(args: GenericType<number>) {
  console.log(args)
}

showTypeTwo({ id: 1, name: 4 })
// Output: {id: 1, name: 4} 
```

这里，我们有另一个例子，它有一个接收泛型类型`T`的接口`GenericType`。因为它是可重复使用的，我们可以先用一个字符串再用一个数字来调用它。

```
interface GenericType<T, U> {
  id: T
  name: U
}

function showType(args: GenericType<number, string>) {
  console.log(args)
}

showType({ id: 1, name: "test" })
// Output: {id: 1, name: "test"}

function showTypeTwo(args: GenericType<string, string[]>) {
  console.log(args)
}

showTypeTwo({ id: "001", name: ["This", "is", "a", "Test"] })
// Output: {id: "001", name: Array["This", "is", "a", "Test"]} 
```

泛型类型可以接收几个参数。这里，我们传入两个参数:`T`和`U`，然后使用它们作为属性的类型注释。也就是说，我们现在可以使用接口并提供不同的类型作为参数。

## 实用程序类型

TypeScript 提供了方便的内置实用工具，有助于轻松操作类型。要使用它们，您需要向`<>`传递您想要转换的类型。

### 部分的

*   `Partial<T>`

Partial 允许您将类型`T`的所有属性设为可选。它会在每个字段旁边添加一个`?`标记。

```
interface PartialType {
  id: number
  firstName: string
  lastName: string
}

function showType(args: Partial<PartialType>) {
  console.log(args)
}

showType({ id: 1 })
// Output: {id: 1}

showType({ firstName: "John", lastName: "Doe" })
// Output: {firstName: "John", lastName: "Doe"} 
```

正如您所看到的，我们有一个接口`PartialType`，它被用作函数`showType()`接收的参数的类型注释。为了使属性可选，我们必须使用`Partial`关键字并传入类型`PartialType`作为参数。也就是说，现在所有字段都变成可选的了。

### 需要

*   `Required<T>`

与`Partial`不同的是，`Required`实用程序使得`T`类型的所有属性都是必需的。

```
interface RequiredType {
  id: number
  firstName?: string
  lastName?: string
}

function showType(args: Required<RequiredType>) {
  console.log(args)
}

showType({ id: 1, firstName: "John", lastName: "Doe" })
// Output: { id: 1, firstName: "John", lastName: "Doe" }

showType({ id: 1 })
// Error: Type '{ id: number: }' is missing the following properties from type 'Required<RequiredType>': firstName, lastName 
```

`Required`实用程序将使所有属性成为必需的，即使我们在使用该实用程序之前先使它们成为可选的。如果省略了一个属性，TypeScript 将抛出一个错误。

### 只读

*   `Readonly<T>`

该实用程序类型将转换类型`T`的所有属性，以使它们不能用新值重新分配。

```
interface ReadonlyType {
  id: number
  name: string
}

function showType(args: Readonly<ReadonlyType>) {
  args.id = 4
  console.log(args)
}

showType({ id: 1, name: "Doe" })
// Error: Cannot assign to 'id' because it is a read-only property. 
```

这里，我们使用实用程序`Readonly`使`ReadonlyType`的属性不可重新分配。也就是说，如果您试图给这些字段中的一个赋予新的值，将会抛出一个错误。

除此之外，您还可以在属性前使用关键字`readonly`使其不可重新分配。

```
interface ReadonlyType {
  readonly id: number
  name: string
} 
```

### 挑选

*   `Pick<T, K>`

它允许您通过选择该类型的一些属性`K`从现有模型`T`中创建一个新类型。

```
interface PickType {
  id: number
  firstName: string
  lastName: string
}

function showType(args: Pick<PickType, "firstName" | "lastName">) {
  console.log(args)
}

showType({ firstName: "John", lastName: "Doe" })
// Output: {firstName: "John"}

showType({ id: 3 })
// Error: Object literal may only specify known properties, and 'id' does not exist in type 'Pick<PickType, "firstName" | "lastName">' 
```

`Pick`与我们之前已经看到的实用程序有点不同。它需要两个参数- `T`是您想要从中选取元素的类型，`K`是您想要选择的属性。您也可以通过用竖线(`|`)符号分隔来选择多个字段。

### 省略

*   `Omit<T, K>`

`Omit`实用程序与`Pick`类型相反。它将从类型`T`中移除`K`属性，而不是选择元素。

```
interface PickType {
  id: number
  firstName: string
  lastName: string
}

function showType(args: Omit<PickType, "firstName" | "lastName">) {
  console.log(args)
}

showType({ id: 7 })
// Output: {id: 7}

showType({ firstName: "John" })
// Error: Object literal may only specify known properties, and 'firstName' does not exist in type 'Pick<PickType, "id">' 
```

这个实用程序类似于`Pick`的工作方式。它期望类型和属性从该类型中省略。

### 提取

*   `Extract<T, U>`

`Extract`允许你通过选择存在于两种不同类型中的属性来构建一个类型。该实用程序将从`T`中提取可分配给`U`的所有属性。

```
interface FirstType {
  id: number
  firstName: string
  lastName: string
}

interface SecondType {
  id: number
  address: string
  city: string
}

type ExtractType = Extract<keyof FirstType, keyof SecondType>
// Output: "id" 
```

这里，我们有两种类型，它们有共同的属性`id`。因此通过使用`Extract`关键字，我们得到了字段`id`,因为它在两个接口中都存在。如果您有多个共享字段，该实用程序将提取所有相似的属性。

### 排除

与`Extract`不同，`Exclude`实用程序将通过排除已经存在于两种不同类型中的属性来构造一种类型。它从`T`中排除了所有可分配给`U`的字段。

```
interface FirstType {
  id: number
  firstName: string
  lastName: string
}

interface SecondType {
  id: number
  address: string
  city: string
}

type ExcludeType = Exclude<keyof FirstType, keyof SecondType>

// Output; "firstName" | "lastName" 
```

正如您在这里看到的，属性`firstName`和`lastName`可分配给`SecondType`类型，因为它们不在那里。通过使用`Extract`关键字，我们得到了预期的这些字段。

### 记录

*   `Record<K,T>`

这个实用程序帮助你用一组给定类型`T`的属性`K`构造一个类型。`Record`在将一种类型的属性映射到另一种类型时非常方便。

```
interface EmployeeType {
  id: number
  fullname: string
  role: string
}

let employees: Record<number, EmployeeType> = {
  0: { id: 1, fullname: "John Doe", role: "Designer" },
  1: { id: 2, fullname: "Ibrahima Fall", role: "Developer" },
  2: { id: 3, fullname: "Sara Duckson", role: "Developer" },
}

// 0: { id: 1, fullname: "John Doe", role: "Designer" },
// 1: { id: 2, fullname: "Ibrahima Fall", role: "Developer" },
// 2: { id: 3, fullname: "Sara Duckson", role: "Developer" } 
```

`Record`的工作方式相对简单。这里，它期望一个`number`作为类型，这就是为什么我们有 0、1 和 2 作为`employees`变量的键。如果你试图使用一个字符串作为属性，就会抛出一个错误。接下来，属性集由`EmployeeType`给出，因此对象具有字段 id、全名和角色。

### 非努拉贝尔

*   `NonNullable<T>`

它允许你从类型`T`中删除`null`和`undefined`。

```
type NonNullableType = string | number | null | undefined

function showType(args: NonNullable<NonNullableType>) {
  console.log(args)
}

showType("test")
// Output: "test"

showType(1)
// Output: 1

showType(null)
// Error: Argument of type 'null' is not assignable to parameter of type 'string | number'.

showType(undefined)
// Error: Argument of type 'undefined' is not assignable to parameter of type 'string | number'. 
```

这里，我们将类型`NonNullableType`作为参数传递给`NonNullable`实用程序，该实用程序通过从该类型中排除`null`和`undefined`来构造一个新类型。也就是说，如果传递一个可空值，TypeScript 将抛出一个错误。

顺便说一下，如果您将`--strictNullChecks`标志添加到`tsconfig`文件，TypeScript 将应用非空性规则。

## 映射类型

映射类型允许您获取一个现有的模型，并将它的每个属性转换成一个新的类型。请注意，前面提到的一些实用程序类型也是映射类型。

```
type StringMap<T> = {
  [P in keyof T]: string
}

function showType(arg: StringMap<{ id: number; name: string }>) {
  console.log(arg)
}

showType({ id: 1, name: "Test" })
// Error: Type 'number' is not assignable to type 'string'.

showType({ id: "testId", name: "This is a Test" })
// Output: {id: "testId", name: "This is a Test"} 
```

会将传入的任何类型转换成一个字符串。也就是说，如果我们在函数`showType()`中使用它，收到的参数必须是一个字符串——否则，TypeScript 将抛出一个错误。

## 防护类型

类型保护允许你用一个操作符来检查一个变量或者一个对象的类型。这是一个使用`typeof`、`instanceof`或`in`返回类型的条件块。

*   `typeof`

```
function showType(x: number | string) {
  if (typeof x === "number") {
    return `The result is ${x + x}`
  }
  throw new Error(`This operation can't be done on a ${typeof x}`)
}

showType("I'm not a number")
// Error: This operation can't be done on a string

showType(7)
// Output: The result is 14 
```

如您所见，我们有一个普通的 JavaScript 条件块，它检查用`typeof`接收的参数的类型。有了这个，你现在可以用这个条件来保护你的类型了。

*   `instanceof`

```
class Foo {
  bar() {
    return "Hello World"
  }
}

class Bar {
  baz = "123"
}

function showType(arg: Foo | Bar) {
  if (arg instanceof Foo) {
    console.log(arg.bar())
    return arg.bar()
  }

  throw new Error("The type is not supported")
}

showType(new Foo())
// Output: Hello World

showType(new Bar())
// Error: The type is not supported 
```

和前面的例子一样，这个例子也是一个类型保护，它检查接收到的参数是否是`Foo`类的一部分，并相应地处理它。

*   `in`

```
interface FirstType {
  x: number
}
interface SecondType {
  y: string
}

function showType(arg: FirstType | SecondType) {
  if ("x" in arg) {
    console.log(`The property ${arg.x} exists`)
    return `The property ${arg.x} exists`
  }
  throw new Error("This type is not expected")
}

showType({ x: 7 })
// Output: The property 7 exists

showType({ y: "ccc" })
// Error: This type is not expected 
```

`in`操作符允许您检查作为参数接收的对象上是否存在属性`x`。

## 条件类型

条件类型测试两种类型，并根据测试结果选择其中一种。

```
type NonNullable<T> = T extends null | undefined ? never : T 
```

这个`NonNullable`实用程序类型的例子检查类型是否为空，并根据它进行处理。正如您所注意到的，它使用了 JavaScript 三元运算符。

感谢阅读。

你可以在[我的博客](https://www.ibrahima-ndaw.com)上找到类似这样的精彩内容，或者在 Twitter 上关注我[以获得通知。](https://twitter.com/ibrahima92_)