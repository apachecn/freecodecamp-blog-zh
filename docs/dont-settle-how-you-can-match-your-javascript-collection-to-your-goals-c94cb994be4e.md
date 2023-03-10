# 不要放弃:如何让您的 JavaScript 集合与您的目标相匹配

> 原文：<https://www.freecodecamp.org/news/dont-settle-how-you-can-match-your-javascript-collection-to-your-goals-c94cb994be4e/>

乔·摩根

# 不要放弃:如何让您的 JavaScript 集合与您的目标相匹配

![H5NZkAJJlj3ziw8M5fBVtMUXxuWctECDURYi](img/a8d61785cbce8e384d7674fdbacc2548.png)

即使全世界都讨厌 JavaScript，我还是喜欢它。但我总是很难证明收藏的合理性。对象是一种键值存储，但不是真的。如果你需要排序或者循环的话，它们就太可怕了。数组更容易循环，但是如果你需要取出一个没有索引的特定值，就显得笨拙了。

可悲的事实是，大多数时候开发人员只会先抓住他们脑海中出现的任何集合。然后用转换来折磨它，以得到他们需要的任何东西。

很痛苦。

现在情况不同了。还有更多收藏类型。还有更多方法。有更好的技术，甚至一行表达式来在集合类型之间切换。那么，为什么当我们选择一个收藏类型时，看起来我们仍然只是在猜测呢？

