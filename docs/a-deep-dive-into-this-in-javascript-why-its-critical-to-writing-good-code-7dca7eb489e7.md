# JavaScript 中对此的深入探究:为什么它对编写好代码至关重要。

> 原文：<https://www.freecodecamp.org/news/a-deep-dive-into-this-in-javascript-why-its-critical-to-writing-good-code-7dca7eb489e7/>

这篇文章用简单的术语和真实的例子解释了什么是`this`以及它为什么有用。

### 这是给你的吗

我注意到 JavaScript 中许多关于`this`的解释都是假设你来自一些面向对象的编程语言，如 Java、C++或 Python。这篇文章面向那些对你认为的`this` 是什么或者它应该是什么没有先入之见的人。我将尝试用简单的方式解释**什么是** `this`以及**为什么**有用，不使用不必要的术语。

也许你推迟了潜入`this`的时间，因为它看起来很怪异和恐怖。或者你使用它只是因为 StackOverflow 说你需要它来做一些反应。

在我们深入了解`this`到底是什么以及为什么你会使用它之前，我们首先需要理解**函数式**编程和**面向对象的**编程之间的区别。

### 函数式编程与面向对象编程

您可能知道也可能不知道 JavaScript 既有函数式的也有面向对象的构造，所以您可以选择只关注其中一种，或者两者都用。

我在 JavaScript 之旅的早期就接受了函数式编程，并像躲避瘟疫一样避开了面向对象编程。我不知道也不理解`this`等面向对象的关键词。我想我不理解它的一个原因是因为我不明白为什么它是必要的。似乎我可以不依靠`this`而做任何我需要做的事情。

我是对的。

算是吧。你也许可以只关注一种范式而不了解另一种范式，但是作为一名 JavaScript 开发人员，你会受到限制。为了说明函数式编程和面向对象编程之间的区别，我将使用一组脸书朋友数据作为例子。

假设您正在构建一个 web 应用程序，用户用脸书登录，您显示一些关于他们脸书朋友的数据。你需要点击一个脸书端点来获取他们朋友的数据。它可能有一些信息，如`firstName`、`lastName`、`username`、`numFriends`、`friendData`、`birthday`和`lastTenPosts`。

```
const data = [
  {
    firstName: 'Bob',
    lastName: 'Ross',
    username: 'bob.ross',    
    numFriends: 125,
    birthday: '2/23/1985',
    lastTenPosts: ['What a nice day', 'I love Kanye West', ...],
  },
  ...
]
```

上面的数据是你从(假的，虚构的)脸书 API 得到的。现在您需要转换它，以便它是对您和您的项目有用的格式。假设您想要为用户的每个朋友显示以下内容:

*   他们的姓名格式为``${firstName} ${lastName}``
*   三个随机的帖子
*   离他们生日还有几天

### 功能方法

函数式方法是将整个数组或数组中的每个元素传递给一个函数，该函数返回您需要的经过处理的数据:

```
const fullNames = getFullNames(data)
// ['Ross, Bob', 'Smith, Joanna', ...]
```

你从原始数据(来自脸书 API)开始。为了将其转换为对您有用的数据，您将数据传递到一个函数中，输出是或包括经过处理的数据，您可以在应用程序中使用这些数据向用户显示。

您可以想象做一些类似的事情来获得三个随机的帖子，并计算离那个朋友的生日还有几天。

函数式方法是获取原始数据，通过一个或多个函数传递，并输出对您和您的项目有用的数据。

### 面向对象的方法

对于编程和学习 JavaScript 的新手来说，面向对象的方法可能更难掌握。这里的想法是，你将每个朋友**转换成**一个拥有开发人员需要的生成**你**所需要的一切的对象。

您可以创建具有一个`fullName`属性的对象，以及两个特定于该朋友的函数`getThreeRandomPosts`和`getDaysUntilBirthday`。

```
function initializeFriend(data) {
  return {
    fullName: `${data.firstName} ${data.lastName}`,
    getThreeRandomPosts: function() {
      // get three random posts from data.lastTenPosts
    },
    getDaysUntilBirthday: function() {
      // use data.birthday to get the num days until birthday
    }
  };
}
const objectFriends = data.map(initializeFriend)
objectFriends[0].getThreeRandomPosts() 
// Gets three of Bob Ross's posts
```

