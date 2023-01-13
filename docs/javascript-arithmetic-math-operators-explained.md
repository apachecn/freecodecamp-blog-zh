# JavaScript 模数，除法，余数和其他数学运算符解释

> 原文：<https://www.freecodecamp.org/news/javascript-arithmetic-math-operators-explained/>

JavaScript 为用户提供了五种算术运算符:`+`、`-`、`*`、`/`和`%`。运算符分别用于加、减、乘、除和余数(或模)。

## **加法**

****语法****

`a + b`

****用法****

```
2 + 3          // returns 5
true + 2       // interprets true as 1 and returns 3
false + 5      // interprets false as 0 and returns 5
true + "bar"   // concatenates the boolean value and returns "truebar"
5 + "foo"      // concatenates the string and the number and returns "5foo"
"foo" + "bar"  // concatenates the strings and returns "foobar"
```

*提示:*有一个方便的[递增](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Increment_()运算符，当你把数字加 1 时，这是一个很好的捷径。

## **减法**

****语法****

`a - b`

****用法****

```
2 - 3      // returns -1
3 - 2      // returns 1
false - 5  // interprets false as 0 and returns -5
true + 3   // interprets true as 1 and returns 4
5 + "foo"  // returns NaN (Not a Number)
```

*提示:*有一个方便的[递减](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Decrement_(--)运算符，当你把数字减 1 时，这是一个很好的捷径。

## **乘法运算**

****语法****

`a * b`

****用法****

```
2 * 3                // returns 6
3 * -2               // returns -6
false * 5            // interprets false as 0 and returns 0
true * 3             // interprets true as 1 and returns 3
5 * "foo"            // returns NaN (Not a Number)
Infinity * 0         // returns NaN
Infinity * Infinity  // returns Infinity
```

## **分部**

****语法****

`a / b`

****用法****

```
3 / 2                // returns 1.5
3.0 / 2/0            // returns 1.5
3 / 0                // returns Infinity
3.0 / 0.0            // returns Infinity
-3 / 0               // returns -Infinity
false / 5            // interprets false as 0 and returns 0
true / 2             // interprets true a 1 and returns 0.5
5 + "foo"            // returns NaN (Not a Number)
Infinity / Infinity  // returns NaN
```

## **余数**

****语法****

`a % b`

****用法****

```
3 % 2          // returns 1
true % 5       // interprets true as 1 and returns 1
false % 4      // interprets false as 0 and returns 0
3 % "bar"      // returns NaN
```

## **增量**

****语法****

`a++ or ++a`

****用法****
//后缀 x = 3；//声明一个变量 y = x++；// y = 4，x = 3
//前缀 var a = 2；b = ++ a；// a = 3，b = 3

## **减量**

****语法****

`a-- or --a`

****用法****
//后缀 x = 3；//声明一个变量 y = x—；// y = 3，x = 3
//前缀 var a = 2；b =—a；// a = 1，b = 1 *！重要！*如你所见，你 ****不能**** 对`Infinity`进行任何操作。

## 关于 JavaScript 中数学的更多信息:

*   [JavaScript 数学函数讲解](https://www.freecodecamp.org/news/math-in-javascript/)
*   [JavaScript 的 math.random()方法讲解](https://www.freecodecamp.org/news/p/b988fbe9-a282-435b-8df0-71eb9193ad5c/)