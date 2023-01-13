# JavaScript 可选链接`？解释了它的工作原理和使用时间

> 原文：<https://www.freecodecamp.org/news/javascript-optional-chaining-explained/>

## 什么是可选链接？

JavaScript 中用`?.`表示的可选链接是 ES2020 中引入的新特性。

可选链接改变了从深层嵌套对象中访问属性的方式。它解决了在 JavaScript 中访问一长串对象属性时必须进行多次空检查的问题。

当前状态:`ECMAScript proposal at stage 4 of the process.`:[https://github.com/tc39/proposal-optional-chaining](https://github.com/tc39/proposal-optional-chaining)

## 用例

1.  访问对象的潜在`null`或`undefined`属性。
2.  从一个可能还不可用的变量中获取结果。
3.  获取默认值。
4.  访问长链属性。

假设您希望 API 返回这样一个对象:

```
obj = {
  prop1: {
    prop2: {
      someProp: "value"
    }
  }
}; 
```

但是您可能不知道这些字段中的每一个是否提前可用。其中一些可能没有被 API 发回，或者它们可能返回了空值。

这里有一个例子:

```
//expected
obj = {
  id: 9216,
  children: [
    { id: 123, children: null },
    { id: 124, children: [{ id: 1233, children: null }] }
  ]
};

//actual
obj = {
  id: 9216,
  children: null
}; 
```

调用 API 的函数经常会发生这种情况。您可能已经看到 React 中的代码试图防止这些问题，如下所示:

```
render = () => {
  const obj = {
    prop1: {
      prop2: {
        someProp: "value",
      },
    },
  };

  return (
    <div>
      {obj && obj.prop1 && obj.prop1.prop2 && obj.prop1.prop2.someProp && (
        <div>{obj.prop1.prop2.someProp}</div>
      )}
    </div>
  );
}; 
```

为了更好地准备这个问题，过去我们经常使用`Lodash`，特别是`_.get`方法:

```
_.get(obj, prop1.prop2.someProp); 
```

如果这些属性中的任何一个是`undefined`，则输出`undefined`。**随意链接正是那个**！现在，这个功能是内置的，而不是使用外部库。

## 可选链接是如何工作的？

`?.`可以用来链接可能是`null`或`undefined`的属性。

```
const propNeeded = obj?.prop1?.prop2?.someProp; 
```

如果这些链接的属性是`null`或`undefined`，JavaScript 将返回`undefined`。

如果我们想返回一些有意义的东西呢？试试这个:

```
let familyTree = {
    us: {
        children: {}
    }
}

// with _.get
const grandChildren = _.get(familyTree, 'us.children.theirChildren', 'got no kids' );

//with optional chaining and null coalescing 
const nullCoalescing = familyTree?.us?.children?.theirChildren ?? 'got no kids'
console.log(nullCoalescing) //got no kids 
```

它也适用于可能是`null`或`undefined`的对象:

```
let user;
console.log(user?.id) // undefined 
```

## 如何获得这一最新功能

1.  在你的浏览器控制台中尝试一下:这是最近增加的，旧的浏览器可能需要 polyfills。你可以在浏览器控制台的 Chrome 或 Firefox 中尝试一下。如果不起作用，可以通过访问`chrome://flags/`并启用“实验 JavaScript”来尝试开启 JavaScript 实验特性。

2.  使用 Babel 在您的节点应用程序中进行尝试:

```
{
  "plugins": ["@babel/plugin-proposal-optional-chaining"]
} 
```

## 资源

1.  [https://dmitripavlutin.com/javascript-optional-chaining/](https://dmitripavlutin.com/javascript-optional-chaining/)
2.  巴别尔的文档:[https://babel js . io/docs/en/babel-plugin-proposal-optional-chaining](https://babeljs.io/docs/en/babel-plugin-proposal-optional-chaining)

## TL；速度三角形定位法(dead reckoning)

对可能是`null`或`undefined`的对象或长链属性使用可选链接`?.`。语法如下:

```
let user = {};
console.log(user?.id?.name) 
```

* * *

对我的更多教程和 JSBytes 感兴趣？注册订阅我的时事通讯。或[在推特上关注我](https://twitter.com/shrutikapoor08)