面向对象的方法是为你的数据创建对象，这些对象有状态并包含所有它们需要的信息，以便生成对你和你的项目有用的数据。

### 这和这个有什么关系？

你可能从未想过要写类似上面的`initializeFriend`这样的东西，你可能认为这样的东西会非常有用。然而，你可能也注意到了，它不是真正的面向对象的**。**

方法`getThreeRandomPosts`或`getDaysUntilBirthday`在上面的例子中能够工作的唯一原因是因为闭包。因为关闭，在`initializeFriend`返回后，他们仍然可以进入`data`。关于闭包的更多信息，请查看[你不知道 JS: Scope & Closures](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch5.md) 。

如果你有另一种方法呢，姑且称之为`greeting`。注意，方法(对于 JavaScript 中的对象)只是一个属性值为函数的属性。我们希望`greeting`做这样的事情:

```
function initializeFriend(data) {
  return {
    fullName: `${data.firstName} ${data.lastName}`,
    getThreeRandomPosts: function() {
      // get three random posts from data.lastTenPosts
    },
    getDaysUntilBirthday: function() {
      // use data.birthday to get the num days until birthday
    },
    greeting: function() {
      return `Hello, this is ${fullName}'s data!`
    }
  };
}
```

那能行吗？

不要！

我们新创建的对象中的所有东西都可以访问`initializeFriend`中的所有变量，但不能访问对象本身的任何属性或方法。当然，你会问这个问题:

> 你就不能用`data.firstName`和`data.lastName`来回复你的问候吗？

是的，你绝对可以。但是，如果我们还想在问候中包括离朋友的生日还有几天呢？我们必须想办法从`greeting`内部调用`getDaysUntilBirthday`。

`this`时间到了！

![CjfAp0G6O8yFJPu4aKOV8tvPjs2kt0eCaWct](img/fe522482b02137560ab88ffd88a713c6.png)

Photo by [sydney Rae](https://unsplash.com/photos/geM5lzDj4Iw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/this?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### 最后，这是什么

`this`可以指不同情况下的不同事物。默认情况下，`this`指的是全局对象(在浏览器中，这是`window`对象)，这不是很有用。现在对我们有帮助的`this`规则如下:

**如果`this`被用在一个对象方法中，并且该方法在该对象的上下文中被调用，`this`指的是对象本身。**

> 你说“在那个对象的上下文中调用”…那到底是什么意思？

别担心，我们稍后会讲到的！

因此，如果我们想从`greeting`内部调用`getDaysUntilBirthday`，我们可以直接调用`this.getDaysUntilBirthday`，因为在这种情况下`this`只是指对象本身。

旁注:不要在全局作用域或另一个函数的作用域中的常规 ole 函数中使用`this`！`this`是一个面向对象的构造。因此，它只在一个对象(或类)的上下文中有意义！

让我们重构`initializeFriend`来使用`this`:

```
function initializeFriend(data) {
  return {
    lastTenPosts: data.lastTenPosts,
    birthday: data.birthday,    
    fullName: `${data.firstName} ${data.lastName}`,
    getThreeRandomPosts: function() {
      // get three random posts from this.lastTenPosts
    },
    getDaysUntilBirthday: function() {
      // use this.birthday to get the num days until birthday
    },
    greeting: function() {
      const numDays = this.getDaysUntilBirthday()      
      return `Hello, this is ${this.fullName}'s data! It is ${numDays} until ${this.fullName}'s birthday!`
    }
  };
}
```

现在，一旦执行了`intializeFriend`,这个对象需要的所有东西都在对象本身的范围内。我们的方法不再依赖于封闭。它们只使用包含在对象本身中的信息。

> 好的，这是使用`this`的一种方式，但是你说过`this`可以是许多不同的东西，这取决于上下文。那是什么意思？为什么它不总是指物体本身？

有些时候你想强迫`this`成为某个特别的东西。事件处理程序就是一个很好的例子。假设我们想在用户点击时打开一个朋友的脸书页面。我们可以给我们的对象添加一个`onClick`方法:

```
function initializeFriend(data) {
  return {
    lastTenPosts: data.lastTenPosts,
    birthday: data.birthday,
    username: data.username,    
    fullName: `${data.firstName} ${data.lastName}`,
    getThreeRandomPosts: function() {
      // get three random posts from this.lastTenPosts
    },
    getDaysUntilBirthday: function() {
      // use this.birthday to get the num days until birthday
    },
    greeting: function() {
      const numDays = this.getDaysUntilBirthday()      
      return `Hello, this is ${this.fullName}'s data! It is ${numDays} until ${this.fullName}'s birthday!`
    },
    onFriendClick: function() {
      window.open(`https://facebook.com/${this.username}`)
    }
  };
}
```

请注意，我们将`username`添加到了我们的对象中，这样`onFriendClick`就可以访问它，这样我们就可以用那个朋友的脸书页面打开一个新窗口。现在我们只需要编写 HTML:

```
<button id="Bob_Ross">
  <!-- A bunch of info associated with Bob Ross -->
