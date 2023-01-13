# 乘法表-使用 JavaScript 编写你自己的乘法表

> 原文：<https://www.freecodecamp.org/news/multiplication-chart-code-your-own-times-table-using-javascript/>

学习乘法表是一项基本技能，也是任何数学教育的基本部分。一张**乘法表**是一个方便的工具，可以将枯燥的记忆变成有趣的逻辑练习。

图表显示了两个数字的乘积。通常，一组数字(1-9)写在左栏，另一组写在顶行。这形成了一个视觉正方形的两边。他们的产品填满了广场的其余部分。看起来像这样:

1234567891123456789224681012141618336912151821242744812162024283236551015202530354045661218243036424854771421283542495663881624324048566472991827364554637281

这种工具的视觉特性通过使用区域的概念来增强学习。**2×3**等于数字 **6** 以及一边为 **2** 另一边为 **3** 的矩形面积。

呈现乘法表样式和功能的方式有很多。每个作者都会添加自己特别的东西。在本文中，我将分享一种设计和编写这种图表的方法。

在开始图表描述之前，我必须提到一个重要的细节。本文中嵌入的代码块之间可能没有任何关系。

然而，在幕后，它们被放在每篇文章的单个`<body>`元素中。因此，确保你使用的 **id** 和**类**属性对于每个块来说都是**唯一的**。否则，跨两个或更多块共享名称的类或 id 可能会干扰并负面影响样式和功能。

## 如何制作乘法表

HTML 部分是罗马数字图表的修改版本。基本构件是一个按钮。你也可以使用普通的 **div** ，但是我发现这个按钮反应更灵敏。

首先将按钮放入行中，然后再放入 flex 容器中。

```
	 <div class='flex-table'> 
	<h2 class='table-title'>Multiplication Table</h2> 
    	<div class='table-row'> 
    		<button class='item header'>1</button> 
		<button class='item core' onmouseover='onePlay()'>1</button> 
        	<button class='item core' onmouseover='twoPlay()'>2</button> 
        	<button class='item core' onmouseover='threePlay()'>3</button> 
		.......................................................... 
              	..........................................................
    	</div>
    	 <div class='table-row'> 				       
                ..........................................................
                ..........................................................
        </div>
                ............................................................. 
     <div> 

```

使用的架构或元素不必是独特的，您可以添加自己的原创风格。我应用了样式和媒体查询，以便在各种设备上舒适地查看。

```
	 /* Mobile phones */
    @media screen and (max-width: 767px){
       .flex-table {
          transform: scale(0.60);
       }
    }
    /* Tablets and iPads */
    @media screen and (min-width: 768px) and (max-width: 1023px){
       .flex-table {
          transform: scale(0.8);
       }
    } 

```

视觉效果是通过 CSS 实现的。我决定使用 JavaScript 引入音频元素。我很高兴发现这个编辑支持它。

代表乘法结果的每个按钮都连接到一个函数。函数返回一个音频元素，播放该元素特有的声音文件。单击事件充当触发器，调用该函数。

此处不支持模板文字。因此，每个函数调用都必须硬连接到元素中，并单独定义。

```
	 <script>

function onePlay() {
  const one = 
  new Audio('https://evolution.voxeo.com/library/audio/prompts/numbers/1.wav')
  one.currentTime = 0
  one.play()
}
function twoPlay() {
  const two = 
  new Audio('https://evolution.voxeo.com/library/audio/prompts/numbers/2.wav')
  two.currentTime = 0
  two.play()
}
   ...............................................................
   ................................................................

   </script> 

```

非常感谢创建和维护这个声音库的专家们。完整的代码可以通过[点击这里](https://github.com/sandroarobeli/MultiplicationChart/blob/master/MultiplicationChart.txt)找到 Github repo。

## 乘法表。悬停并点击

## 如何建立一个乘法游戏

由于练习是最好的学习方法，乘法也不例外，所以我决定写一个小游戏，你可以在下面看到。

## 输入您的答案，然后单击提交

### 正确:

### 不正确:

在左上角的窗口中，有一个挑战问题。旁边是一个接受答案的输入元素。单击 Submit 按钮评估该答案，并输出消息指出它是否正确。

正确答案会添加到绿色的“正确答案”计数器中，而错误答案会添加到旁边的红色计数器中。

评估答案后，将使用随机数生成器生成新的质询问题，并重复该循环。十个问题循环结束后，游戏停止并显示最终结果，同时播放声音文件。

按下重新开始按钮开始一个新的十题游戏。在没有输入答案的情况下按下提交按钮会触发警告消息和声音。

您可以在编辑器的限制范围内轻松地更改元素的视觉设计和位置。此外，这里采用的逻辑也可以应用于设计其他游戏。例如，乘法可以变成电影琐事等等。

通过[点击这里](https://github.com/sandroarobeli/MultiplicationGame/blob/master/MultiplicationGame.txt)可以访问带有注释的完整代码作为 Github repo。