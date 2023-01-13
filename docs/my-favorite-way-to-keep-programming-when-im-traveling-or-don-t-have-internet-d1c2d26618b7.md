# 当我在旅行或没有互联网时，我最喜欢的保持编程的方式

> 原文：<https://www.freecodecamp.org/news/my-favorite-way-to-keep-programming-when-im-traveling-or-don-t-have-internet-d1c2d26618b7/>

这是一个关于提高你的技能和在旅途中保持高效的简短指南。也不包括把脸埋在书里。

### 书籍只能让你到此为止

现在不要误解我，我喜欢一本好的编程书。Jon Duckett 关于 HTML、CSS 和 JavaScript 的系列文章是我作为一名 Web 开发人员成长过程中的指路明灯。罗伯特·C·马丁的开创性巨著《干净的代码》的书页已经弯曲了。它是畸形的，因为每一滴信息都被榨干了。即使是西蒙·霍姆斯的《变得刻薄》，虽然现在已经过时，但也曾在当地的咖啡馆里陪伴过我。在我创建第一个全栈应用时，它是我的伴侣。

只要稍加准备，这些书中的大部分都可以在没有网络，或者更糟糕的是，网速很慢的情况下使用。提前下载软件包。让您的本地环境正常工作。如果这本书足够全面，你可能会在不需要谷歌、GitHub 或 StackOverflow 的情况下取得坚实的进展。

另一方面，作为程序员，我们在面对挑战时表现最好。有一个作者带领我们通过解决方案是很好的，但这还不够。我们提高解决问题能力的最好方法就是解决问题。

