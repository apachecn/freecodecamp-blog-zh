# 在 JavaScript 中冻结一个原型会发生什么

> 原文：<https://www.freecodecamp.org/news/what-happens-when-you-freeze-a-prototype/>

你想知道当你冻结一个物体的原型时会发生什么吗？让我们一起来了解一下。

## 目标

在 JavaScript 中，对象是带有“隐藏”属性的动态属性集合。我们首先使用对象字面语法创建这样一个对象。

```
const counter = {
  count: 0,

  increment(){
    this.count += 1;
    return this.count;
  },

  decrement(){
    this.count -= 1;
    return this.count
  }  
}

console.log(counter.increment())
```

`counter`是一个对象，有一个字段和两个在其上操作的方法。

## 原型

对象可以从原型继承属性。事实上，`counter`对象已经继承了`Object.prototype`对象。

例如，我们可以在`counter`对象上调用`toString()`方法，即使我们还没有定义它。

```
counter.toString();
```

使用原型的最好方法是提取出原型中的方法，然后在具有相同行为的所有对象之间共享它们。让我们使用 [Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) 来实现这一点。

```
const counterPrototype = {
  increment(){
    this.count += 1;
    return this.count;
  },

  decrement(){
    this.count -= 1;
    return this.count
  }
}

const counter = Object.create(counterPrototype);
counter.count = 0;
console.log(counter.increment())
//1
```

`Object.create()`创建一个新对象，使用一个现有对象作为新对象的原型。`counter`以`counterPrototype`为原型。

原型系统很灵活，但也有一些缺点。所有属性都是公共的，可以更改。

例如，我们可以在`counter`对象中重新定义`increment()`对象的实现。

```
const counter = Object.create(counterPrototype);
counter.count = 0;

counter.increment = function(){
  console.log('increment')
}

console.log(counter.increment());
//"increment"
```

## 冷冻原型

让我们看看如果我们冻结原型会发生什么。冻结对象不允许我们添加、删除或更改其属性。

```
const counterPrototype = Object.freeze({
  increment(){
    this.count += 1;
    return this.count;
  },

  decrement(){
    this.count -= 1;
    return this.count
  }
});

counterPrototype.increment = function(){
  console.log('increment')
}
//Cannot assign to read only property 'increment' of object '#'
```

`Object.freeze()`冻结一个对象。冻结的对象不能再被更改。我们不能添加、编辑或删除其中的属性。

现在看看当试图改变继承自`counterPrototype`的`counter`对象中的方法时会发生什么。

```
const counter = Object.create(counterPrototype);
counter.count = 0;

counter.increment = function(){
  console.log('increment')
}
//Cannot assign to read only property 'increment' of object

console.log(counter.increment());
//1
```

正如你现在看到的，原型被冻结了，我们不能改变`counter`对象中的`increment()`方法。

## 概述

对象有一个引用其原型的隐藏属性。

原型通常用于保存不同对象之间共享的方法。

冻结原型不允许我们改变从原型继承的对象的属性。其他属性可以更改。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 BookAuthority 评为[****最佳函数式编程书籍之一！****](https://bookauthority.org/books/best-functional-programming-books)

关于将函数式编程技术应用于 React 的更多信息，请看一下 ****[函数式 React](https://www.amazon.com/dp/B088FZQ1XN) 。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****