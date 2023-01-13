# 让我们了解一下 Set 及其在 JavaScript 中的独特功能？

> 原文：<https://www.freecodecamp.org/news/lets-learn-about-set-and-its-unique-functionality-in-javascript-5654c5c03de2/>

作者:阿西夫·诺尔扎伊

# 让我们了解一下 Set 及其在 JavaScript 中的独特功能？

![yHYXOoOwWcn1h5cpOX41qpwb7KkEVbKQTqfy](img/60bbac9fcc13bd5914f25f15f28bde75.png)

Don’t be a duplicate or else Set will catch you

### 设置？

ES2015/ES6 为我们提供了许多有用的工具和功能，但对我来说最突出的是 Set。它还没有发挥出全部潜力。我希望通过这篇文章让你相信它的价值，这样你就可以获得这个美丽的实用工具的全部好处。

#### 那么，你会问，什么是设定？

> "`**Set**`对象允许你存储任何类型的唯一值，无论是[原始值](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)还是对象引用."， [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) 。

Set 删除重复的条目。

#### **基本功能**？

每当你想使用`Set`时，你必须使用`new`关键字初始化它，并传入一个初始的[可迭代的](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterable_protocol)数据，将其留空或`null`。

```
// All valid ways to initialize a set
const newSet1 = new Set();
const newSet2 = new Set(null);
const newSet3 = new Set([1, 2, 3, 4, 5]);
```

### **设置实用程序/方法**？

`**add**` **、** 顾名思义，将新条目添加到新初始化的集合 const 中。如果在任何时候一个重复的值被添加到集合中，它将被使用`strict equality`丢弃。

```
const newSet = new Set();

newSet.add("C");
newSet.add(1);
newSet.add("C");

// chain add functionality
newSet.add("H").add("C");

newSet.forEach(el => {
  console.log(el);
  // expected output: C
  // expected output: 1
  // expected output: H
});
```

`**has**` 检查你传入的值是否存在于`newSet`常量中。如果该值确实存在，它将返回布尔值`true`，如果不存在，它将返回`false`

```
const newSet = new Set(["A", 2, "B", 4, "C"]);

console.log(newSet.has("A"));
// expected output: true

console.log(newSet.has(4));
// expected output: true

console.log(newSet.has(5));
// expected output: false
```

`**clear**` & `**delete**` 是`Set`的两个最重要的功能，如果你想移除所有条目或删除特定值。

```
const newSet = new Set(["A", 2, "B", 4, "C"]);

newSet.delete("C");
// expected output: true

newSet.delete("C");
// expected output: false

newSet.size
// expected output: 4

newSet.clear();
// expected output: undefined

newSet.size
// expected output: 0
```

`**keys**` 和 **`values`** 都有相同的功能，如果你想一想它们是如何处理 JS 对象的，那就奇怪了。它们都返回一个`iterator`对象。这意味着您可以访问它的`.next()`方法来获取它的值。

```
const newSet = new Set(null);

newSet.add("Apples");
newSet.add(12);

let iterator = newSet.keys();  // same as newSet.values();

console.log(iterator.next().value);
// expected output: Apples

console.log(iterator.next().value);
// expected output: 12

console.log(iterator.next().value);
// expected output: undefined
```

### **把所有的东西放在一起**

让我们为黑客党创建一个简单的函数。该功能仅在用户获得管理员批准的情况下将用户添加到列表中。所以你的条目必须有一个管理员的名字，这是保密的(虽然不在本文中)。节目最后会说邀请的是谁。

```
// The Admins
const allowedAdminUsers = new Set(["Naimat", "Ismat", "Azad"]);

// An empty Set, stored in memory
const finalList = new Set();

// A function to add users to permission list
const addUsers = ({user, admin}) => {

   // Check to see if the admin is the admin 
  // list and that the user isn't already in the set
  if(allowedAdminUsers.has(admin) && !finalList.has(user)) {

    // Return the users list at the end
   return finalList.add(user);

  }
  // Console.log this message if the if the condition doesn't pass
  console.log(`user ${user} is already in the list or isn't allowed`); 
};

// Add some entries
addUsers({user: "Asep", admin: "Naimat"});
addUsers({user: "John", admin: "Ismat"});

// Lets add John again and this time that inner function console error will be shown
addUsers({user: "John", admin: "Azad"});

const inviationList = [...finalList].map(user => 
 `${user} is invited`);

console.log(inviationList);
// Expected output:  ["Asep is invited", "John is invited"]
```

这就足够我们在项目中使用 Set 了。？

![9HUfTsuNCDKzpF6NJsItRYlX-68khdXAb9hk](img/de99276939e5b522edc26129b09e5c8b.png)

**出发前**:如果你喜欢这篇文章，请在这里关注我，也可以在 [Twitter](https://twitter.com/asepnorzai) 上关注我，我会在那里发布和转发与网络相关的内容。