</button> 
```

现在是 JavaScript:

```
const bobRossObj = initializeFriend(data[0])
const bobRossDOMEl = document.getElementById('Bob_Ross')
bobRossDOMEl.addEventListener("onclick", bobRossObj.onFriendClick)
```

在上面的代码中，我们为鲍勃·罗斯创建了一个对象。我们得到了与鲍勃·罗斯相关的 DOM 元素。现在我们想执行`onFriendClick`方法来打开 Bob 的脸书页面。应该按预期工作，对吗？

没有。

哪里出了问题？

注意，我们为 onclick 处理程序选择的函数是`bobRossObj.onFriendClick`。看到问题了吗？如果我们把它改写成这样呢:

```
bobRossDOMEl.addEventListener("onclick", function() {  window.open(`https://facebook.com/${this.username}`)})bobRossDOMEl.addEventListener("onclick", function() {
  window.open(`https://facebook.com/${this.username}`)
})
```

现在你明白问题了吗？当我们将 onclick 处理程序设置为`bobRossObj.onFriendClick`时，我们实际做的是获取存储在`bobRossObj.onFriendClick`中的函数，并将其作为参数传入。它不再“附属”于`bobRossObj`，这意味着`this`不再指`bobRossObj`。它实际上是指全局对象，也就是说`this.username`是未定义的。看来我们在这一点上运气不佳。

`bind`时间到了！

![o36QYF-UudyA0jO8JbooQYneFJo5jeA2oAtS](img/a6017132f462c24e24706f03732b8c78.png)

Photo by [Ksenia Makagonova](https://unsplash.com/photos/KiAZ61Sh17k?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/rope?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### 显式绑定此

我们需要做的是将`this`显式绑定到`bobRossObj`。我们可以通过使用`bind`来实现:

```
const bobRossObj = initializeFriend(data[0])
const bobRossDOMEl = document.getElementById('Bob_Ross')
bobRossObj.onFriendClick = bobRossObj.onFriendClick.bind(bobRossObj)
bobRossDOMEl.addEventListener("onclick", bobRossObj.onFriendClick)
```

之前，`this`是根据默认规则设置的。通过使用`bind`，我们将`bobRossObj.onFriendClick`中`this`的值显式设置为对象本身，或`bobRossObj`。

到目前为止，我们已经看到了为什么`this`是有帮助的，以及为什么您可能想要显式绑定`this`。最后一个关于`this`的话题是箭头函数。

### 箭头功能

你可能已经注意到了箭头函数是时髦的新事物。人们似乎喜欢它们，因为它们简洁而优雅。你可能知道它们和正常的函数有一点不同，但是你可能不太清楚区别在哪里。

也许描述箭头功能不同的最简单方式是这样的:

**无论`this`指的是声明了一个箭头函数的地方，`this`指的是这个箭头函数内部的同一个东西。**

> 好吧…那没有帮助…我认为那是一个正常函数的行为？

下面用我们的`initializeFriend`例子来解释一下。假设我们想在`greeting`中添加一个小助手函数:

```
function initializeFriend(data) {
  return {
    lastTenPosts: data.lastTenPosts,
    birthday: data.birthday,
    username: data.username,    
    fullName: `${data.firstName} ${data.lastName}`,
    getThreeRandomPosts: function() {
      // get three random posts from this.lastTenPosts
    },
    getDaysUntilBirthday: function() {
      // use this.birthday to get the num days until birthday
    },
    greeting: function() {
      function getLastPost() {
        return this.lastTenPosts[0]
      }
      const lastPost = getLastPost()           
      return `Hello, this is ${this.fullName}'s data!
             ${this.fullName}'s last post was ${lastPost}.`
    },
    onFriendClick: function() {
      window.open(`https://facebook.com/${this.username}`)
    }
  };
}
```

这行得通吗？如果不是，我们如何改变它使它工作？

不，它不会工作。因为`getLastPost`不是在对象的上下文中被调用的，所以`getLastPost`中的`this`退回到默认规则，即全局对象。

> 你说它不在“一个对象的上下文中”被调用…难道你不知道它是在从`initializeFriend`返回的对象中被调用的吗？如果这不叫“在对象的上下文中”，那么我不知道什么叫“在对象的上下文中”。

我知道“在对象的上下文中”是模糊的术语。也许确定一个函数是否在“对象的上下文中”被调用的一个好方法是告诉自己这个函数是如何被调用的，并确定一个对象是否“附加”到这个函数上。

让我们讨论一下执行`bobRossObj.onFriendClick()`时会发生什么。“给我抓取对象`bobRossObj`，寻找属性`onFriendClick`，然后**调用分配给该属性**的函数。”

现在让我们讨论一下执行`getLastPost()`时会发生什么。"给我一个名为`getLastPost`的函数，然后调用它."注意到没有提到一个物体吗？

好的，这里有一个比较棘手的来测试你的知识。假设有一个函数`functionCaller`，它所做的只是调用函数:

```
functionCaller(fn) {
  fn()
}
```

如果我们这样做呢:`functionCaller(bobRossObj.onFriendClick)`？你会说`onFriendClick`被称为“在对象的上下文中”吗？`this.username`会被定义吗？

我们来通说一下:“抓住物体`bobRossObj`，寻找属性`onFriendClick`。抓取它的值(恰好是一个函数)，传入`functionCaller`，命名为`fn`。现在，执行名为`fn`的功能请注意，该函数在被调用之前已经从`bobRossObj`中“分离”出来，因此不会“在对象`bobRossObj`的上下文中”被调用，这意味着`this.username`将是未定义的。

箭头对救援的作用:

```
function initializeFriend(data) {
  return {
    lastTenPosts: data.lastTenPosts,
    birthday: data.birthday,
    username: data.username,    
    fullName: `${data.firstName} ${data.lastName}`,
    getThreeRandomPosts: function() {
      // get three random posts from this.lastTenPosts
    },
    getDaysUntilBirthday: function() {
      // use this.birthday to get the num days until birthday
    },
    greeting: function() {
      const getLastPost = () => {
        return this.lastTenPosts[0]
      }
      const lastPost = getLastPost()           
      return `Hello, this is ${this.fullName}'s data!
             ${this.fullName}'s last post was ${lastPost}.`
    },
    onFriendClick: function() {
      window.open(`https://facebook.com/${this.username}`)
    }
  };
}
```

我们的规则来自上面:

**无论`this`指的是声明了一个箭头函数的地方，`this`指的是这个箭头函数内部的同一个东西。**

箭头函数在`greeting`内部声明。我们知道，当我们在`greeting`中使用`this`时，它指的是物体本身。因此，箭头函数中的`this`指的是我们想要的对象本身。

### 结论

`this`是开发 JavaScript 应用程序的一个有时令人困惑但很有用的工具。这绝对不是`this`的全部。未涵盖的一些主题有:

*   `call`和`apply`
*   当涉及到`new`时`this`如何变化
*   `this`如何随 ES6 `class`变化

我鼓励你问问自己，在某些情况下你认为`this`应该是什么样子，然后通过在浏览器中运行这些代码来测试自己。如果你想了解更多关于`this`的信息，请查看你不知道的[JS:this&对象原型](https://github.com/getify/You-Dont-Know-JS/tree/master/this%20%26%20object%20prototypes)。

如果你想测试你自己，看看 [YDKJS 练习:这个&物体原型](https://ydkjs-exercises.com/this-object-prototypes)。

![6MubkHTI9p32BuBFH5wqv-Sqp2DQBxLPhdDj](img/037cb1b49990553d58f4cf45389fb3df.png)

Photo by [Jonas Jacobsson](https://unsplash.com/photos/0FRJ2SCuY4k?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/books?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)