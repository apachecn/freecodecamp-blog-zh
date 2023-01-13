# 第三部分:模板文字、扩展操作符和生成器！

> 原文：<https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-iii-template-literals-spread-operators-generators-592765337294/>

欢迎来到**学习 ES6 的第三部分**，这是一个帮助你轻松理解 ES6 (ECMAScript 6)的系列！

让我们进一步探索 ES6，涵盖三个极具价值的概念:

*   模板文字
*   扩展运算符
*   发电机

#### 模板文字

好处:

*   简单的表达式插值和方法调用！参见下面的例子。
*   以您想要的格式包含复杂的信息很简单！
*   您也不需要担心多引号、多行、空格或使用“+”号！只有两个反勾号能识别其中的所有信息！呜哇！

当心:

*   通常称为“模板字符串”，因为这是它们在 ES2015 / ES6 规范的早期版本中的名称。
*   变量和参数需要用美元符号和花括号包装。*占位符* ${EXAMPLE}。
*   模板文本中的加号“+”实际上充当数学运算，如果也在${}中，则不是串联。更多解释见下面的例子。

#### 迁移到模板文字语法

在回顾了优点和需要注意的事项后，请记下这些示例，并研究使用模板文字的细微差异:

```
// #1
// Before:
function sayHi(petSquirrelName) { console.log('Greetings ' + petSquirrelName + '!'); }
sayHi('Brigadier Sir Nutkins II'); // => Greetings Brigadier Sir Nutkins II!

// After:
function sayHi(petSquirrelName) { console.log(`Greetings ${petSquirrelName}!`); }
sayHi('Brigadier Sir Nutkins II'); // => Greetings Brigadier Sir Nutkins II!

// #2
// Before:
console.log('first text string \n' + 'second text string'); 
// => first text string 
// => second text string

// After:
console.log(`first text string 
second text string`); 
// => first text string 
// => second text string

// #3
// Before:
var num1 = 5;
var num2 = 10;
console.log('She is ' + (num1 + num2) +  ' years old and\nnot ' + (2 * num1 + num2) + '.');
// => She is 15 years old and
// => not 20.

// After:
var num1 = 5;
var num2 = 10;
console.log(`She is ${num1 + num2} years old and\nnot ${2 * num1 + num2}.`);
// => She is 15 years old and
// => not 20.

// #4 
// Before:
var num1 = 12;
var num2 = 8;
console.log('The number of JS MVC frameworks is ' + (2 * (num1 + num2)) + ' and not ' + (10 * (num1 + num2)) + '.');
//=> The number of JS frameworks is 40 and not 200.

// After:
var num1 = 12;
var num2 = 8;
console.log(`The number of JS MVC frameworks is ${2 * (num1 + num2)} and not ${10 * (num1 + num2)}.`);
//=> The number of JS frameworks is 40 and not 200.

// #5
// The ${} works fine with any kind of expression, including method calls:
// Before:
var registeredOffender = {name: 'Bunny BurgerKins'};
console.log((registeredOffender.name.toUpperCase()) + ' you have been arrested for the possession of illegal carrot bits!');
// => BUNNY BURGERKINS you have been arrested for the possession of illegal carrot bits!

// After:
var registeredOffender = {name: 'Bunny BurgerKins'};
console.log(`${registeredOffender.name.toUpperCase()} you have been arrested for the possession of illegal carrot bits!`);
// => BUNNY BURGERKINS you have been arrested for the possession of illegal carrot bits!
```

让我们来看看使用模板文字的更复杂的方法！看看包含所有这些信息是多么容易，而不用担心所有的“+”号、空格、数学逻辑和引号的位置！可以*这么*方便！另外请注意，如果打印价格，您需要在占位符外包含另一个美元符号:

```
function bunnyBailMoneyReceipt(bunnyName, bailoutCash) {
  var bunnyTip = 100;

  console.log(
    `
    Greetings ${bunnyName.toUpperCase()}, you have been bailed out!

      Total: ${bailoutCash}
      Tip: ${bunnyTip}
      ------------
      Grand Total: ${bailoutCash + bunnyTip}

    We hope you a pleasant carrot nip-free day!  

  `
  );

}

bunnyBailMoneyReceipt('Bunny Burgerkins', 200);

// Enter the above code into your console to get this result:
/* 
  Greetings BUNNY BURGERKINS, you have been bailed out!

      Total: $200
      Tip: $100
      ------------
      Grand Total: $300

    We hope you a pleasant carrot nip-free day! 
*/
```

