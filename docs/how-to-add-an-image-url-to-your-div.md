# CSS 背景图片——如何给你的 Div 添加图片 URL

> 原文：<https://www.freecodecamp.org/news/how-to-add-an-image-url-to-your-div/>

假设你想在网页上放一两张图片。一种方法是使用 **`background-image`** CSS 属性。

该属性将一个或多个背景图像应用于一个元素，如 **`<div>`、**文档所解释的。出于美观的原因使用它，例如为您的网页添加纹理背景。

# 添加图像

使用`background-image`属性添加图像很容易。只需在`url()`值中提供图像的路径。

图像路径可以是 URL，如下例所示:

```
div {
   /* Background pattern from Toptal Subtle Patterns */
   background-image: url("https://amymhaddad.s3.amazonaws.com/morocco-blue.png");
   height: 400px;
   width: 100%;
} 
```

也可以是本地路径。这里有一个例子:

```
body {
   /* Background pattern from Toptal Subtle Patterns */
   height: 400px;
   width: 100%;
   background-image: url("./images/oriental-tiles.png");
} 
```

# 添加多个图像

您可以将多个图像应用到`background-image`属性。

```
div {
/* Background pattern from Toptal Subtle Patterns */
   height: 400px;
   width: 100%;
   background-image: 
       url("https://amymhaddad.s3.amazonaws.com/morocco-blue.png"),
       url("https://amymhaddad.s3.amazonaws.com/oriental-tiles.png");
   background-repeat: no-repeat, no-repeat;
   background-position: right, left; 
} 
```

代码太多了。我们来分解一下。

用逗号分隔每个图像`url()`值。

```
background-image: 
    url("https://amymhaddad.s3.amazonaws.com/morocco-blue.png"),
    url("https://amymhaddad.s3.amazonaws.com/oriental-tiles.png"); 
```

现在，通过应用附加属性来定位和增强您的图像。

```
background-repeat: no-repeat, no-repeat;
background-position: right, left; 
```

有几个[子属性](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Backgrounds_and_Borders/Using_multiple_backgrounds)你可以添加到你的背景图片中，比如 **`background-repeat`** 和 **`background-position`** ，我们在上面的例子中使用过。你甚至可以给背景图片添加[渐变](https://developer.mozilla.org/en-US/docs/Web/CSS/gradient)。

看看我们把所有东西放在一起是什么样子。

[https://codepen.io/amymhaddad/embed/preview/VwLqWbm?height=300&slug-hash=VwLqWbm&default-tabs=css,result&host=https://codepen.io](https://codepen.io/amymhaddad/embed/preview/VwLqWbm?height=300&slug-hash=VwLqWbm&default-tabs=css,result&host=https://codepen.io)

# 秩序至关重要

你列出你的图片的顺序是由堆叠顺序决定的。根据[文档](https://www.w3.org/TR/css-backgrounds-3/#layering)，这意味着列出的第一个图像离用户最近。然后，下一个，下一个，等等。

这里有一个例子。

```
div {
/* Background pattern from Toptal Subtle Patterns */
   height: 400px;
   width: 100%;
   background-image: 
       url("https://amymhaddad.s3.amazonaws.com/morocco-blue.png"),
       url("https://amymhaddad.s3.amazonaws.com/oriental-tiles.png");
   background-repeat: no-repeat, no-repeat;
} 
```

我们在上面的代码中列出了两张图片。第一个图像(morocco-blue.png)将位于第二个图像(oriental-tiles.png)的前面。两幅图像大小相同，缺乏不透明度，所以我们只看到第一幅图像。

但是，如果我们将第二张图片(oriental-tiles.png)向右移动 200 像素，那么你可以看到它的一部分(其余部分保持隐藏)。

这是我们把所有东西放在一起看起来的样子。

[https://codepen.io/amymhaddad/embed/preview/oNXrXMo?height=300&slug-hash=oNXrXMo&default-tabs=css,result&host=https://codepen.io](https://codepen.io/amymhaddad/embed/preview/oNXrXMo?height=300&slug-hash=oNXrXMo&default-tabs=css,result&host=https://codepen.io)

## 什么时候应该使用背景图片？

关于`background-image`房产有很多值得喜欢的地方。但是有一个缺点。

文档指出，图像可能无法被所有用户访问，比如那些使用屏幕阅读器的用户。

这是因为您不能向`background-image`属性添加文本信息。因此，屏幕阅读器将会错过该图像。

只有当你需要给页面添加一些装饰时，才使用`background-image`属性。否则，使用 HTML **`<img>`** 元素，如果图像有意义或目的，如[文档](https://developer.mozilla.org/en-US/docs/Web/CSS/background-image)所述。

这样，您可以使用 **`alt`** 属性向图像元素添加文本，其中[描述了图像](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img)。屏幕阅读器将获取提供的文本。

```
<img class="house" 
       src="./images/farnsworth_house.jpeg"
       alt="A glass house designed by Ludwig Mies van der Rohe located in Plano, Illinois."> 
```

可以这么想:`background-image`是 CSS 属性，CSS 侧重于展现或者样式；HTML 侧重于语义或意义。

说到图像，你有很多选择。如果需要一个形象做装饰，那么`background-image`房产可能是你不错的选择。

**我写的是学习编程以及学习编程的最佳方法(**【amymhaddad.com】)。