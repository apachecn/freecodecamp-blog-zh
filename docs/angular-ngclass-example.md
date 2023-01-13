# Angular NgClass 示例–如何添加条件 CSS 类

> 原文：<https://www.freecodecamp.org/news/angular-ngclass-example/>

`ngClass`是 Angular 中的一个[指令，用于在 HTML 元素上添加和删除 CSS 类。在本文中，我们只讨论 Angular 中的`ngClass`，而不是 angular.js 中的`ng-class`](https://angular.io/api/common/NgClass)

## 先决条件–什么是属性绑定？

我们首先要了解的两件事是[属性绑定](https://angular.io/guide/property-binding)和[内插法](https://angular.io/guide/interpolation)。我们以`input`的`placeholder`属性为例。

考虑以下代码:

```
<!-- Normal HTML -->
<input placeholder="some text">

<!-- Interpolation -->
<input placeholder="{{ variable }}">
<!-- Property Binding -->
<input [placeholder]="variable">
```

假设我们已经在组件中定义了`variable = 'some text'`，那么上面所有的代码将做**完全相同的事情**。

我认为插值(`{{ }}`)是一个`eval`。而属性绑定只是实现相同目的的一种不太冗长的方式。就我个人而言，我尽可能多地使用属性绑定。

## Angular 中的 ngClass 是什么？基础知识

对于`ngClass`，它支持三种类型的表达式“返回值”:`String`、`Array`和`Object`。这里有一个来自[官方文件](https://angular.io/api/common/NgClass#description)的例子:

```
<div [ngClass]="'first second'">
<div [ngClass]="['first', 'second']">
<div [ngClass]="{first: true, second: true, third: true}">
<div [ngClass]="{'first second': true}">
```

上面所有的`div`元素都有两个类:`first`和`second`。请注意，在官方文档中，第三个示例中的每个对象键都有一对单引号。但是`first`和`second`都没有破折号或空格。所以我们可以随意删除引号。

然而，在第四个例子中，由于键是`first second`(中间有一个空格)，我们需要添加单引号。

只是提醒一下——`ngClass`的值不一定是字面量，如上所示。请记住，属性绑定计算它的表达式。所以只要表达式可以被解释为`String/Array/Object`，我们就万事大吉了。

## 如何在 angular 中使用 nclass

那么什么是表达式呢？通常情况下，`expression`表示一个值，而`statement`会做一些没有返回值的事情。您可能熟悉负责控制流的`if`语句。注意`if`语句没有返回值。

另一方面，比方说，`num + 10`是一个表达式(假设`num`有一个类似于`1`的值)，它有一个`11`的“返回值”。

在 ECMAScript 中，赋值也被认为是一个`expression`，它确实有一个“返回值”。但是我们不能在 Angular 的属性绑定里放一个赋值表达式，这样会抛出一个错误说“绑定不能包含赋值”。

也就是说，以下所有的`[ngClass]`都是有效的:

```
<!-- Given that val="foo", the class of the following div would be "foofoo" -->
<div [ngClass]="val + val">

<!-- Given that val="foo", the class of the following div would be "foo" -->
<div [ngClass]="[val]">

<!-- Given that func is a function that returns "foo", the class of the following div would be "foo" -->
<div [ngClass]="func()">
```

## 如何在简单条件下使用 angular nclass

我们不能在`ngClass`中写一个`if`语句，因为它是一个语句。但是我们不能使用三元运算符，因为这是一个表达式。

例如，如果我们希望表格单元格中的文本在其值大于`10`时有一个类`red`，否则它应该有一个类`green`，下面是代码:

```
<td [ngClass]="val > 10 ? 'red' : 'green'">{{ val }}</td>
```

如果我们想根据条件切换一个类，我们可以让其中一个表达式为空字符串。

例如，如果我们想在表单无效时添加一个`error`类，并在表单有效时删除`error`类，我们可以这样做:

```
<input type="text" [ngClass]="control.isInvalid ? 'error' : ''" />
```

但是有一个不太冗长的方法。记住`ngClass`也支持对象作为值:

```
<input type="text" [ngClass]="{ error: control.isInvalid }" />
```

实现同样目的的另一种方法是使用[类绑定](https://angular.io/guide/attribute-binding#binding-to-a-single-css-class)。这对于单个类来说是理想的:

```
<input type="text" [class.error]="control.isInvalid" />
```

## 如何使用对象文本和 nclass

当我们使用对象文字时，`key`表示我们将要为元素配置的`class`，而`value`表示该类是否应该应用于元素。

注意只有当`value`为`[truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)`时`key`才会被应用。在上面的例子中，如果`coltrol.isInvalid`是`false`、`undefined`、`''`等等之一，那么`error`的类将**而不是**应用于元素。

还有一点需要注意的是，还不支持[计算属性名](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#computed_property_names)语法。但是在棱角分明的官方 GitHub 回购中有一个[未决问题](https://github.com/angular/angular/issues/13855)。

## 角类和复杂条件

如果条件不仅仅是`true/false`呢？假设当`val`是`0-5`、`6-10`和`>= 11`时，我们需要不同的类名，您可以很容易地用`if/else`语句来表示:

```
if (val >= 0 && val <= 5) {
  return 'low';
} else if (val >= 6 && val <= 10) {
  return 'medium';
} else {
  return 'high'
}
```

如上所述，我们不能在`ngClass`中使用一个`if/else`语句。但是等等，`function`是一个有返回值的有效表达式，我们可以在`function`中使用`if/else`语句:

```
class MyComponent {
  getClassOf(val) {
    if (val >= 0 && val <= 5) {
      return 'low';
    } else if (val > 5 && val <= 10) {
      return 'medium';
    } else {
      return 'high'
    }
  }
}
```

```
<td [ngClass]="getClassOf(val)">{{ val }}</td>
```

到目前为止一切顺利。但我想指出，这实际上并不理想。长话短说，由于`ChangeDetection`在 Angular 中的工作方式，上述函数`getClassOf`可能会运行多次，即使*认为没有必要。*

这远远超出了本文的主题，但是如果你想知道更多，你可以看看这篇文章。

我们仍然可以在不使用`function`的情况下获得相同的结果。对于对象文字，它看起来像这样:

```
<td [ngClass]="{ low: val >= 0 && val <=5, medium: val > 5 && val <= 10, high: val > 10}">
  {{ val }}
</td>
```

看起来有点冗长。考虑到只有一个类应用于该元素，所以如果我们可以首先转换我们的`val`,这是最理想的:

```
type ClassName = 'low' | 'medium' | 'high';

class MyComponent {
  className: ClassName = 'low';

  ngOnChanges(changes: SimpleChanges) {
    if (changes.val) {
      className = mapValToClass(changes.val.currentValue);
    }
  }

  private mapValToClass(val: number): ClassName {
    if (val >= 0 && val <= 5) {
      return 'low';
    } else if (val > 5 && val <= 10) {
      return 'medium';
    } else {
      return 'high'
    }
  }
}
```

我们在模板中需要做的就是:

```
<td [ngClass]="className"></td>
```

有些情况下我们可以使用数组。考虑以下映射关系:

```
{
  1: 'first-element',
  2: 'second-element',
  3: 'third-element',
}
```

这意味着对于一个是`1`的`val`，元素的类将是`first-element`。当我们看到连续的数字如`1`、`2`、`3`时，那么我们可以考虑使用一个数组。这是因为通过从每个值中减去`1`，我们得到`0`、`1`和`2`，它们只是一个数组的索引:

```
type Val = 1 | 2 | 3;

class MyComponent {
  classArr = ['first-element', 'second-element', 'third-element'];
  val: Val = 1;
}
```

```
<td [ngClass]="classArr[val - 1]"></td>
```

一个有趣的事实是，在 ECMAScript 中，数组只是一个具有相当多额外属性和方法的对象。因此，您也可以对一个对象执行上述操作:

```
type Val = 1 | 2 | 3;

class MyComponent {
  classMap = {
    1: 'first-element',
    2: 'second-element',
    3: 'third-element',
  }
  val: Val = 1;
}
```

```
<td [ngClass]="classMap[val]"></td>
```

请注意，您也可以使用嵌套的三元运算符来实现上述目的，这是我一直试图避免的。

## [ngClass]与[class]的角度

在我们总结之前，有一件事值得一提，那就是`[class]`符号。这从`[Ivy](https://angular.io/guide/ivy)`开始可用，它是在 Angular 9 中作为默认编译器和运行时引入的。

`[class]`几乎与`[ngClass]`向后兼容，但有一些差异:

1.  `[ngClass]="{'a b': true}"`确实管用，但是`[class]="{'a b': true}"`不行。参见[本期未决问题](https://github.com/angular/angular/issues/40623)。
2.  `[class]`的值不是“deepwatched”。此处见[。](https://hackmd.io/jzDc7hIDTdWtQblv2TbL9A)

## 结论

感谢阅读！希望现在你知道了`ngClass`是如何工作的，并且可以放心地使用它。