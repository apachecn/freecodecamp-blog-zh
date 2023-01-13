# 如何在 CSS 中使用动画

> 原文：<https://www.freecodecamp.org/news/how-to-use-animations-in-css/>

## **使用 CSS 动画**

CSS 动画为网页增添了美感，并从一种 CSS 样式过渡到另一种样式。

为了创建 CSS 动画序列，我们在 CSS 的`animation`属性中有不同的子属性:

*   `animation-delay`
*   `animation-direction`
*   `animation-duration`
*   `animation-iteration-count`
*   `animation-name`
*   `animation-play-state`
*   `animation-timing-function`
*   `animation-fill-mode`

## 在屏幕上移动文本的 CSS 动画序列示例

在 HTML 部分，我们将有一个包含类`container`的`div`和一个包含文本`Hello World`的`h3`:

```
<div class="container">
    <h3> Hello World ! </h3>
</div>
```

对于 CSS 部分:

```
.container {
    /* We will define the width, height and padding of the container */
    /* The text-align to center */
    width: 400px;
    height: 60px;
    padding: 32px;
    text-align: center;

    /* Use the animation `blink` to repeat infinitely for a time period of 2.5s*/
    animation-duration: 2.5s;           
    animation-iteration-count: infinite;
    animation-direction: normal;        
    animation-name: blink;              

    /* The same can be written shorthand as     */
    /* --------------------------------------   */
    /* animation: 2.5s infinite normal blink;   */
}
@keyframes blink {
    0%, 100% {              /* Defines the properties at these frames */
        background: #333;
        color: white;
    }

    50% {                   /* Defines the properties at these frames */
        background: #ccc;
        color: black;
        border-radius: 48px;
    }
}
```

![Imgur](img/621bb1390e938bd9e526fd895ca3d598.png)

## 有关 CSS 动画的更多信息:

*   [CSS 动画快速介绍](https://www.freecodecamp.org/news/a-quick-introduction-to-css-animations-a1655375ec90/)