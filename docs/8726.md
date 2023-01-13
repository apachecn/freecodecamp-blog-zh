# 我从建造声控机器人中获得的见解

> 原文：<https://www.freecodecamp.org/news/building-a-voice-activated-robot-for-an-advertising-agency-fedaa9f347d3/>

作者:米西

![1*-ALg668bbpj2VEuXIyWPqQ](img/4043eaec8a3985f1d59765dcda988351.png)![1*mEgdqheMQAd4vlBnxHP4Zg](img/9289f9db0e7c7cc0042c6c57d43ee878.png)![1*Ao2FdjrlyNOlixjPtXD7Cg](img/31a8c5254da92a4c3896cf1c2bd40bb6.png)

# 我从建造声控机器人中获得的见解

![1*lbIDINee-izLUwkgIb581g](img/db9d17c6b5ef014a60e5a3ff32a06ab1.png)

我在一家广告公司做了将近一年的创意技术人员。基于创新驱动新业务以及技术可以创造性地应用于品牌活动的观点，我在工作中做了一些有趣的事情。我介绍新技术，对非技术创意人员的技术想法进行可行性检查，以及[原型材料](https://docs.google.com/presentation/d/18nTykqw-Evj0o0kAuIuKrMsknV8-LQz7Obmxvazd1wQ/edit?usp=sharing)，等等。

也许我在工作中做过的最令人兴奋的事情之一就是和机器人一起工作！我监督了一个 45 英寸高的机器人的创建，并为其编写了两个多月的程序。

### 我们是如何打造罗比的

认识一下 Robbie——也被称为[hello bot](http://digit.gitlab.io/digit-x-robot/)——这是一项关于人类和技术如何互动的实验，通过让人们与机器人建立联系，就像人们与其他人建立联系一样。罗比背后的想法是，技术，如机器人，可以通过每次愉快的互动来强化你的品牌。

![1*EwOqyh2QmjSAI-ZsD8fgLg](img/27ef711fa52bffb745637ed50cda00a8.png)![1*CfIj9YaFMg4zsJ1o_4OzMQ](img/f42ef4d01c20892dab247a85b7cf553d.png)![1*ASspWUHMXcbvs_25y15IoQ](img/e43bc3a555d14ab8048a93486b1f76c5.png)![1*IhER4KvG8sF8CpByaBNITw](img/0742529f88ceb225600d6001ad4ff053.png)![1*O1dMfn33Oj88-3gJ3TKm7w](img/5b8caca480fe215c097fd2e3ed2bf0d6.png)

*Some insights I got from the creative process of building the “minimum-viable-product” robot version of Robbie.*

现在比以往任何时候都更容易用现成的零件制造复杂的机器人。

这都要感谢开源硬件(OSHW)社区。制造机器人是可能的，因为我们正站在巨人宽厚的肩膀上。

![1*ytlKHwxvDr0QxF57PGJGmQ](img/2ba5c41c8ff9f04615c56f87708513ad.png)

所有用来制造罗比的电子产品都可以从 [DFRobot](http://dfrobot.com) 、 [Adafruit](http://adafruit.com) 和[爱好王](http://hobbyking.com)那里买到。所有这些都是开源的——原理图、物料清单和 PCB 板设计可供任何人免费下载、复制或修改。

每个组件都不是一个黑匣子，在黑匣子中，您必须依靠一个小型客户支持团队来解决您的问题。当出了问题，你有整个社区来帮助你。

此外，因为大量的信息是免费提供给你的，你会对事物如何运作有更深的理解。

![1*Y8TWQZcSb8afA6YVwZOn3g](img/a6ee9ff928b4f2326f2d9bc979a659f1.png)

我们机器人的大脑是一台 [Raspberry Pi 3](http://raspberrypi.org) ，一台信用卡大小的 WiFi 电脑。它连接到各种外围设备，因此机器人可以从外部世界获得输入和输出。

一些输入是:一个麦克风，让机器人可以听到你的话，一个 800 万像素的摄像头，让机器人可以看到你，以及一个被动红外(PIR)传感器，当“平均辐射水平发生变化”时，它就会激活。这有助于检测人类是否进入或离开机器人的领地。

一些输出是机器人显示其表情的 7 英寸显示器，以及音频扬声器，以便您可以听到它在说什么。树莓 Pi 3 连接 WiFi，使用[谷歌的语音识别库](https://cloud.google.com/speech/)。它使用一个开放的计算机视觉( [OpenCV](http://opencv.org/) )库来识别人脸。

![1*fs7Rac0WtXGOVXOc8QTKBw](img/d526f6d3c06b9c34c5e949380708175b.png)![1*Uby_FKtAS17bEQqoaCgcAA](img/04597fd2f8a75522511f65dbe3059a6e.png)![1*Go_98roPwBS-v29WreJonQ](img/d0268d0c454ff634e46b1bcc3778dcec.png)![1*j9ye4Fc-7IHgN1o3VvJrFw](img/3991a5c82d7d815e7b0d8da9e0217e17.png)![1*XPN79SUj4KgneTd__oVx7Q](img/521a369202cb1ac1c40ace7fc1b7454d.png)

Raspberry Pi 3 与一个 [Arduino Mega](http://arduino.cc) 微控制器通信，以委派 Pi 不擅长的低级任务。Arduino Mega 控制两个电机驱动器(它有四个轮子，你可以独立控制)，这样机器人就可以向左、向右、向前和向后移动。

它还有两个伺服电机(由于内置反馈电路，可以通过角度来控制的特殊电机)来移动它的手臂。

还有连接到 Arduino 的低级外设，如彩色 RGB 可寻址可链接 led“[新像素](https://www.adafruit.com/category/168)”来指示状态，以及三个红外距离传感器来避开障碍物。

![1*eRBynzsjCT4rBkWus6EQDQ](img/bdcfb3faf68f2c64e76bfbe12717f57b.png)![1*5AhcgK5hhx6I7Q8cnFXVFw](img/02c73ecd22171a304800660bd0d8f680.png)![1*d42GY-71f49-BO0qO2az3A](img/d9a61bdbe3f463f13b3110b786d9d2f6.png)![1*UBpqeq23_NSHpTsq6Nr2Wg](img/0ddc49037a516638a252f64c6b03257c.png)![1*C54Waq3zARr3bQjZJdD5pw](img/545653486bf57bb868150b87f80d25d8.png)![1*kqkZnCZ03zp0rl7nKpP2VA](img/0032a261f1e2f8affb68cf4eea7b0009.png)

整个机器人(包括电机)由 14.8v 4500 mAh 锂聚合物电池供电。DC-DC 转换器调节电源，将其降至更低的电压，以安全地为 Raspberry Pi 和 Arduino 供电。

![1*IOu2YTtIzPYhjKJyCOeOUQ](img/531bcbda6b0f9dd52d43a9b8714b3d9b.png)![1*SlxuVaf8IMv3PfJNP52p_Q](img/488c9ba1f2e6e52a5e7cd73ba8ecbbf3.png)![1*meNEUTZBF699GTbpRIuZZw](img/c97280d6543c1e254a82c7cd50a80a4a.png)

如果没有开源硬件社区提供的大量教程，尤其是由[利莫尔“阿达女士”弗里德](https://en.wikipedia.org/wiki/Limor_Fried)开创的[阿达果产业](http://adafruit.com)，我们将无法设计，更不用说理解这一切。

以下是我在这个过程中学到的一些东西。

### 设计“干净的代码”至关重要

![1*7aqCLMcrRsC1z3XL3uqMgg](img/c715a6d8b7236ed683242d862f498477.png)

当你试图建造一个你不仅喜欢与之互动，而且喜欢在其上构建的机器人时，尝试设计“干净”的代码是非常重要的。

受罗伯特·马丁和 T2·桑迪·梅斯的《代码工艺》书籍的启发，我学会了在为这个机器人写代码时要深思熟虑。我不是编写精心制作的代码的老手，但我尽力了。

当你在一个机器人中做代码维护时，你可以真的“爱”或“恨”一个你甚至不认识的人，仅仅因为他们写的代码。

混乱的代码几乎总是伴随着较低的生产率、较低的积极性和较高数量的错误。无数的时间和大量的资源会因为糟糕的代码而丢失，但是事情并不一定是这样。

干净的代码已经在我的脑海中出现了一段时间。一年前，我在菲律宾的 Python 大会上讲述了我如何让一个机器人六足动物跳舞来练习编写干净的代码。我谈到了我对精心编写代码的指导原则。

甚至在更早的时候，我写了当我决定写自己的类时我所考虑的事情。这是我努力应用面向对象的设计哲学来使我的代码干净的一部分。

每当我写代码的时候，尤其是当我为 Robbie 写代码的时候，这些原则和想法仍然在我的脑海里。

![1*UYF78LVNFkCJV4i9W_oavg](img/ea8307eedb4b2a088835c73e33da9e58.png)![1*rCZipTusi6cIbhpGUUYyrQ](img/ad16608284a9204adce26f2a5199b22c.png)![1*n6jAs6mFkdoz-a8RPYlqTA](img/76de5f6b4d13b8ce1af6b5b32147efbf.png)

驻留在 [Arduino](http://github.com/mithi/hellobot-arduino) 部分的代码是用为嵌入式编程设计的简化版 C++编写的。它有两个明显的类，`Motors`和`DistanceSensors`。

`Motors`类负责驱动轮子使机器人左转、写字、前进或后退。

`DistanceSensors`类负责获取距离，检查周围是否有障碍物。

我还用过其他人制作的其他类，比如`Serial`和`Neopixel`。

![1*qfWOpD6_uaTRLjZE5YWn4w](img/33a46e17843fc15dc48db88a0ef802df.png)

驻留在 [Raspberry Pi](http://github.com/mithi/hellobot) 部分的代码是用 Python 编写的。我为它编写的一些类有`Listener`、`Responder`、*、*、`Directive`、`Relayer`和`FaceFinder`。

需要一个`Listener`实例从麦克风的数据中获取短语(字符串格式)，由 GoogleSpeech 解释。

`Responder` 在屏幕上播放视频或显示图片。

`Directive`处理短语以获得在**关键字**之后的单词，该单词用于发出命令让机器人执行。

`Relayer` 与 Arduino 通信。

`FaceFinder`负责检测人脸。

对于更干净的代码，类应该只负责一件事，仅此而已。创建类是为了使事情变得简单，而不是复杂。你知道一个类很简单，当你能像我一样用一句话描述它做什么的时候。

### 从经验中学习才是正确的道路

你需要不断的迭代和用户反馈来为你的产品获得良好的用户体验。

罗比目前的状态只是一个“最低限度的可行产品”。要提高 Robbie 的可靠性、用户体验和整体设计，还有很多事情可以做。

![1*7GRlsGxGrXvTbaHS-YXGVg](img/01a5b6cb60cfc8aa56c996518c7ae35d.png)![1*fFaQQ74ezmW2ttOiE64law](img/069f0faa0b5ad63885ccd807a9c199c6.png)

一开始，我们设计罗比有各种模式(**自主**模式、**摄像头**模式、**遥控**模式、**对话**模式)，可以通过按键切换。

但当人们开始与它互动时，我们意识到罗比 90%的时间都将是一个声控机器人。每当人们遇到罗比，他们的本能是走向机器人，并与它交谈。

![1*XzoRTl4gL7B2lJZgRAcvzQ](img/9be3de1c1f7dfacc7c66db859aa78de7.png)![1*UZ6ERqQ2UYzbicsO58xY2g](img/cd02cac75fbbde859e277ff73c2b4f3d.png)![1*_XoTS1r9mOj3bpPux-u-Sw](img/9c001f8c72db5a501fde212e02a3077a.png)

我们意识到拥有一个更好的麦克风和更好的音频扬声器对 Robbie 来说是多么重要，而不是为了更可靠的避障而增加更多的传感器。

![1*9uhWYeoYf-OdqeVvCfgrYA](img/688baa0807427d163953969e9377e566.png)![1*K_Iif14NoqYip9CN7QXCFw](img/5ce1acf0e7042e5360f965c3d6017fe1.png)![1*vEHZ4Sm_GLDkkRyagsDUgw](img/7c5654b6cfaae171a493c479f7640f97.png)

我们决定使用更好、更强大的全向话筒。目前，你必须在麦克风上方几英寸的地方说话，才能被机器人理解。所以现在的目标是让机器人理解命令，即使人在一米之外说话。

当罗比误解时，人们会感到沮丧。这非常依赖于房间的声音条件。

对话是双向的。改进音响系统也是主要优先事项之一。根据房间的气氛，即使 Robbie 理解这个人，当这个人不理解 Robbie 时，这种互动就不好玩了。

我们还毁坏了电池，因为我们不小心没有插上电池检查器就让机器人开着。电池电量消耗超过了阈值。这对我们来说是一次非常昂贵的学习经历。

![1*rW1NRyRizYpMDe4a4zAyCw](img/1f42f0014b9effd444aad30c1ed9780a.png)![1*ABIUhvk1tQ7ZnngFWOeWrg](img/a15985fa4704c789d2b1f84feab24f9c.png)![1*nHtN7tWVVIsYpDrP51KiNQ](img/caaac9b636492ada42f970417d100d58.png)

机器人的手臂也是疼痛的主要来源。机械臂的机械设计不合理。它们非常脆弱，如此脆弱以至于仅仅是机器人的运输就会严重磨损它们。有时一只胳膊会掉下来，把它装回去会很痛苦。

我们不仅要设计更坚固的手臂，还要设计更模块化的手臂，这样，如果手臂脱落，就可以很容易地装回去。

![1*W8r5hxh0w1NFzbf9ZkrjiA](img/eaa06df41052942cd97368b900c21cc7.png)![1*YmnvLYOmCgq8Ydf4g1GKlg](img/a4d44db5c01a57dd19da4160a1c4f6fa.png)![1*4qO0oWAYqXCrrg-L0B2Wqw](img/75679d19a778bbd04fbe31404922c1db.png)

说到模块化和痛苦，排除机器人电子设备的故障是最痛苦的过程。接近电子设备不是一件容易的事情，因为没有“容易接近的门”,电路板被钻得到处都是。

你必须拆除 20 公斤重的机器人的头部，才能在里面放一个万用表。太可怕了。我怎么强调这一点都不为过。下一次我制造机器人时，模块化设计和故障排除的简易性将是我优先考虑的事情。如果有“干净的代码”，也有“干净的电子集成和组装”。

集成不良的电子设备也能工作。但是，如果事情没有经过深思熟虑的组装或组织，当出现问题时，没有人会想修理你的机器人。

![1*VLwI5e6qmptXA8gWGHsLSQ](img/d75f9a2e0c8a7e924b3ccae05a9a1a16.png)![1*jQBMRB3qKo8Y8IztKD8b8g](img/44c88313cf1b904177c52f665fa93a5c.png)![1*GlbFsRr8XaA3iMrT_s8YaA](img/c99feb9fb37ccf9d40ef6392f560f291.png)

### 包扎

这就是我的三个主要观点。

首先，用现成的零件制造一个复杂的机器人比以往任何时候都容易。这都要感谢开源硬件(OSHW)社区。制造机器人是可能的，因为我们正站在巨人宽厚的肩膀上。

第二，设计“干净”代码的想法对于建造一个你会喜欢并想从事的机器人至关重要。

最后但同样重要的是，你需要通过经验、持续迭代和用户反馈来学习，从而为你的产品实现良好的用户体验。

我感谢我的前雇主给了我成长的机会，并在令人兴奋的项目上工作。在广告公司做一名有创意的技术人员确实是一次很好的学习经历。

![1*sVa4kAS2dueSvy-OzbvtGA](img/dac2b9885fbdfa0b8e9d9363cb13f953.png)

信用

*   Tel Castillo —海报设计
*   apol Sta Maria——创意指导、机器人设计和动画
*   许多其他人，包括但不限于 Dom De Leon、JR Ignacio、Axel Raymundo、Merlee Jayme、Alex Syfu、Owel Alvero、Jopy、Cyri、Cathy、Bonat……

![1*Oc4X-6kCJgUMCh63AeP9_g](img/0f14975614c41a6d9b6e6eefdd412597.png)![1*yOUdk_ouqeS7agPeMJlz2Q](img/e24967c49de2fa24ca46661a53151bb1.png)![1*PtnqkSJsedi_Ye62rxUWdA](img/6676323d6a386ad28bdad4681551972e.png)