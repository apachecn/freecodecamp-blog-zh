# 学习 ES6 的 Dope 方式第二部分:箭头功能和“这”关键字

> 原文：<https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-ii-arrow-functions-and-the-this-keyword-381ac7a32881/>

欢迎来到**以毒品的方式学习 ES6 的第二部分，**这是一个帮助你轻松理解 ES6 的系列(ECMAScript 6)！

#### **那么，究竟是什么=>**；？

你可能到处都见过这些奇怪的埃及象形文字符号，尤其是在别人的代码中，你目前正在调试一个' *this'* 关键字问题。经过一个小时的修补，你现在漫游谷歌搜索栏和跟踪堆栈溢出。听起来熟悉吗？

让我们一起来探讨 **Learn ES6 The Dope Way** 第二部分中的三个主题:

*   如何将' **这个** 关键词关联到 ****= >**** 。
*   如何将功能从 ES5 迁移到 ES6？
*   使用 ****= >**** 时需要注意的重要怪癖。

#### 箭头功能

创建箭头函数是为了简化函数范围，并使使用' *this* '关键字更加简单。他们利用**=&**gt；语法，看起来像一个箭头。尽管我认为它不需要节食，但人们称 I*t“the fat arr*ow”(Ruby 爱好者可能更了解它是 th*e“hash rock*et”)——这是需要注意的事情。

#### “this”关键字与箭头函数的关系

在我们深入研究 ES6 箭头函数之前，首先清楚地了解一下 ES5 代码中的这个绑定到什么是很重要的。

如果'*这个*关键字在一个对象的**方法**(一个属于对象的函数)中，它会引用什么？

```
// Test it here: https://jsfiddle.net/maasha/x7wz1686/
var bunny = {
  name: 'Usagi',
  showName: function() {
    alert(this.name);
  }
};

bunny.showName(); // Usagi
```

正确！它指的是这个物体。我们稍后会谈到原因。

现在，如果'*这个*'关键字在方法的函数内部会怎么样呢？

```
// Test it here: https://jsfiddle.net/maasha/z65c1znn/
var bunny = {
  name: 'Usagi',
  tasks: ['transform', 'eat cake', 'blow kisses'],
  showTasks: function() {
    this.tasks.forEach(function(task) {
      alert(this.name + " wants to " + task);
    });
  }
};

bunny.showTasks();
// [object Window] wants to transform
// [object Window] wants to eat cake
// [object Window] wants to blow kisses

// please note, in jsfiddle the [object Window] is named 'result' within inner functions of methods. 
```

你得到了什么？等等，我们的兔子怎么了…？

啊，你以为这个是指这个方法的内部函数吗？

或许是物体本身？

你这样想是明智的，然而事实并非如此。请允许我教你编码前辈曾经教过我的东西:

编码前辈**:**“*啊对，* t *何编码就强用这个。* *认为‘this’关键字绑定到函数确实很实际，但事实是，‘this’现在已经不在范围之内了……它现在属于……”，*他停顿了一下，好像经历了内心的骚动*，“窗口对象。*”

没错。事情就是这样发生的。

为什么'*这个*会绑定到窗口对象？**因为“*这个*”总是引用它所在函数的所有者，在这种情况下——因为它现在不在范围内——窗口/全局对象。**

当它在对象的方法内部时，函数的所有者就是对象。因此'*这个*'关键字被绑定到对象上。然而，当它在一个函数中时，无论是独立的还是在另一个方法中，它总是引用窗口/全局对象。

```
// Test it here: https://jsfiddle.net/maasha/g278gjtn/
var standAloneFunc = function(){
  alert(this);
}

standAloneFunc(); // [object Window]
```

但是为什么…？

这被称为 JavaScript 怪癖，意思是 JavaScript 中发生的事情并不简单，也不像你想象的那样工作。这也被开发者认为是一个糟糕的设计选择，他们现在用 ES6 的 arrow 函数来补救。

在我们继续之前，重要的是要了解程序员在 ES5 代码中解决' *this* '问题的两种聪明方法，特别是因为你会在一段时间内继续遇到 ES5(并非每个浏览器都已经完全迁移到 ES6):

在方法的内部函数之外创建一个变量。现在“forEach”方法获得了对“ *this* ”的访问，从而获得了对象的属性和值。这是因为' *this* '被存储在一个变量中，而它仍然在对象的直接方法' showTasks '的范围内。

```
// Test it here: https://jsfiddle.net/maasha/3mu5r6vg/
var bunny = {
  name: 'Usagi',
  tasks: ['transform', 'eat cake', 'blow kisses'],
  showTasks: function() {
    var _this = this;
    this.tasks.forEach(function(task) {
      alert(_this.name + " wants to " + task); 
    });
  }
};

bunny.showTasks();
// Usagi wants to transform
// Usagi wants to eat cake
// Usagi wants to blow kisses
```

