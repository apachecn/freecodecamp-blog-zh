# 解释了 JavaScript 中的全局变量

> 原文：<https://www.freecodecamp.org/news/global-variables-in-javascript-explained/>

全局变量在函数外部声明，以便在整个程序中访问，而局部变量存储在使用`var`的函数中，只在该函数的[范围](https://developer.mozilla.org/en-US/docs/Glossary/Scope)内使用。如果你没有使用`var`来声明一个变量，即使它在一个函数中，它仍然会被看作是全局的:

```
var x = 5; // global

function someThing(y) {
  var z = x + y;
  console.log(z);
}

function someThing(y) {
  x = 5; // still global!
  var z = x + y;
  console.log(z);
}

function someThing(y) {
  var x = 5; // local
  var z = x + y;
  console.log(z);
}
```

全局变量也是当前范围的对象，如浏览器窗口:

```
var dog = “Fluffy”;
console.log(dog); // Fluffy;

var dog = “Fluffy”;
console.log(window.dog); // Fluffy
```

最小化全局变量是最佳实践。由于可以在程序中的任何地方访问该变量，它们会导致奇怪的行为。

参考资料:

*   [var -Javascript|MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
*   你不知道 JavaScript:作用域&闭包

## ****[JavaScript 中的全局变量和 window.variable 有什么区别？](https://stackoverflow.com/questions/6349232/whats-the-difference-between-a-global-var-and-a-window-variable-in-javascript)****

JavaScript 变量的范围可以是全局的，也可以是局部的。全局变量是在函数外部声明的，它的值在整个程序中是可访问/可改变的。

你应该总是使用 ****var**** 来声明你的变量(在本地进行)否则它将会全局安装

小心全局变量，因为它们是有风险的。大多数时候你应该使用闭包来声明你的变量。示例:

```
(function(){
  var myVar = true;
})();
```

## **更多信息:**

*   [JavaScript 变量定义和范围的可视化指南](https://www.freecodecamp.org/news/the-visual-guide-to-javascript-variable-definitions-scope-2717ad9f0169/)
*   [JavaScript 变量定义和提升简介](https://www.freecodecamp.org/news/a-basic-introduction-to-javascript-variable-definitions-and-hoisting-93aa38e742eb/)