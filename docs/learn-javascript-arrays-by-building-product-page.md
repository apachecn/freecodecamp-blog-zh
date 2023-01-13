# 通过构建 iPhone 产品页面了解如何使用 JavaScript 数组

> 原文：<https://www.freecodecamp.org/news/learn-javascript-arrays-by-building-product-page/>

我是在浏览 iPhone 官方网站时想到这个教程的。苹果以其优秀的产品和无可挑剔的设计而闻名，如果你花足够的时间浏览它的网站，你可以学到一两件关于品牌和优秀设计的事情。

在浏览 iPhone 13 产品页面时，引起我注意的一件事是用户可以从 3 或 4 种颜色中选择一种手机颜色。你只需点击相应颜色的按钮，手机的颜色就会改变。

当我写这篇文章时，我不知道苹果是如何做到这一点的——但是为了展示我对 JavaScript 数组的了解，我决定构建一个简单版本的 iPhone 产品页面。它有按钮，点击时可以改变手机的颜色。

在建立了页面之后(并且成功了),我意识到这可能不是苹果使用的技术。(我会在本教程的结论部分分享我为什么这么想。)不过，这仍然是一个有趣的项目，也是学习 JS 中数组的一种方式。

以下是我们将要介绍的内容:

*   如何设置 HTML
*   如何设置 CSS
*   如何设置 JavaScript

本教程假设您对 JavaScript、HTML 和 CSS 的 DOM 操作有所了解。如果你这样做了，很多事情会变得更有意义。

我也会，尽我所能详细解释每一段代码。说完这些，我们开始吧。

## 如何设置 HTML

开始之前，从网上下载 3 或 4 部不同颜色的 iPhones 图片。你可以在 iPhone 网站或手机评论网站上找到这样的图片。

确保你下载的图像有透明的背景，大小相同，种类相同，(也就是说，如果一个图像有后置摄像头显示，所有的图像都必须是这样的——但颜色不同，大小相同。).

一旦你有了你的图像，将它们保存在一个文件夹中，并将文件夹命名为 **images。至此，您应该已经为这个项目创建了一个主文件夹。如果你还没有，你现在可以这样做。你可以随意命名你的文件夹，但是要确保你之前创建的图片文件夹在主文件夹中。**

现在你的文件夹已经准备好了，是时候开始编码了。在您最喜欢的代码编辑器(我的是 VSCode)中，导航到您之前创建的主文件夹，并创建一个新的 HTML 文件。我给我的取名为 phone.html，但是你可以给你的取名为任何你喜欢的名字。

为了节省时间，我使用了 emmet 函数来生成 HTML 样板文件——只需按一个感叹号并按回车键。

在 body 标记中，创建一个主 div，并给它一个类“main ”,如下所示:

```
<div class="main">
</div>
```

在这个主 div 中，创建另一个 div，并给它一个 Id“phone ”,如下所示:

```
<div id="phone">
</div>
```

现在，在“phone”div 中，创建一个 h3 标签并输入:“我爱 iphone”或者您可以用表情符号替换“Love”。在我的例子中，我做了这样的事情:

`<h3>I &hearts; iPhones</h3>`

在 h3 标签下面，创建另一个 div，并给它一个 id“phoneshow ”,如下所示:

`<div id="phoneShow"></div>`

让这个 div 为空，但是在它下面创建另一个 div，并给它一个 Id“buttons”。在这个 div 中，创建 4 个 span 标记来表示 4 个按钮——也就是说，如果您下载了 4 个 iPhone 图像。

在每个 span 标记中，创建一个按钮标记，并根据电话图像的颜色为每个按钮赋予不同的 id。这里有一个例子:

```
<div id="buttons">
<span><button id="black"></button></span>
<span><button id="blue"></button></span>
<span><button id="pink"></button></span>
<span><button id="starlight"></button></span>
</div>
```

这样做之后，你就差不多完成了本教程的 HTML 部分。剩下的就是链接 CSS 和 JavaScript 文件。

如果你还没有创建 CSS 和 JavaScript 文件，现在是时候了。在我的例子中，我创建了 phone.css 和 phone.js 文件。接下来，在 head 标记中链接 CSS 文件，如下所示:

`<link rel ="stylesheet" href = "phone.css">`

现在，用以下代码将 JavaScript 文件链接到最后一个结束 div 标记的下方和结束 body 标记的上方:

`<script src = "phone.js"></script>`

完成这些之后，您的 HTML 代码就完成了。

## 如何设置 CSS

这个项目的 CSS 代码非常简单。样式化主体,“主”div,“电话”div,“电话秀”div,“按钮”div 和按钮，如下所示:

```
body{
display: flex;
flex-direction: column;
align-items: center;
justify-content: center;
background-color: rgb(255, 255, 255)
}

.main{
display: flex;
flex-direction: row;
flex-wrap: wrap;
align-items: center;
justify-content: center;
background-color: #ffff;
border-radius: 10px;
}

#phone{
display: flex;
flex-direction: column;
margin-bottom: 5px;
flex-wrap: nowrap;
align-items: center;
justify-content: center;
background-color: rgb(255, 255, 255);
border-radius: 10px;
}

#phoneShow{
display: flex;
flex-direction: row;
flex-wrap: wrap;
align-items: center;
justify-content: center;
background-color: #ffff;
border-radius: 10px;
}

#buttons{
display: flex;
flex-direction: row;
}

button{
margin-right: 10px;
border-radius: 50%;
padding: 15px;
border: none;
}
```