**#2** 使用 bind 将引用该方法的“ *this* ”关键字附加到该方法的内部函数中。

```
// Test it here: https://jsfiddle.net/maasha/u8ybgwd5/
var bunny = {
  name: 'Usagi',
  tasks: ['transform', 'eat cake', 'blow kisses'],
  showTasks: function() {
    this.tasks.forEach(function(task) {
      alert(this.name + " wants to " + task);
    }.bind(this));
  }
};

bunny.showTasks();
// Usagi wants to transform
// Usagi wants to eat cake
// Usagi wants to blow kisses
```

现在介绍…箭头功能！处理'*'这个*问题从未如此简单直接！简单的 ES6 解决方案:

```
// Test it here: https://jsfiddle.net/maasha/che8m4c1/

var bunny = {
  name: 'Usagi',
  tasks: ['transform', 'eat cake', 'blow kisses'],
  showTasks() {
    this.tasks.forEach((task) => {
      alert(this.name + " wants to " + task);
    });  
  }
};

bunny.showTasks();
// Usagi wants to transform
// Usagi wants to eat cake
// Usagi wants to blow kisses
```

在 ES5 ' *中，this* 指的是函数的父函数，而在 ES6 中，arrow 函数使用词法作用域——'*this*指的是它当前的周围作用域，没有其他含义。因此，内部函数知道只绑定到内部函数，而不绑定到对象的方法或对象本身。

#### 如何将功能从 ES5 迁移到 ES6？

```
// Before
let bunny = function(name) {
  console.log("Usagi");
}

// After
let bunny = (name) => console.log("Usagi")

// Step 1: Remove the word ‘function’.
let bunny = (name) {
  console.log("Usagi");
}

// Step 2: If your code is less than a line, remove brackets and place on one line.
let bunny = (name) console.log("Usagi");

// Step 3\. Add the hash rocket.
let bunny = (name) => console.log("Usagi");
```

你做到了！干得好！够简单吧？这里有更多的例子，利用胖-呃瘦箭头，让你的眼睛习惯:

```
// #1 ES6: if passing one argument you don't need to include parenthesis around parameter.
var kitty = name => name;

// same as ES5:
var kitty = function(name) {
  return name;
};

// #2 ES6: no parameters example.
var add = () => 3 + 2;

// same as ES5:
var add = function() {
  return 3 + 2;
};

// #3 ES6: if function consists of more than one line or is an object, include braces.
var objLiteral = age => ({ name: "Usagi", age: age });

// same as ES5:
var objLiteral = function(age) {
  return {
    name: "Usagi",
    age: age
  };
};

// #4 ES6: promises and callbacks.
asyncfn1().then(() => asyncfn2()).then(() => asyncfn3()).then(() => done());

// same as ES5:
asyncfn1().then(function() {
  asyncfn2();
}).then(function() {
  asyncfn3();
}).done(function() {
  done();
});
```

#### 使用箭头函数时需要注意的重要问题

如果将“new”关键字与= >函数一起使用，将会引发错误。箭头函数不能用作构造函数—普通函数通过属性原型和内部方法[[Construct]]支持“new”。箭头函数两者都不使用，因此 new (() => {})抛出一个错误。

要考虑的其他怪癖:

```
// Line breaks are not allowed and will throw a syntax error
let func1 = (x, y)
=> {
  return x + y;
}; // SyntaxError

// But line breaks inside of a parameter definition is ok
let func6 = (
  x,
  y
) => {
	return x + y;
}; // Works!

// If an expression is the body of an arrow function, you don’t need braces:
asyncFunc.then(x => console.log(x));

// However, statements have to be put in braces:
asyncFunc.catch(x => { throw x });

// Arrow functions are always anonymous which means you can’t just declare them as in ES5:
function squirrelLife() {
  // play with squirrels, burrow for food, etc.
}

// Must be inside of a variable or object property to work properly:
let squirrelLife = () => {
  // play with squirrels, burrow for food, etc.
  // another super squirrel action.
}
```

恭喜你。你已经通过了**Learn ES6 The Dope Way**Part II，现在你已经有了箭头函数知识的基础，它给' *this* 带来了词汇上的好处，并且还为你自己获得了一些 JavaScript 技巧！:)

随着越来越多的**学习 ES6,**即将进入中级，通过喜欢和关注保持你的智慧更新！

**第一部分:const、let &有**

**[第二部分:(箭头)= >功能和](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-ii-arrow-functions-and-the-this-keyword-381ac7a32881/)关键字**

**[第三部分:模板文字，传播操作符&生成器！](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-iii-template-literals-spread-operators-generators-592765337294/)**

第四部分:默认参数、析构赋值和一个新的 ES6 方法！

**[第五部分:类，翻译 ES6 代码&更多资源！](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-v-classes-browser-compatibility-transpiling-es6-code-47f62267661/)**

你也可以在 https://github.com/Mashadim 的 github ❤网站上找到我