# JSON 对象示例:解释 Stringify 和 Parse 方法

> 原文：<https://www.freecodecamp.org/news/json-stringify-method-explained/>

## **JSON Stringify**

`JSON.stringify()`方法将一个 *JSON 安全的* JavaScript 值转换成一个 JSON 兼容的字符串。

有人可能会问，JSON 安全的值是什么！让我们列出所有 JSON 不安全的值，任何不在列表上的都可以被认为是 JSON 安全的。

#### **JSON-不安全值:**

*   `undefined`
*   `function(){}`
*   (ES6+) `Symbol`
*   包含循环引用的对象

#### **语法**

```
 JSON.stringify( value [, replacer [, space]])
```

以其最简单和最常用的形式:

```
 JSON.stringify( value )
```

#### **参数**

`value`:要“字符串化”的 JavaScript 值。

`replacer`:(可选)一个函数或数组，作为包含在 JSON 字符串中的 value 对象属性的过滤器。

`space`:(可选)为 JSON 字符串提供缩进的数值或字符串值。如果提供了一个数值，那么多个空格(最多 10 个)将作为每个级别的缩进。如果提供了一个字符串值，则该字符串(最多前 10 个字符)将作为每个级别的缩进。

#### **返回类型**

方法的返回类型为:`string`。

## **描述**

JSON 安全值被转换成相应的 JSON 字符串形式。另一方面，JSON-unsafe 值返回:

*   如果它们作为值传递给方法
*   如果它们作为数组元素被传递
*   如果作为对象的属性传递，则为空
*   如果它是一个有循环引用的对象，抛出一个错误。

```
 //JSON-safe values
  JSON.stringify({});                  // '{}'
  JSON.stringify(true);                // 'true'
  JSON.stringify('foo');               // '"foo"'
  JSON.stringify([1, 'false', false]); // '[1,"false",false]'
  JSON.stringify({ x: 5 });            // '{"x":5}'
  JSON.stringify(new Date(2006, 0, 2, 15, 4, 5))  // '"2006-01-02T15:04:05.000Z"'

  //JSON-unsafe values passed as values to the method
  JSON.stringify( undefined );					// undefined
  JSON.stringify( function(){} );					// undefined

  //JSON-unsafe values passed as array elements
  JSON.stringify({ x: [10, undefined, function(){}, Symbol('')] });  // '{"x":[10,null,null,null]}' 

 //JSON-unsafe values passed as properties on a object
  JSON.stringify({ x: undefined, y: Object, z: Symbol('') });  // '{}'

  //JSON-unsafe object with circular reference on it
  var o = { },
    a = {
      b: 42,
      c: o,
      d: function(){}
    };

  // create a circular reference inside `a`
  o.e = a;

  // would throw an error on the circular reference
  // JSON.stringify( a );
```

如果传递给它的对象定义了一个`toJSON()`方法，那么`JSON.stringify(...)`的行为会有所不同。从`toJSON()`方法返回的值将被序列化，而不是对象本身。

当一个对象包含任何非法的 JSON 值时，这非常方便。

```
 //JSON-unsafe values passed as properties on a object
   var obj = { x: undefined, y: Object, z: Symbol('') };

   //JSON.stringify(obj);  logs '{}'
   obj.toJSON = function(){
    return {
      x:"undefined",
      y: "Function",
      z:"Symbol"
    }
   }
   JSON.stringify(obj);  //"{"x":"undefined","y":"Function","z":"Symbol"}"

  //JSON-unsafe object with circular reference on it
  var o = { },
    a = {
      b: 42,
      c: o,
      d: function(){}
    };

  // create a circular reference inside `a`
  o.e = a;

  // would throw an error on the circular reference
  // JSON.stringify( a );

  // define a custom JSON value serialization
  a.toJSON = function() {
    // only include the `b` property for serialization
    return { b: this.b };
  };

  JSON.stringify( a ); // "{"b":42}"
```