注意:`#buttons`与`buttons`不同。前者是一个 div，后者是一个按钮元素——因此它前面没有选择器。边界半径:50%；会使按钮完全变圆。

这个 CSS 代码使用了 Flexbox。

通过根据您下载的 iPhone 图像的颜色设计不同的 id，为每个按钮赋予不同的背景颜色。这里有一个例子:

```
#black{
background-color: black;
}
```

```
#blue{
background-color: blue;
}
```

```
#pink{
background-color: pink;
}
```

```
#starlight{
background-color: silver;
}
```

完成此操作后，您可以在浏览器中预览您的进度。您应该会在页面中央看到 4 个不同颜色的圆形按钮。

## 如何设置 JavaScript

这是本教程最重要的部分。到目前为止，您已经创建了页面的基本结构和样式。但是要在页面上显示和更改手机的图像，神奇之处就在这里。

首先，创建您在本教程开始时下载的图像目录的数组。还记得之前的图像文件夹吗？你需要做的是将文件夹中每张图片的相对路径以字符串形式存储在一个数组中。像这样:

```
const phoneImages = ["images/Black.png", "images/Blue.png", "images/Pink.png", "images/Starlight.png"]
```

(假设您的图像保存为 Black.png、Blue.png 等。)

接下来，获取将显示电话图像的 div 的 Id。对于本教程，电话将出现在前面的“phoneshow”div 中。将获得的 Id 存储在一个变量中，如下所示:

`let phoneCont = document.getElementById("phoneshow")`

接下来，获取所有按钮的 id 并将它们存储在变量中，下面是一个示例:

`let black=document.getElementById("black")`

`let blue=document.getElementById("blue")`

`let pink=document.getElementById("pink")`

`let starlight=document.getElementById("starlight")`

做完这些，就该做一个 iPhone 图像出现了。为此，创建一个名为“defaultImgItems”的变量，因为为了使页面正常工作，页面上应该有一个用户可以切换的默认图像。

您可以使用以下代码来实现这一点:

```
let defaultImgItems =`<img src= "${phoneImages.at(0)}">`
```

让我解释一下:

使用反斜线``使我们能够在 JavaScript 中插入 HTML 代码。在这种情况下，我希望在变量`defaultImgItems`中嵌入一个图像标签。source 是 phoneImages 数组中的第一项。您可以通过`at()`方法访问它。

要在选定的 div 中显示图像，只需使用下面的代码:

`phoneCont.innerHTML = defaultImgItems`

`phoneCont`是你之前存储 Id 为“phoneshow”的 div 的变量。如果在浏览器中刷新页面，应该会看到显示的第一个 iPhone 图片在`phoneImages`数组中。

完成后，为其他三种颜色创建类似的变量，如下所示:

```
let blueImgItems=`<img src= "${phoneImages.at(1)}">`
let pinkImgItems=`<img src= "${phoneImages.at(2)}">`
let starImgItems=`<img src= "${phoneImages.at(3)}">`
```

(这些变量用于`phoneImages`数组中的第二、第三和第四项。)

### 如何让按钮工作

如果你成功地做到了这一点，下一步就是让按钮正常工作。按钮应该能够改变 iPhone 的颜色，以相应的按钮颜色-也就是说，蓝色按钮应该显示一个蓝色的 iPhone，等等。

为此，将事件监听器附加到按钮上，并让它们更改 phoneCont 的`innerHTML`属性。像这样:

```
black.addEventListener("click", function(){
phoneCont.innerHTML=defaultImgItems
})
```

上面的代码将使黑色按钮，当点击显示一个黑色的 iPhone。剩余的代码片段如下:

```
blue.addEventListener("click", function(){
phoneCont.innerHTML = blueImgItems
})

pink.addEventListener("click", function(){
phoneCont.innerHTML = pinkImgItems
})

starlight.addEventListener("click", function(){
phoneCont.innerHTML = starImgItems
})
```

完成这些操作后，刷新浏览器，依次点击每个按钮。iPhone 图像应该随着每次点击而改变。

## 结论:

在本教程中，您学习了数组在现实项目中的实际应用。您还学习了如何使用？at()方法。

在本教程的开始，我提到我不认为苹果公司使用这种方法来建立他们的 iPhone 产品页面。这是因为当你加载用本教程创建的页面并依次点击每个按钮时，iPhone 图像不会平滑地变化——而是看起来跳跃。只有在所有按钮都被点击之后，当你点击一个新的按钮时，图像才会平滑地改变。尽管如此，我还是希望这篇教程对你有所帮助。

你可以在 Twitter 上与我联系，了解我正在做的事情的最新进展，或者我脑海中闪现的任何新想法，网址是 https://[www.twitter.com/lordsamdev](http://www.twitter.com/lordsamdev)。你也可以让我知道你对本教程中代码的看法——我欢迎你的想法。

感谢阅读！