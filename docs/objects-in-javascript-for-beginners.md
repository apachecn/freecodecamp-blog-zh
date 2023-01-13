# JavaScript 中的对象——初学者指南

> 原文：<https://www.freecodecamp.org/news/objects-in-javascript-for-beginners/>

如果你声明多个变量来保存不同的值，这会使你的程序变得混乱和笨拙。

例如，如果您需要存储 10 个人的三个特征，那么单独声明 30 个变量会使您的程序显得不那么有条理。

因此，您需要一种方法来将具有相似特征的值分组在一起，以使您的代码更具可读性。在 JavaScript 中，对象很适合这个目的。

与其他数据类型不同，对象能够存储复杂的值。正因为如此，JavaScript 非常依赖它们。因此，在深入学习 JavaScript 之前，熟悉什么是对象、如何创建对象以及如何使用对象是很重要的。

本文将向您介绍对象的基础知识、对象语法、创建对象的不同方法、如何复制对象以及如何迭代对象。

为了从本文中获得最大收益，您至少需要对 JavaScript 有一个基本的了解，尤其是变量、函数和数据类型。

## JavaScript 中的对象是什么？

对象是一种可以接受键值对集合的数据类型。

对象与 JavaScript 中的字符串和数字等其他数据类型的主要区别在于，对象可以存储不同类型的数据作为其值。另一方面，数字和字符串等原始数据类型只能分别存储数字和字符串作为它们的值。

键，也称为属性名，通常是一个字符串。如果任何其他数据类型被用作属性名而不是字符串，它将被转换为字符串。

你可以把一个物体想象成一个多用途的架子，里面有放置小配件和装饰品的空间，也有存放书籍和文件的空间。

![shelf1-2](img/5ac5f4812ccb50660f59c57306cc5f62.png)

A shelf with several items in it

对象最容易识别的特征是包含键值对的括号。

```
const emptyObject = {};
console.log(typeof emptyObject); //'object'
```

对象的内容可以由变量、函数或两者组成。对象中的变量是属性，而函数是方法。方法允许对象使用其中的属性来执行某种操作。

例如，在下面的示例代码中， **object1.user** 、 **object1.nationality** 和 **object1.profession** 都是 **object1** 的属性，而 **object1.myBio()** 是一个方法:

```
const object1 = {
    user: "alex",
    nationality: "Nigeria",
    profession: "Software Enginneer",
    myBio() {
        console.log(`My name is ${this.user}. I'm a               ${this.profession} from ${this.nationality}`)
    }
}
console.log(object1.user); //Alex 
console.log(object1.nationality); //Nigeria 
console.log(object1.profession); //Software Engineer 
console.log(object1.myBio()); //My name is alex. I'm a Software Engineer from Nigeria
```

上面示例代码中的关键字是**用户**、**国籍**和**职业**，而它们的值是冒号后面的字符串值。另外，请注意**这个**关键字的使用。**这个**关键字只是指对象本身。

正如本文前面提到的，属性值可以是任何数据类型。在下面的示例代码中，值既是数组又是对象:

```
 const object2 = { 
        users: ["Alex", "James", "Mohammed"], 
        professions: { 
            alex: "software engineer", 
            james: "lawyer", 
            mohammed: "technical writer" 
        } 
    }; 
    console.log(object2.users); //['Alex', 'James', 'Mohammed'] 
    console.log(object2.professions); //{alex: 'software engineer', james: 'lawyer', mohammed: 'technical writer'}
