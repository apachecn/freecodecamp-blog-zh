# 学习 React 中析构道具的基础知识

> 原文：<https://www.freecodecamp.org/news/the-basics-of-destructuring-props-in-react-a196696f5477/>

伊芙琳·陈

# 学习 React 中析构道具的基础知识

![pPG6THcM7ENK4gk0Jy9xcmbEEmaM2VErA49x](img/47f59b31cb1421e7c7e983dfb5758d0f.png)

当我第一次了解 ES6 时，我很犹豫要不要开始使用它。我听说了很多关于改进的好消息，但与此同时，我已经习惯了做事情的好方法，这里有一个新的语法扔给我学习。

在“如果它没有坏就不要修复它”的前提下，我暂时避免了它，但是最近我越来越喜欢它的简单性，以及它正在成为 JavaScript 标准的事实。

有了 React，它完全包含了 ES6 语法，析构为改进代码增加了许多好处。本文将介绍析构对象的基础知识，以及它如何应用于 React 中的道具。

### 破坏的原因

#### **提高可读性**

当你传递道具时，这是 React 的一个巨大优势。一旦你花时间去破坏你的道具，你就可以在每个道具前除掉`props / this.props`。

如果你把你的组件抽象成不同的文件，你也有一个方便的地方来快速引用你传递的道具，而不需要切换标签。这种双重检查有助于您捕捉错误，如传递过多的道具或错别字。

您可以进一步添加`propType`验证，它允许您定义传入的每个道具的类型。当您处于开发环境中时，如果类型不同于定义的类型，这个触发器会记录一个警告。

在复杂的应用程序中，很难跟踪属性，所以在传递属性时清楚地定义它们对任何阅读你的代码的人都有很大的帮助。

#### **更短的代码行**

请参见 ES6 之前的以下内容:

```
var object = { one: 1, two: 2, three: 3 }
```

```
var one = object.one;var two = object.two;var three = object.three
```

```
console.log(one, two, three) // prints 1, 2, 3
```

它又长又笨重，占用了太多的代码行。有了析构，你的代码变得更加清晰。

在下面的例子中，我们有效地将行数减少到了两行:

```
let object = { one: 1, two: 2, three: 3 }
```

```
let { one, two, three } = object;
```

```
console.log(one, two, three) // prints 1, 2, 3
```

#### **句法糖**

它让代码看起来更好，更简洁，就像知道自己在做什么的人写的一样。我在这里重申第一点，但是如果它能提高可读性，为什么不这样做呢？

### 功能组件与类别组件

React 中的析构对于函数组件和类组件都很有用，但实现方式稍有不同。

让我们考虑应用程序中的一个父组件:

```
import React, { Component } from 'react';
```

```
class Properties extends Component {  constructor() {    super();    this.properties = [      {        title: 'Modern Loft',        type: 'Studio',        location: {          city: 'San Francisco',          state: 'CA',          country: 'USA'        }      },      {        title: 'Spacious 2 Bedroom',        type: 'Condo',        location: {          city: 'Los Angeles',          state: 'CA',          country: 'USA'        }      },    ];  }
```

```
render() {    return (      <div>        <Listing listing={this.properties[0]} />        <Listing listing={this.properties[1]} />      </div>    );  }}
```

#### 功能组件

在本例中，我们希望从属性数组中传递一个`listing`对象给子组件进行渲染。

以下是功能组件的外观:

```
const Listing = (props) => (  <div>    <p>Title: {props.listing.title}</p>    <p>Type: {props.listing.type}</p>    <p>      Location: {props.listing.location.city},      {props.listing.location.state},      {props.listing.location.country}    </p>  </div>);
```

这段代码功能齐全，但看起来很糟糕！当我们到达这个`Listing`子组件时，我们已经知道我们正在引用一个列表，所以`props.listing`看起来感觉是多余的。通过析构可以使这段代码看起来更整洁。

我们可以在传递 props 参数的函数参数中实现这一点:

```
const Listing = ({ listing }) => (  <div>    <p>Title: {listing.title}</p>    <p>Type: {listing.type}</p>    <p>      Location: {listing.location.city},      {listing.location.state},      {listing.location.country}    </p>  </div>);
```

更好的是，我们可以进一步析构嵌套的对象，如下所示:

```
const Listing = ({  listing: {    title,    type,    location: {      city,      state,      country    }  }}) => (  <div>    <p>Title: {title}</p>    <p>Type: {type}</p>    <p>Location: {city}, {state}, {country}</p>  </div>);
```

你能看出这有多容易阅读吗？在这个例子中，我们已经析构了`listings`和`listing`中的键。

一个常见的陷阱是像下面这样只析构键，并试图访问对象:

```
{ location: { city, state, country } }
```

在这个场景中，我们不能通过名为 location 的变量访问`location`对象。

为了做到这一点，我们必须首先用一个简单的修正来定义它，如下所示:

```
{ location, location: { city, state, country } }
```

起初这对我来说并不明显，如果我想在析构一个像`location`这样的对象的内容后将它作为道具传递，我偶尔会遇到问题。现在你可以避免我犯的同样的错误了！

#### 类别组件

这个想法在类组件中非常相似，但是执行有点不同。

请看下面:

```
import React, { Component } from 'react';
```

```
class Listing extends Component {  render() {    const {      listing: {        title,        type,        location: {          city,          state,          country        }      }    } = this.props;
```

```
return (      <div>        <p>Title: {title}</p>        <p>Type: {type}</p>        <p>          Location: {city}, {state}, {country}        </p>      </div>    )  }}
```

您可能已经注意到，在父示例中，当我们在类组件中导入`React`时，我们可以析构`Component`对象。这对于功能组件来说是不必要的，因为我们不会为它们扩展`Component`类。

接下来，我们不再在参数中析构，而是在变量被调用的地方析构。例如，如果我们获取同一个`Listing`子组件并将其重构为一个类，我们将在引用属性的`render`函数中析构。

在类组件中析构的缺点是，每次在方法中使用它时，你都会析构相同的属性。虽然这可能是重复的，但我认为积极的一面是它清楚地概述了每种方法中使用的道具。

此外，您不必担心副作用，如意外更改变量引用。这种方法使您的方法保持独立和干净，这对于项目期间的其他操作(如调试或编写测试)来说是一个巨大的优势。

感谢阅读！如果这对你有帮助，请鼓掌和/或分享这篇文章，这样它也可以帮助别人！:)