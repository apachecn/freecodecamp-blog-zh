# CSS 属性–最大宽度、最小宽度、最大高度和最小高度，用例子解释

> 原文：<https://www.freecodecamp.org/news/css-properties-examples/>

如果你正在建立一个网站，让它具有响应性是你要做的最重要的事情之一。

创建在各种屏幕尺寸下都好看的网站可能会很困难。您必须处理呈现元素的宽度或高度，并且使用媒体查询指定不同的宽度大小并不容易。

虽然“百分比”这样的相对单位在某些情况下可能有用，但在其他情况下也可能是灾难性的。

但是不要担心——像最大宽度和最小宽度这样的特征会有所帮助。这些属性允许我们避免使用媒体查询来解决一些响应挑战。

在本文结束时，你将知道什么是最大宽度、最小宽度和最大高度，以及如何使用它们。

## 最大宽度属性

max-width 属性允许您指定元素的最大宽度。这意味着一个元素可以增加宽度，直到它达到一个特定的[绝对或相对单位](https://www.freecodecamp.org/news/css-unit-guide/)，在这一点上，它应该将其宽度固定为该单位。

我要说的是:

想象一下，将一个元素的宽度设置为视窗宽度的 80% 。如果视口的宽度为 **375px** ，那么我们的元素的宽度将为 **300px** ，这还不算太差。

(80/100)* 375 = 300 像素

但是如果我们有一个更宽的视窗，比如说 1300 像素宽的视窗，在这种情况下，我们的元素将会是 1040 像素宽的视窗(T2)。

(80/100)* 1300 = 1040 像素

这就是 max-width 属性派上用场的地方。当您使用相对单位(如百分比)时，在元素上设置 max-width 属性会导致它的宽度不断增加，直到到达我们指定的点。

这里有一个例子:

[https://codepen.io/D_kingnelson/embed/preview/mdmeEOV?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=mdmeEOV](https://codepen.io/D_kingnelson/embed/preview/mdmeEOV?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=mdmeEOV)

注意到我们的卡的大小从来没有超过 **350px** 吗？这就是最大宽度的工作原理。它允许我们的卡片在更小的屏幕上生长。

如果宽度小于 350 像素，无论当前屏幕大小是多少，它都会占据 80%的空间。但是当宽度达到 350 像素时，它会向下夹紧，不会超过设定的宽度。

下面是代码的样子:

```
//The card can not get larger than 350px.

.card{
  margin:0 auto;
  padding:1.5em;
  width:80%;
  max-width:350px;
  height:50%;
  background: #FFFFFF;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.3);
  border-radius:4px;
  overflow:hidden;
} 
```

## 最小宽度属性

与 max-width 属性相反，min-width 属性指定最小宽度。它指示元素的最小可能宽度。

在某些情况下，您可能希望您的元素具有灵活的宽度，因此您可以用相对单位(如百分比)来指定宽度。但是随着屏幕缩小，元素的宽度也会缩小。

这就是最小宽度的用武之地——你可以设置一个最小宽度，这样卡片就不会缩小到小于指定的宽度。

[https://codepen.io/D_kingnelson/embed/preview/zYwdzxW?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=zYwdzxW](https://codepen.io/D_kingnelson/embed/preview/zYwdzxW?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=zYwdzxW)

```
//Here the card element can not get smaller than 250px
.card{
  margin:0 auto;
  padding:1.5em;
  width:80%;
  max-width:350px;
  //here we set the min-width property
  min-width : 250px;
  height:50%;
  background: #FFFFFF;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.3);
  border-radius:4px;
  overflow:hidden;
} 
```

## 最大高度属性

max-height 属性的操作类似于 max-width 属性，但它影响的是高度而不是宽度。

这里有一个例子:

[https://codepen.io/D_kingnelson/embed/preview/gOWxRrb?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=gOWxRrb](https://codepen.io/D_kingnelson/embed/preview/gOWxRrb?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=gOWxRrb)

```
// it fixes the height of an element to a specified unit, effectively stopping it from increasing in height

.card{
  margin:0 auto;
  padding:1.5em;
  width:80%;
  max-width:350px;
  height:70%;
  //here we set the max-height for the card.
  max-height: 400px;
  background: #FFFFFF;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.3);
  border-radius:4px;
  overflow:hidden;
} 
```

## 最小高度属性

与 max-height 相反，min-height 属性提供了元素的最小高度。

当视口收缩并且元素的高度不能降低到超过定义的高度单位时，会出现这种情况。

[https://codepen.io/D_kingnelson/embed/preview/yLboXVz?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=yLboXVz](https://codepen.io/D_kingnelson/embed/preview/yLboXVz?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=yLboXVz)

```
.element{
    height:40vh;
    min-height:200px;
} 
```

## 结论

响应能力是 web 开发中需要考虑的一个重要因素。跟踪事物在多种屏幕尺寸下的显示可能很困难，但是最大宽度、最小宽度、最大高度和最小高度属性有助于应对这些挑战。

这些属性允许您调整元素的大小，而无需使用媒体查询。