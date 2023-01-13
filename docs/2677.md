# 随机数生成器:计算机如何生成随机数？

> 原文：<https://www.freecodecamp.org/news/random-number-generator/>

几千年来，人们一直在使用**随机** **数字**，所以这个概念并不新鲜。从古巴比伦的彩票，到蒙特卡洛的轮盘赌，再到拉斯维加斯的掷骰子游戏，目标都是把最终结果留给随机的机会。

但是抛开赌博不谈，随机性在科学、统计学、密码学等领域有很多用途。然而，使用骰子、硬币或类似的媒体作为随机设备有其局限性。

由于这些技术的机械性质，产生大量的随机数需要大量的时间和工作。由于人类的聪明才智，我们有了更强大的工具和方法。

## 产生随机数的方法

### 真随机数

![image-145-opt](img/98198ad57e9eeaa085c3bdbb96de3052.png)

Picture of analog-input digital-output processing device. Photo by [Harrison Broadbent](https://unsplash.com/@harrisonbroadbent?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

让我们考虑两种产生随机数的主要方法。**第一种方法**是基于一个物理过程的，从一些**预期** **为随机**的物理现象中收获随机性的来源。

这样的现象发生在计算机之外。对其进行测量，并针对测量过程中可能出现的偏差进行调整。例子包括放射性衰变、光电效应、宇宙背景辐射、大气噪声(我们将在本文中使用)等等。

因此，基于这种随机性生成的随机数被称为“**真**随机数。

从技术上来说，硬件部分包括一个将能量从一种形式转换为另一种形式(例如，将辐射转换为电信号)的设备、一个放大器和一个将输出转换为数字的模数转换器。

## 什么是伪随机数？

![image-146-opt](img/47fe89587dff5311e5ff42285951f49a.png)

Picture of computer code flowing through computer screen. Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit).

作为“真”随机数的替代，生成随机数的第二种方法涉及到可以产生明显随机结果的计算算法。

为什么明显是随机的？因为获得的最终结果实际上完全由初始值决定，初始值也被称为**种子值**或**关键字**。因此，如果您知道密钥值和算法如何工作，您就可以重现这些看似随机的结果。

这种类型的随机数发生器通常被称为**伪随机数**发生器，其结果是输出伪随机数。

尽管这种类型的生成器通常不会从自然发生的随机性来源收集任何数据，但在需要时可以收集密钥。

让我们比较一下真随机数发生器或 TRNG 和伪随机数发生器或 T2 和 PRNG 的一些方面

PRNGs 比 TRNGs 快。由于它们的确定性，当您需要重放一系列随机事件时，它们非常有用。例如，这在代码测试中很有帮助。

另一方面，TRNGs 不是周期性的，更适合于安全敏感的角色，如加密。

**周期**是 PRNG 在开始重复之前经历的迭代次数。因此，在其他条件相同的情况下，预测和破解周期更长的 PRNG 需要更多的计算机资源。

## 伪随机数发生器的示例算法

计算机执行基于一组要遵循的规则的代码。对于一般的 PRNGs，这些规则围绕以下内容:

1.  **接受**一些初始输入的数字，那是种子或密钥。
2.  **在一系列数学运算中应用**种子以生成结果。这个结果就是随机数。
3.  **使用**产生随机数作为下一次迭代的种子。
4.  **重复**模拟随机性的过程。

现在我们来看一个例子。

### 线性同余发生器

