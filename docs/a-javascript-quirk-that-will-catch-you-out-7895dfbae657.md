# Javascript 的一个怪癖会让你措手不及

> 原文：<https://www.freecodecamp.org/news/a-javascript-quirk-that-will-catch-you-out-7895dfbae657/>

威廉·伍德黑德

# Javascript 的一个怪癖会让你措手不及

![FtzxYNzGhUIRRt5HoVfsJv3mj3nWcdgYPjfA](img/6a92d6f41a95c4fb3b919e921fadcd8a.png)

Cloning sheep

这篇文章描述了优秀的 Javascript 开发人员应该知道的一个特性。一旦你读了这篇文章，你可以假装和我一样早就知道了。

**注意:**如果你不是 Javascript 开发人员，这可能有点棘手。如果是的话，那就系好安全带来一次极短的旅程。

这一切都与*复制*变量(或克隆，因此是羊)有关。

让我们直入主题吧。

### **复制字符串**

让我们快速编码一下:

```
var initialName = ‘William’;
```

```
var copyName = initialName;copyName = ‘Bill’;
```

```
console.log(initialName);   // prints ‘William’console.log(copyName);      // prints ‘Bill’
```

这一切似乎都在意料之中。我们复制 **initialName** ，然后改变它的值。**姓名**打印 ***'* 威廉'**和**姓名**打印'**比尔'**。

到目前为止一切顺利。让我们用对象代替字符串来做同样的练习。(*期待意想不到的*

### 复制对象

```
var initialObject = { name: ‘William’ };
```

```
var copyObject = initialObject;copyObject.name = ‘Bill’;
```

```
console.log(initialObject.name);   // prints ‘Bill’console.log(copyObject.name);      // prints ‘Bill’
```

嗯，问题就在这里。当我们打印**初始对象**的名称时，它已经变成了**票据**。

那么这里发生了什么？

当我们在 **copyObject、**中设置**名称**时，它也改变了 **initialObject** 中的**名称**。这是因为对象通过引用被复制*。 **copyObject** 只是对底层数据的引用。*

因此，当我们更改 **copyObject** 中的**名称**时，它也会更改 **initialObject** 中的**名称**，因为它引用了底层数据的相同位。

### 在那里你可能会被抓到

在大型应用程序中，这可能导致部分数据结构同时出现在多个地方。

因此，如果您在应用程序代码的某个部分更改了一个对象，您可能会在其他地方更改它。这有时会导致不希望的行为(如重新渲染)，并且可能难以调试和隔离。

虽然这看起来很简单，但是在使用流行框架的复杂 web 应用程序中，这个简单的基础问题会让开发人员非常头疼。

#### 还原/反应示例

我看到开发人员一次又一次被这个问题困扰的一个例子是在 [Redux action creators](https://redux.js.org/docs/basics/Actions.html#action-creators) 中，你在分派动作之前操纵状态。通过操纵传递给动作创建者的对象而不克隆它，您实际上可以改变底层状态，或者在您的分派之前对组件状态做出反应。

### 我们的解决方案

有许多库提供对象和数组的克隆功能，例如 Lodash。

在 [pilcro](https://www.pilcro.com/?utm_source=medium&utm_medium=jsquirk&utm_campaign=awareness) 我们使用[脸书的不可变. js](https://facebook.github.io/immutable-js/) 来避免这个问题。尽管它是一个大型库，但它使我们的开发人员能够编写可预测的函数式 javascript 并避免这个陷阱。我不能推荐它。

这就是我们所知道的，Javascript 中一个非常简单但重要的特性。如果你不是一个 Javascript 开发人员，那么为你能坚持到最后而欢呼吧。

如果你*是*高级 Javascript 开发人员，并且这对你来说是新闻，你应该会有这样的感觉:

![c4C35c05ok6vxcah56o39Yxdc3jpjiq0--sz](img/e5ab17ef3f058ba3768e3cbc741be92a.png)

giphy

*如果你喜欢这个故事，请问？并请与他人分享。另外，请到 ilcro.com 参观我的公司。 Pilcro 帮助初创公司在所有不同的营销渠道中保持品牌一致性。你会喜欢我们所做的！*