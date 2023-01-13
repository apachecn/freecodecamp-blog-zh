# 如何在 JavaScript 中使用超级排序

> 原文：<https://www.freecodecamp.org/news/supercharged-sorts-in-javascript/>

最近有人问我一个关于筛选和排序数组的很棒的问题。起初，这似乎是微不足道的:

> 如果我有一个对象数组，并且我希望能够通过多个属性来`filter()`，我能做到吗？

答案当然是肯定的。绝对的。JavaScript 中`Array.filter()`的工作方式是可链接的。这意味着，当第一个`.filter()`函数返回时，它可以被直接输入到第二个`.filter()`，并且可以输入到任意多个过滤器。

但是如果我们想通过不止一个属性对 T1 进行排序，那就有点麻烦了。毕竟，如果我们先按一个属性排序，然后再按第二个属性排序，我们就失去了第一个属性。

如果我们用类似`.reduce()`的东西来代替呢？我们可以用它来将数组简化为一个对象，它的属性是第一个排序值，然后将每个属性设置为包含这些值的项目数组*，并对它们进行排序！*

就这样，我们掉进了兔子洞。一定有更简单的方法。

碰巧的是，有。一切又回到了从前。

## 第二节，和第一节一样

这里是我们需要开始的地方:考虑一下`Array.sort()`期望它的回调函数返回的值，给定一个回调函数，把`(a, b)`作为它的参数:

*   如果返回值小于零，则在排序顺序中，`a`将保持在`b`之前。
*   如果返回值大于零，`b`将在排序顺序中与`a`交换位置。
*   *如果返回值等于零，`a`和`b`的权重相同，因此保持不变。*

现在，需要注意的是:在这三种情况下，我们有三个值:0，-1 和 1。下面是 JavaScript 如何将它们强制为布尔值(真/假):

```
Boolean(-1) === true; 
Boolean(1) === true; 
// But:
Boolean(0) === false;
```

这对我们有什么帮助？这里我们有一些很好的信息:首先，如果在两个属性之间执行排序，并且属性是相同的，那么比较应该返回 0 或者一个布尔值`false`。因为零是唯一可以强制为假值的数字，所以任何相等的值都将给出错误的比较。

其次，我们可以使用那个`true`或`false`来确定我们是否需要钻得更深。

这是最后一页，对于那些已经看到 going:‌的人来说

```
return <the value of the first comparator, if it coerces to a Boolean true> 
    || <the value of a second one>;
```

## 等等，什么？

哈哈是啊。刚刚发生了什么？我们到底要归还什么？

使用内联 OR，`||`，告诉 return 语句计算要返回的值。第一个比较器是布尔型的`true`吗？如果没有，那么通过`||`树进行第一次比较，如果没有，返回最后一次比较的结果。