如果你是一名职业程序员，那么你很可能每天都在解决自己的问题。如果你是一个业余爱好者，那么你可能会从创建自己的 [JSF**k](http://www.jsfuck.com/) 应用程序中找到乐趣。甚至通过在线解决算法挑战来消磨时间。这就是 CodeWars 或 HackerRank 等网站如此受欢迎的原因。

其中大多数的潜在问题，尤其是后者，在互联网崩溃时仍然存在。或者一开始就没有联系。随着开发人员变得越来越游手好闲，这两种情况都很常见。你如何在从伦敦到上海的 12 小时飞行中消磨时间，同时还能从解决问题中获得回报？

我很不喜欢坐这么长时间的飞机。飞机上有足够的空间将你的笔记本电脑放在折叠托盘上。除此之外的一切都变成了俄罗斯方块游戏，试图让你的舒适和财产适合你的廉价航班上给你的有限空间。所以你把笔记本电脑、耳机、套头衫、零食和水都放在触手可及的地方了？开始觉得局促了吧？试着拿出你 600 页 2 公斤重的编程书。是啊，不可能的。

### 银弹

那么，我是如何克服这个障碍的呢？我重新实现了 Lodash 库。

为什么我选择了一个如此武断的任务？有许多关键原因。有些是我在接受挑战前合理化的，有些是我在过程中发现的。以下是一些最值得注意的例子:

*   每个函数都像是一个微型代码挑战
*   文档在一个 HTML 页面上，易于下载和脱机查看
*   它鼓励在遇到困难时查看源代码
*   它允许您构建自己的一套实用功能
*   这是一个没有依赖关系的库，它让事情变得简单
*   你将会更加熟悉你所选择的编程语言

让我们更深入地了解这几点。

#### **每个功能都像是一次代码挑战**

正如我前面提到的，Codewars 和 HackerRack 是两个非常受欢迎的编程挑战网站。对于那些不熟悉的人，您将在内置的文本编辑器中完成一项编程任务。完成后，您可以针对精选的测试套件运行您完成的代码。挑战的目标是通过所有测试。

自己效仿这个并不难。如果有的话，这是改进你的 TDD(测试驱动开发)方法的好方法。我重新实现函数的一般方法是去掉方法:

```
const concat = (arr, ...otherParams) => {
  // if array is invalid throw error

  // handle no input for second parameter

  // add each item to a new array
    // flatten 1 level if item is array

  // return new array
};
```

const concat = (arr，...otherParams) => { //如果数组无效抛出错误//不处理第二个参数的输入//将每一项添加到新数组//如果项是数组则展平 1 级//返回新数组}；

下一步是用一些我希望我的函数满足的断言创建我的测试套件:

```
const concat = require('../concat');

describe('concat', () => {
  it('should return the expect results with valid inputs', () => {
    expect(concat([1, 2], [1], [2], 4939, 'DDD')).toEqual([1, 2, 1, 2, 4939, 'DDD']);
    expect(concat([], null, 123)).toEqual([null, 123]);
  });

  it('should throw errors with invalid inputs', () => {
    expect(() => concat(23, 23).toThrow(TypeError));
    expect(() => concat([1, 2, 3], -1).toThrow(TypeError));
  });

  it('should correctly handle strange inputs', () => {
    expect(concat([111], null, 'rum ham')).toEqual([111, null, 'rum ham']);
  });
});
```

然后，我将实现代码，以便测试成功运行:

```
const { isValidArray } = require('../helpers');

const concat = (arr, ...otherParams) => {
  if (!isValidArray(arr)) throw new Error('Argument is not a valid array');

  if (otherParams.length === 0) return [];

  const concatenatedArray = otherParams.reduce((acc, item) => {
    if (isValidArray(item)) return [...acc, ...item];

    return [...acc, item];
  }, [...arr]);

  return concatenatedArray
};
```

去掉这些功能中的一个会让你有一种自豪感和成就感。

#### **简单的 HTML 文档**

大多数库都有一个带有 API 参考的 GitHub 页面。这些通常是一个可供下载的减价单页面。从重新组合库中取出一个片段:

### `branch()`

```
branch(
  test: (props: Object) => boolean,
  left: HigherOrderComponent,
  right: ?HigherOrderComponent
): HigherOrderComponent 
```

接受一个测试函数和两个高阶分量。测试函数是由所有者传递的道具。如果返回 true，则将`left`高阶分量应用于`BaseComponent`；否则，应用`right`高阶分量。如果没有提供`right`，默认情况下它将呈现包装的组件。

这里有足够的信息帮助你上路。如果您正在学习 React，并希望了解 HOCs(高阶组件),那么实现这个库可能是一个令人满意的挑战。

#### **查看源代码**

直到最近，我都不会花太多时间去了解我最常用的软件包是如何工作的。没有谷歌或 StackOverflow 让我绝望，所以我开始向内看。我不知道我期望看到什么，但它不是一个缩小的，混乱的混乱。

打开潘多拉魔盒并没有发出一大群嘲笑、仇恨和饥荒来嘲讽我和我的家人。相反，我受到了写得干净、文档记录良好的代码的欢迎。

你甚至可以看一眼 Lodash 的人是如何写出与你不同的解决方案的:

```
 function concat() {
  var length = arguments.length;
  if (!length) {
    return [];
  }
  var args = Array(length - 1),
      array = arguments[0],
      index = length;

  while (index--) {
    args[index - 1] = arguments[index];
  }
  return arrayPush(isArray(array) ? copyArray(array) : [array], baseFlatten(args, 1));
}
```

你会学到实现同样目标的新方法。也许他们的解决方案更有效，也许你的更有效。这仍然是开阔你的眼界，接受新的范例和模式的好方法。

#### **开发自己的实用功能**

Lodash 是一个占地面积很大的库，名声不好。项目可能需要少量的实用程序。我们仍然会将整个库作为一个依赖项导入。

你可以下载一些你使用的功能。为什么不用你在飞越太平洋时花了 8 个小时写的方法呢？它可能不那么健壮。但是每当您实现`_.memoize`时，您总是会想起您的夏威夷角节之旅。

#### **保持事情简单**

旅行很累，坐飞机很紧张。当感到疲惫时，任何阻碍任何规划的官僚主义都会成为障碍。这个想法是选择一个尽可能少摩擦的任务。

我不想在飞往加拿大的夜间航班上挤在两个打鼾者之间，被一堆随机的依赖项和混乱的供应商代码弄得团团转。发现 Lodash 不依赖任何外部模块是一个令人高兴的意外。Lodash 包本身的布局很简单。每个方法都有自己的文件，该文件可以导入一些基本方法或实用方法。

#### **熟悉您选择的工具**

如果您正在阅读这篇文章，那么您可能已经熟悉 JavaScript 了。像大多数其他现代编程语言一样，JavaScript 接受半定期更新。这些更新让您可以使用一些新功能。实现一个库可能会把你带到你所选择的语言中你从未到过的角落。它发生在我身上。

事实上，我最近遇到了一些 JavaScript 的新内置对象。我以前从未在代码中使用过它们，所以我有意识地将它们中的一些集成到我创建的实用方法中:

```
const difference = (arr, ...otherArgs) => {
  if (!isValidArray(arr)) throw new TypeError('First argument must be an array');

  const combinedArguments = otherArgs.reduce((acc, item) => [...acc, ...item], [])
  if (!isValidArray(combinedArguments)) throw new TypeError('2nd to nth arguments must be arrays');

  const differenceSet = new Set([...arr]);
  combinedArguments.forEach(item => {
    if (differenceSet.has(item)) differenceSet.delete(item);
  });

  return [...differenceSet]
}
```

在这里使用`Set()`很有意义。它与普通数组的区别在于只能存储唯一的值。这意味着集合中不能有任何重复的值。当试图创建一个删除重复值的函数时，这很有用。

无论你是吉他手、画家还是分子物理学家，如果你不熟悉你的吉他、颜料或分子，你就不会走得很远？

做程序员也是一样。掌握自己的工具，积极寻找自己知识的缺口。有意识地努力实现你以前没有遇到过的功能。或者用你觉得吓人的。这是最强有力的学习方法之一。

### **结论**

这不是在没有网络的情况下保持高效的唯一方法，但对我来说很有效。事实上，这是我建议人们在编程生涯的早期阶段做的事情。

我很想知道你是否做过类似的事情，或者你是否有自己的方法在没有互联网的情况下保持敏锐。下面让我知道！

你知道任何其他适合重写的包吗？

#### 感谢阅读！

知识共享是开发社区如此伟大的基石之一。请不要犹豫评论您的解决方案。

如果你有兴趣邀请我参加会议、meetup，或者作为演讲嘉宾参加任何活动，那么你可以在 [twitter](https://twitter.com/andricokaroulla?lang=en) 上给我发 DM！

我希望这篇文章教给你一些新的东西。我定期发帖子，所以如果你想了解我的最新消息，你可以关注我。记住，你按住拍手按钮的时间越长，你能发出的拍手声就越多。？？？

#### 你也可以看看我下面的其他文章:

[*用 React.lazy()*](https://codeburst.io/add-a-touch-of-suspense-to-your-web-app-with-react-lazy-374e66ee05af) 为你的 web app 增添一丝悬念

[*如何使用 Apollo 全新的查询组件管理本地状态*](https://medium.com/@andricokaroulla/updated-for-apollo-v2-1-managing-local-state-with-apollo-d1882f2fbb7)

[*不用等节假日，现在就开始装修*](https://codeburst.io/no-need-to-wait-for-the-holidays-start-decorating-now-67b9dabd60d7)

[*用阿波罗和高阶组件管理本地状态*](https://itnext.io/managing-local-state-with-apollo-client-3be522258645)

[*React 大会喝酒游戏*](https://medium.com/@andricokaroulla/the-react-conference-drinking-game-7a996bfbef3)

[*使用 Lerna、Travis 和现在的*](https://codeburst.io/develop-and-deploy-your-own-react-monorepo-app-in-under-2-hours-using-lerna-travis-and-now-2b140d647238) ，在 2 小时内开发和部署您自己的 React monorepo 应用程序