# 如何用几行代码在 iOS 应用程序中创建一致的自定义字体

> 原文：<https://www.freecodecamp.org/news/how-to-use-consistent-custom-font-in-an-ios-app-e07b1ddb7a7c/>

作者:藤木由一

在本文中，你将学习如何用这些简单的技巧在你的应用程序中创建一个统一的自定义外观。

## 我们想做什么

在 iOS 中，有一个叫做`UIAppearance`的机制来定义一个 app 的全局配置。例如，您可以用一行代码设置导航栏的一致背景颜色:

```
UINavigationBar.appearance().barTntColor = UIColor.blue
```

但是，如果对字体应用同样的方法，如下所示:

```
UILabel.appearance().font = UIFont(named: "Gills Sans", size: 14)
```

所有的`UILabel`实例确实都有“Gills Sans”，**，但是大小也是 14pt】。我不认为任何应用程序会希望整个应用程序只有**14pt 字体。由于`UIFont`总是需要初始化尺寸信息，所以`UIKit`中没有**标准方式**只改变字体而不影响尺寸。****

但是，难道你不想改变字体字样，而不追捕所有的 Swift 文件和界面生成器文件吗？想象一下，你有一个有几十个屏幕的应用程序，产品负责人决定改变整个应用程序的字体。或者你的应用程序想覆盖另一种语言，你想为这种语言使用另一种字体，因为这样看起来更好。

这篇短文解释了如何只用几行代码就能做到这一点。

## UIView 扩展

当您创建`UIView`扩展并用`@objc`修饰符声明一个属性时，您可以通过`UIAppearance`设置该属性。

例如，如果您像这样声明一个`UILabel`扩展属性`substituteFontName`:

你可以在`AppDelegate.application(:didFinishLaunching...)`中调用它

```
UILabel.appearance().substituteFontName = "Gills Sans"
```

瞧，所有的`UILabel` 都将是适当大小的“Gills Sans”字体。？

## 但是你也想用粗体，不是吗？

到目前为止，生活还不错，但是如果你想指定同一种字体的两种不同变体，比如粗体，该怎么办呢？你可能认为你可以添加另一个扩展属性`substituteBoldFontName`并像这样调用它，对吗？

```
UILabel.appearance().substituteFontName = fontNameUILabel.appearance().substituteBoldFontName = boldFontName
```

没那么容易。如果你这样做，那么**所有的**和`UILabel`实例都以粗体显示。我会解释原因。

看起来`UIAppearance`只是按照注册的顺序调用所有注册属性的 setter 方法。

所以如果我们像这样实现`UILabel`扩展:

然后通过`UIAppearance`设置这两个属性，在每次`UILabel`初始化时都会产生类似下面的代码序列。

```
font = UIFont(name: substituteFontName, size: font.pointSize)font = UIFont(name: substituteBoldFontName, size: font.pointSize)
```

所以，第一行被第二行覆盖了，你会发现到处都是粗体。？

### 你能做些什么呢？

为了解决这个问题，我们可以自己在原有字体样式的基础上分配合适的字体样式。

我们可以如下更改我们的`UILabel`扩展名:

现在，在`UILabel`初始化时调用的代码序列如下，在一个条件中只调用两个`font`赋值中的一个。

```
if font.fontName.range(of: "Medium") == nil {   font = UIFont(name: newValue, size: font.pointSize)}if font.fontName.range(of: "Medium") != nil {   font = UIFont(name: newValue, size: font.pointSize)}
```

因此，你将拥有一个统一的漂亮文本风格的应用程序，同时常规字体和粗体字体共存，耶！？

您应该能够使用相同的逻辑来添加更多的样式，如斜体字体。此外，您应该能够将相同的方法应用于另一个控件，如`UITextField`。

希望这对 iOS 开发者们有所帮助，编码快乐！！