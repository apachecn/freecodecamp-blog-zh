# 如何使用文本对齐使图像居中:居中

> 原文：<https://www.freecodecamp.org/news/how-to-center-an-image-using-text-align/>

一个`<img>`元素是一个内嵌元素(显示值为`inline-block`)。通过将`text-align: center;` CSS 属性添加到包含它的父元素中，可以很容易地将它居中。

要使用`text-align: center;`使图像居中，必须将`<img>`放在块级元素(如`div`)中。因为`text-align`属性只适用于块级元素，所以您将`text-align: center;`放在包装块级元素上以实现水平居中的`<img>`。

### **例子**

```
<!DOCTYPE html>
<html>
  <head>
    <title>Center an Image using text align center</title>
    <style>
      .img-container {
        text-align: center;
      }
    </style>
  </head>
  <body>
    <div class="img-container"> <!-- Block parent element -->
      <img src="user.png" alt="John Doe">
    </div>
  </body>
</html>
```

****注意:**** 父元素必须是块元素。如果它不是一个 block 元素，你应该在添加`text-align`属性的同时添加`display: block;` CSS 属性。

```
<!DOCTYPE html>
<html>
  <head>
    <title>Center an Image using text align center</title>
    <style>
      .img-container {
        text-align: center;
        display: block;
      }
    </style>
  </head>
  <body>
    <span class="img-container"> <!-- Inline parent element -->
      <img src="user.png" alt="">
    </span>
  </body>
</html>
```

****Demo:****[Codepen](https://codepen.io/aravindio/pen/PJMXbp)

## 对象匹配

一旦图像居中，您可能想要调整它的大小。属性指定一个元素如何响应它的父框的宽度/高度。

该属性可用于图像、视频或任何其他媒体。它还可以与`object-position`属性一起使用，以获得对媒体显示方式的更多控制。

基本上，我们使用`object-fit`属性来定义它如何拉伸或压扁内嵌媒体。

### 句法

```
.element {
    object-fit: fill || contain || cover || none || scale-down;
}
```

### 价值观念

*   `fill` : ****这是默认值**** 。调整内容大小以匹配其父框，而不考虑纵横比。
*   `contain`:使用正确的纵横比调整内容的大小以填充其父框。
*   `cover`:与`contain`类似，但经常裁剪内容。
*   `none`:以原始尺寸显示图像。
*   `scale-down`:比较`none`和`contain`的差异，找出最小的具体物体尺寸。

### 浏览器兼容性

大多数现代浏览器都支持`object-fit`，关于兼容性的最新信息，你可以看看这个[http://caniuse.com/#search=object-fit](http://caniuse.com/#search=object-fit)。

## **文档**

*   [****文本对齐**** - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/text-align)
*   [**目标契合** - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit)
*   [****<img>****-MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img)