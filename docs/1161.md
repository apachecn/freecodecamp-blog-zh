# 学习 React 之前需要了解的几个 JavaScript 概念

> 原文：<https://www.freecodecamp.org/news/top-javascript-concepts-to-know-before-learning-react/>

如果你想学习 React 或者任何 JavaScript 框架，你首先需要理解基本的 JavaScript 方法和概念。

否则，这就像一个年轻人在学会走路之前先学会跑。

许多开发人员在学习 React 时选择“边学边做”的方法。但这通常不会提高生产率，反而会扩大他们 JavaScript 知识的缺口。这种方法使得吸收每个新特性的难度增加了一倍(您可能会开始混淆 JavaScript 和 React)。

React 是一个 JavaScript 框架，用于构建基于 UI 组件的用户界面。它的所有代码都是用 JavaScript 编写的，包括用 JSX 语言编写的 HTML 标记(这使得开发人员可以轻松地将 HTML 和 JavaScript 一起编写)。

在这篇文章中，我们将采取一种实用的方法，回顾你在学习 React 之前需要掌握的所有 JS 思想和技术。

React 使用现代 JavaScript 功能构建，这些功能主要是在 ES2015 中引入的。这就是我们在这篇文章中要讨论的内容。为了帮助你加深学习，我将把不同的链接与每个方法和概念联系起来。

*让我们开始吧……*

# 学习 React 之前需要了解的 JavaScript

## JavaScript 中的回调函数

回调函数是在另一个函数完成执行后执行的函数。它通常作为另一个函数的输入。

理解回调是至关重要的，因为它们用在数组方法(如`map()`、`filter()`等)、`setTimeout()`、事件监听器(如 click、scroll 等)和许多其他地方。

下面是一个带有回调函数的“click”事件侦听器的示例，每当单击按钮时都会运行该函数:

```
//HTML
<button class="btn">Click Me</button>

//JavaScript
const btn = document.querySelector('.btn');

btn.addEventListener('click', () => {
  let name = 'John doe';
  console.log(name.toUpperCase())
})
```

> **NB:** 回调函数可以是普通函数，也可以是箭头函数。

## JavaScript 中的承诺

如前所述，回调函数在原始函数执行后执行。您现在可以开始考虑将如此多的回调函数堆叠在一起，因为您不希望某个特定的函数在父函数完成运行或经过特定时间后才运行。

例如，让我们尝试在 2 秒钟后在控制台中显示 5 个名字，也就是说，第一个名字在 2 秒后出现，第二个在 4 秒后出现，依此类推...

```
setTimeout(() => {
    console.log("Joel");
    setTimeout(() => {
        console.log("Victoria");
        setTimeout(() => {
            console.log("John");
            setTimeout(() => {
                console.log("Doe");
                setTimeout(() => {
                    console.log("Sarah");
                }, 2000);
            }, 2000);
        }, 2000);
    }, 2000);
}, 2000);
```

上面的例子可以工作，但是很难理解、调试，甚至添加错误处理。这就是所谓的**“回调地狱”**。回调地狱是由复杂嵌套回调编码引起的一个大问题。

使用承诺的主要原因是防止回调地狱。有了承诺，我们可以用同步方式编写异步代码。

