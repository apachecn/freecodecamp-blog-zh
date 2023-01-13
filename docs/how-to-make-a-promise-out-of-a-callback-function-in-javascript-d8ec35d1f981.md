# 如何在 JavaScript 中用回调函数做出承诺

> 原文：<https://www.freecodecamp.org/news/how-to-make-a-promise-out-of-a-callback-function-in-javascript-d8ec35d1f981/>

作者:阿德汗·埃尔·班哈维

# 如何在 JavaScript 中用回调函数做出承诺

![FiZfnctqB6-ARf7GY7YCwptZRVxnUtaQZJv9](img/3ac6c4fedc5b6962f98b592ac801542e.png)

后端开发人员在构建应用程序或测试代码时总是会遇到挑战。作为一名相当新的开发人员，我已经熟悉了这些挑战，我从未遇到过比使用**回调函数更频繁或者更令人难忘的挑战或不便。**

我不打算深入研究回调的细节，以及它的优点和缺点，或者像 promises 和 async/await 这样的替代方法。想要更生动的解释，你可以看看这篇文章，这篇文章对它们进行了彻底的解释。

### **回调地狱**

回调是 JavaScript 的一个有用特性，它使 JavaScript 能够进行异步调用。它们是通常作为第二个参数传递给另一个函数的函数，该函数正在获取数据或执行需要时间才能完成的 I/O 操作。

例如，尝试使用`request` 模块进行 API 调用，或者连接到 MongoDB 数据库。但是如果两个调用相互依赖呢？如果您获取的数据是您需要连接的 MongoDB URL，该怎么办？

你必须将这些调用嵌套在一起:

```
request.get(url, function(error, response, mongoUrl) {

  if(error) throw new Error("Error while fetching fetching data");

  MongoClient.connect(mongoUrl, function(error, client) {

    if(error) throw new Error("MongoDB connection error");

    console.log("Connected successfully to server");    
    const db = client.db("dbName");
    // Do some application logic
    client.close();

  });

});
```

好吧…那么问题在哪里？首先，代码的可读性受到这种技术的影响。

当代码库很小的时候，一开始看起来还可以。但是这并不能很好地伸缩，特别是当你深入嵌套回调的更多层时。

不管你的代码格式多么整洁，你最终会得到很多的右括号和花括号，这会让你和其他开发人员感到困惑。有一个名为 [callbackhell](http://callbackhell.com/) 的网站解决了这个具体问题。

我听到你们中的一些人，包括天真的过去的我，告诉我用`async` 函数包装它，然后用`await` 回调函数。这根本行不通。

如果在使用回调的函数之后有任何代码块，该代码块将执行，并且**不会** **等待**回调。

这是我以前犯过错误:

```
var request = require('request');

// WRONG

async function(){

  let joke;
  let url = "https://api.chucknorris.io/jokes/random"

  await request.get(url, function(error, response, data) {

    if(error) throw new Error("Error while fetching fetching data");

    let content = JSON.parse(data);
    joke = content.value;

  });

  console.log(joke); // undefined

};

// Wrong

async function(){

  let joke;
  let url = "https://api.chucknorris.io/jokes/random"

  request.get(url, await function(error, response, data) {

    if(error) throw new Error("Error while fetching fetching data");

    let content = JSON.parse(data);
    joke = content.value;

  });

  console.log(joke); // undefined

};
```

一些更有经验的开发人员可能会说“只需要使用一个不同的库，它使用 promises 来做同样的事情，比如 [axios](https://www.npmjs.com/package/axios) 、*T3，或者只使用 [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 、*。在那种情况下，我当然可以，但那只是逃避问题。**

另外，这只是一个例子。有时你可能会被迫使用一个不支持承诺的库。比如使用软件开发工具包(SDK)与亚马逊网络服务(AWS)、Twitter 或脸书等平台进行交流。

有时候，甚至使用回调来做一个非常简单的调用和一个快速的 I/O 或 CRUD 操作也可以，没有其他逻辑依赖于它的结果。但是您可能会受到运行时环境的约束，比如在一个 [Lambda 函数](https://docs.aws.amazon.com/lambda/latest/dg/lambda-introduction-function.html)中，一旦主线程完成，它就会终止所有进程，而不管任何未完成的异步调用。

### 解决方案 1(简单):使用节点的“util”模块

解决方案出奇的简单。即使您对 JavaScript 中的承诺有点不舒服，您也会喜欢使用它们来解决这个问题。

正如 Erop 和 Robin 在评论中指出的，Nodejs 版本 8 及以上现在支持使用内置的 **util** 模块将回调函数转化为承诺。

```
const request = require('request');

const util = require('util');

const url = "https://api.chucknorris.io/jokes/random";

// Use the util to promisify the request method

const getChuckNorrisFact = util.promisify(request);

// Use the new method to call the API in a modern then/catch pattern

getChuckNorrisFact(url).then(data => {

   let content = JSON.parse(data.body);

   console.log('Joke: ', content.value);

}).catch(err => console.log('error: ', err))
```

上面的代码使用 nodejs 核心库中的[**util . promisify**](https://nodejs.org/docs/latest-v8.x/api/util.html#util_util_promisify_original)**方法巧妙地解决了这个问题。**

**你所要做的就是使用回调函数作为 util.promisify 的参数，并把它存储在一个变量中。在我的情况下，那就是 *getChuckNorrisFact* 。
然后你用那个变量作为一个函数，你可以用它作为一个承诺**。然后是()**和**。catch()** 方法。**

### **解决方案 2(涉及):把回电变成承诺**

**有时，使用 request 和 util 库是不可能的，无论是因为遗留环境/代码库还是从客户端浏览器发出请求，您都必须在回调函数周围加上承诺。**

**让我们以上面的查克·诺里斯为例，把它变成一个承诺。**

```
`var request = require('request');
let url = "https://api.chucknorris.io/jokes/random";

// A function that returns a promise to resolve into the data //fetched from the API or an error
let getChuckNorrisFact = (url) => {
  return new Promise(
    (resolve, reject) => {
      request.get(url, function(error, response, data){
        if (error) reject(error);

let content = JSON.parse(data);
        let fact = content.value;
        resolve(fact);
      })
   }
 );
};

getChuckNorrisFact(url).then(
   fact => console.log(fact) // actually outputs a string
).catch(
   error => console.(error)
);`
```

**![ZXNYPRkv4mC2cHoq-4PIdoAx0WK-DyuUybzA](img/1659aec4ac9ac2c80477be41ccdabb84.png)

works like magic** 

**在上面的代码中，我将基于回调的`request`函数放在承诺包装器`Promise( (resolve, reject) => { //callback function})`中。这个包装器允许我们像使用`****.then()****`和`.catch()`方法一样调用`getChuckNorrisFact`函数。当调用 `**getChuckNorrisFact**`时，它执行对 API 的请求，而 ****等待**** 执行`resolve()`或`reject()`语句。在回调函数中，您只需将检索到的数据传递给 resolve 或 reject 方法。**

**一旦数据(在本例中，是一个非常棒的 Chuck Norris 事实)被获取并传递给解析器，`getChuckNorrisFact`就执行`then()`方法。这将返回结果，您可以在`then()` 的函数中使用该结果来执行您想要的逻辑——在本例中，将结果显示到控制台。**

**您可以在 [MDN 网络文档中了解更多信息。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises#Creating_a_Promise_around_an_old_callback_API)**