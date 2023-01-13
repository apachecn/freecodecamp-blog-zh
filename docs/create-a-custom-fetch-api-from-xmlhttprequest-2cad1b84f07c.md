# 如何从 XMLHttpRequest 创建自定义 fetch API

> 原文：<https://www.freecodecamp.org/news/create-a-custom-fetch-api-from-xmlhttprequest-2cad1b84f07c/>

你最糟糕的噩梦是什么？

这听起来很黑暗，但这不是一个反问句。我真的很想知道，因为我要告诉你我的。在这个过程中，我们将学习一些东西，比如获取 API 是如何工作的，以及函数构造器是如何工作的。

抱歉我跑题了，回到我最糟糕的噩梦。如果你上周问我这个问题，答案会是以下列表，排名不分先后:

*   编写 ES6 之前的语法
*   无提取 API
*   没有 Transpiler (Babel/Typescript)
*   鲍勃叔叔说我令人失望(开玩笑)

如果你的清单和我的相符，那么我不得不说你是一个非常奇怪的人。幸运的是，我被叫去做一个项目，这个项目让我的噩梦清单(不包括最后一个)变成了现实。我要给应用程序添加一个新功能。这是一个遗留的代码库，其 AJAX 请求完全使用 es6 之前的语法和 XMLHttpRequest(恐怖)。

