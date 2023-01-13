# 最好的 jQuery 例子

> 原文：<https://www.freecodecamp.org/news/the-best-jquery-examples/>

jQuery 是使用最广泛的 JavaScript 库，超过一半的主要网站都使用它。它的座右铭是“少写，多做”...!"

jQuery 通过提供许多“助手”功能，使得 web 开发更容易使用。这些帮助开发人员快速编写 DOM(文档对象模型)交互，而不需要他们自己手动编写大量的 JavaScript。

jQuery 添加一个带有所有库方法的全局变量。命名约定是将这个全局变量命名为`$`。通过输入`$.`,您可以随意使用所有的 jQuery 方法。

## 入门指南

开始使用 jQuery 有两种主要方式:

*   **在本地包含 jQuery**:从 jquery.com[下载 jQuery 库](https://jquery.com/)，并将其包含在您的 HTML 代码中。
*   **使用 CDN** :使用 CDN(内容交付网络)链接到 jQuery 库。

```
<head>
  <script src="/jquery/jquery-3.4.1.min.js"></script>
  <script src="js/scripts.js"></script>
</head>
```

```
<head>
  <script src = "https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
  <script src="js/scripts.js"></script>
</head>
```

## 选择器

jQuery 使用 CSS 样式的选择器来选择 HTML 页面的部分或元素。然后，它让您使用 jQuery 方法或函数对元素做一些事情。

要使用其中一个选择器，请键入一个美元符号并在其后加上括号:`$()`。这是`jQuery()`函数的简写。在括号内，添加要选择的元素。您可以使用单引号或双引号。在这之后，在括号和您想要使用的方法后面添加一个点。

在 jQuery 中，类和 ID 选择器类似于 CSS 中的选择器。

下面是一个 jQuery 方法的示例，它选择所有段落元素，并向它们添加一个“selected”类:

```
<p>This is a paragraph selected by a jQuery method.</p>
<p>This is also a paragraph selected by a jQuery method.</p>

$("p").addClass("selected");
```

在 jQuery 中，类和 ID 选择器与 CSS 中的相同。如果您想要选择具有特定类别的元素，请使用点(`.`)和类别名称。如果您想要选择具有特定 ID 的元素，请使用散列符号(`#`)和 ID 名称。请注意，HTML 不区分大小写，因此保持 HTML 标记和 CSS 选择器小写是最佳实践。

### 按类别选择

如果您想要选择具有特定类别的元素，请使用点(。)和类名。

```
<p class="pWithClass">Paragraph with a class.</p>
```

```
$(".pWithClass").css("color", "blue"); // colors the text blue
```

为了更加具体，还可以将类选择器与标记名结合使用。

```
<ul class="wishList">My Wish List</ul>`<br>
```

```
$("ul.wishList").append("<li>New blender</li>");
```

### 按 ID 选择

如果要选择具有特定 ID 值的元素，请使用散列符号(#)和 ID 名称。

```
<li id="liWithID">List item with an ID.</li>
```

```
$("#liWithID").replaceWith("<p>Socks</p>");
```

与类选择器一样，这也可以与标记名结合使用。

```
<h1 id="headline">News Headline</h1>
```

```
$("h1#headline").css("font-size", "2em");
```

### 按属性值选择

如果您想选择具有特定属性的元素，请使用`([attributeName="value"])`。

```
<input name="myInput" />
```

```
$("[name='myInput']").value("Test"); // sets input value to "Test"
```

您还可以将属性选择器与标记名称结合使用，以使其更加具体。

```
<input name="myElement" />`<br>
<button name="myElement">Button</button>
```

```
$("input[name='myElement']").remove(); // removes the input field not the button
```

### 充当过滤器的选择器

还有充当过滤器的选择器——它们通常以冒号开头。例如，`:first`选择器选择的元素是其父元素的第一个子元素。下面是一个包含一些列表项的无序列表的例子。列表下方的 jQuery 选择器选择列表中的第一个`<li>`元素——“一”列表项，然后使用`.css`方法将文本变成绿色。

```
 <ul>
      <li>One</li>
      <li>Two</li>
      <li>Three</li>
   </ul>
```

```
$("li:first").css("color", "green");
```

### 属性选择器

有一些选择器返回匹配某些属性组合的元素，如*属性包含*、*属性以*结尾、*属性以*开头等。下面是一个包含一些列表项的无序列表的例子。列表下面的 jQuery 选择器选择列表中的`<li>`元素——“一”列表项，因为它的值是`"India"`的`data*`属性——然后使用`.css`方法将文本变成绿色。

```
 <ul>
      <li data-country="India">Mumbai</li>
      <li data-country="China">Beijing</li>
      <li data-country="United States">New York</li>
   </ul>
```

```
$("li[data-country='India']").css("color", "green");
```

另一个过滤选择器`:contains(text)`，选择具有特定文本的元素。将您想要匹配的文本放在括号中。这里有一个两个段落的例子。jQuery 选择器将单词“Moto”的颜色改为黄色。

```
 <p>Hello</p>
    <p>World</p>
```

```
$("p:contains('World')").css("color", "yellow");
```

类似地，`:last`选择器选择作为其父元素最后一个子元素的元素。下面的 jQuery 选择器选择列表中的最后一个`<li>`元素——“三”列表项，然后使用`.css`方法将文本变成黄色。

`$("li:last").css("color", "yellow");`

****注意:**** 在 jQuery 选择器中，`World`在单引号中，因为它已经在一对双引号中。始终在双引号内使用单引号，以避免无意中结束字符串。

### 多重选择器

在 jQuery 中，您可以使用多个选择器，通过一行代码将相同的更改应用于多个元素。您可以通过用逗号分隔不同的 id 来做到这一点。例如，如果您想将 id 分别为 cat、dog 和 rat 的三个元素的背景色设置为红色，只需执行以下操作:

```
$("#cat,#dog,#rat").css("background-color","red");
```

## HTML 方法

jQuery `.html()`方法获取 HTML 元素的内容或者设置 HTML 元素的内容。

### 获得

要返回 HTML 元素的内容，请使用以下语法:

```
$('selector').html();
```

例如:

```
$('#example').html();
```

### 环境

要设置 HTML 元素的内容，请使用以下语法:

```
$('selector').html(content);
```

例如:

```
$('p').html('Hello World!');
```

这将把所有`<p>`元素的内容设置为 Hello World！

### 警告

`.html()`方法用于设置 ****HTML**** 格式元素的内容。如果内容是由用户提供的，这可能是危险的。如果需要将非 HTML 字符串设置为内容，可以考虑使用`.text()`方法。

## CSS 方法

jQuery `.css()`方法获取匹配元素集中第一个元素的计算样式属性值，或者为每个匹配元素设置一个或多个 CSS 属性。

### 获得

要返回指定 CSS 属性的值，请使用以下语法:

```
 $(selector).css(propertyName);
```

示例:

```
 $('#element').css('background');
```

注意:这里我们可以使用任何 css 选择器，例如:元素(HTML 标签选择器)。element(类选择器)，#element(ID 选择器)。

### 环境

要设置指定的 CSS 属性，请使用以下语法:

```
 $(selector).css(propertyName,value);
```

示例:

```
 $('#element').css('background','red');
```

要设置多个 CSS 属性，您必须使用如下的对象文字语法:

```
 $('#element').css({
        'background': 'gray',
        'color': 'white'
    });
```

如果要更改标有多个单词的属性，请参考以下示例:

要更改元素的`background-color`

```
 $('#element').css('background-color', 'gray');
```

## 点击方法

当点击一个元素时，jQuery Click 方法触发一个函数。该函数被称为“处理程序”，因为它处理单击事件。函数可以影响使用 jQuery click 方法绑定到 Click 的 HTML 元素，也可以完全改变其他东西。最常用的形式是:

```
$("#clickMe").click(handler)
```

click 方法将处理函数作为参数，并在每次单击元素`#clickMe`时执行它。处理函数接收一个名为 [eventObject](http://api.jquery.com/Types/#Event) 的参数，这个参数对控制动作很有用。

### 例子

当用户单击按钮时，此代码显示一个警告:

```
<button id="alert">Click Here</button>
```

```
$("#alert").click(function () {
  alert("Hi! I'm an alert");
});
```

[eventObject](http://api.jquery.com/Types/#Event) 有一些内置的方法，包括`preventDefault()`，正如它所说的那样——停止元素的默认事件。这里我们避免锚定标签充当链接:

```
<a id="myLink" href="www.google.com">Link to Google</a>
```

```
$("#myLink").click(function (event) {
  event.preventDefault();
});
```

### 点击法的更多玩法

处理函数也可以接受对象形式的附加数据:

```
jqueryElement.click(usefulInfo, handler)
```

数据可以是任何类型。

```
$("element").click({firstWord: "Hello", secondWord: "World"}, function(event){
    alert(event.data.firstWord);
    alert(event.data.secondWord);
});
```

在没有处理函数的情况下调用 click 方法会触发 click 事件:

```
$("#alert").click(function () {
  alert("Hi! I'm an alert");
});

$("#alert").click();
```

现在，每当页面加载时，当我们进入或重新加载页面时，click 事件将被触发，并显示分配的警报。

此外，您应该更喜欢使用`.on("click",...)`而不是`.click(...)`，因为前者可以使用更少的内存，并且可以处理动态添加的元素。

### 常见错误

click 事件在绑定时只绑定到当前在 DOM 中的元素，因此任何后来添加的元素都不会被绑定。要绑定 DOM 中的所有元素，即使它们将在以后创建，也要使用`.on()`方法。

例如，这个 click 方法示例:

```
$("element").click(function() {
  alert("I've been clicked!");
});
```

可在方法示例中更改为:

```
$(document).on("click", "element", function() {
  alert("I've been clicked!");
});
```

### 从单击事件中获取元素

这适用于 jQuery 和普通 JavaScript，但是如果您将事件触发器设置为针对某个类，那么您可以通过使用`this`关键字来获取触发该元素的特定元素。

jQuery 使得遍历 DOM 来查找元素的父元素、兄弟元素和子元素变得非常容易(并且对多浏览器友好)。

假设我有一个满是按钮的表格，我想定位按钮所在的行，我可以简单地将`this`包装在一个 jQuery 选择器中，然后获取它的`parent`和它的父节点的`parent`，如下所示:

```
$( document ).on("click", ".myCustomBtnClassInATable", function () {
    var myTableCell = $(this).parent();
    var myTableRow = myTableCell.parent();
    var myTableBody = myTableRow.parent();
    var myTable = myTableBody.parent();

    //you can also chain these all together to get what you want in one line
    var myTableBody = $(this).parent().parent().parent();
});
```

查看 click 事件的事件数据也很有趣，您可以通过向 click 事件中的函数传递任何变量名来获取这些数据。在大多数情况下，您很可能会看到一个`e`或`event`:

```
$( document ).on("click", ".myCustomBtnClassInATable", function (e) { 
    //find out more information about the event variable in the console
    console.log(e);
});
```

## 鼠标按下方法

当按下鼠标左键时发生 mousedown 事件。要触发所选元素的 mousedown 事件，请使用以下语法:`$(selector).mousedown();`

但是，大多数情况下，mousedown 方法与附加到 mousedown 事件的函数一起使用。语法如下:`$(selector).mousedown(function);`例如:

```
$(#example).mousedown(function(){
   alert("Example was clicked");
});
```

当单击#example 时，该代码将发出页面警告“example is clicked”。

## 悬停方法

jquery hover 方法是`mouseenter`和`mouseleave`事件的组合。语法是这样的:

```
$(selector).hover(inFunction, outFunction);
```

第一个函数 inFunction 将在`mouseenter`事件发生时运行。第二个函数是可选的，但是将在`mouseleave`事件发生时运行。如果只指定了一个函数，另一个函数将为`mouseenter`和`mouseleave`事件运行。下面是一个更具体的例子。

```
$("p").hover(function(){
    $(this).css("background-color", "yellow");
}, function(){
    $(this).css("background-color", "pink");
});
```

因此，这意味着悬停在段落上将改变其背景颜色为黄色，相反的将变回粉红色。

## 动画方法

jQuery 的 animate 方法使得只使用几行代码创建简单的动画变得很容易。基本结构如下:

```
$(".selector").animate(properties, duration, callbackFunction());
```

对于`properties`参数，您需要传递一个 javascript 对象，其中包含您希望动画化为键的 CSS 属性和您希望动画化为值的值。对于`duration`，您需要输入动画应该花费的时间，以毫秒为单位。动画完成后，就会执行`callbackFunction()`。

### 例子

一个简单的例子是这样的:

```
$(".awesome-animation").animate({
	opacity: 1,
	bottom: += 15
}, 1000, function() {
	$(".different-element").hide();
});
```

## 隐藏方法

****最简单的形式。hide()**** 立即隐藏匹配的元素，没有动画。例如:

```
$(".myclass").hide()
```

将隐藏所有类为 *myclass* 的元素。可以使用任何 jQuery 选择器。

### 。hide()作为动画方法

得益于它的选项， ****。hide()**** 可以同时动画显示匹配元素的宽度、高度和不透明度。

*   持续时间可以以毫秒为单位，或者使用慢(600 毫秒)和快(200 毫秒)两种文字。例如:
*   可以指定在动画完成后调用一个函数，每个匹配的元素调用一次。这个回调主要用于链接不同的动画。例如

```
$("#myobject").hide(800)
```

```
$("p").hide( "slow", function() {
  $(".titles").hide("fast");
  alert("No more text!");
});
```

## 显示方法

****最简单的形式。show()**** 立即显示匹配的元素，没有动画。例如:

```
$(".myclass").show();
```

将显示所有类为 *myclass* 的元素。可以使用任何 jQuery 选择器。

但是，这个方法不会覆盖 CSS 样式中的`!important`，比如`display: none !important`。

### 。将()显示为动画方法

得益于它的选项， ****。show()**** 可以同时动画显示匹配元素的宽度、高度和不透明度。

*   持续时间可以以毫秒为单位，或者使用慢(600 毫秒)和快(200 毫秒)两种文字。例如:
*   可以指定在动画完成后调用一个函数，每个匹配的元素调用一次。例如

```
$("#myobject").show("slow");
```

```
$("#title").show( "slow", function() {
  $("p").show("fast");
});
```

## jQuery 切换方法

要显示/隐藏元素，您可以使用`toggle()`方法。如果元素被隐藏`toggle()`将显示它，反之亦然。用法:

```
$(".myclass").toggle()
```

## 下滑法

这个方法可以显示匹配元素的高度。这导致页面的下部向下滑动，为显示的项目让路。用法:

```
$(".myclass").slideDown(); //will expand the element with the identifier myclass for 400 ms.
$(".myclass").slideDown(1000); //will expand the element with the identifier myclass for 1000 ms.
$(".myclass").slideDown("slow"); //will expand the element with the identifier myclass for 600 ms.
$(".myclass").slideDown("fast"); //will expand the element with the identifier myclass for 200 ms.
```

## 装载方法

load()方法从服务器加载数据，并将返回的数据放入选定的元素中。

****注意:**** 还有一种 jQuery 事件方法叫做 load。哪一个被调用，取决于参数。

### 例子

```
$("button").click(function(){
    $("#div1").load("demo_test.txt");
});
```

## 链接

jQuery 链接允许您在同一 jQuery 选择上执行多个方法，所有这些都在一行中。

链接允许我们将多行语句:

```
$('#someElement').removeClass('classA');
$('#someElement').addClass('classB');
```

变成一条语句:

```
$('#someElement').removeClass('classA').addClass('classB');
```

## Ajax Get 方法

发送异步 http GET 请求以从服务器加载数据。它的一般形式是:

```
jQuery.get( url [, data ] [, success ] [, dataType ] )
```

*   `url`:唯一的强制参数。该字符串包含向其发送请求的地址。如果没有指定其他参数，返回的数据将被忽略。
*   `data`:随请求发送给服务器的普通对象或字符串。
*   `success`:请求成功时执行的回调函数。它将返回的数据作为参数。还会向它传递响应的文本状态。
*   `dataType`:服务器预期的数据类型。默认为智能猜测(xml、json、脚本、文本、html)。如果提供了此参数，还必须提供成功回调。

### 例子

向服务器请求`resource.json`，发送额外的数据，并忽略返回的结果:

```
$.get('http://example.com/resource.json', {category:'client', type:'premium'});
```

向服务器请求`resource.json`，发送附加数据，并处理返回的响应(json 格式):

```
$.get('http://example.com/resource.json', {category:'client', type:'premium'}, function(response) {
     alert("success");
     $("#mypar").html(response.amount);
});
```

然而，`$.get`没有提供任何处理错误的方法。

上面的例子(带有错误处理)也可以写成:

```
$.get('http://example.com/resource.json', {category:'client', type:'premium'})
     .done(function(response) {
           alert("success");
           $("#mypar").html(response.amount);
     })
     .fail(function(error) {
           alert("error");
           $("#mypar").html(error.statusText);
     });
```

### Ajax GET 等价

`$.get( url [, data ] [, success ] [, dataType ] )`是一个简写的 Ajax 函数，相当于:

```
$.ajax({
     url: url,
     data: data,
     success: success,
     dataType: dataType
});
```

## Ajax Post 方法

发送异步 http POST 请求以从服务器加载数据。它的一般形式是:

```
jQuery.post( url [, data ] [, success ] [, dataType ] )
```

*   url:这是唯一的强制参数。该字符串包含请求要发送到的地址。如果没有指定其他参数，返回的数据将被忽略
*   数据:随请求发送到服务器的普通对象或字符串。
*   成功:请求成功时执行的回调函数。它将返回的数据作为参数。还会向它传递响应的文本状态。
*   数据类型:服务器预期的数据类型。默认为智能猜测(xml、json、脚本、文本、html)。如果提供了此参数，那么也必须提供成功回调。

#### 例子

```
$.post('http://example.com/form.php', {category:'client', type:'premium'});
```

从服务器请求`form.php`，发送额外的数据并忽略返回的结果

```
$.post('http://example.com/form.php', {category:'client', type:'premium'}, function(response){ 
      alert("success");
      $("#mypar").html(response.amount);
});
```

从服务器请求`form.php`，发送额外的数据并处理返回的响应(json 格式)。这个例子可以写成这样的格式:

```
$.post('http://example.com/form.php', {category:'client', type:'premium'}).done(function(response){
      alert("success");
      $("#mypar").html(response.amount);
});
```

下面的示例使用 Ajax 发布一个表单，并将结果放入一个 div 中

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>jQuery.post demo</title>
  <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
</head>
<body>

<form action="/" id="searchForm">
  <input type="text" name="s" placeholder="Search...">
  <input type="submit" value="Search">
</form>
<!-- the result of the search will be rendered inside this div -->
<div id="result"></div>

<script>
// Attach a submit handler to the form
$( "#searchForm" ).submit(function( event ) {

  // Stop form from submitting normally
  event.preventDefault();

  // Get some values from elements on the page:
  var $form = $( this ),
    term = $form.find( "input[name='s']" ).val(),
    url = $form.attr( "action" );

  // Send the data using post
  var posting = $.post( url, { s: term } );

  // Put the results in a div
  posting.done(function( data ) {
    var content = $( data ).find( "#content" );
    $( "#result" ).empty().append( content );
  });
});
</script>

</body>
</html>
```

以下示例使用 github api 通过 jQuery.ajax()获取用户的存储库列表，并将结果放入一个 div 中

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>jQuery Get demo</title>
  <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
</head>
<body>

<form id="userForm">
  <input type="text" name="username" placeholder="Enter gitHub User name">
  <input type="submit" value="Search">
</form>
<!-- the result of the search will be rendered inside this div -->
<div id="result"></div>

<script>
// Attach a submit handler to the form
$( "#userForm" ).submit(function( event ) {

  // Stop form from submitting normally
  event.preventDefault();

  // Get some values from elements on the page:
  var $form = $( this ),
    username = $form.find( "input[name='username']" ).val(),
    url = "https://api.github.com/users/"+username+"/repos";

  // Send the data using post
  var posting = $.post( url, { s: term } );

  //Ajax Function to send a get request
  $.ajax({
    type: "GET",
    url: url,
    dataType:"jsonp"
    success: function(response){
        //if request if made successfully then the response represent the data

        $( "#result" ).empty().append( response );
    }
  });

});
</script>

</body>
</html>
```

### Ajax POST 等价物

`$.post( url [, data ] [, success ] [, dataType ] )`是一个简写的 Ajax 函数，相当于:

```
$.ajax({
  type: "POST",
  url: url,
  data: data,
  success: success,
  dataType: dataType
});
```