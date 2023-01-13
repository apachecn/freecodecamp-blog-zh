# 如何构建一个简单的标签栏导航组件

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-simple-tab-bar-navigation-component-97277fc5a044/>

我的[每周编码挑战](https://www.florin-pop.com/blog/2019/03/weekly-coding-challenge/)第三周的**主题**是**导航**！所以让我们多了解一点。

### 航行

导航组件在网站上至关重要，因为您希望用户能够轻松地浏览您的页面。你甚至可以在单页网站上找到一个导航组件，允许你跳转到页面的某一部分。所以作为开发人员，知道如何构建这类组件是非常有用的。

在本文中，我决定构建一个[标签栏导航](https://codepen.io/FlorinPop17/full/qvyWxX/)，但是你可以构建任何你想要的导航。

我的灵感来自[这个](https://dribbble.com/shots/5925052-Google-Bottom-Bar-Navigation-Pattern)由[奥雷连恩·所罗门](https://dribbble.com/aureliensalomon)设计的设计。下面是我们将要构建的最终结果:

[https://codepen.io/FlorinPop17/embed/preview/ZZajGB?height=300&slug-hash=ZZajGB&default-tabs=css,result&host=https://codepen.io](https://codepen.io/FlorinPop17/embed/preview/ZZajGB?height=300&slug-hash=ZZajGB&default-tabs=css,result&host=https://codepen.io)

### HTML

HTML 结构将会很简单。我们将有一个`.tab-nav-container`和四个`.tab`在里面:

```
<div class="tab-nav-container">
    <div class="tab active purple">
        <i class="fas fa-home"></i>
        <p>House</p>
    </div>
    <div class="tab pink">
        <i class="far fa-heart"></i>
        <p>Likes</p>
    </div>
    <div class="tab yellow">
        <i class="fas fa-search"></i>
        <p>Search</p>
    </div>
    <div class="tab teal">
        <i class="far fa-user"></i>
        <p>Profile</p>
    </div>
</div>
```

正如你所看到的，每个`.tab`都有一个图标(来自 [FontAwesome](https://fontawesome.con/) )，相应的文本，以及一些额外的类，这些类将用于赋予每个选项卡特定的`background-color`和`color`属性。还有`.active`类，它将用于设置活动标签的样式。很基本，对吧？？

### CSS

首先，让我们来设计一下`.tab-nav-container`:

**注意**:我们在容器上有一个`fixed`宽度，因为我们不希望它在我们改变活动`.tab`时改变大小，因为每个标签可能有一个或长或短的文本(例如 Home (4 个字母)vs Profile (6 个字母))。

我们使用`flexbox`将`.tab`均匀地分布在容器中。除此之外，我相信 CSS 是不言自明的。

接下来……`.tab`的造型:

```
.tab {
    background-color: #ffffff;
    border-radius: 50px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0 20px;
    margin: 0 10px;
    transition: background 0.4s linear;
}

.tab i {
    font-size: 1.2em;
}

.tab p {
    font-weight: bold;
    overflow: hidden;
    max-width: 0;
}

.tab.active p {
    margin-left: 10px;
    max-width: 200px;
    transition: max-width 0.4s linear;
}

.tab.active.purple {
    background-color: rgba(91, 55, 183, 0.2);
    color: rgba(91, 55, 183, 1);
}

.tab.active.pink {
    background-color: rgba(201, 55, 157, 0.2);
    color: rgba(201, 55, 157, 1);
}

.tab.active.yellow {
    background-color: rgba(230, 169, 25, 0.2);
    color: rgba(230, 169, 25, 1);
}

.tab.active.teal {
    background-color: rgba(28, 150, 162, 0.2);
    color: rgba(28, 150, 162, 1);
}
```

这里需要注意几件事:

1.  为了在我们改变`.active`标签时有一个平滑的过渡，我们给`.tab`类设置了一个`transition: background`属性。
2.  默认情况下，`.tab`中的`p`标签有一个`0`的`max-width`，它的`overflow`属性被设置为`hidden`。这是因为我们只想在`.tab`激活时显示文本。
3.  我们使用自定义颜色类(`.purple`、`.pink`等)在标签中使用不同的颜色。

让我们确保它在移动设备上也很好看:

```
@media (max-width: 450px) {
    .tab-nav-container {
        padding: 20px;
        width: 350px;
    }

    .tab {
        padding: 0 10px;
        margin: 0;
    }

    .tab i {
        font-size: 1em;
    }
}
```

正如你所看到的，当视窗的最大宽度为`450px`时，我们正在缩小`.tab-nav-container`，我们也在减少填充，使它看起来更小。

### JavaScript

最后，在 JS 中，我们必须确保当用户点击另一个`.tab`时，`.active`类被添加到其中，并从之前的活动`.tab`中移除:

```
// Get all the tabs
const tabs = document.querySelectorAll('.tab');

tabs.forEach(clickedTab => {
    // Add onClick event listener on each tab
    clickedTab.addEventListener('click', () => {
        // Remove the active class from all the tabs (this acts as a "hard" reset)
        tabs.forEach(tab => {
            tab.classList.remove('active');
        });

        // Add the active class on the clicked tab
        clickedTab.classList.add('active');
    });
});
```

### 结论

这种标签栏导航主要用在移动设备上，所以如果你想在移动应用中重用它，确保你将容器放在屏幕底部(用`position: fixed`)，并重新计算宽度以填充整个屏幕的宽度。

在 Codepen 示例中，我们还更改了单击另一个选项卡时主体的背景颜色。这只是出于视觉目的，与本周的编码主题并不完全相关。但是如果你想知道我是如何做到的，可以查看一下[笔](https://codepen.io/FlorinPop17/pen/qvyWxX)中的 JS 代码(只有 2 行额外的代码)。

### 这个编码挑战的更多例子

在之前的一篇文章中，我演示了如何构建一个响应性导航菜单。如果你想做类似的东西，你也可以去看看。

如果你还没有，确保你阅读了[每周编码挑战](https://www.florin-pop.com/blog/2019/03/weekly-coding-challenge/)“规则”如果你想参加挑战！你为什么不呢？这是提高编码技能的好方法！？

编码快乐！？

*最初发表于[www.florin-pop.com](https://www.florin-pop.com/blog/2019/03/tab-bar-navigation/)。*