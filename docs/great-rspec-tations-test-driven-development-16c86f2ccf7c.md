# 伟大的测试:为什么我喜欢测试驱动的开发

> 原文：<https://www.freecodecamp.org/news/great-rspec-tations-test-driven-development-16c86f2ccf7c/>

作者阿里特·阿玛纳

# 伟大的测试:为什么我喜欢测试驱动的开发

![1*OvF5QBJTAjMURU4G2BSWNA](img/94d20a7f9c28d1df42737ff9b56bb5e1.png)

当我第一次[写测试驱动开发](https://code.likeagirl.io/what-done-looks-like-test-driven-development-e9b0eaa38836)的时候，我认为我爱上了这个概念…但是那只是调情阶段。现在我被鲁布托迷得神魂颠倒了，女朋友！？

不需要启动我的应用程序进行测试所带来的效率提升仅仅是个开始。用 [RSpec](http://rspec.info/) 开发我的应用迫使我认真思考如何定义和构建我的代码。此外，每次我的测试失败时，他们都会忠实地提供提示和线索，帮助我解决丢失、损坏或冗余的问题。这是对一般生活的隐喻…但我离题了。？

作为敏捷开发团队的一员，我正在创建一个在线象棋应用[，本周，我被分配了构建 **move_to 的任务！【x，y】**法。这将移动一个棋子(从现在开始称为**棋子**)到棋盘方格的位置 **(x，y)** 。](https://code.likeagirl.io/no-longer-the-lone-coding-wolf-4fb52360b808)

如果对手的棋子(从现在开始称为**王**)占据了(x，y)，棋子应该捕获它。如果棋子的兄弟占据了(x，y ),该方法应该会引发一个错误消息，并且棋子应该无处可去。

注意:move_to！(x，y)不考虑移动或捕捉是否有效。其他方法会做到这一点。

我配置了 [FactoryBot](https://github.com/thoughtbot/factory_bot) 来生成国际象棋游戏的实例。每个棋子都有以下相关属性: **:location_x** 、 **:location_y** 、**:白色**(一个 boolean**真** =白色)、 **:game_id** 、 **:notcaptured** (一个 boolean**假** =该片已被捕获)。我的第一个测试确定了棋子(当前在 0，0 上)是否移动到空的方块(7，7):

接下来，我开始写 **move_to！(x，y)** 方法，然后运行我的测试:

```
arit (master) chessapp $ rspec spec/models/piece_spec.rb
```

```
.
```

```
Finished in 0.43495 seconds (files took 15.68 seconds to load)
```

```
1 example, 0 failures
```

是啊！没有错误。？？接下来，我编写了一个测试来确定当棋子的目的地被友军占据时，它是否留在原地(我们称之为 ro **ok):**

为什么我没有测试 **rook.notcaptured** 、 **rook.location_x** 和 **rook.location_y** 的值？嗯，车是正在讨论的友军棋子，但我们实际测试的是**找到并保存在*目的地*变量**中的任何棋子(如果有的话)。现在来充实这个方法:

我的测试又通过了！？？感觉非常自信，我继续进行第三个测试:确定对手的国王是否被抓获，棋子是否取代了它的位置:

我也完成了方法:

但是当我运行测试时，我收到了以下错误:

```
arit (master *) chessapp $ rspec spec/models/piece_spec.rb
```

```
..F
```

```
Failures:
```

```
1) Piece captures opponent's piece on destination, then assumes that position
```

```
Failure/Error: expect(destination.notcaptured).to be false
```

```
expected false
```

```
got true
```

```
# ./spec/models/piece_spec.rb:104:in `block (2 levels) in <top (required)>'
```

```
Finished in 0.21787 seconds (files took 4.83 seconds to load)
```

```
1 example, 1 failure
```

```
Failed examples:
```

```
rspec ./spec/models/piece_spec.rb:95 # Piece captures opponent's piece on destination, then assumes that position
```

什么？？？ **destination.notcaptured** 没有更新？为什么？我一遍又一遍地重读我的方法。似乎没有任何东西丢失或损坏(说真的，在 11 行代码中我能错多少呢？).

在决定像一个？慢慢回顾我的 rspec 测试，我突然想到 d**estimation**变量应该会改变。该搬家了 _ 来！(x，y)方法已经更新了它的 l**location _ x，location_y** 和 n **otcaptured** 属性。

然后我突然想到——我需要将数据库中的**目的地**重新加载到 RSpec 中。然后我的三个测试漂亮的通过了:

测试驱动的开发已经永久地影响了我的编码实践，我很高兴有机会让我的其他队友转向它。TDD 是轻量级的、高效的、安全的、有启发性的，它帮助我在尽可能短的时间内产生更高质量的代码。？