```

## 如何在 JavaScript 中访问对象和创建新的对象属性或方法

有两种方法访问对象:点符号和括号符号。在前面的示例代码中，我们使用点符号来访问**对象 1** 和**对象 2** 中的属性和方法。

此外，要在声明对象后创建新的属性和方法，可以使用点符号或括号符号。你只需要陈述新的属性并给它一个值。

例如，我们可以向**对象 2** 添加新属性，如下所示:

```
object2.ages = [30, 32, 40];
object2["summary"] = `Object2 has ${object2.users.length} users`;
console.log(object2);
/*
{
    "users": [
        "Alex",
        "James",
        "Mohammed"
    ],
    "professions": {
        "alex": "software engineer",
        "james": "lawyer",
        "mohammed": "technical writer"
    },
    "ages": [
        30,
        32,
        40
    ],
    "summary": "Object2 has 3 users"
}
*/
```

同样，您可以使用任何一种符号来更改对象的值:

```
object2.users = ["jane", "Williams", "John"];
object2["age"] = [20, 25, 29]
console.log(object2.users); //['jane', 'Williams', 'John']
console.log(object2.ages) //[20, 25, 29
```

尽管点标记法是最常用的访问对象属性和方法的方法，但它有一些限制。现在让我们来看看它们。

### 不能将值用作带有点符号的属性名

如果你想在你的对象中使用变量值作为属性名，你必须使用括号符号而不是点符号。变量值是否在运行时定义无关紧要。

运行时是计算机程序的一个阶段，其中程序在计算机系统上运行或执行。

例如:

```
const object3 = {};
const gadget = prompt("enter a gadget type"); 
object3[gadget] = ["jbl", "sony"]; 
console.log(object3) //(respose entered in prompt): ["jbl","sony"] notice that the property name is the value you enter in the reply to the prompt message
```

如果在代码中定义变量值，并使用点标记法将该值设置为对象的属性名，点标记法将使用变量名而不是变量值创建一个新属性。

```
const computer = "brands"
object3.computer = ["hp", "dell", "apple"]
console.log(object3.brands); //undefined
console.log(object3.computer)//['hp', 'dell', 'apple']

object3[computer] = ["hp", "dell", "apple"]
console.log(object3.brands) //['hp', 'dell', 'apple']
```

注意方括号中省略了引号。这是因为括号中包含了一个变量。

### 您不能将多词属性与点符号一起使用

如果属性名是一个多单词的字符串，那么点符号是不够的。例如:

```
object3.users height = [5.6, 5.4, 6.0];
Console.log(object3.users height); //SyntaxError
```

出现语法错误是因为 JavaScript 将命令读取为`object3.users`，但字符串高度未被识别，因此它返回一个语法错误。

当使用点符号访问对象时，声明变量的通常规则适用。这意味着，如果要使用点符号来访问对象或创建属性，属性名不能以数字开头，不能包含任何空格，只能包含特殊字符 *$* 和 *_。*

为了避免这种错误，你必须使用括号符号。例如，您可以像这样更正上面的示例代码:

```
object3["users height"] = [5.6, 5.4, 6.0];  
console.log(object3["users height"]); //[5.6, 5.4, 6]
```

## 如何用 JavaScript 中的对象构造函数创建对象

创建对象有两种方法:对象文本和对象构造函数。到目前为止，本文中作为示例使用的对象都是对象文字。如果你想创建一个单独的对象，那么对象文字就很有用。

但是如果你想创建一个以上的对象，最好使用对象构造函数。这使您可以避免代码中不必要的重复，也使更改对象的属性变得更容易。

基本上，构造函数是名字通常大写的函数。构造函数名的大小写对对象没有任何影响。它只是一种识别手段。

您可以通过使用 **new** 关键字调用构造函数来使用构造函数创建新对象。 **new** 关键字将创建一个对象的实例，并将 **this** 关键字绑定到新对象。

正如本文前面提到的， **this** 关键字是对对象本身的引用。

对象构造函数的一个例子是:

```
function Profile(name, age, nationality) { 
    this.name = name; 
    this.age = age; 
    this.nationality = nationality; 
    this.bio = function () { 
        console.log(`My name is ${this.name}. I'm ${this.age} years old. I'm from ${this.nationality}`) 
    } 
};

const oladele = new Profile("Oladele", 50, "Nigeria" );
console.log(oladele.bio()); //My name is Oladele. I'm 50 years old. I'm from Nigeria
```

## 如何在 JavaScript 中创建对象副本

与字符串和数字等原始数据类型不同，将现有对象赋给另一个变量不会产生原始对象的副本，而是在内存中产生一个引用。

这意味着原始对象和通过将原始对象赋值而创建的后续对象都引用内存中的同一项。

这意味着任何一个对象的值的变化都会引起其他对象的变化。例如:

```
let x = 10;
let y = x;
x = 20;
console.log(x); //20
console.log(y); //10

