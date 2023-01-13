# Immutable.js 令人生畏。以下是开始的方法。

> 原文：<https://www.freecodecamp.org/news/immutable-js-is-intimidating-heres-how-to-get-started-2db1770466d6/>

威廉·伍德黑德

# Immutable.js 令人生畏。以下是开始的方法。

![c6rT3CXldWH6wrFhcaG3CTMEgadnyNo6uvMw](img/7faad6292768a674cc2b8ea270890d0f.png)

Image [source](https://facebook.github.io/immutable-js/)

您听说您应该使用[不可变的](https://facebook.github.io/immutable-js/)。你知道你应该这样做，但是你不确定为什么。当你进入[文档](https://facebook.github.io/immutable-js/docs/#/)时，第一段代码看起来像这样:

```
identity<T>(value: T): T
```

你想:不…也许下次吧。

所以，这里有一个简单快速的介绍，让你开始使用不可变的。你不会后悔的:

大约 12 个月前，在[皮尔克罗](https://www.pilcro.com/?utm_source=medium&utm_medium=immutable&utm_campaign=awareness)，我们将[不可变](https://facebook.github.io/immutable-js/)引入到我们的应用程序中。这是我们做出的最好的决定之一。我们的应用程序现在可读性更强，更健壮，没有错误，更可预测。

### **基础知识**

#### 转换成不可变的

在普通 JavaScript 中，我们知道两种常见的数据类型:**对象** `{}`和**数组** `[]`。

要将这些转化为不可变的:

*   **对象** `{}` 变成了**贴图** `Map({})`
*   **阵**变为**阵**阵`List([])`

要将普通的 JavaScript 转换成不可变的，我们可以使用不可变提供的**映射**、**列表、**或 **fromJS** 函数:

```
import { Map, List, fromJS } from 'immutable';
```

```
// Normal Javascript
```

```
const person = {  name: 'Will',  pets: ['cat', 'dog']};
```

```
// To create the equivalent in Immutable:
```

```
const immutablePerson = Map({  name: 'Will',  pets: List(['cat', 'dog'])});
```

```
// Or ...
```

```
const immutablePerson = fromJS(person);
```

`fromJS`是一个有用的函数，将嵌套数据转换成不可变的。它在转换中创建了`Maps`和`Lists`。

#### 从不可变的 JavaScript 转换回普通的 JavaScript

将数据从不可变恢复为普通的旧 JavaScript 非常简单。您只需在不可变对象上调用`.toJS()`方法。

```
import { Map } from 'immutable';
```

```
const immutablePerson = Map({ name: 'Will' });const person = immutablePerson.toJS();
```

```
console.log(person); // prints { name: 'Will' };
```

> ***主题演讲:* *数据结构应该被认为是普通的 JavaScript 或者是不可变的。***

### 开始使用不可变

在解释为什么不可变是如此有用之前，这里有三个简单的例子说明不可变可以马上帮你解决问题。

#### 1.从对象获取嵌套值，而不检查它是否存在

普通 JavaScript 中的第一个:

```
const data = { my: { nested: { name: 'Will' } } };
```

```
const goodName = data.my.nested.name;console.log(goodName); // prints Will
```

```
const badName = data.my.lovely.name;// throws error: 'Cannot read name of undefined'
```

而现在在不可改变的:

```
const data = fromJS({ my: { nested: { name: 'Will' } } });
```

```
const goodName = data.getIn(['my', 'nested', 'name']);console.log(goodName); // prints Will
```

```
const badName = data.getIn(['my', 'lovely', 'name']);console.log(badName); // prints undefined - no error thrown
```

在上面的例子中，普通的 JavaScript 代码会抛出错误，而不可变的代码不会。

这是因为我们使用了`getIn()`函数来获取一个嵌套值。如果键路径不存在(也就是说，对象的结构不像您想的那样)，它将返回 undefined，而不是抛出一个错误。

您不需要像在普通 JavaScript 中那样，在嵌套结构中检查所有未定义的值:

```
if (data && data.my && data.my.nested && data.my.nested.name) { ...
```

这个简单的特性使得你的代码可读性更强，更简洁，更健壮。

#### 2.链接操作

普通 JavaScript 中的第一个:

```
const pets = ['cat', 'dog'];pets.push('goldfish');pets.push('tortoise');console.log(pets); // prints ['cat', 'dog', 'goldfish', 'tortoise'];
```

现在在不可变的:

```
const pets = List(['cat', 'dog']);const finalPets = pets.push('goldfish').push('tortoise');
```

```
console.log(pets.toJS()); // prints ['cat', 'dog'];
```

```
console.log(finalPets.toJS());// prints ['cat', 'dog', 'goldfish', 'tortoise'];
```

因为`List.push()`返回操作的结果，我们可以将下一个操作“链接”到它上面。在普通的 JavaScript 中，`push`函数返回新数组的长度。

这是一个非常简单的链接示例，但是它展示了不可变的真正威力。

这使您能够以更实用、更简洁的方式进行各种数据操作。

> ***Keynote:对不可变对象的操作返回操作的结果。***

#### 3.不可变数据

它毕竟叫不可变，所以我们需要谈谈为什么这很重要！

假设您创建了一个不可变的对象并更新了它——使用不可变，初始数据结构不会改变。它是不可改变的。(此处小写！)

```
const data = fromJS({ name: 'Will' });const newNameData = data.set('name', 'Susie');
```

```
console.log(data.get('name')); // prints 'Will'console.log(newNameData.get('name')); // prints 'Susie'
```

在本例中，我们可以看到原始的“数据”对象是如何保持不变的。这意味着当您将名称更新为“Susie”时，不会出现任何不可预测的行为。

这个简单的特性非常强大，尤其是在构建复杂的应用程序时。它是不可变的一切的支柱。

> ***Keynote* : *对不可变对象的操作不会改变对象，而是创建一个新的对象。***

### 为什么不可变是有用的

在[脸书](https://www.facebook.com)的开发人员在文档的[主页](https://facebook.github.io/immutable-js/)上总结了好处，但是读起来很棘手。以下是我对您应该开始使用不可变的原因的看法:

#### 您的数据结构会发生可预见的变化

因为您的数据结构是不可变的，所以您负责如何操作您的数据结构。在复杂的 web 应用程序中，这意味着当您更改 UI 访问的一点数据时，不会出现有趣的重新呈现问题。

#### **强大的数据操作**

通过使用不可变来操作数据结构，您的操作本身更不容易出错。Immutable 为您做了大量艰苦的工作——它捕捉错误，提供默认值，并构建开箱即用的嵌套数据结构。

#### 简明易读的代码

不可变的函数设计一开始可能会令人困惑，但是一旦你习惯了，函数链会使你的代码更短，可读性更好。这对于在相同代码基础上工作的团队来说非常好。

### 后续步骤

学习曲线不可否认是棘手的不可变的，但真的值得。开始玩吧。

这是我们浏览时记录的主题演讲。如果你能记住这些，你将会如鱼得水。

1.  数据结构应该被认为是普通的 JavaScript 或者不可变的。
2.  对不可变对象的操作返回操作的结果。
3.  对不可变对象的操作不会改变对象本身，而是创建一个新的对象。

祝你好运！

*如果你喜欢这个故事，请问？并请与他人分享。也请检查我的公司 p[ilcro.com。](https://www.pilcro.com/?utm_source=medium&utm_medium=immutable&utm_campaign=awareness) Pilcro 是 G-Suite 的品牌软件，面向营销人员和品牌代理。*

![ntTfRnp-62XC61l4808bH5vhCSTaRhOs7opG](img/c21b41c6f4a9c23cd72abb23869c424f.png)