我经常谈论 JavaScript 和代码质量。我[写过关于新语法的书](https://pragprog.com/book/es6tips/simplifying-javascript)。但是我发现大多数开发人员，即使是有经验的开发人员，也不会在他们的集合选择上花太多心思。

是时候结束了。我们不仅有数组和对象。我们也有地图、套装、武器地图和武器套装。

那么应该选择哪个呢？每个系列都有起伏。但总的来说，有三大因素会影响你的选择:

*   **Iterable** :你能直接遍历集合，一次访问一个成员吗？
*   **Keyed** :你能使用一个特殊的键找到一个值而不用担心其他集合成员吗？
*   **可析构的**:你能快速方便地将集合的片段放入变量中吗？

每种集合类型在某些方面很强，而在其他方面很弱。还有其他优点和缺点，但这是大多数代码的三大优点。

### 数组

数组可能是最灵活的集合。这让它们成为探索收藏的绝佳切入点。

这是我小学老师的名单(为了保护无辜，化名)。

```
const myTeachers = ['Cooper', 'Simes', 'Butler'];
```

数组保留了顺序，这很好，因为这是我的一年级、二年级和三年级老师的正确顺序。因为顺序是有意义的，而且我碰巧知道它的意思，所以我可以使用**析构**将项目提取到变量中。

```
const [firstGrade, secondGrade, ...others] = myTeachers.
```

析构是一种将集合中的信息提取到独立变量中的快速方法。它还可以创建信息子集。变量`firstGrade`的值为`Cooper`。下一个变量，`secondGrade`是`Simes`。最后一个变量有一点不同，使用三个点`...`我们保存其余的变量，在这个例子中是一个，作为一个单独的数组。那说明`others`也是一个数组:`['Butler']`。在这种情况下，`...`是一个静止图形。毕竟，我们在保留其余的价值。

回到我收藏的老师。假设你不关心他们教我的顺序，你对按字母顺序排列的列表更感兴趣。现在，在你对数组排序之前，记住，排序是一个变异函数，所以这个动作会改变原来的数组。因为顺序很重要，所以你不想做永久性的改变。幸运的是，这不是一个大问题，因为您可以用 spread 操作符:`[...myTeachers]`创建一个新数组。

您可以使用 spread 操作符的原因是数组有一个内置的迭代器，它允许您一次对一个集合进行操作。换句话说，数组是**可迭代的**。spread 操作符接受这些项目，一次创建一个新列表。将所有内容放在方括号中会创建一个新数组。iterable 属性还可以让你做一些很好的事情，比如使用`for...of`进行循环，或者像使用`.map` 和`.reduce`方法一样对数组的每一项运行一个函数。

回到排序。现在你可以做一个新的数组，你不必担心突变。排序函数是一行程序。

```
const sortedTeachers = [...myTeachers].sort();
```

因为数组是可析构的和可迭代的，所以你可以很容易地提取信息或者一次访问一个项目。看起来很棒，对吧？当然，数组并不完美。假设你不知道数组的顺序，但是你确实想要我的二年级老师。现在你有一个问题。因为数组不是键控的，所以您不能只提取您需要的信息。最好的方法是创建一个复杂的 pairs 数组——一个包含两个条目的数组——以及一个查找所需条目的数组方法。

```
export const myTeacherPairs = [['first', 'Cooper'], ['second', 'Simes'], ['third', 'Butler']];
```

```
return teachers.find(teacherPair => teacherPair[0] === 'second');
```

这有点笨重。pairs 数组非常重要，稍后您会看到更多 pairs，但这是一种非常低效的数据查找方式。

问题是集合不是**键控的**。数组擅长很多事情，但键值存储不是。所以你陷入了困境。你喜欢析构属性和可重复属性。但是检索键值信息并不简单。这通常是开发人员转向对象的时候。但在此之前，这里有一个阵列的总结:

✅可摧毁的

可重复

❌ Keyed

### 目标

大多数 JavaScript 开发人员在需要存储键值对时会本能地使用对象。JavaScript 中的对象会变得非常复杂。对于这个例子来说，可以把它们想象成一种简单的传递数据的方式。没有功能。没有`this`关键字。只有键值对。

首先，将我的教师数组重写为一个对象:

```
const teachers = {  first: "Cooper",  second: "Simes",  third: "Butler",};
```

如果你需要找的只是我的二年级老师，不关心对象的其他部分，可以直接拉出来:`teachers.second`。更好的是，您可以通过析构将值直接放入变量。只要变量匹配对象键，这就是一个简单的赋值。

```
const { second } = teachers;
```

析构可能是传递数据时使用对象的最大原因。您可以轻松地在一个对象中收集多个值，然后将它传递给另一个函数，在那里您可以再次提取信息。

```
function getSecondGradeInfo({ second }) {  const school = "Lakin";  return {    school,    teacher: second  }}
```

当您想要迭代对象时，事情就开始变得混乱了。你要么必须将对象的某个部分转换成一个数组，要么必须使用一个`for...in`循环。

```
Object.keys(teachers).map(grade => `${grade}:${teachers[grade]}`).join(' ');
```

```
let myTeacher = '';for(const grade in teachers) {  myTeacher += ` ${grade}:${teachers[grade]}`;}
```

事情已经变得有点乱了。你要么遍历所有的键，要么改变一个变量。更糟糕的是，对象不能保证秩序。你永远不能相信你的`for...in`循环会按照你想要的顺序给出结果。

所有这些问题都源于对象缺乏可迭代属性。`Object.keys`和`for...in`都使用迭代器，迭代器来自于将键转换成数组。

除此之外，对象还有其他问题。关键选项有限。例如，你不能使用[数字](http://speakingjs.com/es5/ch17.html#object_literals)作为键。它们不如其他系列的[性能](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Keyed_collections_Maps_Sets_WeakMaps_WeakSets)。但是在大多数情况下，它们有利于传递数据而没有很多循环。

总结一下:

✅可摧毁的

❌迭代

✅ Keyed

### 地图

地图是 JavaScript 的一种新的集合类型。它们是一个合适的键值存储。这只是一种说法，他们是专门为此目的而设计的。与对象不同，您仍然需要显式创建一个新实例:

```
const teachers = new Map();
```

之后，通过传递一个键和一个值作为参数来设置每一项。

```
teachers.set('first', 'Cooper').set('second', 'Simes').set('third', 'Butler);
```

要检索一个值，需要调用`get()`方法和键名。

```
teachers.get('second'); // Simes
```

这可能看起来有点熟悉。这和你可能用来在访问网络应用程序之间存储数据的`localStorage.setItem(key, value`没有太大的不同。

作为键值存储，地图非常棒。他们采用了更多种类的钥匙。虽然一个对象的查找时间是线性的，但是一个地图的查找时间将是对数的。

但是最大的优点是它们有一个内置的 iterable。这意味着您可以直接访问键值对:

```
let myTeachers = '';for([grade, name] of teachers) {  myTeachers += ` ${grade}:${name}`;}
```

有几件事要说。看起来好像是在 for 循环的开始处对地图进行了析构。你其实不是。当您迭代一个 map 时，您会得到一个包含键和值的对。所以你实际上是在析构一个数组。

接下来，你可能会注意到你仍然在突变一个变量。那不太好。幸运的是，由于 map 是可迭代的，所以可以使用 spread 操作符将 map 转换成一个数组对。这意味着

```
[...teachers]
```

会变成

```
[  ['first', 'Cooper'],  ['second', 'Simes'],  ['third', 'Butler'],]
```

将映射转换为数组后，现在可以映射项目来创建字符串。

```
[...teachers].map(([grade, name]) => `${grade}:${name}`).join();
```

这意味着您可以用三个短字符访问所有的数组方法。

哦，因为它是可迭代的，所以顺序保持不变。开头加你一年级老师，开头就明白了。

那么缺点是什么呢？最大的问题是缺乏直接的析构。如果不调用`get()`方法或者不转换成数组，就不能取出一个值。这意味着在函数之间传递数据变得有点冗长。您也不能将地图表示为 JSON 字符串。所以你不会很快从 API 中获取地图。

尽管如此，如果你有数据，你知道你会想循环，同时仍然保留的能力，拉数据快速与一个关键，地图是你最好的选择。

总结一下:

❌可摧毁的

可重复

✅ Keyed

### 在之间移动

你可能注意到一个令人沮丧的模式。没有一个系列可以做到这一切。

幸运的是，这并不像你想的那样是个大问题。

您已经看到可以使用 spread 操作符将地图转换为数组。您也可以反其道而行之，通过将数组传递给构造函数，将数组转换为映射。

您还可以通过传递一组对来加速初始创建过程:

```
const myTeachers = [  ['first', 'Cooper'],  ['second', 'Simes'],  ['third', 'Butler'],]const teachers = new Map(myTeachers);
```

此时，当您拥有一个映射时，您基本上可以访问数组的属性和映射的属性，只需稍作转换。

对象越来越好。使用`Object.entries()`你可以从一个对象创建一个数组。

```
const teachers = {  first: "Cooper",  second: "Simes",  third: "Butler",};
```

```
const myTeachers = Object.entries(teachers);
```

```
myTeachers[0]// ['first', 'Cooper']
```

这意味着当你有一个对象时，你可以通过简单的转换来访问数组方法。然而，回去有点困难。目前有一个针对`Object.fromEntries()`的[提议](https://github.com/tc39/proposal-object-from-entries)，它将接受一个 pairs 数组并创建一个对象。但那还是有点远。

然而，一旦它在这里，你不会被锁定到任何特定的集合类型。这是最好的消息。一旦您可以在不同的集合之间轻松快速地移动，您就可以开始利用每个集合的最佳特性了。方便时析构值，需要时迭代。

换句话说，您可以查看您拥有的数据和您需要的代码，并选择合适的集合类型。如果另一个函数需要不同的类型，编写一个简单的转换。不再猜测。不再有挫折。您可以将您的集合与您的目标相匹配，而不是将您的代码与您的集合相匹配。