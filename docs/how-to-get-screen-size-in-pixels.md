# 如何获得以像素为单位的屏幕尺寸

> 原文：<https://www.freecodecamp.org/news/how-to-get-screen-size-in-pixels/>

有时候，您的 JavaScript 应用程序需要知道屏幕需要多大才能执行某些操作。

幸运的是，内置的 JavaScript 函数可以很容易地在用户设备上以像素为单位抓取不同尺寸的屏幕。你用哪种取决于你想做什么。

## **获取用户的分辨率**

您可能希望做一些涉及用户设备分辨率的事情。在这种情况下，您应该使用内置的`screen.width`和`screen.height`属性。这些给你你正在处理的屏幕的大小。

****这不是你在**页面上要处理的区域。T **这些值代表整个屏幕**，也就是**用户的显示分辨率。****

## **获取浏览器大小**

可能有一个有趣的应用程序来处理浏览器的当前大小。如果您需要访问这些维度，请使用`screen.availWidth`和`screen.availHeight`属性来完成。

请记住，这些是整个浏览器窗口的尺寸，从浏览器窗口的顶部到浏览器遇到任务栏或桌面边缘的位置，这取决于您的设置。

****一个有趣的注意事项**** : `screen.availHeight`也可以用来算出电脑上的任务栏有多高。如果你的浏览器分辨率是说`1366 x 768`，并且`screen.availHeight`报告 728 像素，那么你的任务栏是 40 像素高。你也可以通过减去`screen.height`和`screen.availHeight`来计算任务栏高度:

```
var taskbarHeight = parseInt(screen.height,10) - parseInt(screen.availHeight,10) + " pixels";
/*
For a user that has a screen resolution of 1366 x 768 pixels, their taskbar is likely 40 pixels if using Windows 10 with no added accessibility features.
*/
```

## **获取观察窗尺寸**

这些属性很有趣，可以用来创建一些漂亮的效果。您可以使用`window.innerHeight`和`window.innerWidth`来获取用户看到的网页窗口的大小。

请记住，这些值不是静态的，会随着浏览器本身的变化而变化。换句话说，如果浏览器本身很小，这些值会更小，如果浏览器被最大化，它们会更大。

例如，如果您正在使用 Google Chrome，并且您打开了控制台(必须停靠在窗口的一侧)，`window.innerHeight`将会改变以反映控制台的高度，因为窗口的一部分将会被遮挡。

您可以通过调用`window.innerHeight`来测试这一点，记下该值，然后增加控制台的大小，然后再次调用`window.innerHeight`。

如果您的浏览器以任何方式被最大化或调整大小，这些属性也会改变。在浏览器的最大尺寸下，属性`window.innerWidth`与`screen.width`和`screen.availWidth`相同(除非边上有任务栏，在这种情况下`screen.availWidth`将不相等)。`window.innerHeight`等于页面本身的窗口面积(网页的面积)。

## **获取网页的高度和宽度**

如果您需要查看您的 web 页面实际有多高或多宽，有一些属性可以获取这些维度:`document.body.offsetWidth`和`document.body.offsetHeight`。

这些属性代表了页面正文中内容的大小。没有内容的页面的`document.body.offsetHeight`与`window.innerHeight`的值接近，这取决于文档正文上设置的边距/填充。如果在 html 根元素和文档主体上的边距和填充被设置为`0`，那么`document.body.offsetHeight`和`window.innerHeight`将等于没有内容。

这些属性可以用来与您的页面/应用程序进行交互，这取决于您想要做什么。