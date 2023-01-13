# 如何从 JavaScript 对象中移除属性

> 原文：<https://www.freecodecamp.org/news/how-to-remove-a-property-from-a-javascript-object/>

有两种方法可以从 JavaScript 对象中删除属性。有使用 delete 操作符的可变方法和使用对象重构的不可变方法。

让我们在本教程中逐一介绍这些方法。

## 使用删除操作符从 JS 对象中删除属性

`delete`是一个 JavaScript 指令，允许我们从 JavaScript 对象中移除一个属性。有几种方法可以使用它:

*   `delete object.property;`
*   `delete object[‘property’];`

运算符从对象中删除相应的属性。

```
let blog = {name: 'Wisdom Geek', author: 'Saransh Kataria'};
const propToBeDeleted = 'author';
delete blog[propToBeDeleted];
console.log(blog); // {name: 'Wisdom Geek'}
```

删除操作会修改原始对象。这意味着它是一个可变操作。

## 使用对象重构从 js 对象中删除属性

使用对象重构和 rest 语法，我们可以用要移除的属性来析构对象，并创建它的新副本。

在析构之后，对象的一个新副本被创建并赋给一个新的变量，而没有我们选择移除的属性。

```
const { property, ...remainingObject } = object;
```

例如:

```
let blog = {name: 'Wisdom Geek', author: 'Saransh Kataria'};
const { author, ...blogRest } = blog;
console.log(blogRest) // {name: 'Wisdom Geek'};
console.log(blog); // {name: 'Wisdom Geek', author: 'Saransh Kataria'}
```

如果我们想动态地这样做，我们可以这样做:

```
const name = 'propertToBeRemoved';
const { [name]: removedProperty, ...remainingObject } = object; 
```

也可以使用相同的语法删除多个属性。

## 包扎

这是从 JavaScript 对象中移除属性的两种方法。如果您有任何问题，请随时联系我！

****阅读更多我的帖子请点击:【https://www.wisdomgeek.com】****