#### **`replacer`**

如前所述，`replacer`是一个过滤器，指示 JSON 字符串中包含哪些属性。它可以是数组，也可以是函数。当是数组时，replacer 只包含那些要包含在 JSON 字符串中的属性的字符串表示。

```
 var foo = {foundation: 'Mozilla', model: 'box', week: 45, transport: 'car', month: 7};
  JSON.stringify(foo, ['week', 'month']);    // '{"week":45,"month":7}', only keep "week" and "month" properties
```

如果`replacer`是一个函数，它会为对象本身调用一次，然后为对象中的每个属性调用一次，每次传递两个参数， *key* 和 *value* 。要跳过序列化中的一个*键*，应该返回`undefined`。否则，应返回提供的*值*。如果这些*值*中的任何一个本身就是对象，`replacer`函数也会递归地序列化它们。

```
 function replacer(key, value) {
    // Filtering out properties
    if (typeof value === 'string') {
      return undefined;
    }
    return value;
  }

  var foo = {foundation: 'Mozilla', model: 'box', week: 45, transport: 'car', month: 7};
  JSON.stringify(foo, replacer);  // '{"week":45,"month":7}'
```

如果一个数组被传递给`JSON.stringify()`并且`replacer`为它的任何一个元素返回`undefined`，那么这个元素的值将被替换为`null`。`replacer`函数不能从数组中删除值。

```
 function replacer(key, value) {
    // Filtering out properties
    if (typeof value === 'string') {
      return undefined;
    }
    return value;
  }

  var foo = ['Mozilla', 'box', 45, 'car', 7];
  JSON.stringify(foo, replacer);  // "[null,null,45,null,7]"
```

#### **`space`**

用于缩进的`space`参数使`JSON.stringify()`的结果更漂亮。

```
 var a = {
    b: 42,
    c: "42",
    d: [1,2,3]
  };

  JSON.stringify( a, null, 3 );
  // "{
  //    "b": 42,
  //    "c": "42",
  //    "d": [
  //       1,
  //       2,
  //       3
  //    ]
  // }"

  JSON.stringify( a, null, "-----" );
  // "{
  // -----"b": 42,
  // -----"c": "42",
  // -----"d": [
  // ----------1,
  // ----------2,
  // ----------3
  // -----]
  // }"
```

## **JSON 解析**

`JSON.parse()`方法解析一个字符串并构造一个由字符串描述的新对象。

### 语法:

```
 JSON.parse(text [, reviver])
```

### 参数:

`text`要解析为 JSON 的字符串

`reviver`(可选)该函数将接收`key`和`value`作为参数。该函数可用于转换结果值。

下面是一个如何使用`JSON.parse()`的例子:

```
var data = '{"foo": "bar"}';

console.log(data.foo); // This will print `undefined` since `data` is of type string and has no property named as `foo`

// You can use JSON.parse to create a new JSON object from the given string
var convertedData = JSON.parse(data);

console.log(convertedData.foo); // This will print `bar
```

[Repl.it 演示](https://repl.it/MwgK/0)

下面是一个关于`reviver`的例子:

```
var data = '{"value": 5}';

var result = JSON.parse(data, function(key, value) {
    if (typeof value === 'number') {
        return value * 10;
    }
    return value;
});

// Original Data
console.log("Original Data:", data); // This will print Original Data: {"value": 5}
// Result after parsing
console.log("Parsed Result: ", result); // This will print Parsed Result:  { value: 50 }
```

在上面的例子中，所有的数值都乘以`10` - [Repl.it Demo](https://repl.it/Mwfp/0)

## 关于 JSON 的更多信息:

*   [JSON 语法](https://guide.freecodecamp.org/javascript/standard-objects/json/json-syntax)
*   [用 7 行 JSON 把你的网站变成一个移动应用](https://www.freecodecamp.org/news/how-to-turn-your-website-into-a-mobile-app-with-7-lines-of-json-631c9c9895f5/)