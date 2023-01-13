# JavaScript 对象键教程——如何使用 JS 键值对

> 原文：<https://www.freecodecamp.org/news/javascript-object-keys-tutorial-how-to-use-a-js-key-value-pair/>

您可以使用 JavaScript 对象将相关数据组合成一个数据结构，如下所示:

```
const desk = {
   height: "4 feet",
   weight: "30 pounds",
   color: "brown",
   material: "wood",
 }; 
```

对象包含属性或键值对。上面的`desk`对象有四个属性。每个属性都有一个名称(也称为键)和一个对应的值。

例如，关键字`height` 的值为`"4 feet"`。键和值一起构成了一个属性。

```
height: "4 feet", 
```

对象包含一张桌子的数据。事实上，这就是为什么使用 JavaScript 对象的原因:存储数据。检索存储在对象中的数据也很简单。这些方面使得对象非常有用。

本文将帮助您开始使用 JavaScript 对象:

*   如何创建对象
*   如何在对象中存储数据
*   并从中检索数据。

让我们从创建一个对象开始。

# 如何用 JavaScript 创建一个对象

下面我将创建一个名为`pizza`的对象，并向其中添加键值对。

```
const pizza = {
   topping: "cheese",
   sauce: "marinara",
   size: "small"
}; 
```

键在冒号 **`:`** 的左边，值在冒号的右边。每个键值对都是一个`property`。本例中有三个属性:

*   浇头的键**有一个值**“奶酪”**。**
*   键**酱**有一个值**“马里纳拉”**。
*   密钥**大小**有一个值**“小”**。

每个属性由逗号分隔。所有的属性都用花括号括起来。

这是基本的对象语法。但是在创建 JavaScript 对象时，需要记住一些规则。

### JavaScript 中的对象键

JavaScript 对象中的每个键必须是字符串、符号或数字。

仔细看看下面的例子。键名**`1`****`2`**其实是被胁迫成字符串的。

```
const shoppingCart = {
   1: "apple",
   2: "oranges"
}; 
```

当你打印物体时，这种差别就很明显了。

```
console.log(shoppingCart);
//Result: { '1': 'apple', '2': 'oranges' } 
```

关于键名，还有一条规则需要记住:如果键名包含空格，就需要用引号括起来。

看看下面的`programmer`物体。注意最后一个键名，`"current project name"` 。这个键名包含空格，所以我用引号将它括起来。

```
const programmer = {
   firstname: "Phil",
   age: 21,
   backendDeveloper: true,
   languages: ["Python", "JavaScript", "Java", "C++"],
   "current project name": "The Amazing App"
}; 
```

### JavaScript 中的对象值

另一方面，值可以是任何数据类型，包括数组、数字或布尔值。上述示例中的值包含以下类型:字符串、整数、布尔和数组。

你甚至可以使用一个函数作为一个值，在这种情况下，它被称为一个方法。下面的对象中的`sounds()`，就是一个例子。

```
const animal = {
   type: "cat",
   name: "kitty",
   sounds() { console.log("meow meow") }
}; 
```

现在假设您想要添加或删除一个键-值对。或者你只是想检索一个对象的值。

您可以通过使用点或括号符号来做这些事情，我们将在接下来处理。

# 点符号和括号符号在 JavaScript 中如何工作

点符号和括号符号是访问和使用对象属性的两种方式。你可能会发现自己更频繁地使用点符号，所以我们就从这开始吧。

### 如何在 JavaScript 中用点符号添加键值对

下面我将创建一个空的`book` 对象。

```
const book = {}; 
```

要使用点标记法添加键值对，请使用以下语法:

`objectName.keyName = value`

这是将键(作者)和值(“简·史密斯”)添加到`book`对象的代码:

```
book.author = "Jane Smith"; 
```

下面是上述代码的分解:

*   `book`是对象的名称
*   `.`(圆点)
*   `author`是密钥名
*   `=`(等于)
*   `"Jane Smith"`是数值

当我打印 book 对象时，我会看到新添加的键-值对。

```
console.log(book);
//Result: { author: 'Jane Smith' } 
```

我将向`book`对象添加另一个键值对。

```
book.publicationYear = 2006; 
```

`book`对象现在有两个属性。

```
console.log(book);
//Result: { author: 'Jane Smith', publicationYear: 2006 } 
```

### 如何使用点符号访问 JavaScript 对象中的数据

你也可以在键上使用点符号来访问相关的值。

考虑这个`basketballPlayer` 物体。

```
const basketballPlayer = {
   name: "James",
   averagePointsPerGame: 20,
   height: "6 feet, 2 inches",
   position: "shooting guard"
}; 
```

假设您想要检索值“得分后卫”这是要使用的语法:

`objectName.keyName`

让我们使用这个语法来获取和打印“得分后卫”的值。

```
console.log(basketballPlayer.position);
//Result: shooting guard 
```

下面是上述代码的分解:

*   `basketballPlayer`是对象的名称
*   `.`(圆点)
*   `position`是密钥名

这是另一个例子。

```
console.log(basketballPlayer.name);
//Result: James 
```

### 如何在 JavaScript 中删除键值对

要删除一个键值对，使用`delete`操作符。语法如下:

`delete objectName.keyName`

因此，要从`basketballPlayer`对象中删除`height` 键及其值，您应该编写以下代码:

