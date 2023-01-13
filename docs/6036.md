# 如何唯一地命名你的元素并自动测试你的桌面应用

> 原文：<https://www.freecodecamp.org/news/how-to-uniquely-name-your-elements-and-automate-tests-for-your-desktop-app-8fec67eaca4b/>

由红苹果果子露制成

# 如何唯一地命名你的元素并自动测试你的桌面应用

![1*N9MfB3_mghGuOlRHX5YAzw](img/03fba857b07d7f08dc6779c3c1e1466f.png)

Photo by [Ricardo Gomez Angel](https://unsplash.com/photos/KmKZV8pso-s?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/unique?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### 动机

为桌面应用程序编写自动化测试不是一件容易的事情。尤其是当它使用 Windows 演示基础(WPF)时。这允许嵌套控件和复杂的网格和菜单有如此多的可能性。

在 [Clemex](https://www.clemex.com/) 这里，我们使用一个工具来自动化依赖于 WPF `[FrameworkElement.Name](https://msdn.microsoft.com/en-us/library/system.windows.frameworkelement(v=vs.110).aspx)`属性与应用程序交互的桌面测试。因为我们需要基于集合数据创建动态控件，所以它们可能会以多个 UI 元素的相同名称结束。

例如，下面的代码基于面板集合生成一个菜单。

检查元素树，我们会看到现在有多个元素具有相同的名称:“MenuBtn”。

为了避免这种情况并为每个按钮指定唯一的名称，我们提出了四种不同的方法。

*   使用代码隐藏
*   使用数据绑定
*   使用附加属性
*   使用集合索引

### 使用代码隐藏

假设我们可以访问元素数据上下文中的某个惟一 ID。最简单的方法是使用代码隐藏模型，使用`FrameworkElement`的`Loaded`事件来设置唯一的名称。

现在，当我们检查元素树时，我们会看到每个按钮都有唯一的名称。

### 使用数据绑定

使用数据绑定使我们的代码更加清晰，也更容易阅读和理解。如果我们尝试使用数据绑定的类似方法，我们可能会得到如下源代码:

不幸的是，如果我们试图构建这段代码，我们将得到一个编译错误，消息如下:

> Uid 或 Name 属性值不允许 MarkupExtensions，因此“{Binding Panel。PanelType，StringFormat='MenuBtn{0}'} '无效。

这个限制阻止我们直接绑定到`Name`属性。

### 使用附加属性

为了克服前面尝试的限制，我们可以定义一个新的属性来为我们设置名称。要向现有控件添加新属性，我们可以使用[附加属性](https://docs.microsoft.com/en-us/dotnet/framework/wpf/advanced/attached-properties-overview)。

每当我们的属性值改变时，就会触发`OnValueChanged`事件。当这种情况发生时，我们获得新的值，并将其设置为`FrameworkElement`名称。我们把我们的附属财产命名为`Name`。它可以是我们想要的任何东西，比如`CustomName`或`TestName`。

要使用新属性，我们需要向 XAML 添加一个名称空间，并将该属性附加到按钮上。

我们的代码现在编译起来没有任何问题，并且我们将为每个元素指定唯一的名称。

### 使用集合索引

在前面的例子中，我们通过添加属性`Id`创建了唯一的名称。在其他场景中，我们没有条目的 ID 来创建唯一的元素名。为此，我们可以使用集合索引。

让我们尝试将按钮集合绑定到字符串列表。

为了实现这一点，我们可以对转换器使用相同的`AttachedProperty`。它将在集合中查找元素的索引。

在 XAML 中，我们现在将使用[多重绑定](https://msdn.microsoft.com/en-us/library/system.windows.data.multibinding(v=vs.110).aspx)，因为我们需要元素和集合。

查看元素树我们可以看到我们的按钮被命名为`MenuBtn00`、`MenuBtn01`等等。

### 摘要

为动态创建的 WPF 控件生成唯一的名称可以通过使用附加属性和使用带有自定义转换器的多重绑定以一种优雅的方式来完成。