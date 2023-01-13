# 如何在 JavaScript (ES6)中最大限度地使用析构

> 原文：<https://www.freecodecamp.org/news/destructuring-in-javascript-es6-ee963292623a/>

乔安娜·高登

析构是 ES6 的新增功能。它从 Python 等语言中获得灵感，允许您从数组和对象中提取数据到不同的变量中。这听起来像是您在早期版本的 JavaScript 中已经做过的事情，对吗？看两个例子。

第一个从对象中提取数据:

```
const meal = {  name: 'pizza',  type: 'marinara',  price: 6.25};
```

```
const name = meal.name;const type = meal.type;const price = meal.price;
```

```
console.log(name, type, price);
```

印刷品:

> 番茄酱披萨 6.25 英镑

第二个来自数组:

```
const iceCreamFlavors = ['hazelnut', 'pistachio', 'tiramisu'];const flavor1 = iceCreamFlavors[0];const flavor2 = iceCreamFlavors[1];const flavor3 = iceCreamFlavors[2];console.log(flavor1, flavor2, flavor3);
```

印刷品:

> 榛子开心果提拉米苏

然而，问题是，这两个例子实际上都没有使用析构。

#### 什么是解构？

析构让你在赋值的左边指定你想从数组或对象*中提取的元素。这意味着更少的代码和与上面完全相同的结果，而不损失可读性。即使一开始听起来很奇怪。*

让我们重做我们的例子。

#### 解构对象

下面是我们如何从一个对象中析构值:

```
const meal = {  name: 'pizza',  type: 'marinara',  price: 6.25};
```

```
const {name, type, price} = meal;
```

```
console.log(name, type, price);
```

印刷品:

> 番茄酱披萨 6.25 英镑

花括号`{ }`代表被析构的对象，`name`、`type`和`price`代表你要赋值的变量。我们可以跳过从哪里提取值的属性，只要变量的名称与对象属性的名称相对应。

对象析构的另一个重要特性是，您可以选择要在变量中保存哪些值:

`*const {type} = meal;*`只会从`*meal*`对象中选择`*type*`属性。

![kWOMzaEg2A62-CFxEphqLSsopdq6r9ohdxDV](img/85062d1b1f405b9e91b91347fb5764f8.png)

[source](https://unsplash.com/photos/22Vt7JIf7ZI)

#### 解构数组

下面是我们最初的例子是如何处理的:

```
const iceCreamFlavors = ['hazelnut', 'pistachio', 'tiramisu'];
```

```
const [flavor1, flavor2, flavor3] = iceCreamFlavors;
```

```
console.log(flavor1, flavor2, flavor3);
```

印刷品:

> 榛子开心果提拉米苏

括号`[ ]`代表被析构的数组，`flavor1`、`flavor2`和`flavor3`代表你要赋值的变量。使用析构，我们可以跳过数组中值所在的索引。很方便，不是吗？

与对象类似，在析构数组时，可以跳过一些值:

`const [flavor1, , flavor3] = iceCreamFlavors;` 会干脆无视`flavor2`。

![ZaoWWywMgfyk2d4UgeIhJzLwIYD2O46xHzpV](img/6be4264d724b78940f4967e3fc496c97.png)

[source](https://unsplash.com/photos/qg_faAXDawA)

懒惰万岁，它是发明新捷径的强大动力！

你喜欢这篇文章吗？也许你也会觉得这些很有趣:

[**瑜伽和编程有什么关系？**](https://medium.freecodecamp.org/what-does-yoga-have-to-do-with-programming-b17094e3fb3a)
[*你可能会感到惊讶。*medium.freecodecamp.org](https://medium.freecodecamp.org/what-does-yoga-have-to-do-with-programming-b17094e3fb3a)[**扩展运算符和 rest 参数在 JavaScript(ES6)**](https://medium.com/@aska.gaudyn/spread-operator-and-rest-parameter-in-javascript-es6-4416a9f47e5e)
[*中，扩展运算符和 rest 参数都写成三个连续的点(…)。他们还有别的吗…*medium.com](https://medium.com/@aska.gaudyn/spread-operator-and-rest-parameter-in-javascript-es6-4416a9f47e5e)[**JavaScript 迭代器概述**](https://medium.freecodecamp.org/javascript-iterators-17ab32c3cae7)
[*for、for…in 和 for…of loops 的区别*medium.freecodecamp.org](https://medium.freecodecamp.org/javascript-iterators-17ab32c3cae7)