# JQuery Ajax POST 方法

> 原文：<https://www.freecodecamp.org/news/jquery-ajax-post-method/>

发送异步 http POST 请求以从服务器加载数据。它的一般形式是:

```
jQuery.post( url [, data ] [, success ] [, dataType ] )
```

*   url:是唯一的强制参数。该字符串包含请求要发送到的地址。如果没有指定其他参数，返回的数据将被忽略
*   数据:随请求发送到服务器的普通对象或字符串。
*   success:请求成功时执行的回调函数，它将返回的数据作为参数。还会向它传递响应的文本状态。
*   数据类型:服务器预期的数据类型。默认为智能猜测(xml、json、脚本、文本、html)。如果提供了此参数，那么也必须提供成功回调。

#### **例题**

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

### **jQuery.ajax()**

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

`$.ajax()`提供更多可以找到的选项[这里](http://api.jquery.com/jquery.ajax/)

#### **更多信息:**

更多信息请访问[官网](https://api.jquery.com/jquery.post/)