因此，为了让这种体验令人愉快，我决定创建一个函数，抽象出我将发出的所有 AJAX 请求，并公开模仿新的 [fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 的 API(其实不是)。这也是在我观看了前端大师上的 [Javascript:新的硬部件](https://frontendmasters.com/courses/javascript-new-hard-parts/)视频之后，视频中给出了关于 fetch API 如何在引擎盖下工作的惊人解释。我们开始吧。

首先，我必须了解 XMLHttpRequest 是如何工作的。然后我开始写函数。我的第一次迭代是这样的:

```
"use strict";

function fetch() {
  var url = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : '';
  var options = arguments.length > 1 && arguments[1] !== undefined ? arguments[1] : {};

var xhr = new XMLHttpRequest();
  var onFufillment = [];
  var onError = [];
  var onCompletion = [];
  var method = "GET" || options.method;
  xhr.onreadystatechange = function () {
    var _data = this;
    if (this.readyState == 4 && this.status == 200) {
      // Action to be performed when the document is read;
      onFufillment.forEach(function (callback) {
          callback(_data);
      });
     onCompletion.forEach(function (callback) {
        callback(_data);
      });
    } else if (this.readyState == 4 && this.status !== 200) {
      onError.forEach(function (callback) {
        callback(_data);
      });
      onCompletion.forEach(function (callback) {
        callback(_data);
      });
    }
  };
  xhr.open(method, url, true);
  xhr.send();

return {
    then: function then(fufillmentFunction) {
      onFufillment.push(fufillmentFunction);
    },
    catch: function _catch(errorFunction) {
      onError.push(errorFunction);
    },
    finally: function _finally(completionFunction) {
      onCompletion.push(completionFunction);
    }
  };
}
```

让我解释一下这个函数的作用:

*   我们正在检查`url`参数是否被传递到函数中。如果没有传递任何内容，则默认为空字符串
*   我们也在为`options`参数做同样的事情。如果没有传递任何内容，则默认为空对象
*   然后我们创建一个 XMLHttpRequest 的新实例
*   我们创建 4 个变量`onFufillment, onError, onCompletion and method`
*   `onFufillment`是一个数组，存储传递给`then`方法的所有函数
*   `onError`是一个数组，存储传递给`catch`方法的所有函数
*   `onCompletion`是一个数组，存储传递给`finally`方法的所有函数
*   `method`用于存储将要使用的 HTTP 方法，默认为`GET`
*   然后，我们将一个函数传递给`xhr`的`onreadystatechange`方法，当请求的状态改变时，将调用这个函数
*   在函数中，我们将`this`保存到一个`_data`变量中，这样就可以将它传递给 forEach 函数而不会丢失它的上下文(我知道`this`很烦人)
*   然后我们检查请求是否完成(`readyState == 4`)，如果请求成功，那么我们循环遍历`onFufillment and onCompletion`数组，调用每个函数并将`_data`传递给它
*   如果请求失败，我们对`onCompletion and onError`数组做同样的事情
*   然后我们发送带有传入参数的请求
*   然后，我们返回一个包含三个函数的对象。与获取 API 同名的。
*   `catch`将作为参数传递的函数推入`onError`数组
*   `then`对`onFufillment`数组做同样的事情
*   `finally`对`onCompletion`数组也是如此

这个 API 的用法如下所示:

```
var futureData = fetch('https://jsonplaceholder.typicode.com/todos/2');
futureData.then(function(data){
  console.log(data)
})

futureData.finally(function(response){
  console.log(response);
});

futureData.catch(function(error){
  console.log(error);
})
```

有用！！！但远不如真正的获取实现。我们能做得比这更好吗？当然可以。我们仍然可以给这个功能添加更多的特性。我们可以把它做成可链接的，也就是说，我们可以给它把方法链接在一起的能力。

在第二次迭代中，它看起来是这样的:

```
"use strict";

function fetch() {
  var url = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : '';
  var options = arguments.length > 1 && arguments[1] !== undefined ? arguments[1] : {};
var xhr = new XMLHttpRequest();
  var onFufillment = [];
  var onError = [];
  var onCompletion = [];
  var method = "GET" || options.method;
  xhr.onreadystatechange = function () {
    var _data = this;
    if (this.readyState == 4 && this.status == 200) {
      // Action to be performed when the document is read;
      onFufillment.forEach(function (callback) {
          callback(_data);
      });
     onCompletion.forEach(function (callback) {
        callback(_data);
      });
    } else if (this.readyState == 4 && this.status !== 200) {
      onError.forEach(function (callback) {
        callback(_data);
      });
      onCompletion.forEach(function (callback) {
        callback(_data);
      });
    }
  };
  xhr.open(method, url, true);
  xhr.send();

	return {
    	then: function then(fufillmentFunction) {
          onFufillment.push(fufillmentFunction);
          return this;
   		},
    	catch: function _catch(errorFunction) {
      	  onError.push(errorFunction);
      	  return this;
      },
        finally: function _finally(completionFunction) {
         onCompletion.push(completionFunction);
         return this;
    }
  };
}
```

API 的用法如下所示:

```
var futureData = fetch('https://jsonplaceholder.typicode.com/todos/2');

futureData.then(function(data){
  console.log(data)
}).then(function(response){
  console.log(response);
}).catch(function(error){
  console.log(error);
});
```

它做了什么？第二次迭代中唯一的不同是在`then, catch and finally`中，我刚刚返回了`this`，这意味着每个函数返回自己，基本上使它能够被链接(部分地)。

更好对吗？但是我们能做得比这更好吗？当然可以。返回的对象可以放在函数的原型中，这样我们可以在函数被多次使用的情况下节省内存。

这是它在第三次迭代中的样子:

```
"use strict";
function fetch() {
  var fetchMethod = Object.create(fetch.prototype);
  var url = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : '';
  var options = arguments.length > 1 && arguments[1] !== undefined ? arguments[1] : {};
var xhr = new XMLHttpRequest();
  fetchMethod.onFufillment = [];
  fetchMethod.onError = [];
  fetchMethod.onCompletion = [];
  var method = "GET" || options.method;
  xhr.onreadystatechange = function () {
    var _data = this;
    if (this.readyState == 4 && this.status == 200) {
      // Action to be performed when the document is read;
      fetchMethod.onFufillment.forEach(function (callback) {
          callback(_data);
      });
     fetchMethod.onCompletion.forEach(function (callback) {
        callback(_data);
      });
    } else if (this.readyState == 4 && this.status !== 200) {
      fetchMethod.onError.forEach(function (callback) {
        callback(_data);
      });
      fetchMethod.onCompletion.forEach(function (callback) {
        callback(_data);
      });
    }
  };
  xhr.open(method, url, true);
  xhr.send();
  return fetchMethod;
};
fetch.prototype.then = function(fufillmentFunction) {
      this.onFufillment.push(fufillmentFunction);
      return this;
};
fetch.prototype.catch = function(errorFunction) {
      this.onError.push(errorFunction);
      return this;
};
fetch.prototype.finally = function(completionFunction) {
      this.onCompletion.push(completionFunction);
      return this;
};
```

所以这个版本基本上将返回的函数移动到 fetch 的原型中。如果你不理解这句话，那么我推荐你看看这篇关于 Javascript 原型的文章。

这是一种进步吗？是啊！！！我们能做得更好吗？当然可以。我们可以使用`new`关键字，去掉显式的 return 语句。

下一次迭代将如下所示:

```
"use strict";
function Fetch() {
  var url = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : '';
  var options = arguments.length > 1 && arguments[1] !== undefined ? arguments[1] : {};
  var xhr = new XMLHttpRequest();
  this.onFufillment = [];
  this.onError = [];
  this.onCompletion = [];
  var method = "GET" || options.method;
  var internalFetchContext = this;
  xhr.onreadystatechange = function () {
    var _data = this;
    if (this.readyState == 4 && this.status == 200) {
      // Action to be performed when the document is read;
      internalFetchContext.onFufillment.forEach(function (callback) {
          callback(_data);
      });
     internalFetchContext.onCompletion.forEach(function (callback) {
        callback(_data);
      });
    } else if (this.readyState == 4 && this.status !== 200) {
      internalFetchContext.onError.forEach(function (callback) {
        callback(_data);
      });
      internalFetchContext.onCompletion.forEach(function (callback) {
        callback(_data);
      });
    }
  };
  xhr.open(method, url, true);
  xhr.send();
};
Fetch.prototype.then = function(fufillmentFunction) {
      this.onFufillment.push(fufillmentFunction);
      return this;
};
Fetch.prototype.catch = function(errorFunction) {
      this.onError.push(errorFunction);
      return this;
};
Fetch.prototype.finally = function(completionFunction) {
      this.onCompletion.push(completionFunction);
      return this;
};
```

让我解释一下这些变化:

*   将函数的名称从 fetch 改为 Fetch，这只是使用`new`关键字时的惯例
*   因为我使用了关键字`new`，所以我可以将创建的各种数组保存到`this`上下文中。
*   因为传递给`onreadystatechange`的函数有它自己的上下文，所以我不得不将原来的`this`保存到它自己的变量中，这样我就可以在函数中调用它了(我知道，`this`可能很烦人)
*   将原型函数转换为新的函数名。

用法如下所示:

```
var futureData = new 

Fetch('https://jsonplaceholder.typicode.com/todos/1');
futureData.then(function(data){
  console.log(data)
}).then(function(response){
  console.log(response);
}).catch(function(error){
  console.log(error);
})
```

瞧啊。那真的很有趣。但是我们能做得更好吗？当然可以。

但是我会把它留给你。我很想在下面的评论中看到你自己的 API 实现。

如果你喜欢这篇文章(即使你不喜欢)，我会感谢你的掌声(或 50)。谢谢你。