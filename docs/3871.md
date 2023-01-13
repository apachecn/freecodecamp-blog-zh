# JavaScript 逻辑运算符

> 原文：<https://www.freecodecamp.org/news/javascript-logical-operators/>

逻辑运算符比较布尔值并返回布尔响应。有两种类型的逻辑运算符——逻辑 AND 和逻辑 OR。对于 AND，这些运算符通常写成& &；对于 or，这些运算符通常写成||。

## 逻辑与(&&)

AND 运算符比较两个表达式。如果第一个表达式的值为 ["truthy"](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) ，该语句将返回第二个表达式的值。如果第一个表达式的值为[【falsy】](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)，该语句将返回第一个表达式的值。

当只涉及布尔值(或者`true`或者`false`)时，如果两个表达式都为真，则返回真。如果一个或两个表达式都为假，整个语句将返回假。

```
true && true //returns  the second value, true
true && false //returns the second value, false
false && false //returns the first value, false
123 && 'abc' // returns the second value, 'abc'
'abc' && null //returns the second value, null
undefined && 'abc' //returns the first value, undefined
0 && false //returns the first value, 0
```

## 逻辑或(||)

OR 运算符比较两个表达式。如果第一个表达式的值为“falsy ”,该语句将返回第二个表达式的值。如果第一个表达式的计算结果为“truthy”，该语句将返回第一个表达式的值。

当仅涉及布尔值(或者`true`或者`false`)时，如果任一表达式为真，则返回真。两个表达式都可以为真，但结果只需要一个表达式为真。

```
true || true //returns the first value, true
true || false //returns the first value, true
false || false //returns the second value, false
123 || 'abc' // returns the first value, 123
'abc' || null //returns the first value, 'abc'
undefined || 'abc' //returns the second value, 'abc'
0 || false //returns the second value, false
```

## 短路评估

&&和||表现为短路操作符。

在逻辑 AND 的情况下，如果第一个操作数返回 false，则不会计算第二个操作数，而会返回第一个操作数。

在逻辑 OR 的情况下，如果第一个值返回 true，则从不计算第二个值，而是返回第一个操作数。

## 逻辑 NOT(！)

NOT 运算符不像 AND 和 or 运算符那样进行任何比较。此外，它只对一个操作数进行运算。

一个“！”感叹号用于表示 NOT 运算符。

## NOT 运算符的使用

1.  将表达式转换为布尔值。
2.  返回上一步中获得的布尔值的倒数。

```
var spam = 'rinki'; //spam may be equal to any non empty string
var booSpam = !spam;
/*returns false
  since when a non-empty string when converted to boolean returns true
  inverse of which is evaluated to false.
*/

var spam2 = ''; //spam2 here is equal to empty string
var booSpam2 = !spam2;
/*returns true
  since when a empty string when converted to boolean returns false
  inverse of which is evaluated to true.
*/
```

### 小贴士:

两个逻辑运算符都将返回最后计算的表达式的值。例如:

```
"cat" && "dog" //returns "dog"
"cat" && false //returns false
0 && "cat"  //returns 0 (which is a falsy value)

"cat" || "dog" //returns "cat"
"cat" || false //returns "cat"
0 || "cat" //returns "cat"
```

注意，`&&`返回第一个值，`||`返回第二个值，反之亦然。