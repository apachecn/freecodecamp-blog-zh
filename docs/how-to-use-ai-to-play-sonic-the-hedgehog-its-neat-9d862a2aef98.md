# 如何用 AI 玩刺猬索尼克？很整洁！

> 原文：<https://www.freecodecamp.org/news/how-to-use-ai-to-play-sonic-the-hedgehog-its-neat-9d862a2aef98/>

作者:韦丹古普塔

# 如何用 AI 玩刺猬索尼克？很整洁！

![TjFU2j4rphtqO5wEJI7LCx8pau65Q8wmrsol](img/db82b192eccb1f53d4ab3b77e6c3e0fa.png)

Human Evolution

一代又一代，人类已经适应变得更加适应我们的环境。我们最初是生活在吃或被吃的世界中的灵长类动物。最终我们演变成了今天的我们，反映了现代社会。通过进化过程，我们变得更加聪明。我们能够更好地与环境合作，完成我们需要做的事情。

通过进化学习的概念也可以应用于人工智能。我们可以训练人工智能使用简洁的、增强拓扑的神经进化来执行某些任务。简单地说，NEAT 是一种算法，它采用一批人工智能(基因组)试图完成一项给定的任务。表现最好的人工智能“繁殖”出下一代。这个过程会一直持续下去，直到我们有一代人能够完成他们需要完成的事情。

![kB3DscigQ-nDtQhS5em32jfdsFdAKp236CXt](img/acae1c0f115803b874167fcb91122200.png)

Clip of AI playing STH

NEAT 令人惊叹，因为它消除了对训练我们的人工智能所需的预先存在的数据的需要。利用 NEAT 和 OpenAI 的 Gym Retro 的力量，我训练了一个 AI 来扮演 SEGA 创世纪中的刺猬索尼克。让我们学习如何！

### 一个简洁的神经网络(Python 实现)

#### GitHub 知识库

