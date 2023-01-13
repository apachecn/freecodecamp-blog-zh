# CSS 动画快速介绍

> 原文：<https://www.freecodecamp.org/news/a-quick-introduction-to-css-animations-a1655375ec90/>

> 对学习 CSS 感兴趣？获取我的 [CSS 手册](https://flaviocopes.com/page/css-handbook/)

#### 介绍

使用`animation`属性将动画应用于元素。

```
.container { animation: spin 10s linear infinite;}
```

`spin`是动画的名字，我们需要单独定义。我们还告诉 CSS 让动画持续 10 秒，以线性方式执行(没有加速度或速度上的任何差异)，并无限重复。

你必须使用**关键帧**来**定义你的动画如何工作**。下面是一个旋转项目的动画示例:

```
@keyframes spin { 0% {  transform: rotateZ(0); } 100% {  transform: rotateZ(360deg); }}
```

在`@keyframes`定义中，你可以有任意多的中间航路点。

在这种情况下，我们指示 CSS 使 transform 属性将 Z 轴从 0 度旋转到 360 度，完成整个循环。

您可以在这里使用任何 CSS 转换。

请注意，这并没有规定动画应该采用的时间间隔。这是通过`animation`使用时定义的。

#### 一个 CSS 动画示例

我想画四个圆，都有一个共同的起点，彼此相距 90 度。

```
<div class="container">  <div class="circle one"></div>  <div class="circle two"></div>  <div class="circle three"></div>  <div class="circle four"></div></div>
```

```
body {  display: grid;  place-items: center;  height: 100vh;}
```

```
.circle { border-radius: 50%; left: calc(50% - 6.25em); top: calc(50% - 12.5em); transform-origin: 50% 12.5em; width: 12.5em; height: 12.5em; position: absolute; box-shadow: 0 1em 2em rgba(0, 0, 0, .5);}
```

```
.one,.three { background: rgba(142, 92, 205, .75);}
```

```
.two,.four { background: rgba(236, 252, 100, .75);}
```

```
.one { transform: rotateZ(0);}
```

```
.two { transform: rotateZ(90deg);}
```

```
.three { transform: rotateZ(180deg);}
```

```
.four { transform: rotateZ(-90deg);}
```

你可以在这个小故障中看到它们:

让我们让这个结构(所有的圆一起)旋转。为此，我们在容器上应用动画，并将该动画定义为 360 度旋转:

```
@keyframes spin { 0% {  transform: rotateZ(0); } 100% {  transform: rotateZ(360deg); }}
```

```
.container { animation: spin 10s linear infinite;}
```

请点击此处查看:

您可以添加更多关键帧以获得更有趣的动画:

```
@keyframes spin { 0% {  transform: rotateZ(0); } 25% {  transform: rotateZ(30deg); } 50% {  transform: rotateZ(270deg); } 75% {  transform: rotateZ(180deg); } 100% {  transform: rotateZ(360deg); }}
```

参见示例:

### CSS 动画属性

CSS 动画提供了许多不同的参数，您可以调整:

*   **动画名称—** 动画的名称，引用使用关键帧创建的动画
*   **动画持续时间—** 动画应该持续多长时间，以秒为单位
*   **动画-计时-函数—** 动画使用的计时函数(常用值:线性、缓动)。默认:缓解
*   **动画-延迟—** 启动动画前等待的可选秒数
*   **动画-迭代-计数—** 动画应该执行多少次。期望一个数字或无穷大。默认值:1
*   **动画-方向—** 动画的方向。可以是正常、反向、交替或交替反向。在最后两个，它交替前进，然后后退
*   **animation-fill-mode —** 定义当动画结束时，在完成其迭代计数后，如何对元素进行样式化。无或向后返回到第一个关键帧样式。向前和都使用在最后一个关键帧中设定的样式
*   **动画-播放-状态—** 如果设置为暂停，则暂停动画。默认正在运行。

`animation`属性是所有这些属性的简写，顺序如下:

```
.container {  animation: name             duration             timing-function             delay             iteration-count             direction             fill-mode             play-state;}
```

这是我们上面使用的例子:

```
.container { animation: spin 10s linear infinite;}
```

### CSS 动画的 JavaScript 事件

使用 JavaScript，您可以监听以下事件:

*   `animationstart`
*   `animationend`
*   `animationiteration`

小心使用`animationstart`，因为如果动画在页面加载时开始，您的 JavaScript 代码总是在 CSS 被处理后执行。那么动画将已经开始，您不能截取事件。

```
const container = document.querySelector('.container')container.addEventListener('animationstart', (e) => { //do something}, false)container.addEventListener('animationend', (e) => { //do something}, false)container.addEventListener('animationiteration', (e) => { //do something}, false)
```

> 对学习 CSS 感兴趣？获取我的 [CSS 手册](https://flaviocopes.com/page/css-handbook/)