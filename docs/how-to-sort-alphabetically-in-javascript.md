# 在 JavaScript 中按字母顺序排序——如何在 JS 中按名字排序

> 原文：<https://www.freecodecamp.org/news/how-to-sort-alphabetically-in-javascript/>

有时，您可能有一个单词数组，您希望按字母顺序(从 a-z)对每个单词进行排序。或者，您可能有一个包含用户信息(包括姓名)的对象数组，例如，您希望按姓名对用户进行排序。

我们可以在 JavaScript 中通过直接使用`sort()`方法或使用 compare 函数来实现这一点。

万一你赶时间，这里有两种方法:

```
// order an array of names
names.sort();

// order an array of objects with name
users.sort(function (a, b) {
  if (a.name < b.name) {
    return -1;
  }
  if (a.name > b.name) {
    return 1;
  }
  return 0;
}); 
```

现在让我们了解一下我们是如何找到这两种解决方案的。

## 如何按字母顺序对名称数组进行排序

假设我们有一组名字:

```
let names  = ["John Doe", "Alex Doe", "Peter Doe", "Elon Doe"]; 
```

我们可以使用`sort()`方法按字母顺序对这些名字进行排序:

```
let sortedNames = names.sort();
console.log(sortedNames); 
```

这将返回一个按字母顺序排序的名称数组:

```
["Alex Doe","Elon Doe","John Doe","Peter Doe"] 
```

**注意:**在一些名字以大写字母开头而另一些以小写字母开头的情况下，输出会不正确，因为`sort()`方法将大写字母放在小写字母之前:

```
let names = ["John Doe", "alex Doe", "peter Doe", "Elon Doe"];
let sortedNames = names.sort();

console.log(sortedNames); // ["Elon Doe","John Doe","alex Doe","peter Doe"] 
```

所以你需要确保所有的单词都是相同的大小写，否则它不会像我们期望的那样按照字母顺序返回名字。

## 如何在 JavaScript 中按姓名字母顺序排序

在真实的场景中，我们可能有一个用户数组，每个用户的信息都在一个对象中。该信息可以是用户名旁边的任何内容。例如:

```
let users = [
  {
    name: "John Doe",
    age: 17
  },
  {
    name: "Elon Doe",
    age: 27
  },
  {
    name: "Alex Doe",
    age: 14
  }
]; 
```

看上面的对象，之前我们只是在数组上直接应用`sort()`方法的方法是行不通的。相反，它将抛出相同的数组，但项目不会按照我们想要的顺序。

我们将使用`sort()`方法和 compare 函数按名称对这个用户数组进行排序。

我们将使用 compare 函数来定义另一种排序顺序。它根据参数返回负值、零或正值:

语法:

```
function(a, b){return a - b} 
```

当我们将这个比较函数传递给`sort()`方法时，它根据我们设置的条件比较每个值，然后根据返回值(负、零、正)对每个名称进行排序。

*   如果结果是否定的，`a`排在`b`之前。
*   如果结果是肯定的，`b`排在‘a’之前。
*   如果结果是`0`，两个值的排序顺序不变。

使用上面的例子，我们现在可以这样使用`sort()`方法和比较函数:

```
users.sort(function (a, b) {
  if (a.name < b.name) {
    return -1;
  }
  if (a.name > b.name) {
    return 1;
  }
  return 0;
});

console.log(users); 
```

上面的代码比较了每个名字。如果大于，则返回 1。如果小于，则返回-1。否则，它返回 0。返回值用于按字母顺序排列数组的值:

```
[
  {
    name: "Alex Doe",
    age: 14
  },
  {
    name: "Elon Doe",
    age: 27
  },
  {
    name: "John Doe",
    age: 17
  }
] 
```

**注意:**就像我们之前看到的一样，这总是根据字母大小写来工作，并且将大写字母排在小写字母之前:

```
let users = [
  {
    name: "alex Doe",
    age: 14
  },
  {
    name: "Elon Doe",
    age: 27
  },
  {
    name: "John Doe",
    age: 17
  }
];

users.sort(function (a, b) {
  if (a.name < b.name) {
    return -1;
  }
  if (a.name > b.name) {
    return 1;
  }
  return 0;
});

console.log(users); 
```

输出:

```
[
  {
    name: "Elon Doe",
    age: 27
  },
  {
    name: "John Doe",
    age: 17
  },
  {
    name: "alex Doe",
    age: 14
  }
] 
```

## 包扎

在本文中，您已经学习了如何在两种可能的情况下使用`sort()`方法按字母顺序对数组进行排序。

在名称具有不同字母大小写的情况下，最好在使用`sort()`方法之前先将它们转换成特定的字母大小写。

编码快乐！