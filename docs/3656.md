# JavaScript 标准对象:地图

> 原文：<https://www.freecodecamp.org/news/javascript-standard-objects-maps/>

`Map`对象是一个相对较新的标准内置对象，它按照插入的顺序保存`[key, value]`对。

`Map`对象中的键和值可以是任何值(对象和原始值都有效)。

## **语法**

```
new Map([iterable])
```

## **参数**

****可迭代**** 元素为键值对的数组或其他可迭代对象。

## **例子**

```
const myMap = new Map();
myMap.set('foo', 1);
myMap.set('bar', 2);
myMap.set('baz', 3);

myMap.get('foo');   // returns 1
myMap.get('baz');   // returns 3
myMap.get('hihi');  // return undefined

myMap.size;   // 3

console.log(myMap); // Map { 'foo' => 1, 'bar' => 2, 'baz' => 3 }
```

从现有的键-值对的 2D 数组中创建一个新的`Map`很容易:

```
const myArr = [['foo', 1], ['bar', 2], ['baz', 3]];
const arrMap = new Map(myArr);

console.log(arrMap); // Map { 'foo' => 1, 'bar' => 2, 'baz' => 3 }
```

您也可以在`Object.entries`的帮助下将对象转换为`Map`:

```
const myObj = {
  foo: 1,
  bar: 2,
  baz: 3
}
const objMap = new Map(Object.entries(myObj));

console.log(objMap); // Map { 'foo' => 1, 'bar' => 2, 'baz' => 3 }
```

## **Map.prototype.get**

从一个`Map`对象返回指定键的值。

## **语法**

```
myMap.get(key);
```

## **参数**

****键**** 必填。

## **例子**

```
const myMap = new Map();
myMap.set('foo',1);
myMap.set('bar',2);
myMap.set('baz',3);

myMap.get('foo');   // returns 1
myMap.get('baz');   // returns 3
myMap.get('hihi');  // return undefined
```

## **Map.prototype.set**

在`Map`对象中设置或更新具有指定键和值的元素。`set()`方法也返回了`Map`对象。

## **语法**

```
myMap.set(key, value);
```

## **参数**

*   ****键**** 必填
*   ****值**** 必填

## **例子**

```
const myMap = new Map();

// sets new elements
myMap.set('foo', 1);
myMap.set('bar', 2);
myMap.set('baz', 3);

// Updates an existing element
myMap.set('foo', 100);

myMap.get('foo');   // returns 100
```

因为`set()`返回它所操作的`Map`对象，所以该方法很容易被链接。例如，上面的代码可以简化为:

```
const myMap = new Map();

// sets new elements
myMap.set('foo', 1)
  .set('bar', 2)
  .set('baz', 3)
  .set('foo', 100); // Updates an existing element

myMap.get('foo');   // returns 100
```

## **Map.prototype.size**

返回`Map`对象中元素的数量。

## **语法**

```
myMap.size();
```

## **例子**

```
const myMap = new Map();
myMap.set('foo',1);
myMap.set('bar',2);
myMap.set('baz',3);

myMap.size(); // 3
```

## **Map.prototype.keys**

返回一个新的`Iterator`对象，该对象包含`Map`对象中每个元素按插入顺序排列的键。

## **语法**

```
myMap.keys()
```

## **例子**

```
const myMap = new Map();
myMap.set('foo',1);
myMap.set('bar',2);
myMap.set('baz',3);

const iterator = myMap.keys();

console.log(iterator.next().value); // 'foo'
console.log(iterator.next().value); // 'bar'
console.log(iterator.next().value); // 'baz'
```

## **Map.prototype.values**

返回一个迭代器对象，该对象包含了`Map`对象中每个元素的值，按插入顺序排列。

## **语法**

```
myMap.values()
```

## **例子**

```
const myMap = new Map();
myMap.set('foo',1);
myMap.set('bar',2);
myMap.set('baz',3);

const iterator = myMap.values();
console.log(iterator.next().value); // 1
console.log(iterator.next().value); // 2
console.log(iterator.next().value); // 3
```

## **Map.prototype.delete**

从`Map`对象中移除指定元素。返回是否找到密钥并成功删除。

## **语法**

```
myMap.delete(key);
```

## **参数**

****键**** 必填。

## **例子**

```
const myMap = new Map();
myMap.set('foo',1);
myMap.set('bar',2);
myMap.set('baz',3);

myMap.size(); // 3
myMap.has('foo'); // true;

myMap.delete('foo');  // Returns true. Successfully removed.

myMap.size(); // 2
myMap.has('foo');    // Returns false. The "foo" element is no longer present.
```

## **Map.prototype.entries**

返回一个新的`Iterator`对象，它包含按插入顺序排列的`Map`对象中每个元素的`[key, value]`对。

## **语法**

```
myMap.entries()
```

## **例子**

```
const myMap = new Map();
myMap.set('foo',1);
myMap.set('bar',2);
myMap.set('baz',3);

const iterator = myMap.entries();

console.log(iterator.next().value); // ['foo', 1]
console.log(iterator.next().value); // ['bar', 2]
console.log(iterator.next().value); // ['baz', 3]
```

## **Map.prototype.clear**

从一个`Map`对象中移除所有元素并返回`undefined`。

## **语法**

```
myMap.clear();
```

## **例子**

```
const myMap = new Map();
myMap.set('foo',1);
myMap.set('bar',2);
myMap.set('baz',3);

myMap.size(); // 3
myMap.has('foo'); // true;

myMap.clear(); 

myMap.size(); // 0
myMap.has('foo'); // false
```

## **Map.prototype.has**

给定一个包含元素的`Map`，`has()`函数允许您根据传递的键来确定元素是否存在于 Map 中。

`has()`函数返回一个 *`Boolean`图元*(可以是`true`或`false`)，表示地图中是否包含该元素。

您将一个`key`参数传递给`has()`函数，该函数将用于在 Map 中查找带有该键的元素。

示例:

```
// A simple Map
const campers = new Map();

// add some elements to the map
// each element's key is 'camp' and a number
campers.set('camp1', 'Bernardo');
campers.set('camp2', 'Andrea');
campers.set('camp3', 'Miguel');

// Now I want to know if there's an element
// with 'camp4' key:
campers.has('camp4');
// output is `false`
```

`campers`地图当前没有带`'camp4'`键的元素。因此，`has('camp4')`函数调用将返回`false`。

```
// If we add an element with the 'camp4' key to the map
campers.set('camp4', 'Ana');

// and try looking for that key again
campers.has('camp4');
// output is `true`
```

因为地图现在有一个带有`'camp4'`键的元素，所以这次`has('camp4')`函数调用将返回`true`！

在更真实的场景中，您可能不会自己手动将元素添加到地图中，因此在这些情况下,`has()`函数会变得非常有用。

## **Map.prototype.forEach**

对`Map`对象中的每个键值对执行传递的函数。返回`undefined`。

## **语法**

```
myMap.forEach(callback, thisArg)
```

## **参数**

*   ****回调**** 功能对每个元素执行。
*   ****thisArg**** 执行回调时使用的值。

## **例子**

```
const myMap = new Map();
myMap.set('foo',1);
myMap.set('bar',2);
myMap.set('baz',3);

function valueLogger(value) {
  console.log(`${value}`);
}

myMap.forEach(valueLogger);
// 1
// 2
// 3
```