let object4 = { 
    name: "Alex", 
    age: 40 
}; 
let object5 = object4; 
console.log(object5); //{name: 'Alex', age: 40} 
object4.name = "Jane"; 
console.log(object5); //{name: 'Jane', age: 40}
console.log(object4 === object5); //true
```

要创建对象的副本，可以使用“扩展”操作符。

### 什么是传播算子？

展开运算符由三个点 **`...`** 表示。您可以使用 spread 运算符来复制任何 iterable 的值，包括对象。

iterable 是一个可以借助 for 循环或迭代的对象...循环。可迭代的例子包括对象、数组、集合、字符串等等。

要使用 spread 运算符，您必须将其添加到要复制的对象的前缀。例如:

```
let object6 = {...object5}; 
object5.name = "Willaims"; 
console.log(object5); //{name: 'Willaims', age: 40}
console.log(object6); //{name: 'Jane', age: 40}
console.log(object5 === object6); //false
```

如您所见，与前面的代码示例不同，在前面的示例中，**对象 4** 的变化导致了**对象 5** 的变化，**对象 6** 的变化并没有导致**对象 5** 的变化。

### 如何使用 Object.assign()方法

**Object.assign()** 方法将一个对象的所有可枚举属性复制到另一个对象中，然后返回修改后的对象。

该方法接受两个参数。第一个参数是目标对象，它接受复制的属性。第二个参数是源对象，它具有您想要复制的属性。例如:

```
let object7  = Object.assign({}, object6); 
console.log(object7); //{name: 'Jane', age: 40}
console.log(object7); //{name: 'Jane', age: 40}

console.log(object6 === object7); //false
object6.age = 60
console.log(object6); //{name: 'Jane', age: 60}
console.log(object7); //{name: 'Jane', age: 40
```

从上面的示例代码中可以看出， **object6** 的 **age** 属性值的变化并没有导致 **object7** 的 **age** 属性值的变化。

注意，spread 操作符和 **Object.assign()** 方法都只能对一个对象进行浅层复制。

这意味着，如果在源对象中有深度嵌套的对象或数组，那么只有对这些对象的引用才会被复制到目标对象中。因此，任何深度嵌套对象的值的变化都会导致另一个深度嵌套对象的值的变化。例如:

```
let objectX = {
    name: 'Mary', 
    age: 40,
    gadgets: { 
        brand: ["apple", "sony"]
    }
};

let objectY = {...objectX};
objectY.name = "Bianca";
objectY.gadgets.brand[0] = "hp";
console.log(objectX);
/*
{
    "name": "Mary",
    "age": 40,
    "gadgets": {
        "brand": [
            "hp",
            "sony"
        ]
    }
}
*/ 

console.log(objectY);
/*
{
    "name": "Bianca",
    "age": 40,
    "gadgets": {
        "brand": [
            "hp",
            "sony"
        ]
    }
}
*/
```

上述示例代码执行了以下操作:

1.  创建了一个名为 **objectX** 的对象。
2.  给了 **objectX** 三个属性:名字，年龄，小工具。
3.  给了 **objectX** 的 **gadgets** 属性一个对象作为它的值。
4.  给了**小工具**属性的对象值一个**品牌**属性。
5.  给了**品牌**属性一个数组作为它的值。
6.  使用扩展运算符将 **objectX** 中的属性复制到 **objectY** 中。
7.  将**对象**的**名称**属性的值改为**玛丽**。
8.  将**品牌**属性数组值的第一项由**苹果**改为**惠普**。

在示例代码中，数组值是一个深度嵌套的对象。请注意， **objectY** 的 **name** 属性值的变化并没有导致 **objectX** 的 **name** 属性值的变化。但是 **objectY** 的深层嵌套对象的变化导致了 **objectX** 的深层嵌套对象的类似变化。

## 如何在 JavaScript 中迭代对象

使用一个**用于...在**循环中迭代一个对象并选择其属性。**为..在**循环中语法如下:

```
for(let key in object) {
    //perform action(s) for each key
}
```

上面语法中的 **key** 关键字是属性的参数。所以你可以用你选择的任何词来代替它。用要迭代的对象的名称替换 object 关键字。例如:

```
let objectZ = {
    name: "Ade",
    Pronuon: "he",
    age: 60
};
for(let property in objectZ) {
    console.log(`${property}: ${objectZ[property]}`)
}
/* 
name: Ade
Pronuon: he
age: 60
*/
```

请注意，在获取属性值的循环中使用了括号符号。使用点符号代替括号符号将返回 undefined。

## 结论

在 JavaScript 中，对象可能是最重要的数据类型。像面向对象编程这样的编程概念的工作原理是利用对象的灵活性来存储复杂的值，以及它们与对象中的属性和方法进行交互的独特能力。

本文通过解释对象的基础知识，为理解这些高级概念打下了坚实的基础。