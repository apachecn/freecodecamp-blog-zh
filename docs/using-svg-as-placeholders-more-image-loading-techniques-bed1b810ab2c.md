# 如何使用 SVG 作为占位符，以及其他图像加载技术

> 原文：<https://www.freecodecamp.org/news/using-svg-as-placeholders-more-image-loading-techniques-bed1b810ab2c/>

何塞·m·佩雷斯

# 如何使用 SVG 作为占位符，以及其他图像加载技术

![1oy4vwmuG5gvTEKAQktg2PFz39kbJ0oNoPQD](img/8318905a36b0714eaeef0a48f8f0690c.png)

Generating SVGs from images can be used for placeholders. Keep reading!

我热衷于图像性能优化和让图像在网络上快速加载。探索的最有趣的领域之一是占位符:当图像还没有加载时显示什么。

在过去的几天里，我遇到了一些使用 SVG 的加载技术，我想在这篇文章中描述一下。

在本帖中，我们将讨论以下主题:

*   不同类型占位符的概述
*   基于 SVG 的占位符(边缘、形状和轮廓)
*   自动化流程。

### 不同类型占位符的概述

过去[我写过关于占位符和图片惰性加载的文章](https://medium.com/@jmperezperez/lazy-loading-images-on-the-web-to-improve-loading-time-and-saving-bandwidth-ec988b710290)，也有[谈到过这个问题](https://www.youtube.com/watch?v=szmVNOnkwoU)。当进行图像的延迟加载时，考虑将什么渲染为占位符是一个好主意，因为它会对用户的感知性能产生很大的影响。过去我描述了几种选择:

![tq-I1q1T0DwNSNK6lLzP0t9vBta7nMaes2TA](img/6b02b42f2dec58bfdc2f4747d32325b7.png)

加载前填充图像区域的几种策略。

*   为图片留出空间:在响应式设计的世界里，这可以防止内容跳跃。从用户体验的角度来看，这些布局变化是不好的，对性能也是如此。浏览器在每次获取图像尺寸时都被迫重新计算布局，为图像留出空间。
*   **占位符**:假设我们正在显示一个用户的个人资料图片。我们可能希望在背景中显示一个轮廓。这在主图像加载时显示，但在请求失败或用户根本没有设置任何个人资料图片时也会显示。这些图像通常是基于矢量的，由于它们的尺寸很小，所以是内联的好选择。
*   **纯色**:从图像中取一种颜色，用作占位符的背景色。这可以是主色，最鲜艳的颜色…这个想法是基于你正在加载的图像，应该有助于使无图像到加载图像之间的过渡更加平滑。
*   **图像模糊**:也称为模糊技术。你渲染一个小版本的图像，然后过渡到完整的。初始图像在像素和 kBs 上都很小。为了消除伪像，图像被放大和模糊。我以前写过关于[如何通过媒体进行渐进式图像加载](https://medium.com/@jmperezperez/how-medium-does-progressive-image-loading-fd1e4dc1ee3d)、[使用 WebP 创建微小的预览图像](https://medium.com/@jmperezperez/using-webp-to-create-tiny-preview-images-3e9b924f28d6)，以及[更多渐进式图像加载的例子](https://medium.com/@jmperezperez/more-examples-of-progressive-image-loading-f258be9f440b)。

原来还有许多其他的变化，许多聪明人正在开发其他的技术来创建占位符。

其中之一是用渐变代替纯色。渐变可以创建更精确的最终图像预览，开销非常小(增加有效负载)。

![KcH3YSwHpmuSNm6LAc5KcngIIHc6HmnxwNdH](img/394da6026adde2cd9de5390b4227ac58.png)

Using gradients as backgrounds. Screenshot from Gradify, which is not online anymore. Code [on GitHub](https://github.com/fraser-hemp/gradify).

另一种技术是使用基于图像的 SVG，这在最近的实验和黑客攻击中得到了一些支持。

### 基于 SVG 的占位符

我们知道 SVG 是矢量图像的理想选择。在大多数情况下，我们希望加载一个位图，所以问题是如何对图像进行矢量化。一些选项是使用边缘、形状和区域。

#### 优势

在之前的一篇文章中，我解释了如何找出图像的边缘并制作动画。我最初的目标是尝试绘制区域，对图像进行矢量化，但我不知道如何去做。我意识到使用边缘也可以是创新的，我决定让它们产生一种“绘画”的效果。

[**使用边缘检测和 SVG 动画绘制图像**](https://medium.com/@jmperezperez/drawing-images-using-edge-detection-and-svg-animation-16a1a3676d3)
[*过去，SVG 很少被使用和支持。一段时间后，我们开始使用它们作为经典的替代品…*medium.com](https://medium.com/@jmperezperez/drawing-images-using-edge-detection-and-svg-animation-16a1a3676d3)

#### 形状

SVG 还可以用来从图像中绘制区域，而不是边缘/边框。在某种程度上，我们可以矢量化位图图像来创建占位符。

回到过去，我试图用三角形做类似的事情。你可以在我在 CSSConf 和 [Render Conf](https://jmperezperez.com/renderconf17/#/46) 的演讲[中看到结果。](https://jmperezperez.com/cssconfau16/#/45)

上面的代码笔是一个由 245 个三角形组成的基于 SVG 的占位符的概念证明。三角形的生成是基于使用[波桑的 polyserver](https://github.com/possan/polyserver) 的 [Delaunay 三角剖分](https://en.wikipedia.org/wiki/Delaunay_triangulation)。正如所料，SVG 使用的三角形越多，文件就越大。

#### 原语和 SQIP，一种基于 SVG 的 LQIP 技术

托拜厄斯·巴尔多夫一直在研究另一种使用 SVG 的低质量图像占位技术，名为 [SQIP](https://github.com/technopagan/sqip) 。在深入研究 SQIP 之前，我将概述一下[原语](https://github.com/fogleman/primitive)，这是一个 SQIP 基于的库。

原始是相当迷人的，我绝对推荐你去看看。它将位图图像转换成由重叠形状组成的 SVG。它的小尺寸使它适合直接内联到页面中。少了一次往返，并且在初始 HTML 负载中有了一个有意义的占位符。

Primitive 基于三角形、矩形和圆形(以及其他一些形状)生成图像。它每走一步都会增加一个新的。步数越多，生成的图像看起来就越接近原始图像。如果您的输出是 SVG，这也意味着输出代码的大小会更大。

为了理解 Primitive 是如何工作的，我让它看了几张图片。我使用 10 个形状和 100 个形状为艺术品生成 SVG:

![wru4xpNDHyRR3JMz1eIvgtGAQHLjn-d8Cnui](img/f6f0fbdd77347ca5a9deae82919ba70e.png)![063NW6I1LucSf8Oo1Tm4UDXif7mSVXOWK8Sb](img/e4700eef632bd6659b1e6a4d5ed734cf.png)![gtkZMkLD74Bggypce65qt-1O5dltlN9-wyFo](img/f111b1173efdcfcbb768aa3d71eb2be2.png)

Processing [this picture](https://jmperezperez.com/assets/images/posts/svg-placeholders/pexels-photo-281184-square.jpg) using Primitive, using [10 shapes](https://jmperezperez.com/assets/images/posts/svg-placeholders/pexels-photo-281184-square-10.svg) and [100 shapes](https://jmperezperez.com/assets/images/posts/svg-placeholders/pexels-photo-281184-square-100.svg).

![RcbRK2L4F5SN-LcbaAo3YBqi6YyisWSElWD1](img/e103449d4a3b9383e8a54732ec959d3b.png)![0BUVCyRTKGT60ZzgpQi5lFlrYkZa5Rcaivjv](img/0b26bdd2f7fce0dd46aa29c1848c962c.png)![VAetFfTVYQwi8ixmnMgOcJjVqdRDuVYW4lsV](img/2bb0ed11d843fa5d1097cd41977b05ca.png)

Processing [this picture](https://jmperezperez.com/assets/images/posts/svg-placeholders/pexels-photo-618463-square.jpg) using Primitive, using [10 shapes](https://jmperezperez.com/assets/images/posts/svg-placeholders/pexels-photo-618463-square-10.svg) and [100 shapes](https://jmperezperez.com/assets/images/posts/svg-placeholders/pexels-photo-618463-square-100.svg).

当使用 10 个形状的图像时，我们开始掌握原始图像。在图像占位符的上下文中，有可能使用这个 SVG 作为占位符。实际上，包含 10 个形状的 SVG 的代码非常小，大约 1030 个字节，当通过 SVGO 传递输出时，代码减少到大约 640 个字节。

```
<svg xmlns=”http://www.w3.org/2000/svg" width=”1024" height=”1024"><path fill=”#817c70" d=”M0 0h1024v1024H0z”/><g fill-opacity=”.502"><path fill=”#03020f” d=”M178 994l580 92L402–62"/><path fill=”#f2e2ba” d=”M638 894L614 6l472 440"/><path fill=”#fff8be” d=”M-62 854h300L138–62"/><path fill=”#76c2d9" d=”M410–62L154 530–62 38"/><path fill=”#62b4cf” d=”M1086–2L498–30l484 508"/><path fill=”#010412" d=”M430–2l196 52–76 356"/><path fill=”#eb7d3f” d=”M598 594l488–32–308 520"/><path fill=”#080a18" d=”M198 418l32 304 116–448"/><path fill=”#3f201d” d=”M1086 1062l-344–52 248–148"/><path fill=”#ebd29f” d=”M630 658l-60–372 516 320"/></g></svg>
```

用 100 个形状生成的图像比预期的要大，在 SVGO 之后大约重 5kB(之前重 8kB)。它们的有效载荷很小，但细节层次很高。决定使用多少三角形很大程度上取决于图像的类型(如对比度、颜色数量、复杂性)和细节水平。

有可能创建一个类似于 [cpeg-dssim](https://github.com/technopagan/cjpeg-dssim) 的脚本，调整使用的形状数量，直到达到[结构相似度](https://en.wikipedia.org/wiki/Structural_similarity)阈值(或者在最坏的情况下达到最大形状数量)。

这些生成的 SVG 也非常适合用作背景图像。由于尺寸受限和基于矢量，它们是英雄图像和大背景的良好候选，否则会显示伪像。

#### SQP 你好

用托拜厄斯自己的话说:

> SQIP 试图在这两个极端之间找到一个平衡:它利用[原语](https://github.com/fogleman/primitive)生成一个由几个简单形状组成的 SVG，这些形状近似于图像中可见的主要特征，使用 [SVGO](https://github.com/svg/svgo) 优化 SVG，并为其添加高斯模糊过滤器。这产生了一个 SVG 占位符，它只有大约 800–1000 字节，在所有屏幕上看起来都很平滑，并提供了图像内容的视觉提示。

其结果类似于使用一个微小的占位符图像进行模糊处理(就像[媒体](https://medium.com/@jmperezperez/how-medium-does-progressive-image-loading-fd1e4dc1ee3d)和[其他网站](https://medium.com/@jmperezperez/more-examples-of-progressive-image-loading-f258be9f440b)所做的那样)。不同之处在于，占位符是 SVG，而不是使用位图图像，例如 JPG 或 WebP。

如果我们拿 SQIP 和原始图像对比，我们会得到这个:

![hbNTQ6U2ikSx2bBCETSrj9QsjVlJReX-Hlqx](img/652f6981fbb5f2e7a3bedc9824ddb096.png)![8h21TmNbbC7eOxBKNkOo2TXV-Ulwx5a3Z9bd](img/d0d2b7d49adf8fc32f9a29f6fe70a78b.png)

The output images using SQIP for [the first picture](https://jmperezperez.com/assets/images/posts/svg-placeholders/pexels-photo-281184-square-sqip.svg) and [the second one](https://jmperezperez.com/svg-placeholders/(/assets/images/posts/svg-placeholders/pexels-photo-618463-square-sqip.svg).

输出的 SVG 大约是 900 字节，检查代码我们可以发现应用于形状组的`feGaussianBlur`过滤器:

```
<svg  viewBox="0 0 2000 2000"><filter id="b"><feGaussianBlur stdDeviation="12" /></filter><path fill="#817c70" d="M0 0h2000v2000H0z"/><g filter="url(#b)" transform="translate(4 4) scale(7.8125)" fill-opacity=".5"><ellipse fill="#000210" rx="1" ry="1" transform="matrix(50.41098 -3.7951 11.14787 148.07886 107 194.6)"/><ellipse fill="#eee3bb" rx="1" ry="1" transform="matrix(-56.38179 17.684 -24.48514 -78.06584 205 110.1)"/><ellipse fill="#fff4bd" rx="1" ry="1" transform="matrix(35.40604 -5.49219 14.85017 95.73337 16.4 123.6)"/><ellipse fill="#79c7db" cx="21" cy="39" rx="65" ry="65"/><ellipse fill="#0c1320" cx="117" cy="38" rx="34" ry="47"/><ellipse fill="#5cb0cd" rx="1" ry="1" transform="matrix(-39.46201 77.24476 -54.56092 -27.87353 219.2 7.9)"/><path fill="#e57339" d="M271 159l-123–16 43 128z"/><ellipse fill="#47332f" cx="214" cy="237" rx="242" ry="19"/></g></svg>
```

SQIP 还可以输出带有 SVG 内容 Base 64 编码的图像标签:

```
<img width="640" height="640" src="example.jpg” alt="Add descriptive alt text" style="background-size: cover; background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAw…<stripped base 64>…PjwvZz48L3N2Zz4=);">
```

#### 轮廓

我们刚刚看了使用 SVG 的边缘和原始形状。另一种可能性是对图像进行矢量化“追踪”。几天前，Mikael Ainalem 与[分享了一个代码笔](https://codepen.io/ainalem/full/aLKxjm/)，展示如何使用双色剪影作为占位符。结果非常漂亮:

![xIKbor-mLB7y7Ju8LXLibzxgGB5NTQbQNs64](img/c3d357b25427cb4e492d24d755821700.png)

本例中的 SVG 是手工绘制的，但是这种技术很快产生了与工具的集成，从而自动化了这个过程。

*   [Gatsby](https://www.gatsbyjs.org/) ，一个使用 React 的静态站点生成器现在支持这些追踪的 SVG。它使用 potrace 的 JS 端口[对图像进行矢量化。](https://www.npmjs.com/package/potrace)

*   [Craft 3 CMS](https://craftcms.com/) ，也增加了对剪影的支持。它使用了[potrace](https://github.com/nystudio107/craft3-imageoptimize/blob/master/src/lib/Potracio.php)的一个 PHP 端口。

*   [image-trace-loader](https://github.com/EmilTholin/image-trace-loader) ，一个使用 potrace 处理图像的 Webpack 加载器。

看到 Emil 的 webpack loader(基于 potrace)和 Mikael 的手绘 SVG 之间的输出比较也很有趣。

我假设 potrace 生成的输出使用默认选项。然而，调整它们是可能的。检查[图像跟踪加载器](https://github.com/EmilTholin/image-trace-loader#options)的选项，这些选项几乎是[传给 potrace](https://www.npmjs.com/package/potrace#parameters) 的选项。

### 摘要

我们已经看到了从图像生成 SVG 并将它们用作占位符的不同工具和技术。与 WebP 是缩略图的绝佳格式一样，SVG 在占位符中也是一种有趣的格式。我们可以控制细节的级别(因此，大小)，它是高度可压缩的，并且易于使用 CSS 和 JS 操作。

#### 额外资源

这篇文章登上了黑客新闻的榜首。对此我非常感激，也非常感谢在那个页面的评论中分享的其他资源的链接。以下是其中的几个！

*   [Geometrize](https://github.com/Tw1ddle/geometrize-haxe) 是一个用 Haxe 写的原语的端口。还有[一个 JS 实现](https://github.com/Tw1ddle/geometrize-haxe-web)，你可以在你的浏览器上直接试用[。](http://www.samcodes.co.uk/project/geometrize-haxe-web/)
*   [Primitive.js](https://github.com/ondras/primitive.js) ，是 js 中 Primitive 的一个端口。还有， [primitive.nextgen](https://github.com/cielito-lindo-productions/primitive.nextgen) ，这是 primitive 桌面 app 使用 Primitive.js 和 Electron 的一个端口。
*   有几个 Twitter 账户，你可以看到用 Primitive 和 Geometrize 生成的图像的例子。查看 [@PrimitivePic](https://twitter.com/PrimitivePic) 和 [@Geometrizer](https://twitter.com/Geometrizer) 。
*   [imagetracerjs](https://github.com/jankovicsandras/imagetracerjs) ，这是一个用 JavaScript 编写的光栅图像跟踪器和矢量器。还有 [Java](https://github.com/jankovicsandras/imagetracerjava) 和 [Android](https://github.com/jankovicsandras/imagetracerandroid) 的端口。
*   [Canvas-Graphics](http://thomas.weinert.info/Canvas-Graphics/) ，围绕 GD 的 PHP 中 JS Canvas API 的部分实现。

### 相关职位

如果你喜欢这篇文章，看看我写的关于加载图片的技巧的其他文章:

[**Medium 如何渐进加载图像**](https://medium.com/@jmperezperez/how-medium-does-progressive-image-loading-fd1e4dc1ee3d)
[*最近，我在浏览 Medium 上的一个帖子，发现了一个不错的图像加载效果。首先，加载一个小的模糊图像…*medium.com](https://medium.com/@jmperezperez/how-medium-does-progressive-image-loading-fd1e4dc1ee3d)[**使用 WebP 创建微小的预览图像**](https://medium.com/@jmperezperez/using-webp-to-create-tiny-preview-images-3e9b924f28d6)
[*随着图像优化主题的展开，我将更深入地了解脸书创建预览的技术…*medium.com](https://medium.com/@jmperezperez/using-webp-to-create-tiny-preview-images-3e9b924f28d6)[**更多渐进图像加载的示例**](https://medium.com/@jmperezperez/more-examples-of-progressive-image-loading-f258be9f440b)
[*在过去的一篇帖子中，我剖析了一种由媒体使用的技术来显示图像，从模糊图像过渡*](https://medium.com/@jmperezperez/more-examples-of-progressive-image-loading-f258be9f440b)

你可以阅读更多我关于 jmperezperez.com 的文章。