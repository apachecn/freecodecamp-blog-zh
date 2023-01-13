# javascript 布尔解释–如何在 javascript 中使用布尔

> 原文：<https://www.freecodecamp.org/news/booleans-in-javascript-explained-how-to-use-booleans-in-javascript/>

## **布尔型**

布尔是计算机编程语言中常用的一种原始数据类型。根据定义，布尔值有两个可能的值:`true`或`false`。

在 JavaScript 中，经常有隐式类型强制转换为 boolean。例如，如果您有一个 If 语句来检查某个表达式，则该表达式将被强制转换为布尔值:

```
const a = 'a string';
if (a) {
  console.log(a); // logs 'a string'
}
```

只有几个值将被强制为假:

*   false(不是真正的强制，因为它已经是 false)
*   空
*   不明确的
*   圆盘烤饼
*   Zero
*   " "(空字符串)

所有其他值将被强制为真。当一个值被强制为布尔值时，我们也称之为“假”或“真”。

使用类型强制的一种方式是使用 or ( `||`)和 and ( `&&`)运算符:

```
const a = 'word';
const b = false;
const c = true;
const d = 0
const e = 1
const f = 2
const g = null

console.log(a || b); // 'word'
console.log(c || a); // true
console.log(b || a); // 'word'
console.log(e || f); // 1
console.log(f || e); // 2
console.log(d || g); // null
console.log(g || d); // 0
console.log(a && c); // true
console.log(c && a); // 'word'
```

如您所见，*或*操作符检查第一个操作数。如果这是真的或真的，它会立即返回(这就是为什么我们在第一种情况下得到“word”而在第二种情况下得到“T2”true)。如果它不是 true 或 truthy，它返回第二个操作数(这就是为什么我们在第三种情况下得到' word ')。

对于 and 运算符，它以类似的方式工作，但是要使“and”为真，两个操作数都必须为真。因此，如果两个操作数都为 true/truthy，它将始终返回第二个操作数，否则将返回 false。这就是为什么在第四种情况下我们得到真实，而在最后一种情况下我们得到单词。

## **布尔对象**

还有一个包装值的原生 JavaScript 对象。如果需要，作为第一个参数传递的值将被转换为布尔值。如果省略 value、0、-0、null、false、NaN、undefined 或空字符串("")，则对象的初始值为 false。所有其他值，包括任何对象或字符串“false”，创建一个初始值为 true 的对象。

不要混淆原始布尔值 true 和 false 与布尔对象的 true 和 false 值。

## **更多详情**

任何值不是 undefined 或 null 的对象，包括值为 false 的布尔对象，当传递给条件语句时，计算结果为 true。如果为真，将执行该函数。例如，以下 if 语句中的条件计算结果为 true:

```
const x = new Boolean(false);
if (x) {
  // this code is executed
}
```

此行为不适用于布尔原语。例如，以下 if 语句中的条件计算结果为 false:

```
const x = false;
if (x) {
  // this code is not executed
}
```

不要使用布尔对象将非布尔值转换为布尔值。相反，使用布尔函数来执行此任务:

```
const x = Boolean(expression);     // preferred
const x = new Boolean(expression); // don't use
```

如果指定任何对象(包括值为 false 的布尔对象)作为布尔对象的初始值，则新布尔对象的值为 true。

```
const myFalse = new Boolean(false);   // initial value of false
const g = new Boolean(myFalse);       // initial value of true
const myString = new String('Hello'); // string object
const s = new Boolean(myString);      // initial value of true
```

不要用布尔对象代替布尔原语。