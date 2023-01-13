# 如何在 Git 中推送空提交

> 原文：<https://www.freecodecamp.org/news/how-to-push-an-empty-commit-with-git/>

在本文中，我们将讨论如何在不做任何更改的情况下在 Git 中推送提交。

Git 使得推送空提交的过程变得非常简单。除了添加了`--allow-empty`标志之外，这就像推送一个常规的提交。

```
git commit --allow-empty -m "Empty-Commit"
```

您现在需要将它推到主目录。为此，您可以使用以下命令:

```
git push origin main
```

您可以看到，在运行上述命令之后，提交已经被推送到您的分支，没有任何更改。

### 为什么需要推送空提交？

您可能需要在不对项目进行任何更改的情况下开始构建。或者您可能无法手动启动构建。开始构建的唯一方法是使用 Git。您可以通过推送空提交来开始构建，而无需对项目进行任何修改。

就这么定了！不是这么简单吗？🥳

我也定期在我的时事通讯上写作。你可以直接在这里注册。 ****👇👇****

[https://thelearners.substack.com/embed](https://thelearners.substack.com/embed)