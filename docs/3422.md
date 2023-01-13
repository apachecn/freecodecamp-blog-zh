# 如何在 JavaScript 中使用数组和对象析构

> 原文：<https://www.freecodecamp.org/news/array-and-object-destructuring-in-javascript/>

析构赋值是 ES6 附带的一个很酷的特性。析构是一个 JavaScript 表达式，它可以将数组中的值或对象中的属性解包到不同的变量中。也就是说，我们可以从数组和对象中提取数据，并将它们赋给变量。

为什么这是必要的？

假设我们想从一个数组中提取数据。以前，这是如何做到的？

```
let introduction = ["Hello", "I" , "am", "Sarah"];
let greeting = introduction[0];
let name = introduction[3];

console.log(greeting);//"Hello"
console.log(name);//"Sarah" 
```

我们可以看到，当我们想要从一个数组中提取数据时，我们必须一遍又一遍地做同样的事情。

ES6 去结构分配使得提取这些数据更加容易。这是怎么回事？首先，我们将讨论数组的析构赋值。然后我们将继续讨论对象析构。

让我们开始吧。

## 基本数组析构

如果我们想从数组中提取数据，使用析构赋值是非常简单的。

让我们参考第一个数组的例子。与其重复这个过程，我们不如这样做:

```
let introduction = ["Hello", "I" , "am", "Sarah"];
let [greeting, pronoun] = introduction;

console.log(greeting);//"Hello"
console.log(pronoun);//"I" 
```

我们也可以用同样的结果来做这件事。

```
let [greeting, pronoun] = ["Hello", "I" , "am", "Sarah"];

console.log(greeting);//"Hello"
console.log(pronoun);//"I"
```

### 赋值前声明变量

变量可以在赋值前声明，如下所示:

```
 let greeting, pronoun;
[greeting, pronoun] = ["Hello", "I" , "am", "Sarah"];

console.log(greeting);//"Hello"
console.log(pronoun);//"I" 
```

请注意，变量是从左到右设置的。因此，第一个变量获取数组中的第一项，第二个变量获取数组中的第二项，依此类推。

### 跳过数组中的项目

如果我们想得到数组中的第一个和最后一个项，而不是第一个和第二个项，并且我们只想给两个变量赋值，那该怎么办呢？这也可以做到。请看下面的例子:

```
let [greeting,,,name] = ["Hello", "I" , "am", "Sarah"];

console.log(greeting);//"Hello"
console.log(name);//"Sarah" 
```

刚刚发生了什么？

请看变量赋值左边的数组。请注意，我们不是只有一个逗号，而是有三个。逗号分隔符用于跳过数组中的值。所以如果你想跳过数组中的一项，只需使用逗号。

我们再做一个。让我们跳过单子上的第一项和第三项。我们该怎么做？

```
let [,pronoun,,name] = ["Hello", "I" , "am", "Sarah"];

console.log(pronoun);//"I"
console.log(name);//"Sarah" 
```

所以逗号分隔符发挥了神奇的作用。因此，如果我们想跳过所有项目，我们只需这样做:

```
let [,,,,] = ["Hello", "I" , "am", "Sarah"]; 
```

### 分配数组的其余部分

如果我们想将数组的一部分赋给变量，将数组中的其余项赋给一个特定的变量呢？在这种情况下，我们会这样做:

```
let [greeting,...intro] = ["Hello", "I" , "am", "Sarah"];

console.log(greeting);//"Hello"
console.log(intro);//["I", "am", "Sarah"] 
```

使用这种模式，您可以将数组的剩余部分解包并赋给一个变量。

### 用函数析构赋值

我们也可以从函数返回的数组中提取数据。假设我们有一个函数返回一个数组，如下例所示:

```
function getArray() {
    return ["Hello", "I" , "am", "Sarah"];
} 
let [greeting,pronoun] = getArray();

console.log(greeting);//"Hello"
console.log(pronoun);//"I" 
```

我们得到同样的结果。

### 使用默认值

如果从数组中提取的值是`undefined`，则可以将默认值分配给变量:

```
let [greeting = "hi",name = "Sarah"] = ["hello"];

console.log(greeting);//"Hello"
console.log(name);//"Sarah" 
```

所以`name`回退到“Sarah”，因为它没有在数组中定义。

### 使用析构赋值交换值

还有一件事。我们可以使用析构赋值来交换变量的值:

```
let a = 3;
let b = 6;

[a,b] = [b,a];

console.log(a);//6
console.log(b);//3 
```

接下来，让我们继续对象析构。

## 对象析构

首先，让我们看看为什么需要对象析构。

假设我们想从一个对象中提取数据并赋给新的变量。在 ES6 之前，这是如何实现的？

```
let person = {name: "Sarah", country: "Nigeria", job: "Developer"};

let name = person.name;
let country = person.country;
let job = person.job;

console.log(name);//"Sarah"
console.log(country);//"Nigeria"
console.log(job);//Developer" 
```

看提取所有数据有多繁琐。我们不得不反复做同样的事情。ES6 解构真的拯救了世界。让我们直接开始吧。

### 基本对象析构

让我们用 ES6 重复上面的例子。我们可以使用左边的对象提取数据，而不是逐个赋值:

```
 let person = {name: "Sarah", country: "Nigeria", job: "Developer"};

let {name, country, job} = person;

console.log(name);//"Sarah"
console.log(country);//"Nigeria"
console.log(job);//Developer" 
```

