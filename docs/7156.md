# 在不到 5 分钟的时间里学会这些简洁的 JavaScript 技巧

> 原文：<https://www.freecodecamp.org/news/9-neat-javascript-tricks-e2742f2735c3/>

作者阿尔西德斯·奎罗斯

# 在不到 5 分钟的时间里学会这些简洁的 JavaScript 技巧

#### 专业人士使用的省时技巧

![QuQa8vSRsqE3AzHx-lx1Sdxf4jKSObxgHKqd](img/27c77bfabd47a459fae072f244bd0c68.png)

#### 1.清除或截断数组

在不重新分配数组的情况下，清除或截断数组的一种简单方法是更改其`length`属性值:

```
const arr = [11, 22, 33, 44, 55, 66];
```

```
// truncantingarr.length = 3;console.log(arr); //=> [11, 22, 33]
```

```
// clearingarr.length = 0;console.log(arr); //=> []console.log(arr[2]); //=> undefined
```

#### 2.用对象析构来模拟命名参数

当您需要将一组可变选项传递给某个函数时，您很可能已经在使用配置对象了，如下所示:

```
doSomething({ foo: 'Hello', bar: 'Hey!', baz: 42 });
```

```
function doSomething(config) {  const foo = config.foo !== undefined ? config.foo : 'Hi';  const bar = config.bar !== undefined ? config.bar : 'Yo!';  const baz = config.baz !== undefined ? config.baz : 13;  // ...}
```

这是一种古老但有效的模式，它试图模拟 JavaScript 中的命名参数。函数调用看起来没问题。另一方面，配置对象处理逻辑不必要地冗长。借助 ES2015 对象析构，您可以规避这一缺点:

```
function doSomething({ foo = 'Hi', bar = 'Yo!', baz = 13 }) {  // ...}
```

如果您需要使 config 对象可选，也非常简单:

```
function doSomething({ foo = 'Hi', bar = 'Yo!', baz = 13 } = {}) {  // ...}
```

#### 3.数组项的对象析构

使用对象析构将数组项赋给单个变量:

```
const csvFileLine = '1997,John Doe,US,john@doe.com,New York';
```

```
const { 2: country, 4: state } = csvFileLine.split(',');
```

#### 4.切换范围

> 注意:经过一番思考，我决定将这个技巧与本文中的其他技巧区分开来。这是**而不是**一种省时的技术，**也不是**一种适合现实生活的代码。记住:“如果”在这种情况下总是更好。与这篇文章中的其他技巧不同，它更像是一种好奇心，而不是真正有用的东西。
> 反正**因为历史原因我还是留在这篇文章里吧。**

这里有一个在 switch 语句中使用范围的简单技巧:

```
function getWaterState(tempInCelsius) {  let state;    switch (true) {    case (tempInCelsius <= 0):       state = 'Solid';      break;    case (tempInCelsius > 0 && tempInCelsius < 100):       state = 'Liquid';      break;    default:       state = 'Gas';  }
```

```
 return state;}
```

#### 5.用 async/await 等待多个异步函数

使用`Promise.all`可以完成`await`多个异步函数:

```
await Promise.all([anAsyncCall(), thisIsAlsoAsync(), oneMore()])
```

#### 6.创建纯对象

你可以创建一个 100%纯的对象，它不会从`Object`继承任何属性或方法(例如，`constructor`，`toString()`等等)。

```
const pureObject = Object.create(null);
```

```
console.log(pureObject); //=> {}console.log(pureObject.constructor); //=> undefinedconsole.log(pureObject.toString); //=> undefinedconsole.log(pureObject.hasOwnProperty); //=> undefined
```

#### 7.格式化 JSON 代码

可以做的不仅仅是简单的字符串化一个对象。您还可以用它来美化您的 JSON 输出:

```
const obj = {   foo: { bar: [11, 22, 33, 44], baz: { bing: true, boom: 'Hello' } } };
```

```
// The third parameter is the number of spaces used to // beautify the JSON output.JSON.stringify(obj, null, 4); // =>"{// =>    "foo": {// =>        "bar": [// =>            11,// =>            22,// =>            33,// =>            44// =>        ],// =>        "baz": {// =>            "bing": true,// =>            "boom": "Hello"// =>        }// =>    }// =>}"
```

#### 8.从数组中删除重复项

通过将 ES2015 Sets 与 Spread 运算符一起使用，您可以轻松地从阵列中移除重复项:

```
const removeDuplicateItems = arr => [...new Set(arr)];
```

```
removeDuplicateItems([42, 'foo', 42, 'foo', true, true]);//=> [42, "foo", true]
```

#### 9.展平多维数组

使用 Spread 运算符展平数组是很简单的:

```
const arr = [11, [22, 33], [44, 55], 66];
```

```
const flatArr = [].concat(...arr); //=> [11, 22, 33, 44, 55, 66]
```

不幸的是，上面的技巧只适用于二维数组。但是通过递归调用，我们可以使它适用于二维以上的数组:

```
function flattenArray(arr) {  const flattened = [].concat(...arr);  return flattened.some(item => Array.isArray(item)) ?     flattenArray(flattened) : flattened;}
```

```
const arr = [11, [22, 33], [44, [55, 66, [77, [88]], 99]]];
```

```
const flatArr = flattenArray(arr); //=> [11, 22, 33, 44, 55, 66, 77, 88, 99]
```

现在你知道了！希望这些整洁的小技巧能帮助你写出更好更漂亮的 JavaScript。