哇，简单多了！！太令人兴奋了…啊！！

![PLbJ5KyPdRVJmbS-4MXy5XC5eZ5EcHtDw6f4](img/0a26bebf095b1f09bdc2c616ea11dcbf.png)

#### 扩展运算符

如果在一个数组中有多个参数要插入到一个函数调用中，或者有多个数组和/或数组元素要无缝地插入到另一个数组中，请使用 Spread 运算符！

好处:

*   轻松地将数组连接到其他数组中。
*   将数组放在数组内部的任意位置。
*   轻松将参数添加到函数调用中。
*   在数组名前只有 3 个点“…”。
*   与 *function.apply* 类似，但可以与 *new* 关键字一起使用，而 *function.apply* 不能。

让我们看一下这样一种情况，我们希望在不使用 Spread 运算符的情况下将几个数组添加到另一个主数组中:

```
var squirrelNames = ['Lady Nutkins', 'Squirrely McSquirrel', 'Sergeant Squirrelbottom'];
var bunnyNames = ['Lady FluffButt', 'Brigadier Giant'];
var animalNames = ['Lady Butt', squirrelNames, 'Juicy Biscuiteer', bunnyNames];

animalNames;
// => ['Lady Butt', ['Lady Nutkins', 'Squirrely McSquirrel', 'Sergeant Squirrelbottom'], 'Juicy Biscuiteer', ['Lady FluffButt', 'Brigadier Giant']]

// To flatten this array we need another step:
var flattened = [].concat.apply([], animalNames);
flattened;
// => ['Lady Butt', 'Lady Nutkins', 'Squirrely McSquirrel', 'Sergeant Squirrelbottom', 'Juicy Biscuiteer', 'Lady FluffButt', 'Brigadier Giant'] 
```

使用 Spread 操作符，您的数组会自动插入并连接到您想要的任何位置。不需要任何额外的步骤:

```
var squirrelNames = ['Lady Nutkins', 'Squirrely McSquirrel', 'Sergeant Squirrelbottom'];
var bunnyNames = ['Lady FluffButt', 'Brigadier Giant'];
var animalNames = ['Lady Butt', ...squirrelNames, 'Juicy Biscuiteer', ...bunnyNames];

animalNames;
// => ['Lady Butt', 'Lady Nutkins', 'Squirrely McSquirrel', 'Sergeant Squirrelbottom', 'Juicy Biscuiteer', 'Lady FluffButt', 'Brigadier Giant'] 
```

另一个有用的例子:

```
var values = [25, 50, 75, 100]

// This:
console.log(Math.max(25, 50, 75, 100)); // => 100

// Is the same as this:
console.log(Math.max(...values)); // => 100

/* 
  NOTE: Math.max() typically does not work for arrays unless you write it like:
        Math.max.apply(null, values), but with Spread Operators you can just insert it
        and voila! No need for the .apply() part! Wohoo! :)
*/
```

#### 可能比更有用。应用()

如果在一个函数中有多个参数该怎么办？您可以使用好的 ol' *函数:*

```
function myFunction(x, y, z) {
  console.log(x + y + z)
};
var args = [0, 1, 2];
myFunction.apply(null, args);
// => 3
```

或者使用扩展运算符:

```
function myFunction(x, y, z) {
  console.log(x + y + z);
}
var args = [0, 1, 2];
myFunction(...args);
// => 3
```

在 ES5 中，不可能用*应用*方法组合*新的*关键字。由于引入了扩展操作符语法，您现在可以！

```
var dateFields = readDateFields(database);
var d = new Date(…dateFields);
```

#### 发电机

好处:

*   允许您暂停功能，稍后再继续。
*   更容易创建异步函数。
*   通常与 *setTimeout()* 或 *setInterval()* 一起使用，对异步事件进行计时。

请注意:

*   如果你看到*和单词*产生*，你就知道你在看一台发电机。
*   你需要每次都调用这个函数，以便调用其中的下一个函数，否则它不会运行，除非它在一个 *setInterval()* 中。
*   结果自然就以对象的形式出来了，加。*值*仅获取值。
*   对象带有设置为假的 *done* 属性，直到所有的 *yield* 表达式都被打印出来。
*   当所有的函数/值都被调用时，或者当一个*返回*语句出现时，生成器结束。

示例:

```
function* callMe() {
  yield '1';
  yield '…and a 2';
  yield '…and a 3';
  return;
  yield 'this won’t print';
}

var anAction = callMe();

console.log(anAction.next());
//=> { value: ‘1’, done: false }

console.log(anAction.next());
//=> { value: ‘…and a 2’, done: false }

console.log(anAction.next());
//=> { value: ‘…and a 3’, done: false }

console.log(anAction.next());
//=> { value: ‘undefined’, done: true }

console.log(anAction.next());
//=> { value: ‘undefined’, done: true }

// NOTE: To get only the value use anAction.next().value otherwise the entire object will be printed. 
```

对于异步函数调用，生成器非常有用。假设你有三个不同的函数存储在一个数组中，你想在一段时间后一个接一个地调用它们:

```
// Bunny needs to go grocery shopping for her friend’s birthday party.
var groceries = '';

// Let’s create three functions that need to be called for Bunny.
var buyCarrots = function () {
  groceries += 'carrots';
}

var buyGrass = function () {
  groceries += ', grass';
}

var buyApples = function () {
  groceries += ', and apples';
}

// Bunny is in a hurry so you have to buy the groceries within certain amount of time!
// This is an example of when you would use a timer with a Generator.
// First store the functions within an array:
var buyGroceries = [buyCarrots, buyGrass, buyApples];

// Then loop through the array within a Generator:
// There are some issues doing this with the forEach, recreate this using a for loop.
function* loopThrough(arr) {
  for(var i=0; i<arr.length; i++) {
    yield arr[i]();
  }
}

// add the array of three functions to the loopThrough function.
var functions = loopThrough(buyGroceries);

// Lastly, set the time you want paused before the next function call 
// using the setInterval method(calls a function/expression after a specific set time).
var timedGroceryHunt = setInterval(function() {
  var functionCall = functions.next();
  if(!functionCall.done) {
    console.log(`Bunny bought ${groceries}!`);
  }else {
    clearInterval(timedGroceryHunt);
    console.log(`Thank you! Bunny bought all the ${groceries} in time thanks to your help!`);
  }
}, 1000);

// Enter this code into your console to test it out!
// after 1 second: => Bunny bought carrots!
// after 1 second: => Bunny bought carrots, grass!
// after 1 second: => Bunny bought carrots, grass, and apples!
// after 1 second: => Thank you! Bunny bought all the carrots, grass, and apples in time thanks to your help! 
```

同样，这也可以通过*承诺*(一个尚未完成，但预计在未来完成的操作)来完成。开发人员有时会在他们的代码中同时使用承诺和生成器，所以了解这两者是有好处的。

恭喜你。您已经通过了**Learn ES6 The Dope Way**Part III，现在您已经获得了三个超级有价值的概念！现在，您可以在代码中安全地整理并有效地使用模板文字、扩展操作符和生成器。呜哇！去你的！

尽管如此，你可能想要等待，因为 ES6 仍然存在浏览器问题，在发布代码之前使用像 *Babel* 这样的编译器或者像 *Webpack* 这样的模块捆绑器是很重要的。所有这些都将在未来的**中讨论。感谢阅读** **❤**

随着越来越多的**学习 ES6,**即将进入中级，通过喜欢和关注保持你的智慧更新！

**第一部分:const、let &有**

**[第二部分:(箭头)= >功能和](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-ii-arrow-functions-and-the-this-keyword-381ac7a32881/)关键字**

**[第三部分:模板文字，传播操作符&生成器！](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-iii-template-literals-spread-operators-generators-592765337294/)**

第四部分:默认参数、析构赋值和一个新的 ES6 方法！

**[第五部分:类，翻译 ES6 代码&更多资源！](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-v-classes-browser-compatibility-transpiling-es6-code-47f62267661/)**

你也可以在 https://github.com/Mashadim 的 github ❤网站上找到我