你会得到同样的结果。将变量赋给未声明的对象也是有效的:

```
let {name, country, job} = {name: "Sarah", country: "Nigeria", job: "Developer"};

console.log(name);//"Sarah"
console.log(country);//"Nigeria"
console.log(job);//Developer" 
```

### 赋值前声明的变量

对象中的变量可以在用析构赋值之前声明。让我们试试:

```
let person = {name: "Sarah", country: "Nigeria", job: "Developer"}; 
let name, country, job;

{name, country, job} = person;

console.log(name);// Error : "Unexpected token =" 
```

等等，刚刚发生了什么？！哦，我们忘了在花括号前加`()`。

在没有声明的情况下使用对象文本析构赋值时，赋值语句周围的`( )`是必需的语法。这是因为左边的`{}`被认为是一个块，而不是一个对象文字。所以正确的做法是这样的:

```
let person = {name: "Sarah", country: "Nigeria", job: "Developer"};
let name, country, job;

({name, country, job} = person);

console.log(name);//"Sarah"
console.log(job);//"Developer" 
```

同样需要注意的是，当使用这种语法时，`()`前面应该有一个分号。否则，它可能用于执行前一行中的函数。

注意，左侧对象中的变量应该与对象`person`中的属性键同名。如果名称不同，我们将得到`undefined`:

```
let person = {name: "Sarah", country: "Nigeria", job: "Developer"};

let {name, friends, job} = person;

console.log(name);//"Sarah"
console.log(friends);//undefined 
```

但是如果我们想使用一个新的变量名，我们可以。

### 使用新的变量名

如果我们想将一个对象的值赋给一个新变量，而不是使用属性的名称，我们可以这样做:

```
let person = {name: "Sarah", country: "Nigeria", job: "Developer"};

let {name: foo, job: bar} = person;

console.log(foo);//"Sarah"
console.log(bar);//"Developer" 
```

因此提取的值被传递给新变量`foo`和`bar`。

### 使用默认值

默认值也可以在对象析构中使用，以防变量`undefined`在它想要从中提取数据的对象中:

```
let person = {name: "Sarah", country: "Nigeria", job: "Developer"};

let {name = "myName", friend = "Annie"} = person;

console.log(name);//"Sarah"
console.log(friend);//"Annie" 
```

因此，如果该值不是未定义的，该变量存储从对象中提取的值，就像在`name`的情况中一样。否则，它会像对`friend`一样使用默认值。

我们还可以在给新变量赋值时设置默认值:

```
let person = {name: "Sarah", country: "Nigeria", job: "Developer"};

let {name:foo = "myName", friend: bar = "Annie"} = person;

console.log(foo);//"Sarah"
console.log(bar);//"Annie" 
```

所以`name`被从`person`中提取出来并赋给一个不同的变量。另一方面，`friend`是`person`中的`undefined`，所以新变量`bar`被赋予默认值。

### 计算属性名

计算属性名是另一个对象文字特性，也适用于析构。如果将属性放在方括号中，则可以通过表达式指定属性的名称:

```
let prop = "name";

let {[prop] : foo} = {name: "Sarah", country: "Nigeria", job: "Developer"};

console.log(foo);//"Sarah" 
```

### 组合数组和对象

数组也可以在对象析构中与对象一起使用:

```
let person = {name: "Sarah", country: "Nigeria", friends: ["Annie", "Becky"]};

let {name:foo, friends: bar} = person;

console.log(foo);//"Sarah"
console.log(bar);//["Annie", "Becky"] 
```

### 对象析构中的嵌套

析构时对象也可以嵌套:

```
let person = {
    name: "Sarah",
    place: {
        country: "Nigeria", 
        city: "Lagos" }, 
    friends : ["Annie", "Becky"]
};

let {name:foo,
     place: {
         country : bar,
         city : x}
    } = person;

console.log(foo);//"Sarah"
console.log(bar);//"Nigeria" 
```

### 对象析构中的 Rest

rest 语法还可以用来挑选析构模式尚未挑选的属性键。这些键及其值被复制到一个新对象中:

```
let person = {name: "Sarah", country: "Nigeria", job: "Developer" friends: ["Annie", "Becky"]};

let {name, friends, ...others} = person;

console.log(name);//"Sarah"
console.log(friends);//["Annie", "Becky"]
console.log(others);// {country: "Nigeria", job: "Developer"} 
```

这里，其键不属于所列变量名的其余属性被分配给变量`others`。这里的 rest 语法是`...others`。`others`可以重命名为你想要的任何变量。

最后一件事——让我们看看如何在函数中使用对象析构。

### 对象析构和函数

对象析构可用于为函数分配参数:

```
function person({name: x, job: y} = {}) {
    console.log(x);
}

person({name: "Michelle"});//"Michelle"
person();//undefined
person(friend);//Error : friend is not defined 
```

注意参数对象右侧的`{}`。它使得我们可以调用函数而不传递任何参数。这就是我们得到`undefined`的原因。如果我们删除它，我们会得到一个错误信息。

我们还可以为参数分配默认值:

```
function person({name: x = "Sarah", job: y = "Developer"} = {}) {
    console.log(x);
}

person({name});//"Sarah" 
```

正如我们在上面的例子中看到的，我们可以用数组和对象析构做很多事情。

感谢您的阅读。:)