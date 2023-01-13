# 放下 Javascript:先学习 HTML 和 CSS

> 原文：<https://www.freecodecamp.org/news/put-down-the-javascript-learn-html-css/>

前端开发的一个日益增长的趋势是，你可以一头扎进 Javascript 并取得成功。老实说，不管是好是坏，你都可以。但是你只是建立在一个脆弱的基础之上，这个基础会回来咬你。

### 为什么我需要 HTML 或者 CSS？

我们今天所知的 UI 框架，如 [React](https://reactjs.org/) 和 [Vue](https://vuejs.org) 建立在网页的基本构件之上:HTML 和 CSS。尽管这些 UI 框架通过一些很酷的工具和 Javascript 强化了这些基础知识，但你所构建的基本上与 1996 年的[太空堵塞网站](https://www.spacejam.com/archive/spacejam/movie/jam.htm)是一样的。

但是我明白了，手工编写 HTML 和 CSS 已经过时了，对吗？

### 了解您的工具在做什么

当您开发和调试应用程序时，至少对幕后发生的事情有一个基本的了解会对您有很大的帮助。

你可能会在浏览器中遇到一些奇怪的事情，比如为什么 HTML 在那里转换代码？以下面的例子为例:

```
<style>
p {
  color: purple;
}
</style>
<h1>My Cool Page</h1>
<p>
  Some cool stuff
  <div>Is this still cool?</div>
</p>
```

当你在 Chrome 中加载时，你会注意到一些变化…

![Image-on-2019-08-17-at-20-15-44](img/a2d0a3ce06d9d7336e21da96f7a49bb3.png)

Example browser fixed HTML

为什么我的段落不都是冷色和紫色的？

事实证明，你的浏览器很有帮助，会自动修复你的代码。一个段落标签(`<p>` ) [不能包含另一个块级元素](https://www.w3.org/TR/html401/struct/text.html#h-9.3.1)，所以 Chrome 和其他浏览器会动态调整你的 HTML 使其有效。HTML 这样就很宽大了！但是这是一个常见的错误，它难倒了不熟悉 HTML 工作原理的新老开发人员。

### 了解 CSS 的魔力

如今 CSS 可以做很多事情。这不仅仅是设置一些颜色，而是让您能够在整个应用程序中提供一致的 UI 模式。

不要害怕！如果你从 Javascript 开始，你可能会想在那里做任何事情，但是你会很快发现在你的 JS 中管理 CSS 的所有真正力量是一件痛苦的事情，坦白地说，[没有必要，除非你是脸书](https://www.colbyfayock.com/2019/07/you-dont-need-css-in-js-why-i-use-stylesheets)。

你能做什么？用纯 CSS 搭建[外星人电影片头场景](https://www.colbyfayock.com/2019/07/you-dont-need-css-in-js-why-i-use-stylesheets)。为你的按钮抓取一些[悬停效果](https://ianlunn.github.io/Hover/)。或者只是[动画什么的](https://daneden.github.io/animate.css/)！

我最喜欢的是创建一个奇特的类似脸书的 loading animation 类，它可以将动画渐变背景应用到你添加的任何东西上:

```
.loading {
  background: linear-gradient(90deg, #eff1f1 30%, #f7f8f8 50%, #eff1f1 70%);
  background-size: 400%;
  animation: loading 1.2s ease-in-out infinite;
}

@keyframes loading {
  0% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0 50%;
  }
}
```

![loading-animation](img/bb835b866268a4fda363ff263bb46a4e.png)

Example loading animation

打开[一个密码笔](https://codepen.io/colbyfayock/pen/aKKoJP)自己试试吧！

### 让你的搜索结果相关

搜索引擎尽最大努力找出你写的内容如何与搜索它的用户相关。但是你如何编写你的 HTML，对于帮助他们确定这个价值是很重要的。我看到的一个常见错误是不正确地使用[标题](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements)元素，或者根本不使用它们。

```
<h1>All</h1>
<h1>My</h1>
<h1>Content</h1>
<h1>Is</h1>
<h1>Important</h1>
```

考虑这篇博文的大纲:

```
- Put Down the Javascript - Learn HTML & CSS
  - Why do I need HTML or CSS?
  - Understand what Your tools are doing
  - Learn the magic of CSS
...
```

“学习 CSS 的魔力”并不是该页面的关键要点，所以我不想强调它是最重要的。然而，这篇文章的标题“放下 Javascript——学习 HTML & CSS”反映了整个故事，使它成为最重要的，所以我想把它列为第一。

所以对于我的 HTML，我想让它看起来更像:

```
<h1>Put Down the Javascript - Learn HTML & CSS</h1>
<h2>Why do I need HTML or CSS?</h2>
<h2>Understand what Your tools are doing</h2>
<h2>Put Down the JS - Learn HTML & CSS/h2>
```

这让 Google、Bing 和所有其他搜索引擎确切地知道什么应该是页面最重要的部分，并帮助识别总体层次结构。

### 通过包容性发展推动无障碍环境

不负责任地编码，我们就自动排除了人们访问我们辛辛苦苦建立的网站。通常，这些人对正在建造的东西的关心程度不亚于你我。

通过第一次做一点功课，多花一秒钟思考我们写的内容，我们可以包容所有访问我们网站的朋友。

就拿今天大多数网站常见的简单导航列表来说吧。你可能会忍不住写下一些`div`因为它们工作正常？

```
<div className="nav">
  <div><a href="#">Link 1</a></div>
  <div><a href="#">Link 2</a></div>
  <div><a href="#">Link 3</a></div>
</div>
```

问题是，它们不容易被屏幕阅读器识别。为了解决这个问题，你/技术上/可以写更少的 HTML ( `div`是 3 个字符，`ul`和`li`是 2？).

```
<ul className="nav">
  <li><a href="#">Link 1</a></li>
  <li><a href="#">Link 2</a></li>
  <li><a href="#">Link 3</a></li>
</ul>
```

更进一步，如果这是你的导航菜单，将它包装在一个 [HTML 5 导航元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/nav) ( `<nav>`)和[中，用户现在将能够直接访问菜单](https://www.w3.org/WAI/tutorials/menus/structure/#identify-menus)。

查看 A11y 项目以获得更多关于可访问性的好建议。

### 简化您的代码，拥抱原生功能

你会惊讶于现代浏览器中有如此多的功能[，比你可能需要的更多的浏览器支持(对那些仍然支持 IE9 的人抱歉)。](https://dev.to/ananyaneogi/html-can-do-that-c0n)

使用一些基本的 HTML，您可以构建一个文本输入，在下拉列表中包含可搜索的、类似自动完成的文本:

```
<label>My Favorite Color</label>
<input type="text" name="color" list="colors">
<datalist id="colors">
  <option value="Magenta">
  <option value="Purple">
  <option value="Ultraviolet">
</datalist>
```

![Screen-Capture-on-2019-07-30-at-21-49-55](img/5bafece96430e0de399c81dd30a0a9c2.png)

Favorite color selector

利用 CSS 伪选择器，我们可以根据复选框类型的元素是否被选中来动态地设置它的样式:

```
<style>
.is-checked {
    display: none;
}

#my-checkbox:checked + span .is-checked {
    display: inline;
}

#my-checkbox:checked + span .not-checked {
    display: none;
}
</style>

<label for="my-checkbox">
  <input id="my-checkbox" type="checkbox" />
  <span>
    <span class="not-checked">Not Checked</span>
    <span class="is-checked">Checked</span>
    </span>
</label>
```

![Screen-Capture-on-2019-07-30-at-21-48-43](img/7e779d94c6dc7e07080929b4d6137d68.png)

Dynamic checkbox

### 这是你的手艺，注意点

![Screen-Shot-2019-08-28-at-09.11.40](img/42960c5ce4209b66b8a16fa5d3b412b5.png)

[https://twitter.com/markdalgleish/status/1155430223963234304](https://twitter.com/markdalgleish/status/1155430223963234304)

我敢打赌，大多数阅读这篇文章的人这样做是因为他们关心自己的代码，对自己的工作充满热情。就像开发之前的任何其他技能一样，练习和关注基础知识会增强你作为开发人员的能力。此外，通过帮助你的工作更具包容性，让更多人加入你的申请，你将轻松获胜！

[![Follow me for more Javascript, UX, and other interesting things!](img/1c93a38209f03fa2ee013e5b17071f07.png)](https://twitter.com/colbyfayock)

*   [？在 Twitter 上关注我](https://twitter.com/colbyfayock)
*   [？️订阅我的 Youtube](https://youtube.com/colbyfayock)
*   [✉️注册我的简讯](https://www.colbyfayock.com/newsletter/)

*最初发布于[https://www . colbyfayock . com/2019/08/put-down-the-JavaScript-learn-html-CSS](https://www.colbyfayock.com/2019/08/put-down-the-javascript-learn-html-css)*