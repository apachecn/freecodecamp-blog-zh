# 网页图像优化的基本介绍

> 原文：<https://www.freecodecamp.org/news/image-optimization/>

图像是大多数网站的重要组成部分。图片的视觉质量直接影响品牌形象和这些图像传达的信息。而图像的权重通常占到网络上传输数据的 40-60%。

与 JavaScript 等其他资源相比，这通常对加载时间影响最大。因此，无论我们是在创建还是运营一个网站，我们都应该建立一个图像转换和优化管道。

有许多方法可以做到这一点，从基于开源库和套件(如 ImageMagick)的内部开发到基于云的工具和 API。

无论我们在部署中使用什么工具，至少任何管道都应该完成四个主要任务。

### 调整图像大小。

调整图像大小是第一步，也是最重要的一步。只要我们不使它小于显示分辨率，它会对重量产生很大影响，而不会影响视觉质量。

我们应该在我们的网站上设置和执行最大的图像分辨率，例如 2000 像素的宽度。理想情况下，我们应该通过设置不同的断点并提供适合用户显示的图像来使我们的网站具有响应性。

如果你需要选择断点的帮助，看看这篇关于[最佳网页图片尺寸](https://abraia.me/docs/best-image-sizes-for-web/)的文章。

### 转换成正确的格式。

JPEG 是网站中的默认格式。PNG 可以更好地处理以纯色为特色的图形设计，但在这些情况下，它可能会产生更低的重量和更好的质量。

你可以考虑为 Chrome、Edge、Firefox 和 Android 用户提供 WEBP，保留 JPEG 格式作为 Safari 和 iOS 的后备。它可以节省 10-30%的图像重量，但质量非常相似，可能会有更多的模糊和更少的振铃。

关于最新的版本，你可以看看这篇关于 web 的[图片格式的文章。](https://abraia.me/docs/best-image-formats-for-web/)

### 适当压缩图像。

我们可以用一个强大的开源套件来做到这一点，如 [ImageMagick](https://imagemagick.org/index.php) ，并简单地为 JPEG(和 WEBP)图像设置一个质量因子(通常为 75 到 85)。你仍然可以使用感知指标来更好地保护质量和进一步压缩重量——这是一些云[图像优化工具](https://abraia.me/docs/image-optimization/#automatic-image-optimization-for-web)中的一个选项。

### 去掉元数据。

从拍摄到编辑，图像积累元数据，像 [exif 数据](https://abraia.me/docs/exif-data-orientation/)。虽然它们可能对编辑和管理有用，但是它们对图像在我们的网站上的显示没有影响。它们的权重很容易达到每张图像 20 KB 或更多。

在发布到网络上之前，我们应该去掉元数据。我们只需确保图像按照正确的方向和 sRGB 配置文件进行编码，遵守良好的[色彩管理](https://abraia.me/docs/color-management-for-web/)实践。