让我们通过一个实际的例子来解决这个问题(在 Tech.io 上运行代码 **[这里](https://tech.io/snippet/oJ0Ueod)** )。考虑一个由四个成员组成的数组:

```
const myArray = [
  {
    firstName: 'Bob',
    lastName: 'Francis', 
    age: 34,
    city: 'Holland', 
    state: 'Massachusetts', 
    country: 'USA', 
    online: true
  }, {
    firstName: 'Janet',
    lastName: 'Francis',
    age: 41,
    city: 'Holland',
    state: 'Massachusetts',
    country: 'USA',
    online: false 
  },{
    firstName: 'Alexia',
    lastName: 'Francis',
    age: 39,
    city: 'Paris',
    state: 'Ile de France',
    country: 'France',
    online: true,
  },{
    firstName: 'Lucille',
    lastName: 'Boure',
    age: 29,
    city: 'Paris',
    state: 'Ile de France',
    country: 'France',
    online: true,
  }
];
```

我们有这四个用户，我们希望按他们的姓氏对他们进行排序:

```
const sortByLastName = function(a, b){
  return a.lastName.localeCompare(b.lastName)
};

console.log(myArray.sort(sortByLastName) );
```

第一行定义了我们的排序函数，我们将把它传递给`myArray.sort(...)`。`localeCompare()`函数是一个方便的 JavaScript 函数，用于比较一个字符串和另一个字符串，避免大小写差异，等等。它与`sort()`一起工作，返回 1、0 或-1，这取决于每对记录如何匹配。

因此，这个排序函数的结果(这是一个非常简单的例子)按照姓氏对数组进行排序:

```
[
  {
    firstName: 'Lucille',
    lastName: 'Boure',
    // ... 
  },{
    firstName: 'Bob',
    lastName: 'Francis'
    //... 
  },{
    firstName: 'Janet',
    lastName: 'Francis',
    // ... 
  },{
    firstName: 'Alexia',
    lastName: 'Francis',
    // ... 
  }
]
```

没什么了不起的，真的——我们已经按姓氏排序了，但是姓和名呢？我们能做到吗？

## 我们有力量！

答案当然是肯定的。如果你已经读到这里，如果我引诱你而不给你一个好的答案，那就太傻了。

要记住的技巧是，如果第一次比较返回一个 falsy 值(在本例中为`0`)，那么我们可以进入第二次比较。如果我们愿意，还有第三个或第四个...

下面是比较器函数的样子，先按`lastName`排序，然后按`firstName`排序:

```
const sortByLastAndFirst = function(a, b){
  return (a.lastName.localeCompare(b.lastName) ) 
      || (a.firstName.localeCompare(b.firstName) )
};
```

还有这里的*中的那一个。该返回中的括号只是为了使内容更具可读性，但逻辑是这样的:*

```
*`comparing a and b in a sort function, return:

* if a.lastName comes before or after b.lastName,
  : return the value of that comparison.

* if a.lastName and b.lastName are the same, we get a false value, so 
  : go on to the next comparison, a.firstName and b.firstName`*
```

## *继续之前回顾一下*

*因此，在这一点上，我们知道我们可以将 sort `return`子句串在一起。这是强大的。它给了我们一些深度，让我们的排序更灵活一些。我们可以让它更具可读性，也更“即插即用”。*

*现在我要稍微改变一下，我将使用 ES6 *粗箭头函数*:*

```
*`// Let's put together some smaller building blocks...
const byLast = (a, b)=>a.last.localeCompare(b.last);
const byFirst = (a, b)=>a.first.localeCompare(b.first);

// And then we can combine (or compose) them! 
const byLastAndFirst = (a, b) => byLast(a, b) || byFirst(a, b);`*
```

*这和我们刚才做的是一样的，但是更容易理解。读取这个`byLastAndFirst`函数，我们可以看到它先按最后排序，然后按第一排序。*

*但是这有点痛苦——我们每次都必须编写相同的代码？看看最后一个例子中`byLast`和`byFirst`。除了属性名称之外，它们是相同的。我们能修复它吗，这样我们就不用一遍又一遍地写同样的函数了？*

## *第三节，同...没关系。*

*当然啦！让我们从尝试创建一个通用的`sortByProp`函数开始。它将接受一个属性名和两个对象，并对它们进行比较。*

```
*`const sortByProp = function(prop, a, b){
  if (typeof a[prop] === 'number')
    return a[prop]-b[prop];

  // implied else - if we're here, then we didn't return above 
  // This is simplified, I'm only expecting a number or a string.
  return a[prop].localeCompare(b[prop]); };`*
```

*因此我们可以在排序函数中使用比较器:*

```
*`myArray.sort((a, b)=> sortByProp('lastName', a,b) 
                   || sortByProp('firstName', a, b) );`*
```

*这看起来很棒，对吧？我的意思是，我们现在只有一个函数，我们可以通过任何属性进行比较。嘿，它包括一个比较数字和字符串的检查，为了胜利！*

*是啊，但这让我很困扰。我喜欢能够使用那些更小的函数(T0 和 T1)，并且知道它们仍然可以与 T2 一起工作——但是对于我们的 T3，我们不能使用参数签名！排序不知道我们的`prop`功能。*

## *dev 能做什么？*

*我们在这里做的是，写一个函数，返回一个函数。这些被称为**高阶函数**，它们是 JavaScript 的一个强大特性。*

*我们想要创建一个函数(我们仍然称它为`sortByProp()`)，我们可以传入一个属性名。作为回报，我们得到一个函数，该函数在其内部范围内记住我们的属性名，但可以接受排序函数的`(a, b)`参数签名。*

*这种模式所做的是创建一个“闭包”。该属性作为参数传递给外部函数，因此它只存在于该外部函数的范围内。*

*但是在里面，我们返回一个函数，可以引用里面的值。一个闭包需要两个部分:一个私有范围和一些访问私有范围的方法。这是一项强大的技术，我将在未来进一步探索。*

*我们从这里开始:首先，我们需要重新定义我们的`sortByProp`函数。我们知道它需要一个属性，并且需要返回一个函数。此外，返回的函数应该接受`sort()`将传入的两个属性:*

```
*`const sortByProp = function(prop){
  return function(a,b){
    /* here, we'll have something going on */ 
  } 
}`*
```

*当我们调用这个函数时，我们会得到一个函数。所以我们可以把它赋给一个变量，以便以后能够再次调用它:*

```
*`const byLast = sortByProp('lastName');`*
```

*在那一行中，我们捕获了返回的函数，并将其存储到`byLast`中。此外，我们刚刚创建了一个*闭包*，一个对存储我们的`prop`变量的封闭作用域的引用，我们可以在以后调用我们的`byLast`函数时使用它。*

*现在，我们需要重新访问那个`sortByProp`函数，并填充里面发生的事情。这与我们在第一个`sortByProp`函数中所做的一样，但是现在它被我们可以使用的函数签名所包围:*

```
*`const sortByProp = function(prop){
  return function(a,b){
    if(typeof a[prop] === 'number')
      return a[prop]-b[prop];

    return a[prop].localeCompare(b[prop]); 
  } 
}`*
```

*要使用它，我们只需:*

```
*`const byLast = sortByProp('lastName'); 
const byFirst = sortByProp('firstName'); 
// we can now combine, or "compose" these two: 
const byLastAndFirst = function(a, b){
  return byLast(a, b) 
      || byFirst(a, b); 
} 

console.log( myArray.sort(byLastAndFirst) );`*
```

*请注意，我们可以将它扩展到任何我们想要的深度:*

```
*`const byLast = sortByProp('lastName'); 
const byFirst = sortByProp('firstName'); 
const byCountry = sortByProp('country'); 
const byState = sortByProp('state'); 
const byCity = sortByProp('city'); 
const byAll = (a, b)=> byCountry(a, b) || byState(a, b) || byCity(a, b) || byLast(a, b) || byFirst(a, b); 

console.log(myArray.sort(byAll) );`*
```

*最后一个例子非常深刻。而且是故意的。我的下一篇文章将是做同样事情的另一种方式，而不必像那样手工编码所有的比较。*

*对于那些喜欢看到完整图片的人，我完全期待关于相同`sortByProp`功能的 ES6 版本的问题，只是因为它们很漂亮。当然，在含蓄的回报和可爱的三元之间，它们很漂亮。在这里，这里是那个的 **[Tech.io](https://tech.io/snippet/imU4elI)** :*

```
*`const byProp = (prop) => (a, b) => typeof(a[prop])==='number'
             ? a[prop]-b[prop] 
             : a[prop].localeCompare(b[prop]);`*
```

*请注意，这个版本并不比其他版本更好或更差。它看起来很圆滑，并且利用了一些很棒的 ES6 功能，但是牺牲了可读性。初级开发人员可能会看着它，然后放弃。请不要为了聪明而牺牲可维护性。*

*感谢大家的阅读！*