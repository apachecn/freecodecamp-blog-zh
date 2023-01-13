# 这些常见的错误会导致不朽的 bug。学习如何避免它们。

> 原文：<https://www.freecodecamp.org/news/these-common-mistakes-will-lead-to-immortal-bugs-learn-how-to-avoid-them-eee79ee43cd5/>

作者:余伟

# 这些常见的错误会导致不朽的 bug。学习如何避免它们。

![ce6LgvNhzwjs7xke-BxFBd4hkfAsFVl-2GxW](img/9c45d7a5eb82ee6f93ed25b5505f91c3.png)

“people using desktop computer inside office” by [Giu Vicente](https://unsplash.com/@giuvicente?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们不是已经解决了吗？

在产品经理发布了 bug 的快照后，你或你的队友会问的问题。

整个事件感觉似曾相识。您试图追溯您在提交中修复该 bug 的时间，但是这有什么意义呢？

现实是，如果你不找到问题的根源，它就会像勒纳的九头蛇一样卷土重来。

![m8XUwZGMf2YTjk5zY2UhvOKC8qrJ2D5JEkIA](img/9e9220f6c69f97abaeba74e01087d6d7.png)

Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Hercules_and_the_Hydra_of_Lerna-_Hercules_grasps_his_club_with_both_hands_and_confronts_the_seven-headed_hydra,_from_the_series_%27The_Labors_of_Hercules%27_MET_DP832529.jpg)

下面是一些导致不死 bug 的模式。

### 硬编码值

当我们进行前端开发时，我们经常使用虚拟数据来快速构建用户界面。那很好。

问题是在多个位置有虚拟数据。然后推代码的时候很容易忘记一两个。

以下是一些建议:

*   对虚拟数据使用单个变量，并在不同的组件之间共享它
*   写一个类似`// TODO: remove later`的注释，以便以后方便搜索
*   不要依赖后端数据总是相同的

```
// ? 
```

```
<img src={require(`./assets/${backendData.fileName}.png`)} />
```

```
// This will break if fileName is an unexpected string
```

```
// ? 
```

```
let img;
```

```
try {
```

```
 img = require(`./assets/${backendData.fileName}.png`)
```

```
} catch(e) {  // Catch error if file doesn't exist
```

```
}<img src={img} />
```

```
// ? 
```

```
<img src={backendData.imgUrl} />
```

### 让我们稍后重构

随着工作的进行，您的代码库只会越来越大。你的客户或你的老板不会知道不重构代码的负面影响，因为表面上一切看起来都很好。

另外，写一些新东西并展示给其他人看总是更令人满意。“嘿，看看它现在能做什么。”

那么我们如何知道什么时候重构代码呢？

根据[重构大师](http://2018-10-28	14:00	21:00	2018-10	¥300.00	7:00:00	-¥2,100.00	David	Front-End	Bug fixes: speaker/headphone control. Countdown timer based on start_time plus duration. Update call status from in_progress to ended)的说法，遵循三条法则

1.  当你第一次做一件事时，就把它做完。
2.  当你第二次做类似的事情时，不要害怕重复，但还是要做同样的事情。
3.  当你第三次做一件事的时候，开始重构。

### 将所有内容保存在一个文件中

这伴随着代码重构。当项目是新的时，很难预测一个功能会变得多大。

过去，我有一个增长到数千行代码的文件。重构代码就像做手术一样，很微妙，但最终是值得的。

大多数人对于他们的项目是如何构建的都有一个原则。按页面、功能、开发方式分离文件。或者生产、私有或公共方法、MVC 模型等。

如何构建项目文件夹本身就是一个大讨论。我将把它留给另一篇文章。

### 不要为 API 编写规范

“等等，为什么图片不再显示了？”产品经理问前端开发者。

"等等，为什么数据不再有图片网址了？"前端开发人员检查控制台的网络响应。

“哦，是的，现在照片有三种尺寸。large_pic_url，med_pic_url，small_pic_url。"后端开发者听到了讨论，也跳了进来。

听起来很熟悉？

每个人都在努力做好自己的工作。但是协同作用不会发生在筒仓中。传达需求是每个人的职责。

这里有一些简单的解决方案。

*   在构建 API 之前，从前端和后端开发人员都可以访问的数据的 JSON 文件开始。
*   API 完成后，用[https://github.com/apidoc/apidoc](https://github.com/apidoc/apidoc)生成文档
*   部署前通知重大变更

有更复杂的方法来编写规格和处理文档。我很想在评论区听到你的团队使用了什么方法。

### 合并代码而不产生读取冲突

破坏东西不那么令人担心，因为你总是可以恢复到以前的提交。

问题是你或你的队友的代码在这个过程中消失了。

当你想“节省时间”并继续前进时，这种情况经常发生。

如果有疑问，请联系做出与您的承诺相冲突的承诺的开发人员。

搞砸的时候，`git merge --abort`或者`git-reset --hard`。

当它无法修复时，删除项目并再次克隆它。

做`git push -f`时三思。

### 无需测试即可修复可重用组件

可重用组件是每个开发人员的锦囊妙计。就像当客户向你推销包含几页相同日期选择器的线框时。

在你的脑海中，你想，“我要做一个很棒的日期选择器组件。少写代码。多做。”

几个月后，客户说，“我希望这个页面中的日期选择器像烟花一样爆炸，而在其他页面上，不同的小猫代表几个月”。现在，您需要日期选择器比以前更加动态。

然后在你做出改变后，你会发现小猫身上散发出火花。

如果你在一个更大的团队中，你可以将质量保证委托给另一个团队。

但是，如果您的组织中没有这样的功能，您将必须对您的组件名称进行全局搜索，以查看您的组件影响了什么。

把这些文件记下来。根据这些文件的用途，从视觉上和功能上测试它们。

### 任何时候你说，“现在很好”

你知道你以后还得回去。关键是“不要忘记”

当你在软件的开发速度和稳定性之间做出决定时，我们必须小心不要总是把速度放在第一位。这就像开车时从来不检查车况一样。

你可以决定速度和质量保证之间的比例。也许是两个快速迭代和一个质量保证迭代，无论哪个对你的团队有意义。

### 结论

*   尽可能避免硬编码值。如果非要，给自己留个条。
*   当你第三次做同样的事情时，重构。
*   经常拆分代码和重构
*   前端和后端工程师必须一起工作，就正在传递的数据达成一致。
*   如果合并冲突对您没有意义，请与编写提交的人仔细核对。
*   当对可重用组件进行更改时，测试它的使用位置。
*   为你的团队在速度和质量之间找到一个健康的平衡。

### 当我们在这里的时候…

如果你想接触到微信的 10 亿用户，你得先了解它。这里有一个 [**免费词汇表**](https://pages.convertkit.com/b2469604dd/0c671fdd2d) 来入门。