这个发生器产生一系列伪随机数。给定一个初始种子**X[0]和整数参数 **a** 为乘数， **b** 为增量， **m** 为模数，生成器由线性关系定义:**X[n]≦(aX[n-1]+b】mod m**。或者使用更加编程友好的语法:**X[n]=(a * X[n-1]+b)% m**。**

这些成员中的每一个都必须满足以下条件:

*   **m > 0** (模数为正)，
*   **0 < a < m** (乘数为正但小于模数)，
*   **0**≤**b<m**(**增量非负但小于模数)，并且**
*   ****0**≤**X[0]m**(种子非负但小于模数)。**

**让我们创建一个 JavaScript 函数，它将初始值作为参数，并返回一个给定长度的随机数数组:**

```
 `// x0=seed; a=multiplier; b=increment; m=modulus; n=desired array length; 
	const linearRandomGenerator = (x0, a, b, m, n) => {
        const results = []
        for (let i = 0; i < n; i++) {
        	x0 = (a * x0 + b) % m
            results.push(x0)
        }
        return results
    }` 
```

**线性同余发生器是最古老和最著名的 PRNG 算法之一。**

**至于可由计算机执行的随机数生成器算法，它们最早可以追溯到 20 世纪 40 年代和 50 年代(例如，[中平方方法](https://en.wikipedia.org/wiki/Middle-square_method)和[莱默生成器](https://en.wikipedia.org/wiki/Lehmer_random_number_generator)，并且今天还在继续编写( [Xoroshiro128+](https://en.wikipedia.org/wiki/Xoroshiro128%2B) 、[平方 RNG](https://en.wikipedia.org/wiki/Counter-based_random_number_generator_(CBRNG)#Squares_RNG) ，等等)。**

## **一个样本随机数发生器**

**当我决定写这篇关于在网页中嵌入随机数生成器的文章时，我需要做出选择。**

**我本可以使用 JavaScript 的 **`Math.random()`** 函数作为基础，像我在以前的文章中一样生成伪随机数输出(参见[乘法表——编写自己的乘法表](https://www.freecodecamp.org/news/multiplication-chart-code-your-own-times-table-using-javascript/))。**

**但是这篇文章本身是关于生成随机数的。所以我决定学习如何收集“真正的”随机数据，并与你分享我的发现。**

**所以下面是“真”随机数发生器。设置参数并点击生成。**

**True Random Number GeneratorResult: 

代码从一个 API 中获取数据，这要归功于[**Random.org**](https://www.random.org/)。这个在线资源有大量有用的、可定制的工具，并附带优秀的文档。

随机性来自大气噪声。我能够使用异步函数。这是未来的一大优势。核心函数如下所示:

```
    // Generates a random number within user indicated interval
   	const getRandom = async (min, max, base) => {
   	 const response = await 	fetch("https://www.random.org/integers/?num=1&min="+min+"
    &max="+max+"&col=1&base="+base+"&format=plain&rnd=new")
        return response.text() 
   	} 

```

它所采用的参数允许用户定制随机数输出。例如，**最小**和**最大**允许您设置生成输出的下限和上限。而**基**决定输出是打印成二进制、十进制还是十六进制。

同样，我选择了这种配置，但在[源](https://www.random.org/)还有更多可用的配置。

当点击生成按钮时，调用`handleGenerate()`函数。它依次调用`getRandom()`异步函数，管理错误处理，并输出结果:

```
	 // Output handling
    const handleGenerate = () => {
    	handleActive(generateButton)
        const base = binary.checked ? 2 : decimal.checked ? 10 : 16
        if (!minimum.value || !maximum.value) {
            prompter.style.color = 'red' 
        	prompter.textContent = "Enter Min & Max values"
        } else {
        	getRandom(minimum.value, maximum.value, base).then((data) => {
        		resultValue.textContent = data
        		prompter.textContent = ""    
        	}).catch((error) => {
        		resultValue.textContent = 'ERROR'
        		prompter.textContent = 'Connection error. Unable to 						generate';    
        	})
       		 handleRestart()
        }

   } 

```

剩下的代码处理 HTML 结构、外观和样式。

该代码可以嵌入并在该网页中使用。我把它分成几个部分，并提供了详细的注释。它很容易被修改。您还可以根据需要修改功能和样式。

这是 GitHub 回购完整代码的链接:[https://github.com/sandroarobeli/random-generator](https://github.com/sandroarobeli/random-generator)**