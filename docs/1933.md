# 如何使用 Flexbox 和网格✨将 CSS 中的任何内容居中

> 原文：<https://www.freecodecamp.org/news/how-to-center-objects-using-css/>

今天我将向你展示如何用 CSS 居中对齐你的内容。一路上，我们将看看各种**对准技术**。所以，让我们开始吧！🥇

## 目录->

*   如何使用 **Flexbox** 来
    *   [水平居中](#how-to-center-anything-horizontally-using-flexbox)
    *   [垂直居中](#how-to-center-anything-vertically-using-flexbox)
    *   [水平居中&垂直居中](#how-to-center-a-div-horizontally-vertically-using-flexbox)
*   如何使用**网格**来
    *   [水平居中](#how-to-center-anything-horizontally-using-css-grid)
    *   [垂直居中](#how-to-center-anything-vertically-using-css-grid)
    *   [水平居中&垂直居中](#how-to-center-a-div-horizontally-vertically-using-css-grid)
*   [变换&位置属性](#how-to-use-the-css-position-property-to-center-anything)
*   [保证金属性](#how-to-use-the-margin-property-to-center-anything)
*   [**附加资源**](#additional-resources)
*   [结论](#conclusion)

![Frame-73](img/390c3c3c17138f7305453e96c12c0e2e.png)

Methods

## 如果你喜欢，你也可以在 YouTube 上观看这个教程:

[https://www.youtube.com/embed/RTEzXS_CT5w?feature=oembed](https://www.youtube.com/embed/RTEzXS_CT5w?feature=oembed)

## 但是....等一下！

![Frame-35--3-](img/b855dda9be3e11ca1563dcd96c026296.png)

首先，我们来了解一下:

*   主茎
*   横轴

## CSS 中的主轴是什么？

您也可以将其称为:

*   **X 轴**
*   主茎
*   水平线

从**左**到**右**的直线为主轴。

![Frame-71](img/51ac1f37804b4fa79694833d0428a80a.png)

Main Axis

## CSS 中的横轴是什么？

您也可以将其称为:

*   **Y 轴**
*   横轴
*   垂直线

从**顶部**到**底部**的线是十字轴。

![Frame-72](img/75dfd0d9c16aba7b53c609f24b95f3fa.png)

Cross Axis

# 项目设置

![Frame-54](img/dc4c03534dd91326d85c96951666ea4d.png)

若要试验所有属性和值，请在代码编辑器中编写以下代码。

### 超文本标记语言

在 body 标记内编写以下代码:

```
<div class="container">

   <div class="box-1"> </div>

</div>
```

### 半铸钢ˌ钢性铸铁(Cast Semi-Steel)

清除**默认**浏览器样式，以便我们能够更准确地工作:

```
*{
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}
```

选择**。容器**类，并将其设置为 100vh。否则，我们无法在**垂直轴**上看到我们的结果:

```
.container{
   height: 100vh;
}
```

样式**。box-1** 类这样:

```
.box-1{
   width: 120px;
   height: 120px;
   background-color: skyblue;
   border: 2px solid black;
}
```

我们都准备好了，现在让我们开始编码吧！

![Frame-3--5-](img/a4f24f91db405015c332d0fa125ea40d.png)

## 如何使用 Flexbox 使任何东西居中

![Thumbnail-hashnode](img/644ae2ee753cd11dd558d469d5323d90.png)

我们可以使用 Flexbox 沿 X 轴和 Y 轴对齐我们的内容`div`。为此，我们需要在`.container`类中编写`display: flex;`属性:

```
.container{
   display: flex;

   height: 100vh;
}
```

我们将对这两个属性进行实验:

*   `justify-content`
*   `align-items`

## 如何使用 Flexbox 将任何东西水平居中

我们可以使用这些值使用 **justify-content** 属性来实现这一点👇

![Justify-content-1](img/7222060355e84e2ecc3e34975a5565eb.png)

**values of flexbox justify-content property**

要试验这些值，请编写以下代码👇

```
.container{
   display: flex;
   height: 100vh;

 /* Change values to  👇 experiment*/
   justify-content: center;
}
```

结果将是这样的👇

![Frame-6--2-](img/ce6bf8696c4d0c58231b8706edc694a3.png)

result of justify-content flexbox

## 如何使用 Flexbox 将任何东西垂直居中

我们可以使用这些值通过 **`align-items`** 属性来实现👇

![align-items-1](img/5672cb6488ffe2cb005b01ed0723ee92.png)

**values of Flexbox align-items property**

要试验这些值，请编写以下代码👇

```
.container{
   height: 100vh;
   display: flex;

 /* Change values 👇 to experiment*/
   align-items: center;
} 
```

结果看起来像这样👇

![Frame-7--4-](img/7a18dc3e439df1eae7a46c4b3a3404bb.png)

Result of align-items flexbox

## 如何使用 Flexbox 将 div 水平和垂直居中

这里，我们可以结合 **`justify-content`** 和 **`align-items`** 属性来水平和垂直对齐一个 div。

编写以下代码👇

```
.container{
   height: 100vh;
   display: flex;

/* Change values 👇 to experiment*/
   align-items: center;
   justify-content: center;
}
```

结果看起来像这样👇

![Frame-8--1-](img/33f1962e17ee20c1ef088ce4dde1ffed.png)

Centering a div Horizontally & vertically

您可以查看此[备忘单](https://www.freecodecamp.org/news/css-flexbox-tutorial-with-cheatsheet/)以了解更多关于 Flexbox 的各种属性。

## 如何使用 CSS 网格将任何内容居中

![Frame-70](img/6f9252027abe364f2a5647ea46caa0f7.png)

我们可以使用**网格**沿着 X 轴和 Y 轴对齐我们的内容`div`。为此，我们需要在`.container`类中编写`display: grid;`属性:

```
.container{
   display: grid;

   height: 100vh;
}
```

我们将对这两个属性进行实验:

*   `justify-content`
*   `align-content`

**或者**，您可以使用这两个属性:

*   `justify-items`
*   `align-items`

## 如何使用 CSS 网格水平居中

我们可以使用这些值通过 **`justify-content`** 属性来实现👇

![justify-content-1--1-](img/d7559d8cb2ee5e671a02b53f686fd13e.png)

**values of Grid justify-content property**

编写以下代码👇

```
.container{
   height: 100vh;
   display: grid;

  /* Change  values   👇 to experiment*/
   justify-content: center;
}
```

结果看起来像这样👇

![Frame-6--2--1](img/0241de87ad5a5fe47c08a2fa6f07c835.png)

**result of justify-content grid**

## 如何使用 CSS 网格垂直居中

我们可以使用这些值通过 **`align-content`** 属性来实现👇

![align-content-1](img/28120fa28be53777920901f2a6a67db4.png)

Values of CSS grid align-content property

编写以下代码👇

```
.container{
   height: 100vh;
   display: grid;

  /*  Change values 👇 to experiment*/
   align-content: center;
}
```

结果将是这样的👇

![Frame-7--4--1](img/43213191dd2aec5745c36358ced89142.png)

result of align-content grid

## 如何使用 CSS 网格使一个 div 水平和垂直居中

这里，我们可以结合 **`justify-content`** 和 **`align-content`** 属性来水平和垂直对齐一个 div。

编写以下代码👇

```
.container{
   height: 100vh;
   display: grid;

/* Change  values  👇 to experiment*/
   align-content: center;
   justify-content: center;
}
```

结果看起来像这样👇

![Frame-8--1--1](img/cea0b8d3e6d2be7f5b442e2a03dd3e2b.png)

Centering a div Horizontally & vertically with Grid

## 替代方式

您也可以使用 **`justify-items`** 和 **`align-items`** 属性来复制相同的结果:

```
.container{
   height: 100vh;
   display: grid;

/* Change  values  👇 to experiment*/
   align-items: center;
   justify-items: center;
}
```

## CSS 网格中的位置内容属性

这是 CSS 网格- >的两个属性的**简写**

*   `justify-content`
*   `align-content`

跟着走👇

```
.container{
   height: 100vh;
   display: grid;

   place-content: center;
}
```

我们得到同样的结果👇

![Frame-8--1--2](img/304d3d3c88742a116653fb9e74a82e90.png)

Centering a div Horizontally & vertically

检查此[备忘单](https://www.freecodecamp.org/news/css-grid-tutorial-with-cheatsheet)以找出各种网格属性之间的差异。

## 休息一下！

到目前为止还不错——休息一下。

![Frame-67--1-](img/8c0029642336fb70e611901451b8d9ca.png)

## 如何使用 CSS Position 属性将任何内容居中

![Frame-12-1](img/4337202600a83bab669ddfa738398ff2.png)

这是这些属性的组合->

*   `position`
*   `top, left`
*   `transform, translate`

编写以下代码👇

```
.container{
   height: 100vh;
   position: relative;
}
```

与此同时:

```
.box-1{
   position: absolute;

   width: 120px;
   height: 120px;
   background-color: skyblue;
   border: 2px solid black;
}
```

## 第一...了解分区的中心点

默认情况下，这是 div 的中心点👇

![Frame-9](img/6289998cde42ead4dc7500314792a802.png)

**Default Center point of a div**

这就是为什么我们会看到这种奇怪的行为👇

![Frame-8--2-](img/d6a4f962114bbde89095c2f1fac3f97e.png)

**Box is not at exact center **

请注意，盒子并不在上图中的**中心**处。👆

通过写这一行👇

```
transform: translate(-50%,-50%); 
```

我们解决问题👇

![Frame-10--2-](img/5f89bfbd8ec876a5d9388e36e37ada69.png)

**New Center point of our div**

我们得到了这个结果👇

![Frame-11--1-](img/84d1badcb0832b09a833b9bd6ee780ef.png)

**Box is at exact center point**

## CSS 中的 Translate 属性是什么？

Translate 是 3 个属性的简写->

*   `translateX`
*   `translateY`
*   `translateZ`

## 如何使用 CSS Position 属性将 div 水平居中

我们将在``.box-`类中使用`left`属性。跟着走👇

```
.box-1{
/* other codes are here*/	

   left: 50%;
   transform: translate(-50%);
}
```

我们得到了这个结果👇

![Frame-6--2--2](img/168be04d5467d0fbaa27c74835b9cffc.png)

**result of left & transform property**

## 如何使用 CSS Position 属性将一个 div 垂直居中

我们将在``box-`类中使用`top`属性。跟着走👇

```
.box-1{
/* Other codes are here*/	

   top: 50%;
   transform: translate(0,-50%);
}
```

我们得到了这个结果👇

![Frame-7--4--2](img/7fb8447657ad2174c212e11edf731251.png)

**result of top & transform property**

## 如何使用 CSS position 属性使一个 div 水平和垂直居中

为了达到这个结果，我们要将这些属性结合在一起->

*   `top, left`
*   `transform, translate`

跟着走👇

```
.box-1{
/*Other codes are here */	

   top: 50%;
   left: 50%;
   transform: translate(-50%,-50%);
}
```

我们得到了这个结果👇

![Frame-8--1--3](img/99c8d68f53184d1b7e97893733aa58b5.png)

result of position & transform property

## 如何使用 margin 属性将任何内容居中

![Frame-73--2-](img/3a3785c1f65925d4662b0921dbefabdd.png)

margin 属性是 4 个属性的简写

*   `margin-**top**`，`margin-**bottom**`
*   `margin-**left**`，`margin-**right**`

编写以下代码来设置它👇

```
.container{
   height: 100vh;

   display: flex;
}

.box-1{
   width: 120px;
   height: 120px;
   background-color: skyblue;
   border: 2px solid black;
}
```

## 如何使用 CSS margin 属性将一个 div 水平居中

我们将在`.box-1`类中使用`margin`属性。编写以下代码👇

```
.box-1{
 //Other codes are here 

  margin: 0px auto;	
}
```

结果看起来像这样👇

![Frame-6--2--3](img/5dea79bb18af7aefcf59f4adaa41d20c.png)

****result of** CSS margin Property**

## 如何使用 CSS margin 属性将 div 垂直居中

我们将在`.box-1`类中使用`margin`属性。编写以下代码👇

```
.box-1{
 //Other codes are here 

  margin: auto 0px;	
}
```

结果看起来像这样👇

![Frame-7--4--3](img/2bcba2f31d458afb735c0cc9c41f8260.png)

****result of** CSS margin property**

## 如何使用 CSS margin 属性使一个 div 水平和垂直居中

我们将在``.box-`类中使用`margin`属性。编写以下代码👇

```
.box-1{
 //Other codes are here 

  margin: auto auto;	
}
```

结果看起来像这样👇

![Frame-8--1--4](img/476559a5db5808c729a213ef038d1354.png)

**Result of CSS margin property**

## 额外资源

*   [使用备忘单完成 Flexbox 教程](https://www.freecodecamp.org/news/css-flexbox-tutorial-with-cheatsheet/)
*   [使用备忘单完成 CSS 网格教程](https://www.freecodecamp.org/news/css-grid-tutorial-with-cheatsheet/)

# 信用

*   [拔毛](https://www.flaticon.com/packs/unicorn-4)，[小猫](https://www.flaticon.com/packs/kitty-avatars-3)
*   [艺人](https://www.freepik.com/free-vector/collection-people-enjoying-their-free-time_4931926.htm#position=7)，[吉](https://www.freepik.com/free-vector/cute-cat-unicorn-play-box-cartoon-icon-illustration_12567355.htm#position=0)

# 结论

现在，你可以自信地使用 CSS 中这四种方法中的任何一种来对齐或居中你的内容。

这是你的**奖章**，奖励你一直读到最后，❤️

## 建议和批评是高度赞赏❤️

![Alt Text](img/e4c60320fc07a98e9df1ec3e3c91f0a4.png)

**YouTube**

**LinkedIn[/Joy Shaheb](https://www.linkedin.com/in/joyshaheb/)**

**推特[/JoyShaheb](https://twitter.com/JoyShaheb)**

**insta gram[/JoyShaheb](https://www.instagram.com/joyshaheb/)**