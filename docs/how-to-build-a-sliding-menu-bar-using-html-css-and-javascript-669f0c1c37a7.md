# 如何使用 HTML、CSS 和 JavaScript 构建滑动菜单栏

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-sliding-menu-bar-using-html-css-and-javascript-669f0c1c37a7/>

作者:苏普里亚·沙希瓦桑

# 如何使用 HTML、CSS 和 JavaScript 构建滑动菜单栏

![1*x_Hcn2GhZZoiwhWBSnVGTA](img/abf135fe96f8ab26f768d1584ceb90ec.png)

Photo by [rawpixel](https://unsplash.com/photos/ti6zU07Xfjw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/computer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当你登陆一个网站时，菜单就是你要找的东西。它有选项，让你访问网站提供给你的一切。你肯定会说它是一个网站的重要组成部分，对吗？

这个月，我和我的朋友 Girish patil 开始为前沿开发者发布一份双周通讯。第一份时事通讯以滑动菜单栏为特色，所以我在这里写我们是如何构建它的。

在我们开始之前，为你的整个网页准备一个容器，并根据你的要求设计宽度和高度。现在，在容器内部，你必须放置一个滑动菜单。在这篇文章中，我们将解释如何创建一个左滑动菜单。

### 我们开始吧

![1*1Jjz_X5KuI7U3Qs-lyKFSw](img/7d895aa00aaa0b778249added8970496.png)

Inspiration !!!! ;)

滑块的 HTML 代码如下所示。它是一个基本的裸版本。

```
<div class="slider-container">
```

```
<a href="#" class="slider-trigger">   Click here   </a>
```

```
<div class="slider-parent">    <h1>Slider</h1>    <a href="https:/twitter.com/giyaletter">Twitter&lt;/a> <br>    &lt;a href="https:/twitter.com/s_omeal">@Supriya</a> <br>    <a href="https:/twitter.com/g__patil">@Girish</a> <br>   </div>
```

```
</div>
```

一个**锚标签**用于在点击时打开菜单。这就是触发菜单打开的原因，所以你可以明白为什么它被称为**滑动触发**。菜单组件位于**滑动条父类**中。

现在用 CSS 设计菜单栏。注意设计细节。

```
.slider-container {  position: relative; }  .slider-container .slider-parent {    height: 70vh;    max-width: 250px;    width: 100%;    background: #6C7A89;    position: absolute;    left: -250px;    top: 50px;    visibility: hidden;    opacity: 0;    pointer-events: none;    transition: .2s all linear; }   .slider-container .slider-parent.active {      visibility: visible;      pointer-events: inherit;      transition: .2s all ease-in-out;      opacity: 1;      left: 0; }
```

现在让我们分解上面的片段，讨论它是如何工作的。

**Maxwidth** 定义了 div 可以占据的最大宽度。在一个更小的窗口中，它可以占用不到 250 像素。当窗口在屏幕上完全展开时，div 占据 250 像素。

有时，用户可能会在小得多的屏幕上查看网站，所以我们希望我们的 div 能够相应地调整大小。

继续，我们来看看**为什么左:-250px？**这样做是为了获得菜单平滑滑动动作。注意，left 的值是负数，这告诉我们菜单从起始位置(0)的左边 250px 开始。因此它目前不在可视区域内。

我们根本不希望滑动菜单被看到，这就是为什么我们添加了**不透明**并使其**可见性隐藏**。每个人都喜欢动画，它给人一种有趣的视觉感受。这个动画可以使用**过渡**组件来完成。

#### YAYYY！基本滑块做好了！

![1*UJtn-JxIMOnn6D6jLGqFgw](img/5ad70946b7b9c074f322c9c29a8ad94d.png)

I am sure you will dance better :P

现在基本的滑动条已经完成，让我们来理解当滑动条处于活动状态时会发生什么——也就是说，当打开菜单栏的锚标记被单击时。

关注上面给出的 CSS 代码中的**活动**类。请注意，**不透明度**和 **可见度**的值发生了变化。这一改变是为了使滑块(之前是隐藏的)在屏幕上可见。

此外，你可能会想:为什么现在是**左:0？**以前，滑块在屏幕外。现在我们希望菜单从屏幕的左侧开始，我们将 left 的值改为 0。

哦！动画！再次添加**过渡**组件，以便当滑块活动时，它从左侧平滑地进入。

搞定了！你已经设计了组件，那么下一步是什么？JavaScript！它会把这一切付诸行动。

### 添加一些 JavaScript

```
var sliderTrigger = document.getElementsByClassName("slider-trigger")[0];var slider = document.getElementsByClassName('slider-parent')[0];
```

```
sliderTrigger.addEventListener( "click" , function(el){
```

```
if(slider.classList.contains("active")){  slider.classList.remove("active"); }else{  slider.classList.add("active"); }
```

```
});
```

让我们看看 JavaScript 是如何包装一切并让滑块工作的。我们希望当点击锚标签**滑块触发器**时滑块打开。因此，我们将该元素放入变量 **sliderTrigger *中。*** 稍后我们将整个滑块元素放入变量**滑块**中。现在，我们添加一个事件监听器，它在点击 **sliderTrigger** 元素时实现一个函数。

```
sliderTrigger.addEventListener( "click" , function(el) {} );
```

编写的函数控制滑动菜单栏的打开和关闭机制。请记住，我们有一个活动的和普通的**滑块父类**。

我们这里实现的 hack 是在点击 **sliderTrigger** 元素时添加活动类，再次点击同一元素时移除活动类。为此，我们使用下面给出的代码来检查变量是否包含 active 类。

```
slider.classList.contains("active")
```

如果值为真，我们从列表中删除活动的类。接下来会发生什么？滑动菜单栏关闭。如果值为 false，我们将 active 类添加到 classlist 中。现在发生了什么？是的，会显示滑动菜单栏。就这么简单。

```
slider.classList.add("active")
```

```
slider.classList.remove("active")
```

### 瞧，完成了！！看看是谁在鼓掌；)

![1*FPKDw_SRNiaOfjL1fPBBRg](img/4bfdba3b1a4f45893a5ff25b1346627e.png)

下面的代码栏显示了相同代码的工作方式。

虽然这是一个基本的例子，但我会在我的时事通讯中发送更复杂和不同类型的滑动菜单栏的例子。

[Giyaletter 的 Github repo](https://github.com/girishpatil/giya)

推特账号:[苏普里亚 S](https://twitter.com/s_omeal) 和[吉里什帕蒂尔](https://twitter.com/theevilhead)

谢谢你。快乐编码:)

查看我们的产品:

[paybackhub.com](http://paybackhub.com)和[certhive.com](http://certhive.com)