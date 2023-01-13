# ES2018: Javascript 在 2018 年的新特性

> 原文：<https://www.freecodecamp.org/news/es9-javascripts-state-of-art-in-2018-9a350643f29c/>

弗拉维奥·h·弗雷塔斯

# ES2018: Javascript 在 2018 年的新特性

我们来自 TC39 的朋友已经为我们钟爱的 JavaScript 语言发布了新的更新。

![bocZipyVNaJoZ43Q0DuUVShsFUBtYgbmhOAH](img/e2c5c7e78b42e48051dfc4fff5fad93c.png)

如果您想了解委员会发布新版本的过程，您可以访问此[链接](https://github.com/tc39/proposals/blob/master/finished-proposals.md)。批准和做出变更的过程分为五个阶段:

*   阶段 0:稻草人—允许输入到规范中
*   第 1 阶段:提案——提出增加的理由；描述溶液的形状；识别潜在挑战
*   阶段 2:草稿——使用正式规范语言精确描述语法和语义
*   阶段 3:候选——指出进一步的细化将需要来自实现和用户的反馈
*   阶段 4:完成——表示添加内容已经准备好包含在正式的 ECMAScript 标准中

更多细节可以看[这里](https://tc39.github.io/process-document/)。如果你想了解更多以前的变化，请查看 [ES6](https://medium.com/@flaviohfreitas/es6-javascript-does-love-you-f36c532c87db) 、 [ES7](https://medium.com/@flaviohfreitas/es7-a-simple-and-useful-guide-to-master-it-6aba54abb4df) 和 [ES8](https://medium.freecodecamp.org/es8-the-new-features-of-javascript-7506210a1a22) 。

所以，让我们看看他们在过去的一年中增加或更新了什么:

### 1.大量正则表达式更改

我们对正则表达式做了四点修改。让我们看看他们:

#### 正则表达式的`s` ( `dotAll`)标志

使用正则表达式时，您希望点`.`匹配单个字符，但这并不总是正确的。一个例外是行结束符:

```
/hello.bye/.test('hello\nbye') // prints false
```

解决方案是新的标志(来自单线):

```
/hello.bye/s.test('hello\nbye')  // prints true
```

#### RegExp 命名的捕获组

这是从日期中获取年、月、日的老方法:

```
const REGEX = /([0-9]{4})-([0-9]{2})-([0-9]{2});const results = REGEX.exec('2018-07-12');console.log(results[1]); // prints 2018console.log(results[2]); // prints 07console.log(results[3]); // prints 12
```

如果你正在处理一个长正则表达式，你就会知道跟踪组、括号和索引有多难。使用新命名的采集组，可以:

```
const REGEX = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2});const results = REGEX.exec('2018-07-12');console.log(results.groups.year);  // prints 2018console.log(results.groups.month); // prints 07console.log(results.groups.day);   // prints 12
```

#### 正则表达式隐藏在断言之后

有两种版本的后视断言:肯定的和否定的。

**a)阳性(？<** =…)

```
'$foo #foo @foo'.replace(/(?<=#)foo/g, 'XXX')// prints $foo #XXX @foo
```

这个`(?<=#)fo` o/g regex 说这个单词必须以#开头，它不会影响整个匹配的字符串(所以它不会替换#字符)。

**b)阴性(？<T1！…)**

```
'$foo #foo @foo'.replace(/(?<!#)foo/g, 'XXX')// prints $XXX #foo @XXX
```

相反，这个断言保证它不以#开头。

#### RegExp Unicode 属性转义

现在我们可以通过在`\p{}`中提到字符的 Unicode 字符属性来搜索字符

```
/\p{Script=Greek}/u.test('μ') // prints true
```

您可以点击此处的查看更多酒店信息。

### 2.休息/传播属性

rest 操作符`(...)`复制没有提到的剩余属性键。让我们看一个例子:

```
const values = {a: 1, b: 2, c: 3, d: 4};const {a, ...n} = values;console.log(a);   // prints 1console.log(n);   // prints {b: 2, c: 3, d: 4}
```

### 3.`Promise.prototype finally`

不管是否调用了 catch，这个新的回调将总是被执行。

```
fetch('http://website.com/files').then(data => data.json()).catch(err => console.error(err)).finally(() => console.log('processed!'))
```

### 4.异步迭代

终于！

现在我们可以在循环声明中使用`await`。

```
for await (const line of readLines(filePath)) {  console.log(line);}
```

这些都是今年以来的变化。让我们拭目以待，看看他们明年会给我们带来什么。

如果你喜欢这篇文章，一定要喜欢它，给我很多掌声——它对作者来说意味着一切？如果你想阅读更多关于文化、技术和创业的文章，请关注我。

Flávio H. de Freitas 是一名企业家、工程师、技术爱好者、梦想家和旅行家。先后在**【巴西】**【硅谷】和欧洲**担任 **CTO** 。**