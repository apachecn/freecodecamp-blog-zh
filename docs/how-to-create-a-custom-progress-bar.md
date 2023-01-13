# 如何创建自定义进度条

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-custom-progress-bar/>

*原载于[www.florin-pop.com](https://www.florin-pop.com/blog/2019/06/how-to-create-a-custom-progress-bar/)*

[每周编码挑战](https://florin-pop.com/blog/2019/03/weekly-coding-challenge/)第 14 周的**主题**是:

## 进度条

进度条用于显示用户操作在完成之前还在进行的程度。一个很好的例子是下载进度条，它显示文件已经下载了多少(或者如果你上传文件，它也可以是上传进度条？).

在本文中，我们将构建这种[进度条](https://codepen.io/FlorinPop17/full/jjPWbv/):

[https://codepen.io/FlorinPop17/embed/preview/jjPWbv?height=300&slug-hash=jjPWbv&default-tabs=css,result&host=https://codepen.io](https://codepen.io/FlorinPop17/embed/preview/jjPWbv?height=300&slug-hash=jjPWbv&default-tabs=css,result&host=https://codepen.io)

## HTML

对于 HTML 结构，我们需要两样东西:

1.  一个*容器*，它将显示进度条- `.progress-bar`的总长度(100%)
2.  基本上跟踪当前进度的实际进度要素(如 20%) - `.progress`

```
<div class="progress-bar">
    <div data-size="20" class="progress"></div>
</div> 
```

如您所见，`.progress` div 有一个`data-size`属性。这将在 **JavaScript** 中使用，以实际设置进度的`width`。你马上就会明白我的意思，但是首先让我们来设计这两个元素。？

## CSS

```
.progress-bar {
    background-color: #fefefe;
    border-radius: 3px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
    margin: 15px;
    height: 30px;
    width: 500px;
    max-width: 100%;
}

.progress {
    background: #ad5389;
    background: -webkit-linear-gradient(to bottom, #3c1053, #ad5389);
    background: linear-gradient(to bottom, #3c1053, #ad5389);
    border-radius: 3px;
    height: 30px;
    width: 0;
    transition: width 0.5s ease-in;
} 
```

关于上面的 CSS，有几件事需要注意:

1.  两个元素都是具有相同高度(`30px`)和相同`border-radius`的矩形
2.  最初的`.progress`宽度设置为`0`，我们将在下面的 **JavaScript** 代码中更新它
3.  另外`.progress`有一个漂亮的`linear-gradient`来自 [uiGradients](https://uigradients.com/)
4.  添加到`.progress`中的`transition`用于在它的`data-size`属性的值动态变化时创建一个漂亮的动画

## JavaScript

为此，我们需要循环所有的`.progress`元素(在我们的例子中只有一个，但是你可以在一个应用程序中添加多个)来获得它们的`data-set`值，并将其设置为它们的宽度。在这种情况下，我们将使用百分比(`%`)。

```
const progress_bars = document.querySelectorAll('.progress');

progress_bars.forEach(bar => {
    const { size } = bar.dataset;
    bar.style.width = `${size}%`;
}); 
```

我们使用了一点点的[对象析构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)。

`const { size } = bar.dataset`

与以下内容相同:

`const size = bar.dataset.size`

但是你可能已经知道了？。

## 结论

您可以做许多事情来改进这个组件。其中一些是:

1.  通过不同的*类*添加多种颜色变体
2.  添加百分比值
3.  通过更改大小值使其动态动画。

我希望你喜欢它，并确保与我分享你的创作！

编码快乐！？