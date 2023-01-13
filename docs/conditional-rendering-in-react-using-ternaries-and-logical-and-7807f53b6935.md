# React 中使用三元组和逻辑 and 的条件呈现

> 原文：<https://www.freecodecamp.org/news/conditional-rendering-in-react-using-ternaries-and-logical-and-7807f53b6935/>

多纳文·韦斯特

# React 中使用三元组和逻辑 and 的条件呈现

![1*eASRJrCIVgsy5VbNMAzD9w](img/4e960712f9d7632908053a9c45390e7c.png)

Photo by [Brendan Church](https://unsplash.com/photos/pKeF6Tt3c08?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/road-sign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

React 组件可以通过多种方式决定要呈现的内容。您可以使用传统的`if`语句或`switch`语句。在本文中，我们将探索一些替代方案。但是要注意，如果你不小心的话，有些人会有自己的陷阱。

### 三元 vs if/else

假设我们有一个组件被传递了一个`name`属性。如果字符串非空，我们显示一个问候语。否则我们告诉用户他们需要登录。

这里有一个无状态功能组件(SFC)可以做到这一点。

```
const MyComponent = ({ name }) => {  if (name) {    return (      <div className="hello">        Hello {name}      </div>    );  }  return (    <div className="hello">      Please sign in    </div>  );};
```

很简单。但是我们可以做得更好。这里是用一个**条件三元运算符**编写的同一个组件。

```
const MyComponent = ({ name }) => (  <div className="hello">    {name ? `Hello ${name}` : 'Please sign in'}  </div>);
```

请注意，与上面的示例相比，这段代码非常简洁。

一些需要注意的事情。因为我们使用的是 arrow 函数的单语句形式，所以隐含了`return`语句。此外，使用三元组允许我们消除重复的`<div className="hell` o" >标记。？

### 三元 vs 逻辑与

正如你所看到的，ternaries 对于`if/else`条件来说是很棒的。但是简单的`if`条件呢？

让我们看另一个例子。如果`isPro`(布尔值)是`true`，我们将显示一个奖杯表情符号。我们也要渲染星星的数量(如果不是零)。我们可以这样做。

```
const MyComponent = ({ name, isPro, stars}) => (  <div className="hello">    <div>      Hello {name}      {isPro ? '?' : null}    </div>    {stars ? (      <div>        Stars:{'⭐️'.repeat(stars)}      </div>    ) : null}  </div>); 
```

但是请注意“else”条件返回`null`。这是因为三元组需要一个 else 条件。

对于简单的`if`条件，我们可以使用更合适的东西:**逻辑 AND 运算符**。下面是使用逻辑 AND 编写的相同代码。

```
const MyComponent = ({ name, isPro, stars}) => (  <div className="hello">    <div>      Hello {name}      {isPro && '?'}    </div>    {stars && (      <div>        Stars:{'⭐️'.repeat(stars)}      </div>    )}  </div>); 
```

没有太大的不同，但是请注意我们是如何在每个三元组的末尾消除`: null`(即 else 条件)的。一切都应该像以前一样渲染。

嘿！约翰怎么了？有一个`0`不应该呈现任何内容。这就是我上面提到的问题。原因如下。

[根据 MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators) ，一个逻辑与(即`&&`):

> `*expr1* && *expr2*`

> 如果可以转换为`false`则返回`expr1`；否则，返回`expr2`。因此，当与布尔值一起使用时，如果两个操作数都为真，`&&`将返回`true`；否则，返回`false`。

好了，在你开始拔头发之前，让我给你分解一下。

在我们的例子中，`expr1`是变量`stars`，它的值是`0`。因为 zero 为 falsey，所以返回并呈现`0`。看，还不算太糟。

我会简单地写这个。

> 如果`expr1`为假，则返回`expr1`，否则返回`expr2`。

因此，当使用非布尔值的逻辑 AND 时，我们必须让 falsey 值返回 React 不会呈现的内容。比如说一个值`false`。

有几种方法可以实现这一点。让我们试试这个。

```
{!!stars && (  <div>    {'⭐️'.repeat(stars)}  </div>)}
```

注意`stars`前面的双爆炸运算符(即`!!`)。(嗯，其实没有“双爆炸算子”。我们只是使用了两次 bang 运算符。)

第一个 bang 运算符会将`stars`的值强制转换为布尔值，然后执行 NOT 运算。如果`stars`是`0`，那么`!stars`就会产生`true`。

然后我们执行第二个非运算，所以如果`stars`为 0，`!!stars`就会产生`false`。正是我们想要的。

如果你不是`!!`的粉丝，你也可以像这样强制一个 boolean(我觉得有点罗嗦)。

```
{Boolean(stars) && (
```

或者简单地给出一个产生布尔值的比较器(有些人可能会说这更符合语义)。

```
{stars > 0 && (
```

#### 关于字符串的一个词

空字符串值遇到了与数字相同的问题。但是因为呈现的空字符串是不可见的，所以这不是一个您可能必须处理或者甚至会注意到的问题。然而，如果你是一个完美主义者，不希望你的 DOM 上有一个空字符串，你应该像我们对上面的数字一样采取类似的预防措施。

### 另一种解决方案

一个可能的解决方案是创建一个单独的`shouldRenderStars`变量，这也是一个将来可以扩展到其他变量的解决方案。然后，您将在逻辑 AND 中处理布尔值。

```
const shouldRenderStars = stars > 0;
```

```
return (  <div>    {shouldRenderStars && (      <div>        {'⭐️'.repeat(stars)}      </div>    )}  </div>);
```

然后，如果在未来，业务规则是你也需要登录，拥有一只狗，喝淡啤酒，你可以改变如何计算`shouldRenderStars`,返回的将保持不变。您也可以将此逻辑放在可测试的地方，并保持呈现的明确性。

```
const shouldRenderStars =   stars > 0 && loggedIn && pet === 'dog' && beerPref === 'light`;
```

```
return (  <div>    {shouldRenderStars && (      <div>        {'⭐️'.repeat(stars)}      </div>    )}  </div>);
```

### 结论

我认为你应该充分利用这种语言。对于 JavaScript，这意味着对`if/else`条件使用条件三元运算符，对简单的`if`条件使用逻辑 And 运算符。

虽然我们可以退回到我们安全舒适的地方，在那里我们到处使用三元运算符，但你现在拥有知识和力量向前发展并取得成功。

我也为美国运通工程博客写稿。在 [AmericanExpress.io](http://americanexpress.io/) 查看我的其他作品和我才华横溢的同事的作品。你也可以[在推特](https://twitter.com/donavon)上关注我。