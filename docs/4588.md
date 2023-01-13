# 如何创建分页组件

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-pagination-component/>

[每周编码挑战](https://www.florin-pop.com/blog/2019/03/weekly-coding-challenge/)第 17 周的**主题**是:

## 页码

分页组件用在网站上，当网站上的可用内容多于您希望一次显示给用户的内容时，您可以将它拆分到多个页面上。通过将不同页面上的内容分开，你也为用户节省了大量的带宽，因为用户不需要一次下载所有的信息。

一些有分页的例子:有多个页面的博客，有多个产品的网上商店，等等。

在本文中，我们将构建这个[分页组件](https://codepen.io/FlorinPop17/full/BgrvgX/):

[https://codepen.io/FlorinPop17/embed/preview/BgrvgX?height=300&slug-hash=BgrvgX&default-tabs=css,result&host=https://codepen.io](https://codepen.io/FlorinPop17/embed/preview/BgrvgX?height=300&slug-hash=BgrvgX&default-tabs=css,result&host=https://codepen.io)

**注意**:分页是没有功能的，它只是为了演示的目的(视觉)。作为一个额外的挑战，你可以在一个真实的网站上链接这个。

## HTML

对于 HTML 结构，我们将使用一个`ul`作为具有多个`li`的包装器。每个`li`内部都将有一个`a`标签，因为它是可点击的(并且是语义的),它会将用户发送到所需的页面(如果需要)。

我们也使用 [FontAwesome](https://fontawesome.com/) 作为图标(左、右和圆点图标)。

```
<ul class="pagination">
	<li>
		<a href="#"><i class="fas fa-chevron-left"></i></a>
	</li>
	<li>
		<a href="#"><i class="fas fa-ellipsis-h"></i></a>
	</li>
	<li><a href="#">2</a></li>
	<li class="active"><a href="#">3</a></li>
	<li><a href="#">4</a></li>
	<li><a href="#">5</a></li>
	<li>
		<a href="#"><i class="fas fa-ellipsis-h"></i></a>
	</li>
	<li>
		<a href="#"><i class="fas fa-chevron-right"></i></a>
	</li>
</ul>
```

如你所见，我还在其中一个`li`中添加了一个`.active`类——这只是为了突出显示我们所在的页面。

## CSS

我将粘贴 CSS，然后我们将讨论重要的部分。

```
.pagination {
	border: 2px solid #aaa;
	border-radius: 4px;
	display: flex;
	list-style-type: none;
	overflow: hidden;
	padding: 0;
}

.pagination li {
	background-color: #fff;
}

.pagination li:hover,
.pagination li.active {
	background-color: #aaa;
}

.pagination li a {
	color: #555;
	display: block;
	font-weight: bold;
	padding: 10px 15px;
	text-decoration: none;
}

.pagination li:hover a,
.pagination li.active a {
	color: #fff;
}
```

需要注意的事项:

1.  这个`ul` / `.pagination`是一个 **flex** 容器——这是因为使用 flexbox 更容易将`li`放入其中(谁不喜欢 flexbox 呢？？)
2.  因为我们在`ul`上有一点点`border-radius`,我们需要添加`overflow: hidden`,因为否则`li`的角将在`ul`的边框顶部可见(移除溢出，你就会明白我的意思)
3.  我们在`li:hover`和`li.active`上有相同的样式，但是如果你愿意，你可以区分这些 to 状态

除此之外，我相信这很简单，但是如果你有任何问题，请让我知道。？

## 结论

网络上还有其他分页的变体。找一个自己喜欢的，转换成代码。

一定要和我分享你的成果！

编码快乐！？