[**Vedant-Gupta523/sonicNEAT**](https://github.com/Vedant-Gupta523/sonicNEAT)
[*通过在 GitHub 上创建一个账号，为 ve dant-Gupta 523/sonicNEAT 的开发做贡献。*github.com](https://github.com/Vedant-Gupta523/sonicNEAT)

**注意:**本文和上面的报告中的所有代码都是 Lucas Thompson 的 Sonic AI Bot 的略微修改版本，使用了 Open-AI 和 NEAT [YouTube 教程](https://www.freecodecamp.org/news/how-to-use-ai-to-play-sonic-the-hedgehog-its-neat-9d862a2aef98/Sonic%20AI%20Bot%20Using%20Open-AI%20and%20NEAT%20Tutorial)和[代码](https://gitlab.com/lucasrthompson/Sonic-Bot-In-OpenAI-and-NEAT)。

#### 了解开放式健身房

如果你还不熟悉 OpenAI Gym，请浏览下面的术语。它们将在整篇文章中频繁使用。

**智能体—** 人工智能玩家。在这种情况下，它将是音速的。

**环境—** 代理的完整环境。游戏环境。

**动作—** 代理可以选择做的事情(例如向左移动、向右移动、跳跃、不做任何事情)。

**步骤—** 执行 1 个动作。

**状态—** 环境的一个框架。人工智能目前所处的情况。

**观察—** 人工智能从环境中观察到的东西。

**健身—** 我们的人工智能表现得有多好。

**done —** 当人工智能已经完成它的任务或者不能再继续时。

#### 安装依赖项

下面是 OpenAI 和 NEAT 的 GitHub 链接以及安装说明。

**OpenAI**:【https://github.com/openai/retro 

**整齐**:【https://github.com/CodeReclaimers/neat-python】T2

**Pip 安装**库，如 cv2、numpy、pickle 等。

#### 导入库并设置环境

首先，我们需要导入我们将使用的所有模块:

```
import retro
import numpy as np
import cv2
import neat
import pickle
```

我们还将定义我们的环境，包括游戏和状态:

```
env = retro.make(game = "SonicTheHedgehog-Genesis", state = "GreenHillZone.Act1")
```

为了训练一个 AI 玩刺猬索尼克，你需要游戏的 ROM(游戏文件)。最简单的方法是花 5 美元从 Steam 上购买游戏。你也可以在网上找到免费的 ROM 下载，但是这是非法的，所以不要这样做。

在 OpenAI 资源库中的 **retro/retro/data/stable/** 你会找到一个刺猬索尼克创世纪的文件夹。将游戏的 ROM 放在这里，并确保它的名称为 rom.md。州文件。您可以选择一个并将状态参数设置为等于它。我选择了绿山地带第一幕，因为这是游戏的第一关。

#### 了解 data.json 和 scenario.json

在刺猬索尼克文件夹中你会有这两个文件:

**data.json**

```
{
  "info": {
    "act": {
      "address": 16776721,
      "type": "|u1"
    },
    "level_end_bonus": {
      "address": 16775126,
      "type": "|u1"
    },
    "lives": {
      "address": 16776722,
      "type": "|u1"
    },
    "rings": {
      "address": 16776736,
      "type": ">u2"
    },
    "score": {
      "address": 16776742,
      "type": ">u4"
    },
    "screen_x": {
      "address": 16774912,
      "type": ">u2"
    },
    "screen_x_end": {
      "address": 16774954,
      "type": ">u2"
    },
    "screen_y": {
      "address": 16774916,
      "type": ">u2"
    },
    "x": {
      "address": 16764936,
      "type": ">i2"
    },
    "y": {
      "address": 16764940,
      "type": ">u2"
    },
    "zone": {
      "address": 16776720,
      "type": "|u1"
    }
  }
}
```

**scenario.json**

```
{
  "done": {
    "variables": {
      "lives": {
        "op": "zero"
      }
    }
  },
  "reward": {
    "variables": {
      "x": {
        "reward": 10.0
      }
    }
  }
}
```

这两个文件都包含关于游戏及其训练的重要信息。

听起来，data.json 文件包含关于不同游戏特定变量的信息/数据(例如，Sonic 的 x 位置，他的生命数，等等。).

scenario.json 文件允许我们与数据变量的值同步执行操作。例如，每当索尼克的 x 位置增加时，我们可以奖励他 10.0。当索尼克的生命值达到 0 时，我们也可以将我们的完成条件设置为真。

#### 了解简单前馈配置

配置前馈文件可以在上面链接的 GitHub 库中找到。它就像一个设置菜单来设置我们的培训。指出几个简单的设置:

```
fitness_threshold     = 10000 # How fit we want Sonic to become
pop_size              = 20 # How many Sonics per generation
num_inputs            = 1120 # Number of inputs into our model
num_outputs           = 12 # 12 buttons on Genesis controller
```

有大量的设置你可以试验，看看它如何影响你的人工智能的训练！要了解更多关于 NEAT 和前馈配置中不同设置的信息，我强烈推荐阅读文档[这里](https://neat-python.readthedocs.io/en/latest/)

#### 将所有内容放在一起:创建培训文件

**设置配置**

我们的前馈配置被定义并存储在变量 config 中。

```
config = neat.Config(neat.DefaultGenome, neat.DefaultReproduction, neat.DefaultSpeciesSet, neat.DefaultStagnation, 'config-feedforward')
```

**创建一个函数来评估每个基因组**

我们首先创建函数 eval_genomes，它将评估我们的基因组(一个基因组可以比作一群声波中的一个声波)。对于每个基因组，我们重置环境并采取随机行动

```
for genome_id, genome in genomes:
        ob = env.reset()
        ac = env.action_space.sample()
```

我们还将记录游戏环境的长度、宽度和颜色。我们用长度和宽度除以 8。

```
inx, iny, inc = env.observation_space.shape
inx = int(inx/8)
iny = int(iny/8)
```

我们使用 NEAT 库创建一个[递归神经网络](https://searchenterpriseai.techtarget.com/definition/recurrent-neural-networks) (RNN ),并输入基因组和我们选择的配置。

```
net = neat.nn.recurrent.RecurrentNetwork.create(genome, config)
```

最后，我们定义几个变量:current_max_fitness(当前种群中最高的适应度)、fitness_current(基因组的当前适应度)、frame(帧数)、counter(计算我们的代理所走的步数)、xpos(Sonic 的 x 位置)和 done(是否达到了我们的适应度目标)。

```
current_max_fitness = 0
fitness_current = 0
frame = 0
counter = 0
xpos = 0
done = False
```

虽然我们还没有达到我们的完成要求，但是我们需要运行环境，增加我们的帧计数器，并使我们的观察模拟游戏的观察(仍然针对每个基因组)。

```
env.render()
frame += 1
ob = cv2.resize(ob, (inx, iny))
ob = cv2.cvtColor(ob, cv2.COLOR_BGR2GRAY)
ob = np.reshape(ob, (inx,iny))
```

我们将把我们的观察结果放在一个一维数组中，这样我们的 RNN 就能理解它。我们通过将这个数组提供给我们的 RNN 来接收输出。

```
imgarray = []
imgarray = np.ndarray.flatten(ob)
nnOutput = net.activate(imgarray)
```

利用 RNN 的输出，我们的人工智能向前迈了一步。从这一步，我们可以提取新的信息:一个新的观察、一个奖励、我们是否达到了我们的完成要求，以及我们的 data.json (info)中变量的信息。

```
ob, rew, done, info = env.step(nnOutput)
```

在这一点上，我们需要评估我们的基因组的适合度，以及它是否满足完成的要求。

我们查看 data.json 中的“x”变量，并检查它是否超过了级别的长度。如果有，我们将通过我们的健康阈值增加我们的健康，表示我们完成了。

```
xpos = info['x']

if xpos >= 10000:
        fitness_current += 10000
        done = True
```

否则，我们将通过执行该步骤获得的奖励来增加当前的体能。我们还检查我们是否具有新的最高适应度，并相应地调整我们的 current_max_fitness 的值。

```
fitness_current += rew

if fitness_current > current_max_fitness:
        current_max_fitness = fitness_current
        counter = 0
else:
        counter += 1
```

最后，我们检查我们是否完成了或者我们的基因组是否已经走了 250 步。如果是这样，我们就在被模拟的基因组上打印信息。否则，我们继续循环，直到满足两个要求中的一个。

```
if done or counter == 250:
        done = True
        print(genome_id, fitness_current)

genome.fitness = fitness_current
```

**定义人数、打印训练统计数据等**

我们需要做的最后一件事是定义我们的人口，打印出我们训练的统计数据，保存检查点(以防你想暂停和继续训练)，并腌制我们获胜的基因组。

```
p = neat.Population(config)

p.add_reporter(neat.StdOutReporter(True))
stats = neat.StatisticsReporter()
p.add_reporter(stats)
p.add_reporter(neat.Checkpointer(1))

winner = p.run(eval_genomes)

with open('winner.pkl', 'wb') as output:
    pickle.dump(winner, output, 1)
```

剩下的就是跑程序的事了，看着索尼克慢慢学习怎么打关卡！

![caPrOTLL9OmL9C2V3BLMmUYtx1g0ckxZF1wu](img/68e062f13e3141e8ad9fc4490c0a1f67.png)![FsO5NOjcc5S9cQiDjO56TbQLlOQIzFfdwmwc](img/439a0ec38b7029e9bb5083d83568db75.png)

Earlier generation vs Later generation

要查看所有代码，请查看我的 GitHub 存储库中的 Training.py 文件。

#### 奖励:平行训练

如果你有一个多核 CPU，你可以一次运行多个训练模拟，成倍地提高你训练人工智能的速度！虽然我不会在本文中详细介绍如何做到这一点，但我强烈建议您查看我的 GitHub 存储库中的 **sonicTraning.py** 实现。

### 结论

这就是全部了！经过一些调整，这个框架适用于 NES、SNES、SEGA 创世纪等等的任何游戏。如果你有任何问题或者你只是想打个招呼，请随时发邮件给我，邮箱:vedantgupta 523[at]Gmail[dot]com？

此外，一定要看看 Lucas Thompson 的声波人工智能机器人，它使用开放人工智能和简洁的 YouTube 教程和[代码](https://gitlab.com/lucasrthompson/Sonic-Bot-In-OpenAI-and-NEAT)，看看这篇文章的最初灵感是什么。

### 关键要点

1.  **增强拓扑的神经进化(NEAT)** 是一种用于训练 AI 执行某些任务的算法。它是模仿基因进化的。
2.  **NEAT** 在训练 AI 时，不需要预先存在的数据。
3.  实现 **OpenAI** 和 **NEAT** 使用Python 训练一个 AI 玩任何游戏的过程。