**Gotcha:** 你可以通过[这篇文章](https://www.freecodecamp.org/news/synchronous-vs-asynchronous-in-javascript/)通过 [TAPAS ADHIKARY](https://www.freecodecamp.org/news/author/tapas/) 了解 JavaScript 中同步和异步是什么意思。

承诺是一个对象，它返回一个你预期将来会看到但现在看不到的值。

承诺的一个实际应用是在 HTTP 请求中，您提交了一个请求，但没有立即收到响应，因为这是一个异步活动。只有当服务器响应时，您才会收到答案(数据或错误)。

JavaScript promise 语法:

```
const myPromise = new Promise((resolve, reject) => {  
    // condition
});
```

承诺有两个参数，一个表示成功(解决)，一个表示失败(拒绝)。每个承诺都有一个条件，必须满足该条件，才能兑现承诺，否则承诺将被拒绝:

```
const promise = new Promise((resolve, reject) => {  
    let condition;

    if(condition is met) {    
        resolve('Promise is resolved successfully.');  
    } else {    
        reject('Promise is rejected');  
    }
});
```

承诺对象有 3 种状态:

*   **待定:**默认情况下，这是初始状态，在承诺成功或失败之前。
*   **已解决:**已完成承诺
*   **拒绝:**承诺失败

最后，让我们尝试将回调地狱重新实现为一个承诺:

```
function addName (time, name){
  return new Promise ((resolve, reject) => {
    if(name){
      setTimeout(()=>{
        console.log(name)
        resolve();
      },time)
    }else{
      reject('No such name');
    }
  })
}

addName(2000, 'Joel')
  .then(()=>addName(2000, 'Victoria'))
  .then(()=>addName(2000, 'John'))
  .then(()=>addName(2000, 'Doe'))
  .then(()=>addName(2000, 'Sarah'))
  .catch((err)=>console.log(err))
```

你可以通过 [Cem Eygi](https://www.freecodecamp.org/news/author/cemeygi/) 的[这篇文章](https://www.freecodecamp.org/news/javascript-es6-promises-for-beginners-resolve-reject-and-chaining-explained/)来更好的理解承诺。

## JavaScript 中的 Map()

最常用的方法之一是`Array.map()`，它允许你迭代一个数组并使用回调函数修改它的元素。回调函数将在每个数组元素上运行。

假设我们有一个包含用户信息的用户数组。

```
let users = [
  { firstName: "Susan", lastName: "Steward", age: 14, hobby: "Singing" },
  { firstName: "Daniel", lastName: "Longbottom", age: 16, hobby: "Football" },
  { firstName: "Jacob", lastName: "Black", age: 15, hobby: "Singing" }
];
```

我们可以使用 map 循环并修改它的输出

```
let singleUser = users.map((user)=>{
  //let's add the firstname and lastname together
  let fullName = user.firstName + ' ' + user.lastName;
  return `
    <h3 class='name'>${fullName}</h3>
    <p class="age">${user.age}</p>
  `
});
```

您应该注意到:

*   总是返回一个新数组，即使它是一个空数组。
*   与 filter 方法相比，它不会改变原始数组的大小
*   当创建新数组时，它总是利用原始数组中的值。

**明白:**map 方法的工作方式几乎和其他 JavaScript 迭代器一样，比如`forEach()`，但是当你打算**返回**一个值时，总是使用 map 方法是合适的。

![image-83](img/b997402eeab6b2ae55ca76ceb492d35a.png)

Here is a perfect description by [Simon Høiberg](https://www.linkedin.com/in/simonhoiberg?miniProfileUrn=urn%3Ali%3Afs_miniProfile%3AACoAAB5jWWUBOCaKeVgc2EIi88ksmqZBpvsi930&lipi=urn%3Ali%3Apage%3Ad_flagship3_detail_base%3BT8SFpAr6QJeDVQ2s0XRfgg%3D%3D&licu=urn%3Ali%3Acontrol%3Ad_flagship3_detail_base-actor_container&lici=cWhtZHO%2BTnmbu6ZNSGKubw%3D%3D)

我们使用 map 的一个重要原因是，我们可以将数据封装在一些 HTML 中，而对于 React 来说，使用 JSX 就可以做到这一点。

你可以在这里阅读更多关于 map() [的内容。](https://www.freecodecamp.org/news/javascript-map-how-to-use-the-js-map-function-array-method/)

## JavaScript 中的 Filter()和 Find()

`Filter()`根据特定标准提供新数组。与 map()不同，它可以改变新数组的大小，而`find()`只返回一个实例(这可能是一个对象或项目)。如果存在多个匹配项，则返回第一个匹配项，否则返回 undefined。

假设您有一个不同年龄注册用户的数组集合:

```
let users = [
  { firstName: "Susan", age: 14 },
  { firstName: "Daniel", age: 16 },
  { firstName: "Bruno", age: 56 },
  { firstName: "Jacob", age: 15 },
  { firstName: "Sam", age: 64 },
  { firstName: "Dave", age: 56 },
  { firstName: "Neils", age: 65 }
];
```

您可以选择按年龄组对这些数据进行排序，比如年轻人(1-15 岁)、老年人(50-70 岁)等等...

在这种情况下，filter 函数会派上用场，因为它会根据标准生成一个新数组。让我们来看看它是如何工作的。

```
// for young people
const youngPeople = users.filter((person) => {
  return person.age <= 15;
});

//for senior people
const seniorPeople = users.filter((person) => person.age >= 50);

console.log(seniorPeople);
console.log(youngPeople); 
```

这会生成一个新数组。如果条件不满足(不匹配)，它将生成一个空数组。

你可以在这里阅读更多关于这个[的内容。](https://www.freecodecamp.org/news/javascript-array-filter-tutorial-how-to-iterate-through-elements-in-an-array/)

### 查找()

与`filter()`方法一样，`find()`方法遍历数组，寻找满足指定条件的实例/项目。一旦找到它，它就返回那个特定的数组项，并立即终止循环。如果没有发现匹配，函数返回 undefined。

**例如:**

```
const Bruno = users.find((person) => person.firstName === "Bruno");

console.log(Bruno);
```

你可以在这里阅读更多关于 find()方法的内容。

## 在 JavaScript 中析构数组和对象

析构是 ES6 中引入的一个 JavaScript 特性，允许更快更简单地从数组和对象中访问和解包变量。

在引入析构之前，如果我们有一个水果数组，并想分别得到第一个、第二个和第三个水果，我们最终会得到这样的结果:

```
let fruits= ["Mango", "Pineapple" , "Orange", "Lemon", "Apple"];

let fruit1 = fruits[0];
let fruit2 = fruits[1];
let fruit3 = fruits[2];

console.log(fruit1, fruit2, fruit3); //"Mango" "Pineapple" "Orange"
```

这就像一遍又一遍地重复同样的事情，可能会变得很麻烦。让我们来看看如何分解它来得到前三个果实。

```
let [fruit1, fruit2, fruit3] = fruits;

console.log(fruit1, fruit2, fruit3); //"Mango" "Pineapple" "Orange"
```

您可能想知道，如果您只想打印第一个和最后一个结果，或者第二个和第四个结果，如何跳过数据。您可以使用逗号，如下所示:

```
const [fruit1 ,,,, fruit5] = fruits;
const [,fruit2 ,, fruit4,] = fruits;
```

### 对象析构

现在让我们看看如何去构造一个对象——因为在 React 中，你将会做很多对象的构造。

假设我们有一个 user 对象，其中包含他们的名字、姓氏等等，

```
const Susan = {
  firstName: "Susan",
  lastName: "Steward",
  age: 14,
  hobbies: {
    hobby1: "singing",
    hobby2: "dancing"
  }
};
```

在过去，获取这些数据可能会很有压力，而且充满了重复:

```
const firstName = Susan.firstName;
const age = Susan.age;
const hobby1 = Susan.hobbies.hobby1;

console.log(firstName, age, hobby1); //"Susan" 14 "singing"
```

但是有了析构就容易多了:

```
const {firstName, age, hobbies:{hobby1}} = Susan;

console.log(firstName, age, hobby1); //"Susan" 14 "singing"
```

我们也可以在函数中这样做:

```
function individualData({firstName, age, hobbies:{hobby1}}){
  console.log(firstName, age, hobby1); //"Susan" 14 "singing"
}
individualData(Susan);
```

你可以在这里阅读更多关于析构数组和对象[的内容。](https://www.freecodecamp.org/news/array-and-object-destructuring-in-javascript/)

## JavaScript 中的 Rest 和 Spread 运算符

JavaScript spread 和 rest 操作符使用三个点`...`。rest 操作符收集项目——它将一些特定用户提供的值的“剩余部分”放入一个 JavaScript 数组/对象中。

假设你有一系列水果:

```
let fruits= ["Mango", "Pineapple" , "Orange", "Lemon", "Apple"];
```

我们可以通过析构来获取第一个和第二个水果，然后通过使用 rest 操作符将水果的“其余部分”放在一个数组中。

```
const [firstFruit, secondFruit, ...rest] = fruits

console.log(firstFruit, secondFruit, rest); //"Mango" "Pineapple" ["Orange","Lemon","Apple"]
```

看看结果，你会看到前两项，然后第三项是一个数组，由我们没有析构的剩余水果组成。我们现在可以对新生成的数组进行任何类型的处理，例如:

```
const chosenFruit = rest.find((fruit) => fruit === "Apple");

console.log(`This is an ${chosenFruit}`); //"This is an Apple"
```

重要的是要记住，这必须始终放在最后(位置非常重要)。

我们刚刚处理了数组——现在让我们处理对象，它们是完全相同的。

假设我们有一个用户对象，它包含他们的名字、姓氏以及更多信息。我们可以破坏它，然后提取剩余的数据。

```
const Susan = {
  firstName: "Susan",
  lastName: "Steward",
  age: 14,
  hobbies: {
    hobby1: "singing",
    hobby2: "dancing"
  }
};

const {age, ...rest} = Susan;
console.log(age, rest);
```

这将记录以下结果:

```
14
{
firstName: "Susan" ,
lastName: "Steward" ,
hobbies: {...}
}
```

现在让我们了解一下 spread 运算符是如何工作的，最后通过区分这两种运算符进行总结。

### 传播算子

顾名思义，spread 运算符用于展开数组项。它给了我们从数组中获取参数列表的能力。spread 运算符的语法与 rest 运算符相似，只是它的运算方向相反。

**注意:**扩展操作符只有在数组文字、函数调用或初始化的属性对象中使用时才有效。

例如，假设你有不同种类动物的数组:

```
let pets= ["cat", "dog" , "rabbits"];

let carnivorous = ["lion", "wolf", "leopard", "tiger"];
```

您可能希望将这两个数组合并成一个动物数组。让我们试一试:

```
let animals = [pets, carnivorous];

console.log(animals); //[["cat", "dog" , "rabbits"], ["lion", "wolf", "leopard", "tiger"]]
```

这不是我们想要的——我们希望所有的项目都在一个数组中。我们可以使用 spread 运算符来实现这一点:

```
let animals = [...pets, ...carnivorous];

console.log(animals); //["cat", "dog" , "rabbits", "lion", "wolf", "leopard", "tiger"]
```

这也适用于对象。值得注意的是，spread 运算符不能扩展对象文本的值，因为 properties 对象不是 iterable 对象。但是我们可以用它将一个对象的属性克隆到另一个对象中。

例如:

```
let name = {firstName:"John", lastName:"Doe"};
let hobbies = { hobby1: "singing", hobby2: "dancing" }
let myInfo = {...name, ...hobbies};

console.log(myInfo); //{firstName:"John", lastName:"Doe", hobby1: "singing", hobby2: "dancing"}
```

你可以在这里阅读更多关于 JavaScript Spread 和 Rest 操作符的内容。

## JavaScript 中的唯一值集()

最近，我试图为一个应用程序创建一个 categories 选项卡，我需要从一个数组中获取 categories 值。

```
let animals = [
  {
    name:'Lion',
    category: 'carnivore'
  },
  {
    name:'dog',
    category:'pet'
  },
  {
    name:'cat',
    category:'pet'
  },
  {
    name:'wolf',
    category:'carnivore'
  }
]
```

第一件事是遍历数组，但我得到了重复的值:

```
let category = animals.map((animal)=>animal.category);
console.log(category); //["carnivore" , "pet" , "pet" , "carnivore"]
```

这意味着我需要建立一个条件来避免重复。直到我遇到 ES6 提供的`set()`构造函数/对象之前，事情有点棘手。

集合是唯一的项目的集合，也就是说没有任何元素可以重复。让我们看看如何轻松实现这一点。

```
//wrap your iteration in the set method like this
let category = [...new Set(animals.map((animal)=>animal.category))];

console.log(category); ////["carnivore" , "pet"]
```

**NB:** 我决定把数值分散到一个数组里。你可以在这里阅读更多关于独特价值[的内容。](https://www.geeksforgeeks.org/sets-in-javascript/)

## JavaScript 中的动态对象键

这使我们能够使用方括号符号添加对象键。这可能现在对你来说没有意义，但是随着你继续学习 React 或者开始与团队合作，你可能会遇到它。

在 JavaScript 中，我们知道对象通常由属性/键和值组成，我们可以使用点符号来添加、编辑或访问一些值。举个例子:

```
let lion = {
  category: "carnivore"
};

console.log(lion); // { category: "carnivore" }
lion.baby = 'cub';
console.log(lion.category); // carnivore
console.log(lion); // { category: "carnivore" , baby: "cub" }
```

我们也可以选择使用方括号符号，当我们需要**动态对象键时就可以使用。**

我们所说的动态对象键是什么意思？这些键可能不遵循对象中属性/键的标准命名约定。[标准命名约定](https://stackoverflow.com/questions/55413572/what-is-the-standard-naming-convention-of-properties-in-a-object-camelcase-or-s)只允许 camelCase 和 snake_case，但是通过使用方括号符号我们可以解决这个问题。

例如，假设我们用单词间的破折号来命名我们的键，例如(`lion-baby`):

```
let lion = {
  'lion-baby' : "cub"
};

// dot notation
console.log(lion.lion-baby); // error: ReferenceError: baby is not defined
// bracket notation
console.log(lion['lion-baby']); // "cub"
```

您可以看到点符号和括号符号之间的区别。让我们看看其他例子:

```
let category = 'carnivore';
let lion = {
  'lion-baby' : "cub",
  [category] : true,
};

console.log(lion); // { lion-baby: "cub" , carnivore: true }
```

您还可以通过使用方括号中的条件来执行更复杂的操作，如下所示:

```
const number = 5;
const gavebirth = true;

let animal = {
  name: 'lion',
  age: 6,
  [gavebirth && 'babies']: number
};

console.log(animal); // { name: "lion" , age: 6 , babies: 5 }
```

你可以在这里阅读更多关于这个[的内容。](https://hackmamba.io/blog/2020/11/dynamic-javascript-object-keys/)

## JavaScript 中的 reduce()

这可以说是最强大的数组函数。它可以取代`filter()`和`find()`方法，在对大量数据使用`map()`和`filter()`方法时也非常方便。

当您将 map 和 filter 方法链接在一起时，您最终要做两次工作——首先过滤每个值，然后映射其余的值。另一方面，`reduce()`允许您在一次操作中进行过滤和映射。这种方法很强大，但是也有点复杂和棘手。

我们迭代数组，然后获得一个回调函数，类似于`map()`、`filter()`、`find()`等等。主要的区别是，它将我们的数组简化为一个值，这个值可以是一个数字、数组或对象。

关于 reduce()方法要记住的另一件事是，我们传入两个参数，自从您开始阅读本教程以来，情况就不是这样了。

第一个参数是所有计算的总和，第二个参数是当前迭代值(您很快就会明白)。

例如，假设我们有一份员工的工资清单:

```
let staffs = [
  { name: "Susan", age: 14, salary: 100 },
  { name: "Daniel", age: 16, salary: 120 },
  { name: "Bruno", age: 56, salary: 400 },
  { name: "Jacob", age: 15, salary: 110 },
  { name: "Sam", age: 64, salary: 500 },
  { name: "Dave", age: 56, salary: 380 },
  { name: "Neils", age: 65, salary: 540 }
];
```

我们想为所有员工计算 10%的什一税。我们可以用 reduce 方法很容易地做到这一点，但在此之前，让我们做一些更简单的事情:让我们先计算总工资。

```
const totalSalary = staffs.reduce((total, staff) => {
  total += staff.salary;
  return total;
},0)
console.log(totalSalary); // 2150
```

注意:我们传递了第二个参数，它是总数，可以是任何东西，比如一个数字或者一个对象。

现在让我们计算所有职员的 10%什一税，并得到总数。我们可以从总数中提取 10%,或者先从每份工资中提取，然后再加起来。

```
const salaryInfo = staffs.reduce(
  (total, staff) => {
    let staffTithe = staff.salary * 0.1;
    total.totalTithe += staffTithe;
    total['totalSalary'] += staff.salary;
    return total;
  },
  { totalSalary: 0, totalTithe: 0 }
);

console.log(salaryInfo); // { totalSalary: 2150 , totalTithe: 215 }
```

**明白:**我们使用了一个对象作为第二个参数，还使用了动态对象键。你可以在这里阅读更多关于 reduce 方法[的内容。](https://www.freecodecamp.org/news/reduce-f47a7da511a9/)

## JavaScript 中的可选链接

可选链接是在 JavaScript 中访问嵌套对象属性的一种安全方式，而不是在访问一长串对象属性时进行多次空检查。这是 ES2020 中引入的新功能。

例如:

```
let users = [
{
    name: "Sam",
    age: 64,
    hobby: "cooking",
    hobbies: {
      hobb1: "cooking",
      hobby2: "sleeping"
    }
  },
  { name: "Bruno", age: 56 },
  { name: "Dave", age: 56, hobby: "Football" },
  {
    name: "Jacob",
    age: 65,
    hobbies: {
      hobb1: "driving",
      hobby2: "sleeping"
    }
  }
];
```

假设你正试图从上面的数组中获取爱好。让我们试一试:

```
users.forEach((user) => {
  console.log(user.hobbies.hobby2);
});
```

当你查看你的控制台时，你会注意到第一次迭代已经完成，但是第二次迭代没有爱好。所以它不得不抛出一个错误并中断迭代——这意味着它不能从数组中的其他对象获取数据。

**输出:**

```
"sleeping"
error: Uncaught TypeError: user.hobbies is undefined
```

这个错误可以用可选的链接来修复，尽管有几种方法可以修复它(例如，使用条件)。让我们看看如何使用条件和可选链接来实现这一点:

### 条件渲染方法:

```
users.forEach((user) => {
  console.log(user.hobbies && user.hobbies.hobby2);
});
```

### 可选链接:

```
users.forEach((user) => {
  console.log(user ?.hobbies ?.hobby2);
});
```

输出:

```
"sleeping"
undefined
undefined
"sleeping"
```

这可能现在对你来说没有意义，但是当你在未来做更大的事情时，它会变得有意义！这里可以阅读更多[。](https://www.freecodecamp.org/news/javascript-optional-chaining-explained/)

## 获取 JavaScript 中的 API 和错误

fetch API，顾名思义，是用来从 API 获取数据的。它是一个浏览器 API，允许您使用 JavaScript 发出基本的 AJAX(异步 JavaScript 和 XML)请求。

因为它是由浏览器提供的，所以您可以使用它，而不必安装或导入任何软件包或依赖项(如 axios)。它的配置很容易掌握。默认情况下，fetch API 提供一个承诺(我在本文前面已经介绍过承诺)。

让我们看看如何通过 fetch API 获取数据。我们将使用一个免费的 API，其中包含数千个随机引用:

```
fetch("https://type.fit/api/quotes")
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((err) => console.log(err));
```

我们在这里做的是:

*   **第 1 行:**我们从 API 获得数据，它返回一个承诺
*   **第 2 行:**然后我们得到了`.json()`格式的数据，这也是一个承诺
*   第 3 行:我们得到了现在返回 JSON 的数据
*   第 4 行:我们得到了错误，以防有任何错误

我们将在下一节看到如何用 async/await 实现这一点。你可以在这里阅读更多关于获取 API 的信息。

### 如何处理 Fetch API 中的错误

现在让我们看看如何在不依赖 catch 关键字的情况下处理来自 fetch API 的错误。对于网络错误，`fetch()`函数会自动抛出一个错误，但对于 HTTP 错误，如 400 到 5xx 响应，则不会。

好消息是`fetch`提供了一个简单的`response.ok`标志，指示请求是否失败或者 HTTP 响应的状态代码是否在成功范围内。

这很容易实现:

```
fetch("https://type.fit/api/quotes")
  .then((response) => {
    if (!response.ok) {
      throw Error(response.statusText);
    }
    return response.json();
  })
  .then((data) => console.log(data))
  .catch((err) => console.log(err));
```

你可以在这里阅读更多关于获取 API 错误的信息。

## JavaScript 中的异步/等待

Async/Await 允许我们以同步的方式编写异步代码。这意味着你不需要继续嵌套回调。

异步函数**总是**返回一个承诺。

您可能绞尽脑汁想知道同步和异步的区别。简单地说，同步意味着作业一个接一个地完成。异步意味着任务独立完成。

注意，我们总是在函数前面有 async，并且我们只能在有 async 时使用 await。你很快就会明白的！

现在让我们使用 async/await 来实现我们之前处理过的 Fetch API 代码:

```
const fetchData = async () =>{
  const quotes = await fetch("https://type.fit/api/quotes");
  const response = await quotes.json();
  console.log(response);
}

fetchData();
```

这更容易阅读，对吗？

您可能想知道我们如何用 async/await 处理错误。没错。您可以使用 try 和 catch 关键字:

```
const fetchData = async () => {
  try {
    const quotes = await fetch("https://type.fit/api/quotes");
    const response = await quotes.json();
    console.log(response);
  } catch (error) {
    console.log(error);
  }
};

fetchData();
```

你可以在这里阅读更多关于异步/等待的信息。

## 结论

在本文中，我们已经学习了 10 多个 JavaScript 方法和概念，每个人在学习 React 之前都应该彻底理解它们。

你应该知道很多其他的方法和概念，但是这些可能是你在学习 JavaScript 时没有真正注意到的。在你学习反应之前，了解这些是很重要的。

假设你刚刚开始学习 JavaScript——我已经整理了一个很棒的资源列表，可以帮助你学习 JavaScript 概念和主题[这里](https://github.com/olawanlejoel/Awesome-Javascript)。不要忘记明星和分享！:).