```
delete basketballPlayer.height; 
```

结果，`basketballPlayer`对象现在有三个键值对。

```
console.log(basketballPlayer);
//Result: { name: 'James', averagePointsPerGame: 20, position: 'shooting guard' } 
```

您可能会发现自己经常使用点符号，尽管需要注意某些要求。

使用点符号时，键名不能包含空格、连字符或以数字开头。

例如，假设我试图添加一个包含使用点符号的空格的键。我会得到一个错误。

```
basketballPlayer.shooting percentage = "75%";
//Results in an error 
```

所以点符号并不是在所有情况下都有效。这就是为什么有另一种选择:括号符号。

### 如何在 JavaScript 中使用括号符号添加键值对

就像点符号一样，您可以使用括号符号向对象添加键值对。

括号符号比点符号更灵活。这是因为键名*可以*包含空格和连字符，并且可以以数字开头。

下面我将创建一个`employee` 对象。

```
const employee = {}; 
```

现在我想用括号符号添加一个键值对。这是语法:

`objectName[“keyName”] = value`

这就是我如何将键(职业)和值(销售额)添加到 employee 对象中:

```
employee["occupation"] = "sales"; 
```

下面是上述代码的分解:

*   `employee`是对象的名称
*   `"occupation"`是密钥名
*   `=`(等于)
*   `"sales"`是数值

下面是几个使用括号符号的灵活性来添加各种键值对的例子。

```
//Add multi-word key name
employee["travels frequently"] = true;

//Add key name that starts with a number and includes a hyphen
employee["1st-territory"] = "Chicago";

//Add a key name that starts with a number
employee["25"] = "total customers"; 
```

当我打印`employee`对象时，它看起来像这样:

```
{
  '25': 'total customers',
  occupation: 'sales',
  'travels frequently': true,
  '1st-territory': 'Chicago'
} 
```

记住这些信息，我们可以从上面给`basketballPlayer`对象添加“拍摄百分比”键。

```
const basketballPlayer = {
   name: "James",
   averagePointsPerGame: 20,
   height: "6 feet, 2 inches",
   position: "shooting guard"
}; 
```

您可能还记得，当我们试图添加一个包含空格的键时，点符号给我们留下了一个错误。

```
basketballPlayer.shooting percentage = "75%";
//Results in an error 
```

但是括号符号让我们没有错误，正如你在这里看到的:

```
basketballPlayer["shooting percentage"] = "75%"; 
```

这是我打印对象时的结果:

```
{
  name: 'James',
  averagePointsPerGame: 20,
  height: '6 feet, 2 inches',
  position: 'shooting guard',
  'shooting percentage': '75%'
} 
```

### 如何使用括号符号访问 JavaScript 对象中的数据

你也可以在键上使用括号符号来*访问*相关值。

从文章开头回忆一下`animal`对象。

```
const animal = {
   type: "cat",
   name: "kitty",
   sounds() { console.log("meow meow") }
}; 
```

让我们获取与键`name`相关联的值。为此，请将键名引号括起来并放在括号中。这是语法:

`objectName[“keyName”]`

下面是您用括号符号编写的代码:`animal["name"];`。

这是上述代码的分解:

*   `animal`是对象的名称
*   `["name"]`是方括号中的键名吗

这是另一个例子。

```
console.log(animal["sounds"]());

//Result: 
meow meow
undefined 
```

注意，`sounds()` 是一个方法，这就是为什么我在末尾添加了括号来调用它。

这就是使用点符号调用方法的方式。

```
console.log(animal.sounds());

//Result: 
meow meow
undefined 
```

# JavaScript 对象方法

您知道如何访问特定的属性。但是如果你想从一个对象中得到所有键的*或所有值的*或*呢？*

有两种方法可以给你提供你需要的信息。

```
const runner = {
   name: "Jessica",
   age: 20,
   milesPerWeek: 40,
   race: "marathon"
}; 
```

使用`Object.keys()`方法从一个对象中检索所有的键名。

这是语法:

`Object.keys(objectName)`

我们可以在上面的`runner`对象上使用这个方法。

```
Object.keys(runner); 
```

如果您打印结果，您将得到一个对象键的数组。

```
console.log(Object.keys(runner));
//Result: [ 'name', 'age', 'milesPerWeek', 'race' ] 
```

同样，您也可以使用`Object.values()`方法获取一个对象的所有值。以下是语法:

`Object.values(objectName)`

现在我们将从`runner`对象中获取所有的值。

```
console.log(Object.values(runner));
//Result: [ 'Jessica', 20, 40, 'marathon' ] 
```

我们已经走过了很多地方。以下是主要观点的总结:

**对象:**

*   使用对象将数据存储为属性(键值对)。
*   键名必须是字符串、符号或数字。
*   值可以是任何类型。

**访问对象属性:**

*   点符号:`objectName.keyName`
*   括号符号:`objectName[“keyName”]`

**删除一个属性:**

*   `delete objectName.keyName`

你可以对物品做很多事情。现在您已经掌握了一些基础知识，可以利用这个强大的 JavaScript 数据类型了。

********我在上写了一些关于学习编程以及如何去做的最好方法。**** 我** 也 **关于编程、学习、生产力的推文:****[@ amymhaddad](https://twitter.com/amymhaddad)*******。*