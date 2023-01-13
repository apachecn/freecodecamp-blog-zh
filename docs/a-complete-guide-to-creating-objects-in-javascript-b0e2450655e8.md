# 如何用 JavaScript 创建对象

> 原文：<https://www.freecodecamp.org/news/a-complete-guide-to-creating-objects-in-javascript-b0e2450655e8/>

凯山·侯赛因

在用编程语言编写代码时，我们都以这样或那样的方式处理对象。在 JavaScript 中，对象为我们提供了一种通过网络存储、操作和发送数据的方式。

JavaScript 中的对象与其他主流编程语言(如 Java)中的对象有许多不同之处。我会试着在另一个话题中讨论这个问题。这里，让我们只关注 JavaScript 允许我们创建对象的各种方式。

在 JavaScript 中，将对象视为“键:值”对的集合。这给我们带来了用 JavaScript 创建对象的第一种也是最流行的方式。

我们开始吧。

#### 1.使用对象文字语法创建对象

这真的很简单。您所要做的就是将由“:”分隔的键值对放在一组花括号({ })中，您的对象就可以被提供(或使用)了，如下所示:

```
const person = {
  firstName: 'testFirstName',
  lastName: 'testLastName'
};
```

这是用 JavaScript 创建对象的最简单也是最流行的方式。

#### 2.使用“new”关键字创建对象

这种对象创建方法类似于基于类的语言(如 Java)中创建对象的方式。顺便说一下，从 ES6 开始，类也是 JavaScript 的原生类，在本文末尾，我们将通过定义类来创建对象。因此，要使用“new”关键字创建一个对象，需要有一个构造函数。

这里有两种使用“新”关键字模式的方法

**a)使用带有‘内置对象构造函数’的‘new’关键字**

要创建一个对象，使用 new 关键字和`Object()`构造函数，如下所示:

```
const person = new Object();
```

现在，要给这个对象添加属性，我们必须这样做:

```
person.firstName = 'testFirstName';
person.lastName = 'testLastName';
```

您可能已经意识到这个方法的输入时间有点长。此外，不推荐这种做法，因为有一个作用域解析发生在后台，用于确定构造函数是内置的还是用户定义的。

**b)通过用户定义的构造函数**使用“new”

使用“Object”构造函数的另一个问题是，每次我们创建一个对象时，都必须手动将属性添加到创建的对象中。

如果我们必须创建数百个人对象会怎么样？你可以想象现在的痛苦。因此，为了摆脱手动向对象添加属性，我们创建了自定义(或用户定义)函数。我们首先创建一个构造函数，然后使用“new”关键字来获取对象:

```
function Person(fname, lname) {
  this.firstName = fname;
  this.lastName = lname;
}
```

现在，任何时候你想要一个“人”的对象，只要这样做:

```
const personOne = new Person('testFirstNameOne', 'testLastNameOne');
const personTwo = new Person('testFirstNameTwo', 'testLastNameTwo');
```

#### 3.使用 Object.create()创建新对象

当我们被要求从其他现有对象创建对象，而不是直接使用“new”关键字语法时，这种模式非常方便。让我们看看如何使用这个模式。如 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) 上所述:

> `**Object.create()**`方法创建一个新对象，使用一个现有对象作为新创建对象的原型。

要理解`**Object.create**`方法，只需记住它需要两个参数。第一个参数是一个强制对象，作为要创建的新对象的原型。第二个参数是一个可选对象，它包含要添加到新对象中的属性。

我们现在不会深入研究原型和继承链，而是把注意力集中在这个主题上。但是作为一个要点，你可以把原型看作是其他对象可以借用它们需要的属性/方法的对象。

假设您有一个由`orgObject`表示的组织

```
const orgObject = { company: 'ABC Corp' };
```

并且您希望为该组织创建员工。显然，您想要所有的雇员对象。

```
const employee = Object.create(orgObject, { name: { value: 'EmployeeOne' } });

console.log(employee); // { company: "ABC Corp" }
console.log(employee.name); // "EmployeeOne"
```

#### 4.使用 Object.assign()创建新对象

如果我们想要创建一个需要拥有多个对象的属性的对象呢？来帮助我们。

如 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 上所述:

> `Object**.**assign**()**`方法用于将所有可枚举的自身属性的值从一个或多个源对象复制到一个目标对象。它将返回目标对象。

`Object***.***assign`方法可以接受任意数量的对象作为参数。第一个参数是它将创建并返回的对象。传递给它的其余对象将用于将属性复制到新对象中。让我们通过扩展前面看到的例子来理解它。

假设您有如下两个对象:

```
const orgObject = { company: 'ABC Corp' }
const carObject = { carName: 'Ford' }
```

现在，您想要一个驾驶“福特”汽车的“ABC 公司”的雇员对象。您可以在如下`Object.assign`的帮助下完成:

`const employee = Object.assign({}, orgObject, carObject);`

现在，你得到一个属性为`company`和`carName`的`employee`对象。

```
console.log(employee); // { carName: "Ford", company: "ABC Corp" }
```

#### 5.使用 ES6 类创建对象

你会注意到这个方法类似于使用' new '和用户定义的构造函数。构造函数现在被类所取代，因为它们受到 ES6 规范的支持。现在让我们看看代码。

```
class Person {
  constructor(fname, lname) {
    this.firstName = fname;
    this.lastName = lname;
  }
}

const person = new Person('testFirstName', 'testLastName');

console.log(person.firstName); // testFirstName
console.log(person.lastName); // testLastName 
```

这些都是我所知道的用 JavaScript 创建对象的方法。我希望你喜欢这篇文章，现在明白如何创建对象。

感谢您花时间阅读本文。如果你喜欢这篇文章，并且对你有帮助，请点击拍手？按钮以示支持。继续多学习！