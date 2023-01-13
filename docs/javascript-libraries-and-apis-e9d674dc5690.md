# JavaScript 库和 API

> 原文：<https://www.freecodecamp.org/news/javascript-libraries-and-apis-e9d674dc5690/>

作者:亚当·雷夫洛赫

# API 就像一盒巧克力

![1*Co2qNTU1Es6kAf3b9MSOqw](img/495804ef995ebfe2bb3fe95da0b857f2.png)

如果您以前为 DOM 编写过 JavaScript，那么您可能知道它会变得多么笨拙。我的意思是 *getElementById* 有 7 个音节和 14 个字符长。没有更简单的方法吗？

输入 jQuery goodness。jQuery 是一个 JavaScript 库。

图书馆？就像那些我可以免费借书和租 90 年代电影的地方？不会。从编程的角度来说，库是一个简化编码逻辑的代码库，任何人都可以免费使用。没有滞纳金！

你可能不明白，但我有例子。

假设一下，假设我想把一个 *div* 的背景改成红色。为了简单起见，我给那个 *div* 一个 *id* 为‘红色’。在 JavaScript 中，我会这样写:

```
document.body.getElementById(‘red’).style.backgroundColor = ‘red’;
```

它工作并完成任务，这是我们真正需要的。但是现在让我们看看 jQuery 中的相同功能。

```
$(‘#red’).css(‘background-color’, ‘red’);
```

哇，真漂亮！这是程序员努力追求的一切:简洁明了的解决方案。

现在您清楚地看到了 jQuery 的好处。它旨在使 JavaScript 编程更容易。

但是也有一些缺点。你可能认为 jQuery 无所不能，用 jQuery 编程和用 JavaScript 编程是一样的。这两种观点都是错误的。

从我的角度来看，使用 jQuery 就像使用 Rails，这是 Ruby on Rails 的 web 开发框架的一半。您可以在不深入学习 Ruby 的情况下使用 Rails。

然而，我的观点是，除非你主要用它来运行你的程序，否则你不可能真正理解或欣赏 JavaScript。也就是说，使用 jQuery 有一些真正的好处供您探索。我们今天要讨论的好处是使用 jQuery 进行 API 调用。

API 哈？API 代表应用程序编程接口。这是“交战规则”的一个花哨的缩写。

许多软件应用程序都有一套规则来控制它们如何与其他程序交互。我相信这对你来说是完全有意义的，但我会给你一些例子来衡量。

例如，如果您想在 HTML 页面的正文中添加一个段落，您可以编写如下内容:

```
var paragraph = document.body.createElement(‘P’);paragraph.textContent = ‘I am using an API, woohoo!’;document.body.appendChild(paragraph);
```

在上面的例子中，我使用了以下规则:

```
1\. document.body to access the body object 2\. createElement method to create an element3\. textContent method to insert text into that element4\. appendChild method to append that paragraph to the body
```

这里我们将使用 DOM API。DOM 代表文档对象模型，它是页面上 HTML 元素的组织。

用外行人的话来说，DOM API“提供了一个网站的结构化表示”。这成为允许编程语言与页面的 HTML 交互的入口点。

DOM API 提供了诸如 *createElement* 和 *textContent* 之类的方法，这样你就可以使用你喜欢的 JavaScript 来操作 DOM。

你注意到我说的编程语言了吗？实际上，你可以使用任何语言进行交互，但你可能想使用网络的实际语言——JavaScript。我只是说说。

现在你有了一些背景，让我们开始制作一个 Giphy 搜索应用程序。

这会很有趣的！这次我是认真的。

我们将从查看一个*响应*对象开始。一个*响应*对象是向 API 发出请求后返回的数据。

好吧，让我来解释一下。当你在搜索栏中输入一个 URL 比如 eyeluvkats.com——并按回车键后，一个请求就会通过互联网发送到托管 eyeluvkats.com 域名的服务器上。

然后，向用户发送一个包含网站内容的响应:HTML、CSS、JavaScript、图像、视频等。

现在，我们不再请求 eyeluvkats.com，而是向 Giphy API 发出一个请求，返回一个数据对象。该请求将是:

[http://API . giphy . com/v1/gif/random？api_key=dc6zaTOxFJmzC &标签=cat](http://api.giphy.com/v1/gifs/random?api_key=dc6zaTOxFJmzC&tag=cat)

您的第一个 API 调用！呜哇！

这里调用了 Giphy API，在 URL 的参数中，我们请求了一种特定类型的 gif:

gifs/ **随机**？API _ key = DC 6 zatoxfjmzc&tag = cat。

我们使用 random 参数和 cat 的 tag 参数请求了一个随机的 cat gif。我就是这样的千禧一代，对不起！您现在应该在浏览器中看到一个 JSON 对象，其中包含与一个随机的 cat gif 相关的数据。

JSON 代表 JavaScript 对象符号。这就是 JavaScript 在 web 上发送数据的方式，通常被用作大型 API 数据集的响应对象。你以前创建过 JSON 对象吗？没有吗？现在让我们开始吧，这样你就知道对象在 JavaScript 中是如何工作的了。

要创建一个 JavaScript 对象，也称为对象文字，可以声明一个变量等于一对花括号。例如:

```
var object = {};
```

这些对象通常用于存储数据，所以让我们继续将您的简历放入其中。对象中的数据存储为键/值对，即*名:【Adam】*，用逗号分隔。

现在开始，添加您的其他详细信息，如您的生日、社会保险号和信用卡号。完成后，把那个东西送到 adam@youjustgotscammed.com。

对我来说，我的信息应该是这样的:

```
var me = {    firstName: ‘Adam’,    middleName: ‘Elliott’,    lastName: ‘Recvlohe’,    favoriteFood: ‘Chipotle’,    favoriteDrink: ‘Kombucha: Gingerade’,    race: ‘Human’,    favoriteVideo: 'https://www.youtube.com/watch?v=dQw4w9WgXcQ'}
```

这很好，但是我们如何获取这些信息呢？嗯，有几种方法可以做到。一种方法是用“点符号”，另一种是用“括号符号”。

```
var me = {    firstName: ‘Adam’,    middleName: ‘Elliott’,    lastName: ‘Recvlohe’,    favoriteFood: ‘Mexican’,    favoriteDrink: ‘Kombucha’,    race: ‘Human’}// Dot Notationvar myFirstName = me.firstName;// Bracket Notationvar myLastName = me[‘lastName’];
```

记住，当对象嵌套得越多，你需要的点或括号就越多。

```
var data = {    address: {        city: ‘Tampa’,        state: ‘Florida’    }}var myCity = data.address.cityvar myState = data[‘address’][‘state’];
```

注:JSON 标准要求对象属性和值用双引号括起来。我向您展示了 JavaScript 对象文字，因为您从每个对象访问数据的方式都是相同的，JSON 的语法只是略有不同。

现在我们已经解决了这个问题，我们可以开始处理 JSON 对象中的数据了，这是我们通过访问 Giphy API 得到的。对象相当大，但我真正想要的是随机猫 gif 的 URL。我该如何将 URL 赋给一个变量？既然你还在这里，我就告诉你我会怎么做:

```
var gifURL = object.data.url;
```

现在这没有任何作用，但是我想首先向你们展示的是我们将如何处理数据，然后在以后操纵它。

我们现在已经有了处理响应对象的基础。让我们继续创建一个函数来调用这些数据，然后在屏幕上操作这些数据。

到目前为止，我们并不真正需要 CodePen，但我们现在需要它。如果你想继续跟进，我建议你开一个 [codepen.io](http://codepen.io) 账户，然后创建一个新的笔。

随着你的笔打开，我们需要先做一些事情来设置场景。我们需要将 jQuery 库添加到我们的笔中。在 JavaScript 选项卡中，在 JavaScript 的左侧，您应该会看到一个齿轮图标。那是设置按钮。点击这里，接下来应该打开的是您的 JavaScript 设置。在**添加外部 JavaScript** 下面有一个叫做**快速添加**的下拉框。选择 jQuery。好了，现在我们可以走了。

我们希望何时执行对 Giphy API 的调用？按照惯例，这通常是在文件准备好的时候完成的。您可能永远也不会想到我们会在 jQuery 中使用 *ready* 方法。它看起来有点像这样:

```
$(document).ready(function() {})
```

在 JavaScript 中，内容加载有几个不同的阶段。还有*互动*、*完成*，还有其他几个。这就是说，当 DOM 准备好时，这个函数将被执行。

试试看！在 *ready* 回调函数中用一些独特的东西刷新浏览器，比如 console.log('Hello，World！').

```
$(document).ready(function() {    console.log(‘Hello, World!’);})
```

如果你看看你的控制台，你应该看到打印的*你好，世界！*。

在你的代码笔窗口的底部，你应该会看到**控制台。**点击那个按钮，你应该会看到控制台打开。

jQuery 附带了许多其他方便的函数。下一个我们需要的是 *getJSON* 方法。这将在 JSON 中向我们返回一个响应，并给我们一个回调函数，然后用该数据做一些事情。这听起来可能有点武断。我举个具体的例子。

```
$(document).ready(function() {  var url = 'http://api.giphy.com/v1/gifs/random?api_key=dc6zaTOxFJmzC&tag=cat';      $.getJSON(url, function(object) {    console.log(object);  });});
```

我打赌你猜不出这是干什么的？好吧，你证明我错了。当然，您知道方法 *getJSON* 向 *url* 发送请求，并返回数据，然后记录在控制台中。你这个狡猾的家伙！

这很好，但我们可以做得更好。我们可以获取该对象，并使用 *url* 属性在 DOM 中放置一个 gif。头脑…炸了…轰！

我们将坚持使用 jQuery 并动态创建图像。要在 jQuery 中创建图像，我们可以将图像标签作为包含所有必要信息的字符串传递，如下所示:

```
var imageSource = object.data.image_original_url;var image = $('<img src=' + imageSource + ' />');
```

现在我们所要做的就是把它附加到 DOM 的主体中，这样就好了。休斯顿，所有系统启动！

```
$(document).ready(function() {  var url = 'http://api.giphy.com/v1/gifs/random?api_key=dc6zaTOxFJmzC&tag=cat';      $.getJSON(url, function(object) {    var imageSource = object.data.image_original_url;    var image = $('<img src=' + imageSource + ' />');    image.appendTo($('body'));  });});
```

这是 codepen 上的:

> 我妈妈总是说，“生活就像一个应用程序。你永远不知道你会得到什么。”— JS 阿甘

现在，每次你刷新页面，你应该得到一个新的猫 gif。不客气！

我们可以添加更多的功能。我们可以搜索我们想要的 gif，并在页面上显示一些，而不是每次只随机得到一只猫。听起来很酷。

在此之前，我们需要一个输入字段。输入字段将获取您的搜索词，并将其作为参数添加到发送给 Giphy API 的请求中。在您的 HTML 中添加一个输入字段。

```
<input type=‘text’ />
```

到目前为止，我们一直在调用“random”API。这一次，我们要搜索。不知道那是什么 API？哦，我明白了，这叫‘搜索’。在 Giphy 的主页上，我们有一个很好的示例 URL:

[http://api.giphy.com/v1/gifs/search?q=funny+cat&API _ key = DC 6 zatoxfjmzc](http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC)

这是一个小盒子价值的数据！默认情况下，Giphy 将返回 25 个结果。正如你在 URL 中看到的，搜索这些项目的方式是使用 *q* 参数，在这种情况下等于*滑稽+猫*。如果我们想要搜索一些其他的术语，那么我们所要做的就是从用户那里获取输入，当他们点击 enter 时，一个新的请求被发送到 API。让我们现在做那件事。

为了捕获用户的输入，我们需要对输入使用 jQuery *val* 方法。

```
$(document).ready(function() {   var value = $('input').val();});
```

但是我们实际上是如何得到这个值的呢？我们需要添加一个事件侦听器，以便当用户点击 enter 时，我们可以捕获该值并将其发送给 *getJSON* 方法。在 jQuery 中，事件监听器也更短。我们将监听*按键*事件，但只监听 enter 键，它被指定为 13。请记住，事件侦听器提供一个回调函数，从 DOM 传入事件。不过，还有一个小问题。我们返回的值将是一个字符串，这意味着每个单词之间不会有“+”运算符。因为 jQuery 是用 JavaScript 编写的，这允许我们在脚本中使用普通的 JavaScript 方法。我将使用*替换*的方法，用“+”操作符替换掉空格。

```
$(document).ready(function() {  $(’input’).keypress(function(event) {    if(event.which == 13) {      var value = $(’input’).val();      value = value.trim().replace(/\s+/g, '+’);      console.log(value);      event.preventDefault();    }         }); });
```

JavaScript 内置的 *replace* 方法有两个参数:一个正则表达式和您希望用什么来替换匹配。在我的正则表达式中，我使用了一个特殊字符， *\s* 。这表示空白。我在末尾添加了一个'+'操作符，表示我想捕获字符串中任意数量的空格， *\s+* 。我的想法是，用户可能会在单词之间放置多个空格，如果出现这种情况，我想纠正这个错误。

这是一个非常原始的解决方案，但对我们的目的来说，它是可行的。如果您在控制台中查看，您应该看到您的字符串文本与'+'操作符结合在一起。很好，我们记下了。现在我们可以创建一个搜索 API 的 URL 和我们要搜索的值。

```
$(document).ready(function() {  $(’input’).keypress(function(event) {    if(event.which == 13) {      var value = $(’input’).val();      value = value.trim().replace(/\s+/g, '+’);      var url = 'http://api.giphy.com/v1/gifs/search?api_key=dc6zaTOxFJmzC&q=' + value;      console.log(url);      event.preventDefault();    }         }); });
```

在您的控制台中，您应该会看到我们发送给 Giphy API 的新 URL。接下来，我们实际上可以使用 *getJSON* 发出调用。

```
$(document).ready(function() {  $(’input’).keypress(function(event) {    if(event.which == 13) {      var value = $(’input’).val();      value = value.trim().replace(/\s+/g, '+’);      var url = 'http://api.giphy.com/v1/gifs/search?api_key=dc6zaTOxFJmzC&q=' + value;      $.getJSON(url, function(object) {        console.log(object);      });    event.preventDefault();    }         }); });
```

JSON 响应对象将会非常大，你将无法使用 CodePen 控制台看到它。相反，在右上角显示**改变视图的地方，**点击并向下滚动到**调试模式**。一个新窗口将会打开。右击新窗口，向下滚动到**检查**。屏幕右侧或底部的面板应该会显示。现在，点击显示**控制台**的标签。如果您刷新浏览器，在输入字段中输入一些单词并单击 enter，您应该会看到响应对象显示在日志中。

现在，如果你在输入中输入一些单词，你应该会得到一个毛球大小的物体，由 25 个物体组成。好了，我们有数据了，但它仍然没有显示在屏幕上。我们需要做的是遍历数组，获取每个图片 URL，然后为每个图片创建一个 *img* 标签，并将每个图片添加到 DOM 中。很简单吧？好吧，我们走一遍。

```
object.data.forEach(function(gif) {  var url = gif.images.original.url;  var image = $('<img src=' + url + ' />');  image.appendTo($('body'));});
```

*forEach* 方法，一个普通的 JavaScript 方法，将遍历所有数组，我们将每个数组对象赋给 *gif* 变量。在每个 *gif* 对象中是 *images.original.url* 属性，这是我们需要设置每个图像标签的 *src* 属性的 url。

有了这些数据之后，我们现在可以创建 *img* 标签，将 URL 分配给 *src* 属性。然后，我们将该图像附加到 DOM 中。

吧嗒吧，吧嗒吧，你现在指尖上有无穷无尽的 gif 祝福。

完成的投影现在看起来像这样:

这是 CodePen 上的:

您可能不认为访问和使用 API 是如此容易。但是正如你所看到的，从一行小小的代码中，你可以动态地将你的网站从一个漂亮的随机猫图像应用程序变成一个更强大的 Giphy 搜索应用程序。

你也可以看出，这些款式看起来很悲伤。利用你到目前为止在 web 开发中学到的东西，用你的附加风格把它变成一个很棒的应用程序。要有创造性，但更重要的是要有乐趣！

附注:我们使用的 API 密钥仅用于开发目的，不用于生产。这意味着您不能疯狂地对 Giphy API 进行一百万次调用。他们会让你关门大吉。

我希望您又一次愉快地学习了 JavaScript 的奇妙世界。直到下一次，愿你和 APIs 形影不离！