# 如何检测用户的设备，以便您可以改善他们的用户体验

> 原文：<https://www.freecodecamp.org/news/exploring-device-detection-for-better-user-experiences-in-2020/>

几个月前，我在 Chrome Dev 峰会上观看了一场关于慢速设备性能的精彩演讲。

[https://www.youtube.com/embed/puUPpVrIRkc?feature=oembed](https://www.youtube.com/embed/puUPpVrIRkc?feature=oembed)

脸书在识别设备以创造更好的用户体验方面所做的工作让我大吃一惊。快进到现在，我决定对这个话题多研究一点，看看我能在[thinkfic](https://www.thinkific.com)(我工作的公司)做些什么。

## 用户代理

用户代理是开发者熟知的。我们使用它们来检测机器人，将用户重定向到我们网站的特定版本，或者在我们的页面上添加 CSS 类，以便我们可以创建不同的体验。

在 Thinkific，我们已经使用了[浏览器 Ruby gem](https://github.com/fnando/browser) 来解析用户代理并获取相关信息(例如僵尸检测)。因此，我决定将主要信息保存在 visitor_device 表中——模式如下:

```
tenant_id: the course creator school the visitor is checking
raw: the raw ua
type: desktop / mobile / tablet / bot / other
browser_name
browser_version
platform_name
platform_version
hardware: hstore containing memory, processor, device_model, device_name
connection: hstore containing downlink_max, connection_type
```

你可能注意到了一些东西在 UA 字符串中是没有的。新 JavaScript APIs 的时代到了:

## 使用 JavaScript 获取硬件信息

正如 Chrome Dev Summit 视频中提到的，我们可以使用 JS 来获取这些信息。

### 记忆

`navigator.deviceMemory`将返回一个浮点数。这里需要考虑一些事情:

*   它只适用于 HTTPS
*   支持相当有限(基本上只有 Chrome)

更多信息:

*   来自 W3C 的规范
*   [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/deviceMemory)
*   [我可以使用设备内存吗](https://caniuse.com/#feat=mdn-api_navigator_devicememory)

### 处理器

`navigator.hardwareConcurrency`将返回用户 CPU 的逻辑核心数。对此的支持是[体面的](https://caniuse.com/#feat=hardwareconcurrency)。

## 使用 JavaScript 检测连接信息

`navigator.connection`是一个新的 API，包含关于系统连接的信息，如用户设备的当前带宽或连接是否计量。支持相当有限(基本上只有 Chrome ),但前景看好。

更多信息:

*   [Chrome 示例](https://googlechrome.github.io/samples/network-information/)
*   [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/connection)
*   [我可以使用网络信息 API](https://caniuse.com/#feat=netinfo)

## 检测设备型号

用户代理*可以*返回一些关于型号名称的信息。 [userstack](https://userstack.com/) 是一种基于用户代理为你提供信息的服务。它工作得很好，也很容易集成，然而，取决于你的需求，他们帮不上忙。

以 iDevices 为例。他们的用户代理基本上是一样的，所以你无法区分 iPad Pro 和运行最新 iOS 的旧 iPad。对于这些情况，您可能需要根据浏览器中显示的分辨率、像素密度和其他硬件信息进行更好的检测。我对此做了一些快速的研究，目前发现了 3 款产品: [WURFL.io](https://web.wurfl.io/#wurfl-js) 、 [DeviceAtlas](https://deviceatlas.com/products/web) 和 [51Degrees](https://51degrees.com/) 。我还没有时间尝试他们的产品，但我期待着这样做(并张贴出来)

## 常见问题解答

*问题:为什么不在这里使用 Google Analytics/Mixpanel/ki Bana/New Relic/your tool？*

我们可以在其他工具中获取浏览器信息。但是作为一个 SaaS 产品，我们不使用我们自己的谷歌分析属性(客户添加他们自己的)。此外，广告拦截器可能会阻止这些第三方工具。最后但同样重要的是，有了这些信息，我们可以更好地适应。

*问题:你有低端/高端设备的清单吗？*

不。也许这可以结合处理器和内存的数量来构建，但我没有在这方面投入太多时间。在这个项目中，我的同事创建了一个 Rails 助手，它可以确定用户是使用基于硬件的网站的精简版还是默认版。关于这个话题，重要的是要提到脸书有一个名为 [Device Year Class](https://github.com/facebook/device-year-class/) 的 Android 库。

*也发表在[我的博客](http://bit.ly/2FWop4N)上。如果你喜欢这些内容，请在 [Twitter](https://twitter.com/leozera) 和 [GitHub](https://github.com/leonardofaria) 上关注我。*

顺便说一下，Thinkific 正在为几个职位 [招聘](https://bit.ly/thnk-senior-full-stack-eng)[。](https://bit.ly/thnk-senior-front-end-eng)