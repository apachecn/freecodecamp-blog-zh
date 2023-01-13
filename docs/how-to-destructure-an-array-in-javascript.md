# 如何在 JavaScript 中析构数组

> 原文：<https://www.freecodecamp.org/news/how-to-destructure-an-array-in-javascript/>

数组析构是从存储在数组中的数据中提取多个值的有效方法。

在本教程中，我们将学习数组析构。我们将通过例子来学习数组析构是如何工作的。

我还制作了这个教程的视频:

[https://www.youtube.com/embed/x-ih5R5-DCc?feature=oembed](https://www.youtube.com/embed/x-ih5R5-DCc?feature=oembed)

让我们开始吧。

让我们打开 web 浏览器，然后打开 JavaScript 控制台，我们将在那里编写代码。你可以在这里找到如何打开控制台[的说明。](https://balsamiq.com/support/faqs/browserconsole/)

## 如何从数组中析构元素

接下来，我们将创建一个名为 animals 的数组，并添加 dog、cat 和 horse 的值。

`const animals = ['Dog', 'Cat', 'Horse']`

接下来，假设我们想要创建一个只包含 dog 值的变量。我们将这个变量称为`dogVar`，是 dog 变量的简称。在 ES6 中引入数组析构之前，我们会这样做:

`dogVar = animals[0]`

接下来，我们希望猫和马的值也包含在它们自己的变量中。我们会说:

`const catVar = aniamls[1]`

`const horseVar = animals[2]`

这里我们写了 3 行独立的代码。让我们改用数组析构，写 1 行代码而不是 3 行。

## 解构是如何运作的

使用数组析构，我们可以只写出一行代码:

`const [firstElement, secondElement, thirdElement] = animals`

这看起来像是我们在这里创建了一个数组，但我们没有。我们正在**破坏**这个阵列。析构使得将数组中的值解包到不同的变量中成为可能。

析构获取左侧数组中的每个变量，并将其映射到`animals`数组中*相同索引*处的元素。

当我们写出`firstElement`时，我们说我们想要访问动物数组中的第一个元素，然后*将它*赋给变量`firstElement.`

使用`secondElement`，我们说我们想要访问数组中的第二个元素，并将它赋给变量`secondElement`。同样的事情也适用于`thirdElement`变量。

这里关键的一点是这些名字并不重要。重要的是顺序。

查看我们的析构的*顺序*会告诉我们数组中的哪些元素被分配给了哪些变量。

让我们看一下我们的 1 行代码，在这里我们析构了数组。让我们把它的这一部分`[firstElement, secondElement, thirdElement]`想象成一个数组。

如果这是一个数组，`firstElement`将位于数组的位置`0`。JavaScript 将看到这个`firstElement`变量在位置`0`，然后它将进入`animals`数组并找到位置`0`的元素，并将该元素赋给变量`firstElement`。

(记住数组是零索引的，这意味着我们从 0 开始计数，而不是从 1 开始。)

在析构的时候，我们可以给变量取任何我们想要的名字。再说一遍，顺序才是最重要的，而不是命名。如果我们想，我们可以写:

`const [dog, cat, horse] = animals`

现在我们有了所有的价值观。如果我们写出`dog, cat, horse`，我们可以看到所有变量名都有正确的值:

`dog // returns 'Dog'`

`cat // returns 'Cat'`

`horse // returns 'Horse'`

如果我们回到这个例子开始时的代码，我们有 3 行代码为狗、猫和马创建变量。使用数组析构，我们只用了一行代码。析构只是一种捷径。这是一种从数组中提取多个值的简单快捷的方法。

但是，如果您只想从数组中获取一个元素，比如说数组中的第二个或第三个元素，并将该元素存储在一个变量中，该怎么办呢？

## 如何析构数组中的第二个或第三个元素

接下来，假设我们有一个水果数组:

`const Fruits = ['banana', 'apple', 'orange']`

如果我们只想得到苹果的值，并把它赋给苹果的变量名呢？

我们不能只做`const [apple] = animals`。为什么不呢？如果我们这样做，那么变量`apple`将会错误地被赋予`'banana'`的值。这是为什么呢？

这还是因为，顺序很重要。用`const [apple] = fruits`，JavaScript 查看`apple`，看到它在位置`0`，然后找到`fruits`数组中位置 0 的元素，也就是`'banana'`，把那个元素赋给 apple 的变量。

我们不希望这种事情发生。那我们该怎么办？

相反，我们可以写:`const [, apple] = fruits`

这个逗号充当一种占位符。这个逗号告诉 JavaScript 就像第一个元素在那里一样，所以这个`apple`变量现在是这里的第二个元素。换句话说，`apple`现在位于`1`位置。

假设我们只想知道变量中`orange`的值，我们不关心苹果或香蕉元素。我们可以像这样使用逗号:

`const [, , orange] = fruits`

如果我们在控制台中写出`orange`，我们会看到我们成功地创建了橙色变量，它的值为`'orange'`。

最后要注意的是，如果你学习了 React，你可能会经常在 React 钩子上使用数组析构。例如，您可能会看到这样的内容:

`const [count, setCount] = useState(0)`

我们走吧。我们已经学习了所有关于数组析构的知识。

感谢您的阅读！

如果你喜欢这篇文章， ********加入我的[编码俱乐部](https://madisonkanna.us14.list-manage.com/subscribe/post?u=323fd92759e9e0b8d4083d008&id=033dfeb98f)，******** ，在那里我们每周日一起应对编码挑战，并在学习新技术时相互支持。

如果你对这篇文章有反馈或问题，或者在 Twitter 上找到我。