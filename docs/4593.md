# 如何协议缓冲蓝牙低能耗服务第 3 部分

> 原文：<https://www.freecodecamp.org/news/how-to-protocol-buffer-bluetooth-low-energy-service-part-3/>

**本帖最初来自[www.jaredwolff.com](https://www.jaredwolff.com/how-to-protocol-buffer-bluetooth-low-energy-service-part-3/)**

在第 1 部分中，我们已经学习了协议缓冲区的结构。在[第二部分](https://www.jaredwolff.com/how-to-protocol-buffer-bluetooth-low-energy-service-part-2)中，我们已经了解了蓝牙低能耗服务是如何在 Nordic NRF52 SDK 上拼凑起来的。这最后一篇文章汇集了端到端工作示例中的所有元素。

我们走吧！

附:这个帖子很长。如果你想下载一些东西，点击这里获得一份格式精美的 PDF 文件。(额外奖励，这个系列的三个部分 PDF 都有！)

## 设置好一切

![Goose attack](img/18336c4172e94832f4a7b488c3d3dd4b.png)

去看看每个示例存储库中的`Readme.md`。(你将不得不克隆这些库，[在这里注册以获得代码](https://www.jaredwolff.com/files/protobuf))。

我已经使固件的设置非常简单。对于 javascript 来说，这有点麻烦，但却是可行的！我还将在这里包括说明:

### 固件设置(适用于 Mac)

1.  初始化完整的存储库(有子模块！):`git submodule update --init`
2.  使用自制软件安装`protoc`:`brew install protobuf`
3.  运行`make sdk`。这将下载您的 SDK 文件。
4.  运行`make tools_osx`。这将下载您的 ARMGCC 工具链(用于 Mac)。对于其他环境，请参见下文。
5.  运行`make gen_key`一次(并且只能运行一次)！这将为 DFU 设置您的密钥。
6.  运行`make`，这将构建你的引导程序和主应用程序。

**注意:**步骤 1-5 只需做一次。

### Javascript 应用程序设置(适用于 Mac)

**先决条件:**你将需要 Xcode 命令行工具。你可以在这里得到那些。

1.  将此回购克隆到您计算机上的某个位置
2.  确保您已经安装了[nvm](https://github.com/nvm-sh/nvm/blob/master/README.md)
3.  运行`nvm install v8.0.0`
4.  运行`nvm install v10.15.3`
5.  运行`nvm use v8.0.0`
6.  运行`yarn`(如果没有纱线`npm install yarn -g`)
7.  安装后，运行`nvm use v10.15.3`
8.  然后运行`node index.js`开始这个例子

使用 NVM 有助于缓解蓝牙库的编译问题。你的里程可能会有所不同。

## 这到底是怎么回事？

在这个项目中，协议缓冲区有两个功能。第一个作为对蓝牙设备的“命令”。其次，来自设备对命令的“响应”。

我们的示例 javascript 应用程序命令使用“This is”作为有效负载。借助一些字符串运算的力量，让我们用它来造一个完整的句子吧！

这一系列事件看起来像这样:

1.  测试应用程序使用我们的蓝牙低能耗服务连接并发送数据。
2.  在固件上解码消息。
3.  固件通过添加“cool”来修改原始数据。结果是“这很酷”
4.  固件对有效载荷进行编码，并使数据可供读取。
5.  应用程序最终读取特征，解码并显示结果！

看起来很复杂，但也有一些好处:

1.  在协议缓冲区中，数据结构被很好地定义。这意味着它有一些强大的错误检查能力。如果您使用任何类型的数据结构，您可能需要编写自己的数据验证代码。
2.  编码和解码简单明了。您可以在不同的平台上对相同的数据进行编码，并总是得到相同的解码结果。

想让示例运行起来吗？毕竟是眼见为实。我们开始吧。

## 这东西开着吗？

![Phone in hand running NRF connect](img/80ce7ebbea8baa99fa528d5659380baa.png)

一旦你完成了设置(如上)，让我们来给这个固件编程吧！你真幸运，只有两步路。

1.  插入 NRF52 DK 板
2.  运行`make flash_all`。(这将编译并刷新*项目的所有*代码。)

一旦闪烁，最容易知道事情是否工作的方法是当 Led 1 闪烁。这意味着它是广告，并准备连接。

![NRF52 DK](img/fa8b7dca008c993edf3a54d69f65c37c.png)

让我们确保它是正确的广告。你可以在 iOS 或 Android 上使用 NRF 连接这样的工具。或者，你可以选择浅蓝色。(也适用于 [iOS](https://apps.apple.com/ru/app/lightblue-explorer/id557428110?l=en) 和 Android)

打开其中一个应用程序，扫描广告设备。你要找的是 **Nordic_Buttonless** 。

![BLE Apps scanning](img/4e91890f73a824594a14f1f56ce1b683.png)

现在，让我们连接并仔细检查我们的可连接服务和特征:

![BLE Apps showing services](img/b4941b8f7b30fae916a6b3c17d31f2de.png)

如您所见，有两种服务。一个是北欧航空的 DFU 服务。另一个是我们的协议缓冲服务！

可以将`ble_protobuf.h`中的`PROTOBUF_UUID_BASE`与“未知服务”的 UUID 进行比较。Nordic 的芯片是小端格式，而这里的数据是大端格式。(也就是说，您必须反转字节才能看到它们是相同的！)

你甚至可以进一步点击查看 NRF 连接的特征。在浅蓝色的例子中，已经显示了特有的 UUIDs。

## 使用 Javascript 应用程序

所以，一旦我们开始做广告，就该运行应用程序了。

![Bluetooth javascript app results](img/8fde4fae281c766287bf5d2114ca3641.png)

只需切换到`ble-protobuf-js`目录并运行`node index.js`

如果您已经正确安装了所有东西，您应该会看到如下输出:

```
example started
scanning
peripheral with ID 06f9b62ec5334454875b9f53d2f3fa74 found with name: Nordic_Buttonles 
```

然后，它应该立即连接并向设备发送数据。它应该立即收到响应，并将其打印到屏幕上。

```
connected to Nordic_Buttonles
Sent: This is
Received: This is cool. 
```

答对了。

## 你成功了！

？如果你已经完成了教程，那么恭喜你。现在，您应该有足够的信心开始使用软件代码了。也许你会做些很酷的东西？

这里有一些资源的链接，你可能会觉得很方便。

*   [协议缓冲文件](https://developers.google.com/protocol-buffers)
*   NanoPB -(我们在这个项目中使用的协议缓冲区的实现)
*   [北欧 SDK 文档](https://www.nordicsemi.com/DocLib/Content/SDK_Doc/nRF5_SDK/v15-2-0/index)
*   同样，这里有到第一部分和第二部分的链接
*   如果你还没有，[获取这个项目的所有代码](https://www.jaredwolff.com/files/protobuf)

## 结论

协议缓冲区非常通用和方便。将它们与蓝牙低能耗服务结合使用是您可以利用的一种方式。随着物联网世界的不断扩展，我毫不怀疑我们将在未来看到更多的使用案例！

我希望这启发了您将协议缓冲区合并到您自己的项目中。确保您[下载了示例代码](https://www.jaredwolff.com/files/protobuf)并立即开始。

这个教程对你有用吗？让我知道你的想法。