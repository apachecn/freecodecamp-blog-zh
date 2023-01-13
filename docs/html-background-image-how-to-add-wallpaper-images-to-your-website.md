# HTML 背景图片——如何给你的网站添加壁纸图片

> 原文：<https://www.freecodecamp.org/news/html-background-image-how-to-add-wallpaper-images-to-your-website/>

背景图片可以帮助美化网站，使其对用户更具吸引力。

在本文中，您将了解到:

*   如何使用 CSS `background-image`属性给你的网站添加背景图片。
*   图像的其他 CSS 背景属性。

## 如何给你的网站添加壁纸图片

在对网站进行编码时，使用图像作为网站的背景图像与使用`img`元素在 HTML 中插入图像是不同的。

要使用图像作为网站的背景，你需要使用 CSS。

这里有一个例子:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="style.css">
    <title>Background Image</title>
</head>

<body>
    <h1>Background image</h1>
</body>
</html>
```

```
h1{
    text-align: center;
}
```

我们上面有两个代码块 HTML 代码在网页上显示“背景图片”的文本，而 CSS 代码在页面上居中显示文本。

要在网站上添加一张覆盖整个页面的壁纸图片，你必须为`body`元素编写一些 CSS 规则。方法如下:

```
body{
    background-image: url('bg-image.jpg');
}
```

在上面的代码中，我们使用了`background-image`属性来给网页的主体添加图像。图像的路径/位置作为参数传递给`url()`函数:`url('bg-image.jpg')`。

这是网页现在的样子:

![full-bg-image](img/c3558883e551d63ad4b3313b805e3ea2.png)

## 背景图片比浏览器窗口小怎么办？

在图像比浏览器小的情况下，图像会重复几次，以覆盖剩余的空间。

这种重复并不适合每张图片。以下是上一节中使用的图像的缩小版在浏览器中的样子:

![repeat](img/f26768f0e8920d1a9bd637fa8cdb4233.png)

这幅图像被分成了四个不均匀的部分。除非这是您想要的效果，否则您可以使用`background-repeat`属性来修复它。

以下是解决图像重复问题的方法:

```
body{
    background-image: url('bg-image-small.jpg');
    background-repeat: no-repeat;
} 
```

在上面的代码中，我们给`background-repeat`属性分配了一个`no-repeat`值。

这是网页现在的样子:

![small-image](img/4e8c4032851ece1678bbcb54d448cd36.png)

图像不再在页面上重复出现，但是我们有了一个新问题——图像不再覆盖整个页面。

为了解决这个问题，我们使用了`background-size`和`background-attachment`属性:

```
body{
    background-image: url('bg-image-small.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    background-attachment: fixed;
}
```

将`background-size`属性的值设置为`cover`会使图像覆盖整个元素(在我们的例子中是`body`/整个页面)。

使用`background-attachment`属性的`fixed`值，图像的位置是固定的。这样，即使你在页面上滚动，它也保持在同一位置。

这是现在的图像:

![cover-image](img/2eaba33d77a03eedadc2fb6de3a59c4e.png)

拉伸一个小图像来覆盖整个页面的缺点是图像在被拉伸时质量下降，变得模糊。在这种情况下，在使用小图片作为网站背景图片之前，你应该考虑到这一点。

## 摘要

在这篇文章中，我们讨论了给网站添加壁纸图片。

你可以使用 CSS `background-image`属性给你的网站添加背景图片。

我们还学习了如何使用其他 CSS 背景属性，如`background-repeat`、`background-size`和`background-attachment`。

编码快乐！