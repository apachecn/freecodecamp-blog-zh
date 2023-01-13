# 如何用 HTML、CSS 和 JavaScript 创建幻灯片

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-slideshow/>

web 幻灯片是图像或文本的序列，由在特定时间间隔内显示序列中的一个元素组成。

对于本教程，您可以按照以下简单步骤创建幻灯片:

### **写一些标记**

```
 <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Slideshow</title>
      <link rel="stylesheet" href="style.css">
    </head>
    <body>
      <div id="slideshow-example" data-component="slideshow">
        <div role="list">
          <div class="slide">
            <img src="" alt="">
          </div>
          <div class="slide">
            <img src="" alt="">
          </div>
          <div class="slide">
            <img src="" alt="">
          </div>
        </div>
      </div>
    <script src="slideshow.js"></script>
    </body>
  </html>
```

## 编写隐藏幻灯片和只显示一张幻灯片的样式。

要隐藏幻灯片，你必须给他们一个默认的风格。它将指示您只显示一张幻灯片，如果它是活动的或者如果您想要显示它。

```
 [data-component="slideshow"] .slide {
    display: none;
  }

  [data-component="slideshow"] .slide.active {
    display: block;
  }
```

## 按时间间隔更换幻灯片。

更改幻灯片放映的第一步是选择幻灯片包装，然后选择其幻灯片。

当您选择幻灯片时，您必须仔细检查每张幻灯片，并根据您想要显示的幻灯片添加或删除活动类别。然后只要在一定的时间间隔内重复这个过程。

请记住，当您从幻灯片中删除一个活动类时，由于上一步中定义的样式，您正在隐藏它。但是当你添加一个活动类到幻灯片时，你覆盖了样式`display:none to display:block`，所以幻灯片会显示给用户。

```
 var slideshows = document.querySelectorAll('[data-component="slideshow"]');

  // Apply to all slideshows that you define with the markup wrote
  slideshows.forEach(initSlideShow);

  function initSlideShow(slideshow) {

    var slides = document.querySelectorAll(`#${slideshow.id} [role="list"] .slide`); // Get an array of slides

    var index = 0, time = 5000;
    slides[index].classList.add('active');  

    setInterval( () => {
      slides[index].classList.remove('active');

      //Go over each slide incrementing the index
      index++;

      // If you go over all slides, restart the index to show the first slide and start again
      if (index === slides.length) index = 0; 

      slides[index].classList.add('active');

    }, time);
  }
```

#### **[Codepen 示例以下本教程](https://codepen.io/AndresUris/pen/rGXpvE)**