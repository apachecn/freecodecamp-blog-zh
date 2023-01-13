# JavaScript 中的错误值

> 原文：<https://www.freecodecamp.org/news/falsy-values-in-javascript/>

## **描述**

假值是指计算结果为假的东西，例如在检查变量时。JavaScript 中只有六个 falsey 值:`undefined`、`null`、`NaN`、`0`、`""`(空字符串)，当然还有`false`。

## **检查变量上的错误值**

可以用一个简单的条件来检查变量中的错误值:

```
if (!variable) {
  // When the variable has a falsy value the condition is true.
}
```

## **一般例子**

```
var string = ""; // <-- falsy

var filledString = "some string in here"; // <-- truthy

var zero = 0; // <-- falsy

var numberGreaterThanZero // <-- truthy

var emptyArray = []; // <-- truthy, we'll explore more about this next

var emptyObject = {}; // <-- truthy
```

## **有趣的数组**

```
if ([] == false) // <-- truthy, will run code in if-block

if ([]) // <-- truthy, will also run code in if-block

if ([] == true) // <-- falsy, will NOT run code in if-block

if (![]) // <-- falsy, will also NOT run code in if-block
```

## **警告**

在布尔上下文中计算值时，请注意数据类型。如果值的数据类型是一个*数字*，真/假评估可能会导致意外的结果:

```
const match = { teamA: 0, teamB: 1 }
if (match.teamA)
  // The following won't run due to the falsy evaluation
  console.log('Team A: ' + match.teamA);
}
```

上述用例的替代方法是使用`typeof`评估值:

```
const match = { teamA: 0, teamB: 1 }
if (typeof match.teamA === 'number')
  console.log('Team A: ' + match.teamA);
}
```

## **更多信息**

*   [博文-真&假](http://james.padolsey.com/javascript/truthy-falsey/)
*   [Falsy | Glossary | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)
*   [Truthy 和 Falsy:当 JavaScript 中所有不相等时](https://www.sitepoint.com/javascript-truthy-falsy/)