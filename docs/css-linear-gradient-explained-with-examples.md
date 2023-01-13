# CSS 线性渐变举例说明

> 原文：<https://www.freecodecamp.org/news/css-linear-gradient-explained-with-examples/>

在线性渐变中，颜色向一个方向流动，例如从左到右、从上到下或您选择的任何角度。

![A gradient with Two colour stops](img/825f78e80a7c316b0935e9639f36ac69.png)

A linear gradient with two color stops

## 句法

要创建线性渐变，您必须定义至少两个色标。它们是创建过渡时使用的颜色。在 ****背景**** 或 ****背景-图像**** 属性上声明。

```
background: linear-gradient(direction, colour-stop1, colour-stop2, ...);
```

如果没有指定方向，默认转换是从上到下。

## 例子

### 从上到下:

```
background: linear-gradient(red, yellow);
```

![Top to Bottom](img/b342c641620316022ea9295f3dd37256.png)

****左至** r **右:****

为了使它从左到右，您在以单词 ****到**** 开始的`linear-gradient()`的开头添加一个附加参数，它指示方向:

```
background: linear-gradient(to right, red , yellow);
```

![Left to Right](img/74b5bdc33e117d2f42631fff85ce19b3.png)

****对角线**渐变 **:****

您也可以通过指定水平和垂直起始位置(例如，左上或右下)来对角过渡渐变。

以下是从左上角开始的渐变示例:

```
 background: linear-gradient(to bottom right, red, yellow);
```

![Top-left](img/32dd9fc68ae0a39301cba1cc49c8b67c.png)

### **使用角度来指定渐变的方向**

您也可以使用角度来更准确地指定渐变的方向:

```
background: linear-gradient(angle, colour-stop1, colour-stop2);
```

该角度被指定为水平线和渐变线之间的角度。

```
background: linear-gradient(90deg, red, yellow);
```

![90 degrees](img/1612fd48fc9b2cab0232881c2edccc37.png)

### **使用两种以上的颜色**

您不仅限于两种颜色，您可以使用任意多的逗号分隔颜色。

```
background: linear-gradient(red, yellow, green); 
```

![A gradient with 3 colour stops](img/0b51115d47c5f41f762b914a22ead54d.png)

您也可以使用其他颜色语法，如 RGB 或十六进制代码来指定颜色。

### **硬颜色停止**

您不仅可以使用渐变来过渡渐暗的颜色，还可以使用渐变来立即从一种纯色变为另一种纯色:

```
background: linear-gradient(to right,red 15%, yellow 15%);
```

![Hard colour stops](img/9a74d9856b757005859477226ce23fa5.png)

## 更多信息:

*   《CSS 手册:开发人员使用 CSS 的便捷指南》