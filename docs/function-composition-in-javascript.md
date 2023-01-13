# JavaScript 中的函数组合

> 原文：<https://www.freecodecamp.org/news/function-composition-in-javascript/>

函数组合是一个函数对另一个函数结果的逐点应用。开发人员每天在嵌套函数时都以手动方式进行:

```
compose = (fn1, fn2) => value => fn2(fn1(value))
```

但这很难读懂。使用函数组合有更好的方法。而不是从里到外阅读它们:

```
add2AndSquare = (n) => square(add2(n))
```

我们可以用一个更高阶的函数将它们有序地链接起来。

```
add2AndSquare = compose( add2, square)
```

compose 的一个简单实现是:

```
compose = (f1, f2) => value => f2( f1(value) );
```

为了获得更大的灵活性，我们可以使用 reduceRight 函数:

```
compose = (...fns) => (initialVal) => fns.reduceRight((val, fn) => fn(val), initialVal);
```

从左到右阅读 compose 可以清楚地链接高阶函数。现实世界的例子是添加认证、日志和上下文属性。这是一种在最高层次上实现可重用性的技术。下面是一些如何使用它的例子:

```
// example
const add2        = (n) => n + 2;
const times2      = (n) => n * 2;
const times2add2  = compose(add2, times2);
const add6        = compose(add2, add2, add2);

times2add2(2);  // 6
add2tiems2(2);  // 8
add6(2);        // 8
```

你可能认为这是高级函数式编程，与前端编程无关。但它在单页应用程序中也很有用。例如，您可以通过使用更高阶的组件向 React 组件添加行为:

```
function logProps(InputComponent) {
  InputComponent.prototype.componentWillReceiveProps = function(nextProps) {
    console.log('Current props: ', this.props);
    console.log('Next props: ', nextProps);
  };
  return InputComponent;
}

// EnhancedComponent will log whenever props are received
const EnhancedComponent = logProps(InputComponent);
```

总之，功能组合在很高的层次上实现了功能的可重用性。如果这些功能结构良好，就能让开发者在现有行为的基础上创造新的行为。

它还增加了实现的可读性。除了嵌套函数之外，您还可以清晰地链接函数，并使用有意义的名称创建更高阶的函数。