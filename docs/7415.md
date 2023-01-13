# JavaScript 对象、方括号和算法

> 原文：<https://www.freecodecamp.org/news/javascript-objects-square-brackets-and-algorithms-e9a2916dc158/>

作者:德米特里·格拉博夫

# JavaScript 对象、方括号和算法

![Rj27VVmEt699nVSwP3J30foFIDCxaCVMwePu](img/e118800934adf7541174f9cc78231372.png)

JavaScript 最强大的方面之一是能够动态引用对象的属性。在这篇文章中，我们将看看这是如何工作的，以及这会给我们带来什么好处。我们将快速浏览一下计算机科学中使用的一些数据结构。此外，我们还会看到一种叫做大 O 符号的东西，它用来描述算法的性能。

### 对象介绍

让我们从创建一个代表汽车的简单对象开始。每个对象都有一个名为`properties`的东西。属性是属于对象的变量。我们的汽车对象将有三个属性:`make`、`model`和`color`。

让我们看看它会是什么样子:

```
const car = {  make: 'Ford',  model: 'Fiesta',  color: 'Red'};
```

我们可以用点符号来表示一个物体的各个属性。例如，如果我们想知道我们的汽车是什么颜色，我们可以像这样使用点符号`car.color`。

我们甚至可以使用`console.log`输出它:

```
console.log(car.color); //outputs: Red
```

引用属性的另一种方式是使用方括号符号:

```
console.log(car['color']); //outputs: Red
```

在上面的例子中，我们使用属性名作为方括号内的字符串来获取对应于该属性名的值。方括号符号的有用之处在于，我们还可以使用变量来动态获取属性。

也就是说，我们可以将其指定为变量中的字符串，而不是硬编码特定的属性名:

```
const propertyName = 'color';const console.log(car[propertyName]); //outputs: Red
```

### 使用带方括号符号的动态查找

让我们来看一个我们可以使用它的例子。假设我们经营一家餐馆，我们希望能够获得菜单上项目的价格。一种方法是使用`if/else`语句。

让我们编写一个接受商品名称并返回价格的函数:

```
function getPrice(itemName){  if(itemName === 'burger') {    return 10;  } else if(itemName === 'fries') {    return 3;  } else if(itemName === 'coleslaw') {    return 4;  } else if(itemName === 'coke') {    return 2;  } else if(itemName === 'beer') {    return 5;  }}
```

虽然上述方法可行，但并不理想。我们已经在代码中硬编码了菜单。现在，如果我们的菜单发生变化，我们将不得不重写代码并重新部署它。此外，我们可能会有一个很长的菜单，而必须编写所有这些代码将会很麻烦。

更好的方法是将我们的数据和逻辑分开。数据将包含我们的菜单，逻辑将从菜单中查找价格。

我们可以将`menu`表示为一个对象，其中属性名(也称为键)对应于一个值。

在这种情况下，关键字是商品名称，值是商品价格:

```
const menu = {  burger: 10,  fries: 3,  coleslaw: 4,  coke: 2,  beer: 5};
```

使用方括号符号，我们可以创建一个接受两个参数的函数:

*   菜单对象
*   带有项目名称的字符串

并返回该项目的价格:

```
const menu = {  burger: 10,  fries: 3,  coleslaw: 4,  coke: 2,  beer: 5};
```

```
function getPrice(itemName, menu){  const itemPrice = menu[itemName];  return itemPrice;}
```

```
const priceOfBurger = getPrice('burger', menu);console.log(priceOfBurger); // outputs: 10
```

这种方法的巧妙之处在于，我们将数据与逻辑分离开来。在这个例子中，数据存在于我们的代码中，但是它也可以很容易地来自数据库或 API。它不再与我们将商品名称转换为商品价格的查找逻辑紧密耦合。

### 数据结构和算法

在计算机科学术语中，映射是一种数据结构，它是键/值对的集合，其中每个键映射到相应的值。我们可以用它来查找对应于特定键的值。这就是我们在前面的例子中所做的。我们有一个键，它是一个项目名称，我们可以使用菜单对象查找该项目的相应价格。我们使用一个对象来实现一个地图数据结构。

