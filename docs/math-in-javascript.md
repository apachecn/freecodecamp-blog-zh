# JavaScript 数学函数解释

> 原文：<https://www.freecodecamp.org/news/math-in-javascript/>

## **数学**

`Math`是 JavaScript 的全局或标准内置对象之一，可以在任何可以使用 JavaScript 的地方使用。它包含有用的常数，如π和欧拉常数，以及函数，如`floor()`、`round()`和`ceil()`。

在这篇文章中，我们将看看这些函数的例子。但首先，让我们了解更多关于`Math`对象的知识。

### **例子**

以下示例显示了如何使用`Math`对象编写一个计算圆面积的函数:

```
function calculateCircleArea(radius) {
  return Math.PI * Math.pow(radius, 2);
}

calculateCircleArea(1); // 3.141592653589793
```

## **数学最大值**

`Math.max()`是从作为参数传递的数值列表中返回最大值的函数。如果一个非数值作为参数传递，`Math.max()`将返回`NaN`。

可以使用`spread (...)`或`apply`将数值数组作为单个参数传递给`Math.max()`。但是，当数组值的数量过多时，这两种方法都可能失败。

### **语法**

```
Math.max(value1, value2, value3, ...);
```

### **参数**

数字或有限的数字数组。

### **返回值**

给定数值中的最大值，或者如果任何给定值是非数值，则为`NaN`。

### **例题**

*作为参数的数字*

```
Math.max(4, 13, 27, 0, -5); // returns 27
```

*无效参数*

```
Math.max(4, 13, 27, 'eight', -5); // returns NaN
```

*数组作为参数，使用 Spread(…)*

```
let numbers = [4, 13, 27, 0, -5];

Math.max(...numbers); // returns 27
```

*数组作为参数，使用 Apply*

```
let numbers = [4, 13, 27, 0, -5];

Math.max.apply(null, numbers); // returns 27
```

## 数学最小值

Math.min()函数返回零个或多个数字中的最小值。

你可以给它传递任意数量的参数。

```
Math.min(7, 2, 9, -6);
// returns -6
```

## 数学 PI

`Math.PI`是数学对象的静态属性，定义为圆的周长与其直径之比。圆周率约为 3.14149，通常用希腊字母π表示。

## **例题**

```
Math.PI \\ 3.141592653589793
```

#### **更多信息:**

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/PI)

## **数学能力**

`Math.pow()`返回一个数字的另一个数字的幂。

#### **语法**

`Math.pow(base, exponent)`，其中`base`是基数，`exponent`是将`base`提高的数。

`pow()`是`Math`的静态方法，因此它总是被称为`Math.pow()`，而不是另一个对象的方法。

#### **例题**

```
Math.pow(5, 2); // 25
Math.pow(7, 4); // 2401
Math.pow(9, 0.5); // 3
Math.pow(-8, 2); // 64
Math.pow(-4, 3); // -64
```

## **数学 Sqrt**

函数`Math.sqrt()`返回一个数的平方根。

如果输入负数，则返回`NaN`。

`sqrt()`是`Math`的静态方法，因此它总是被称为`Math.sqrt()`，而不是另一个对象的方法。

#### **语法**

`Math.sqrt(x)`，其中`x`为数字。

#### **例题**

```
Math.sqrt(25); // 5
Math.sqrt(169); // 13
Math.sqrt(3); // 1.732050807568
Math.sqrt(1); // 1
Math.sqrt(-5); // NaN
```

## **数学中继**

`Math.trunc()`是 Math standard 对象的一种方法，通过简单地删除小数单位，只返回给定数字的整数部分。这导致整体舍入为零。任何不是数字的输入都将导致 n an 的输出。

小心:该方法是 ECMAScript 2015 (ES6)的一项功能，因此不被旧版本的浏览器支持。

### **例题**

```
Math.trunc(0.1)   //  0
Math.trunc(1.3)   //  1
Math.trunc(-0.9)  // -0
Math.trunc(-1.5)  // -1
Math.trunc('foo') // NaN
```

## 数学细胞

`Math.ceil()`是 Math standard 对象的一个方法，它将给定的数字向上舍入到下一个整数。请注意，对于负数，这意味着该数字将“向 0”舍入，而不是绝对值较大的数字(参见示例)。

### **例题**

```
Math.ceil(0.1)  //  1
Math.ceil(1.3)  //  2
Math.ceil(-0.9) // -0
Math.ceil(-1.5) // -1
```

## 数学地板

`Math.floor()`是 Math standard 对象的一种方法，它将给定的数字向下舍入到下一个整数。请注意，对于负数，这意味着该数字将“远离 0”取整，而不是取较小绝对值的数字，因为`Math.floor()`返回小于或等于给定数字的最大整数。

### **例题**

```
Math.floor(0.9)  //  0
Math.floor(1.3)  //  1
Math.floor(0.5)  //  0
Math.floor(-0.9) // -1
Math.floor(-1.3) // -2
```

### math.floor 的一个应用:如何创建一个 JavaScript 老虎机

在这个练习中，我们必须使用一个特定的公式而不是一般的公式来生成三个随机数。`Math.floor(Math.random() * (3 - 1 + 1)) + 1;`

```
slotOne = Math.floor(Math.random() * (3 - 1 + 1)) + 1;
slotTwo = Math.floor(Math.random() * (3 - 1 + 1)) + 1;
slotThree = Math.floor(Math.random() * (3 - 1 + 1)) + 1;
```

### 另一个例子:求余数

### 例子

```
5 % 2 = 1 because
Math.floor(5 / 2) = 2 (Quotient)
2 * 2 = 4
5 - 4 = 1 (Remainder)
```

### 使用

在数学中，可以通过检查一个数除以 2 的余数来检查该数是偶数还是奇数。

```
17 % 2 = 1 (17 is Odd)
48 % 2 = 0 (48 is Even)
```

****注意**** 不要与*混淆，模数* `%`对负数不起作用。

## 更多数学相关文章:

*   [将 am/pm 时钟转换为 24 小时制](https://guide.freecodecamp.org/mathematics/converting-am-pm-to-24-hour-clock/)
*   [辛普森法则](https://www.freecodecamp.org/news/simpsons-rule/)
*   什么是六边形？