# ES6 中数组析构简介

> 原文：<https://www.freecodecamp.org/news/array-destructuring-in-es6-30e398f21d10/>

by Kevwe Ochuko

JavaScript 中的**destructing**是一种从数组中提取多个属性的简化方法，通过使用类似于数组文字的语法，通过赋值将结构分解成自己的组成部分。

它创建一个模式，描述您所期望的值的种类，并进行赋值。数组析构使用位置。

请参见下面的代码片段。

```
var [first, second, third] = ["Laide", "Gabriel", "Jets"];
```

*析构的语法。*

```
var first = "laide",
    second = "Gabriel",
    third = "Jets";
```

*没有析构的语法。*

> 你不能用数字来解构。Numbers 会抛出错误，因为 numbers 不能是变量名。

```
var [1, 2, 3] = ["Laide", "Ola", "Jets"];
```

这个语法抛出一个错误。

**析构**使得从数组中提取数据变得非常简单和易读。想象一下，试图从一个有 5 或 6 层的嵌套数组中提取数据。那会非常乏味。在赋值的左边使用一个数组文字。

```
var thing = ["Table", "Chair", "Fan"];
var [a, b, c] = thing;
```

它接受左侧数组文本中的每个变量，并将其映射到数组中相同索引处的相同元素。

```
console.log(a); // Output: Table
console.log(b); //Output: Chair
console.log(c); //Output: Fan
```

声明和赋值可以在析构中分别完成。

```
var first, second;
[first, second] = ["Male", "Female"];
```

如果传递给析构数组文字的变量个数大于数组中的元素个数，那么没有映射到数组中任何元素的变量返回`undefined` *。*

```
var things = ["Table", "Chair", "Fan", "Rug"];
var [a, b, c, d, e] = things;
console.log(c); //Output: Fan
console.log(d); //Output: Rug
console.log(e); //Output: undefined
```

如果传递给析构数组的变量数小于数组中的元素数，则只剩下没有要映射的变量的元素。没有任何错误。

```
var things = ["Table", "Chair", "Fan", "Rug"];
var [a, b, c] = things;
console.log(c); // Output: Fan
```

### **析构返回的数组**

析构使得使用以值形式返回数组的函数更加精确。它适用于所有的可迭代对象。

```
function runners(){
    return ["Sandra", "Ola", "Chi"];
}

var [a, b, c] = runners();
console.log(a); //Output: Sandra
console.log(b); //Output: Ola
console.log(c); //Output: Chi
```

### **默认值**

如果没有值或`*undefined*` 被传递，析构允许将默认值赋给变量。这就像是在一无所获时提供一个退路。

```
var a, b;
[a = 40, b = 4] = [];
console.log(a); //Output: 40
console.log(b); //Output: 4

[a = 40, b = 4] = [1, 23];
console.log(a); //Output: 1
console.log(b); //Output: 23
```

默认值也可以引用其他变量，包括同一数组中的变量。

```
var [first = "Cotlin", second = first] = [];
console.log(first); //Output: Cotlin
console.log(second); //Output: Cotlin

var [first = "Cotlin", second = first] = ["Koku"];
console.log(first); //Output: Koku
console.log(second); //Output: Koku

var [first = "Cotlin", second = first] = ["Koku", "Lydia"];
console.log(first); //Output: Koku
console.log(second); //Output: Lydia
```

### 忽略一些价值观

析构允许你将一个变量映射到你感兴趣的元素。通过使用尾随逗号，可以忽略或跳过数组中的其他元素。

```
var a, b;
[a, , b] = ["Lordy", "Crown", "Roses"];

console.log(a); //Output: Lordy
console.log(b); //Output: Roses
```

### **其余参数和展开语法**

ES6 中新增的 **(…)** **运算符** 可以用于析构。如果 **(…)运算符** 出现在析构时的左侧，则是一个 [****剩余参数****](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters) ****。**** 一个 rest 参数用来映射数组中所有剩余的元素，这些元素还没有映射到 Rest 变量本身。这就像收集遗留下来的东西一样。Rest 变量必须总是最后一个，否则抛出`SyntaxError`。

```
var planets = ["Mercury", "Earth", "Venus", "Mars", "Pluto", "Saturn"];
var [first, , third, ...others] = planets;

console.log(first); //Output: Mercury
console.log(third); //Output: Venus
console.log(others); //Output: ["Mars", "Pluto", "Saturn"]
```

如果(…)运算符出现在析构中的右边，那么它是一个 [**展开语法**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) **。** It 获取数组中没有映射变量的所有其他元素，然后将其映射到其余变量。

```
var planets = ["Mercury", "Earth", "Venus", "Mars", "Pluto", "Saturn"];

var [first, second, ...rest] = ["Mercury", "Earth", ...planets, "Saturn"];

console.log(first); //Output: Mercury
console.log(second); //Output: Earth
console.log(rest); //Output: ["Venus", "Mars", "Pluto", "Saturn"]
```

当左侧有更多变量时，它会将数组中的单个元素均等地映射到变量。

```
var planets = ["Mercury", "Earth", "Venus", "Mars", "Pluto", "Saturn"];

var [first, second, ...rest] = ["Mercury", ...planets];

console.log(first); //Output: Mercury
console.log(second); //Output: Mercury
console.log(rest); //Output: ["Earth", "Venus", "Mars", "Pluto", "Saturn"]

var planets = ["Mercury", "Earth", "Venus", "Mars", "Pluto", "Saturn"];

var [first, second, third, fourth ...rest] = ["Mercury", "Earth", ...planets];

console.log(first); //Output: Mercury
console.log(second); //Output: Earth
console.log(third); //Output: Mercury
console.log(fourth); //Output: Earth
console.log(rest); //Output: ["Venus", "Mars", "Pluto", "Saturn"]
```

### **互换或交换变量**

一个析构表达式可以用来交换两个变量的值。

```
var a, b;
[a, b] = ["Male", "Female"];
[a, b] = [b, a];

console.log(a); //Output: Female
console.log(b); //Output: Male
```

### **嵌套数组析构**

也可以用数组进行嵌套析构。相应的项必须是一个数组，以便使用嵌套的析构数组文本将其中的项赋给局部变量。

```
var numbers = [8, [1, 2, 3], 10, 12];
var  [a, [d, e, f]] = numbers;

console.log(a); // Output: 8
console.log(d); // Output: 1
console.log(e); // Output: 2
```

### 多重数组解构

在同一个代码段中，可以多次析构数组。

```
var places = ["first", "second", "third", "fourth"];
var [a, b, , d] = [f, ...rest] = places;

console.log(a); //Output: first
console.log(d); //Output: fourth
console.log(f); //Output: first
console.log(rest); //Output: ["second", "third", "fourth"]
```

### **结论**

你可以把代码复制粘贴到 [Babel 的网站](https://babeljs.io/en/repl.html#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=Q&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015%2Ces2016%2Creact%2Cstage-2&prettier=false&targets=&version=6.26.0&envVersion=)上，看看如果析构不存在的话代码会是什么样子。你应该写更多的代码行，但是析构化简化了这一切。