让我们看一个例子，为什么我们可能要使用地图。假设我们经营一家书店，有一份书单。每本书都有一个唯一的标识符，称为国际标准书号(ISBN)，是一个 13 位数字。我们将书籍存储在一个数组中，并希望能够使用 ISBN 来查找它们。

一种方法是遍历数组，检查每本书的 ISBN 值，如果匹配就返回:

```
const books = [{  isbn: '978-0099540946',  author: 'Mikhail Bulgakov',  title: 'Master and Margarita'}, {  isbn: '978-0596517748',  author: 'Douglas Crockford',  title: 'JavaScript: The Good Parts'}, {  isbn: '978-1593275846',  author: 'Marijn Haverbeke',  title: 'Eloquent JavaScript'}];
```

```
function getBookByIsbn(isbn, books){  for(let i = 0; i < books.length; i++){    if(books[i].isbn === isbn) {      return books[i];    }  }}
```

```
const myBook = getBookByIsbn('978-1593275846', books);
```

这在本例中很好，因为我们只有三本书(这是一家小书店)。然而，如果我们是亚马逊，迭代数百万本书可能会非常慢，并且计算量很大。

[大 O 符号](https://rob-bell.net/2009/06/a-beginners-guide-to-big-o-notation/)在计算机科学中用于描述算法的性能。例如，如果 *n* 是我们收藏的书籍数量，那么在最坏的情况下使用迭代查找一本书的成本(我们查找的书在列表中是最后一本书)将是 *O(n)* 。这意味着如果我们收藏的书籍数量增加一倍，使用迭代查找一本书的成本也会增加一倍。

让我们看看如何通过使用不同的数据结构使我们的算法更有效。

如前所述，映射可用于查找对应于某个键的值。我们可以通过使用对象将数据组织成 map 而不是数组。

键是 ISBN，值是相应的 book 对象:

```
const books = {  '978-0099540946':{    isbn: '978-0099540946',    author: 'Mikhail Bulgakov',    title: 'Master and Margarita'  },  '978-0596517748': {    isbn: '978-0596517748',    author: 'Douglas Crockford',    title: 'JavaScript: The Good Parts'  },  '978-1593275846': {    isbn: '978-1593275846',    author: 'Marijn Haverbeke',    title: 'Eloquent JavaScript'  }};
```

```
function getBookByIsbn(isbn, books){  return books[isbn];}
```

```
const myBook = getBookByIsbn('978-1593275846', books);
```

我们现在可以通过 ISBN 使用简单的地图查找来获得我们的值，而不是使用迭代。我们不再需要检查每个对象的 ISBN 值。我们使用键直接从地图中获取值。

就性能而言，映射查找将提供比迭代更大的改进。这是因为映射查找在计算方面具有恒定的成本。这可以用大 O 符号写成 *O(1)* 。不管我们有 300 万还是 300 万本书，通过使用 ISBN 键进行地图查找，我们都可以很快找到我们想要的书。

### 概述

*   我们已经看到，我们可以使用点符号和方括号符号来访问对象属性的值
*   我们学习了如何通过使用带方括号符号的变量来动态查找属性值
*   我们还学习了映射数据结构将键映射到值。我们可以使用键直接在一个映射中查找值，这个映射是使用一个对象实现的。
*   我们已经初步了解了如何使用 *Big O* 符号来描述算法性能。此外，我们还看到了如何通过将对象数组转换为映射并使用直接查找而不是迭代来提高搜索性能。

想测试你新发现的技能吗？试试 Codewars 上的[崩溃覆盖](https://www.codewars.com/kata/crash-override/javascript)练习。

想学习如何使用 JavaScript 编写 web 应用程序吗？我在伦敦运营[构造器实验室](http://constructorlabs.com/)，这是一个为期 12 周的 [JavaScript 编码训练营](http://constructorlabs.com/course)。传授的技术有 **HMTL** 、 **CSS** 、 **JavaScript** 、 **React** 、 **Redux** 、 **Node** 和 **Postgres** 。从头开始创建一个完整的 web 应用程序并获得业内第一份工作所需的一切。费用为 3000 英镑，下一批将于 5 月 29 日开始。[申请现已开放](http://constructorlabs.com/admission)。