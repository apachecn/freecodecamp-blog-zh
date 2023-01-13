# HTML5 视频:如何在你的 HTML 中嵌入视频

> 原文：<https://www.freecodecamp.org/news/html5-video/>

在 HTML5 之前，为了在网页上播放视频，你需要使用像 Adobe Flash Player 这样的插件。随着 HTML5 的引入，你现在可以将视频直接放入页面本身。

这使得在为移动设备设计的页面上播放视频成为可能，因为像 Adobe Flash Player 这样的插件不能在 Android 或 iOS 上工作。

HTML `<video>`元素用于在 web 文档中嵌入视频。它可能包含一个或多个视频源，使用`src`属性或[源](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/source)元素表示。

要嵌入视频文件，只需添加以下代码片段，并将`src`更改为视频文件的路径:

```
<video controls>
  <source src="tutorial.ogg" type="video /ogg">
  <source src="tutorial.mp4" type="video /mpeg">
  Your browser does not support the video element. Kindly update it to latest version.
</video >
```

所有现代浏览器都支持`<video>`元素。但是，并非所有浏览器都支持相同的视频文件格式。MP4 文件是最广泛接受的格式，其他格式如 WebM 和 Ogg 在 Chrome、Firefox 和 Opera 中也受支持。

为了确保你的视频可以在大多数浏览器中播放，最好的做法是将它们编码成 Ogg 和 MP4 两种格式，并像上面的例子一样将这两种格式都包含在`<video>`元素中。浏览器将使用第一个被识别的格式。

如果由于某种原因，浏览器不能识别任何格式，文本“您的浏览器不支持视频元素。请将其更新至最新版本”的信息。

您可能还注意到了`<video>`标签中的`controls`。这个元素包括许多有用的属性，用于定制播放体验。

## `<video>`属性

### `controls`

`controls`属性处理诸如播放/暂停按钮或音量滑块之类的控件是否出现。

这是一个布尔属性，意味着它可以设置为 true 或 false。要将其设置为 true，只需将其添加到`<video>`标签中。如果它不在标签中，那么它将被设置为 false，控件将不会出现。

#### `autoplay`

“自动播放”可以设置为真或假。通过将它添加到标记中来将其设置为 true，如果它不在标记中，则将其设置为 false。如果设置为 true，视频将在足够的视频缓冲后开始播放。许多人认为自动播放视频具有破坏性或令人讨厌。所以要谨慎使用这个特性。还要注意，一些移动浏览器，比如 iOS 版的 Safari，会忽略这个属性。

这是另一个布尔属性。通过将`autoplay`包含在`<video>`标签中，一旦足够多的视频被缓冲，嵌入的视频将开始播放。

```
<video autoplay>
  <source src="video.mp4" type="video/mp4">
</video> 
```

请记住，许多人认为自动播放视频具有破坏性或令人讨厌，因此请谨慎使用该功能。另请注意，一些移动浏览器(如 iOS 版 Safari)完全忽略了这个属性。

#### `poster`

`poster`属性是显示在视频上的图像，直到用户点击播放它。

```
<video poster="poster.png">
  <source src="video.mp4" type="video/mp4">
</video>
```

### 视频可能很贵

虽然在页面上包含视频比以往任何时候都容易，但通常更好的做法是将视频上传到 YouTube、Vimeo 或 Wistia 等服务，然后嵌入它们的代码。这是因为提供视频可能会很贵，无论是对你来说还是对你的观众来说，如果他们的数据套餐有限的话。

托管自己的视频文件也可能导致带宽问题，这可能意味着缓慢加载视频的口吃。最重要的是，在播放视频时，浏览器的质量往往会有所不同，所以很难准确控制观众会看到什么。下载嵌入了`<video>`标签的视频也很容易，所以如果你担心盗版，你可能想看看其他选项。

有了它，你就可以随心所欲地将视频嵌入其中。或者不，这是你的选择。