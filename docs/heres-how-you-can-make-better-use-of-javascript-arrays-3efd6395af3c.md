# 以下是如何更好地利用 JavaScript 数组的方法

> 原文：<https://www.freecodecamp.org/news/heres-how-you-can-make-better-use-of-javascript-arrays-3efd6395af3c/>

按 pacdiv

# 以下是如何更好地利用 JavaScript 数组的方法

![1*IZcJKz3761vChU1VFHfzkw](img/cc4b10d8b5418090e37135a2ba2b51df.png)

Photo by Ben White on Unsplash

快速阅读，我保证。在过去的几个月里，我注意到在我检查的拉请求中，完全相同的四个错误不断出现。我贴这篇文章也是因为我自己也犯过这些错误！让我们浏览它们以确保我们正确地使用了数组方法！

### 用 Array.includes 替换 Array.indexOf

“如果你在你的数组中寻找什么，使用 Array.indexOf”我记得在我学习 JavaScript 的课程中读到过这样一句话。这句话很对，毫无疑问！

MDN 文档称，Array.indexOf“返回可以找到给定元素的第一个索引”。因此，如果我们稍后在代码中使用返回的索引，Array.indexOf 就是解决方案。

但是，如果我们只需要知道我们的数组是否包含值呢？看起来像是一个是/否的问题，一个布尔型的问题。对于这种情况，我建议使用 Array.includes，它返回一个布尔值。

### 使用 Array.find 而不是 Array.filter

Array.filter 是一个非常有用的方法。它从另一个数组创建一个新数组，所有的项都传递回调参数。顾名思义，我们必须使用这个方法进行过滤，以获得一个更短的数组。

但是，如果我们知道我们的回调函数只能返回一个项目，我不推荐这样做——例如，当使用通过唯一 ID 过滤的回调参数时。在这种情况下，Array.filter 将返回一个只包含一项的新数组。通过查找特定的 ID，我们的意图可能是使用数组中包含的唯一值，使这个数组变得无用。

来说说性能吧。要返回与回调函数匹配的所有项，Array.filter 必须浏览整个数组。此外，假设我们有数百个项目满足回调参数。我们过滤后的数组会很大。

为了避免这些情况，我推荐 Array.find，它需要一个像 Array.filter 这样的回调参数，它返回满足这个回调的第一个元素的值。此外，Array.find 会在项目满足回调时立即停止。不需要浏览整个数组。此外，通过使用 Array.find 来查找项目，我们可以更清楚地了解我们的意图。

### 用 Array.some 替换 Array.find

我承认这个错误我犯过很多次了。然后有个好心的朋友跟我说查一下 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#Methods_2)有更好的办法。事情是这样的:这与我们上面的 Array.indexOf/Array.includes 案例非常相似。

在前一个例子中，我们看到 Array.find 需要一个回调函数作为参数，并返回一个元素。如果我们需要知道我们的数组是否包含一个值，找到最佳解？可能不会，因为它返回一个值，而不是一个布尔值。

对于这种情况，我推荐使用 Array.some，它返回所需的布尔值。此外，从语义上讲，使用 Array.some 强调了我们不需要找到的项这一事实。

### 使用 Array.reduce 而不是链接 Array.filter 和 Array.map

面对现实吧，Array.reduce 并不容易理解。是真的！但是，如果我们运行 Array.filter，然后运行 Array.map，我们会感觉缺少了什么，对吗？

我的意思是，我们在这里浏览数组两次。第一次过滤并创建一个较短的数组，第二次创建一个新的数组(再次！)包含基于我们从 Array.filter 获得的值的新值。每个方法都有自己的回调函数和一个我们以后不能使用的数组——由 Array.filter 创建的数组。

为了避免这方面的低性能，我的建议是使用 Array.reduce。同样的结果，更好的代码！Array.reduce 允许我们过滤并添加满意的项目到一个累加器中。例如，这个累加器可以是要递增的数字、要填充的对象、要连接的字符串或数组。

在我们的例子中，由于我们一直在使用 Array.map，我建议使用 Array.reduce 和一个数组来连接作为累加器。在下面的例子中，根据 *env* 的值，我们将把它添加到我们的累加器中，或者保持这个累加器不变。

#### 就是这样！

希望这有所帮助。如果您对本文有任何想法或有任何其他用例要展示，请务必留下评论。如果你觉得有帮助，给我一些掌声？分享一下。感谢阅读！

PS:你可以[在 Twitter 这里](https://twitter.com/pacdiv_io)关注我。

*注:*如[马尔戈斯塔普](https://www.freecodecamp.org/news/heres-how-you-can-make-better-use-of-javascript-arrays-3efd6395af3c/undefined)和[大卫·皮普格拉斯](https://www.freecodecamp.org/news/heres-how-you-can-make-better-use-of-javascript-arrays-3efd6395af3c/undefined)在评论中提到的，请在使用 Array.find 和 Array.includes 之前检查支持，Internet Explorer 目前还不支持。