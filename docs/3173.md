# 箭头函数 JavaScript 教程——如何用新的 ES6 语法声明 JS 函数

> 原文：<https://www.freecodecamp.org/news/arrow-function-javascript-tutorial-how-to-declare-a-js-function-with-the-new-es6-syntax/>

你可能见过用几种不同方式编写的箭头函数。

```
//example 1
const addTwo = (num) => {return num + 2;};

//example 2
const addTwo = (num) => num + 2;

//example 3
const addTwo = num => num + 2;

//example 4
const addTwo = a => {
 const newValue = a + 2;
 return newValue;
}; 
```

有些参数有括号，有些没有。有些使用花括号和`return` 关键字，有些不使用。一个甚至跨越多行，而其他的只包含一行。

有趣的是，当我们用相同的参数调用上面的箭头函数时，我们得到了相同的结果。

```
console.log(addTwo(2));
//Result: 4 
```

如何知道使用哪种箭头函数语法？这就是本文将要揭示的:如何声明一个箭头函数。

## 一个主要的区别

箭头函数是另一种更简洁的编写函数表达式的方式。但是，它们没有自己绑定到 **`this`** 关键字。

```
//Function expression
const addNumbers = function(number1, number2) {
   return number1 + number2;
};

//Arrow function expression
const addNumbers = (number1, number2) => number1 + number2; 
```

当我们用相同的参数调用这些函数时，我们会得到相同的结果。

```
console.log(addNumbers(1, 2));
//Result: 3 
```

需要注意一个重要的语法差异:箭头函数使用箭头 **`=>`** 而不是 **`function`** 关键字。当你写箭头函数的时候，还有其他的不同需要注意，这是我们接下来要探讨的。

## 圆括号

一些箭头函数在参数周围有括号，而另一些没有。

```
//Example with parentheses
const addNums = (num1, num2) => num1 + num2;

//Example without parentheses
const addTwo = num => num + 2; 
```

事实证明，arrow 函数的参数数量决定了我们是否需要包含括号。

带有零参数的箭头函数需要括号。

```
const hello = () => "hello";
console.log(hello());
//Result: "hello" 
```

带有一个参数的箭头函数**不需要圆括号*不需要圆括号*。换句话说，括号是可选的。**

```
const addTwo = num => num + 2; 
```

所以我们可以在上面的例子中添加括号，箭头函数仍然有效。

```
const addTwo = (num) => num + 2;
console.log(addTwo(2));
//Result: 4 
```

带有多个参数的箭头函数需要括号。

```
const addNums = (num1, num2) => num1 + num2;
console.log(addNums(1, 2));
//Result: 3 
```

箭头功能还支持**静止参数**和**析构**。这两个特性都需要括号。

这是一个带有**休止参数**的箭头函数的例子。

```
const nums = (first, ...rest) => rest;
console.log(nums(1, 2, 3, 4));
//Result: [ 2, 3, 4 ] 
```

这是一个使用**析构**的例子。

```
const location = {
   country: "Greece",
   city: "Athens"
};

const travel = ({city}) => city;

console.log(travel(location));
//Result: "Athens" 
```

总结一下:如果只有一个参数——并且没有使用 rest 参数或析构——那么括号是可选的。否则，一定要包含它们。

## 功能体

现在我们已经讨论了括号规则，让我们转向箭头函数的函数体。

一个箭头函数体可以有[“简洁体”或者“块体”](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#:~:text=An%20arrow%20function%20expression%20is,cannot%20be%20used%20as%20constructors.)。正文类型影响语法。

首先是“简洁体”语法。

```
const addTwo = a => a + 2; 
```

“简洁体”语法就是:它很简洁！我们不使用`return` 关键字或花括号。

如果你有一个单行的箭头函数(就像上面的例子)，那么这个值是隐式返回的。所以可以省略`return` 关键字和花括号。

现在让我们看看“块主体”语法。

```
const addTwo = a => {
    const total = a + 2;
    return total;
} 
```

注意，在上面的例子中，我们同时使用了*花括号和 **`return`** 关键字。*

*当函数体超过一行时，通常会看到这种语法。这是一个关键点:用花括号将多行箭头函数的主体括起来，并使用`return` 关键字。*

### *对象和箭头功能*

*还有一个语法上的细微差别需要了解:当您想要返回一个[对象文字表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)时，请将函数体括在括号中。*

```
*`const f = () => ({
 city:"Boston"
})
console.log(f().city)`* 
```

 *如果没有括号，我们会得到一个错误。

```
const f = () => {
   city:"Boston"
}
//Result: error 
```

如果您发现 arrow 函数的语法有点混乱，您并不孤单。需要一些时间来熟悉它。但是意识到你的选择和需求是朝着那个方向前进的步骤。

**我写的是学习编程以及学习编程的最佳方法(**【amymhaddad.com】)。*