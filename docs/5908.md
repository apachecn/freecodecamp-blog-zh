# 如何使用 RxJS 和 Node 构建一个简单的可定制的 web scraper

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-simple-customizable-web-scraper-using-rxjs-and-node-6858cfe82a39/>

雅各布·高

# 如何使用 RxJS 和 Node 构建一个简单的可定制的 web scraper

![DOxS6rxFG8OzYk321KoM5eHWUK-j2ElqtfTx](img/3dee12cbec5bb538e43d9e8beb362d3a.png)

### 介绍

了解 RxJS 后(感谢 Angular！)，我意识到它非常适合处理网页抓取操作。

我在一个兼职项目中尝试过，我想和你分享我的经验。希望这能让你看到反应式编程是如何让你的生活变得更简单的。

代码可以在[https://github.com/jacobgoh101/web-scraping-with-rxjs](https://github.com/jacobgoh101/web-scraping-with-rxjs)上找到

### 要求

*   结节
*   RxJS 及其中级理解
*   cheerio :它允许你使用类似 jQuery 的语法从 HTML 代码中提取信息
*   [请求-承诺-本机](https://www.npmjs.com/package/request-promise-native):用于发送 HTTP 请求

### 假设目标

每个人都喜欢好的喜剧电影。

让我们把从 IMDB 搜集一份好的喜剧电影列表作为我们的目标。

目标数据只需要满足 3 个要求:

*   这是一部电影(不是电视节目、音乐录影带等)
*   这是一部喜剧
*   它的评级为 7 或更高

### 开始

让我们设置我们的基本 URL 并定义一个使用基本 URL 作为初始值的 BehaviorSubject `allUrl$`。

(一个行为主体是一个[主体](https://www.youtube.com/watch?v=rdK92pf3abs)，有一个初始值。)

```
const { BehaviorSubject } =  require('rxjs');const  baseUrl  =  `https://imdb.com`;const  allUrl$  =  new  BehaviorSubject(baseUrl);
```

`allUrl$`将是所有抓取操作的起点。每个 URL 都将被传入`allUrl$`并在稍后被处理。

#### 确保每个 URL 只抓取一次

在[不同的](https://rxjs-dev.firebaseapp.com/api/operators/distinct)操作符和 [normalize-url](https://www.npmjs.com/package/normalize-url) 的帮助下，我们可以很容易地确保我们不会抓取同一个 url 两次。

```
// ...const { map, distinct, filter } =  require('rxjs/operators');const  normalizeUrl  =  require('normalize-url');
```

```
// ...
```

```
const  uniqueUrl$  =  allUrl$.pipe(    // only crawl IMDB url    filter(url  =>  url.includes(baseUrl)),    // normalize url for comparison    map(url  =>  normalizeUrl(url, { removeQueryParameters: ['ref', 'ref_']     })),    // distinct is a RxJS operator that filters out duplicated values    distinct());
```

#### 是时候开始刮了

我们将向每个唯一的 URL 发出请求，并将每个 URL 的内容映射到另一个可观察对象中。

为此，我们使用 [mergeMap](https://www.learnrxjs.io/operators/transformation/mergemap.html) 将请求的结果映射到另一个可观察对象。

```
const { BehaviorSubject, from } =  require('rxjs');const { map, distinct, filter, mergeMap } = require('rxjs/operators');const rp = require('request-promise-native');const  cheerio  =  require('cheerio');
```

```
//...const urlAndDOM$ = uniqueUrl$.pipe(  mergeMap(url => {    return from(rp(url)).pipe(      // get the cheerio function $      map(html => cheerio.load(html)),      // add URL to the result. It will be used later for crawling      map($ => ({        $,        url      }))    );  }));
```

`urlAndDOM$`会发出一个由两个属性组成的物体，分别是`$`和`url`。`$`是一个 Cheerio 函数，你可以使用类似`$('div').text()`的东西从原始 HTML 代码中提取信息。

#### 抓取所有网址

```
const { resolve } =  require('url');//...
```

```
// get all the next crawlable URLsurlAndDOM$.subscribe(({ url, $ }) => {  $('a').each(function(i, elem) {    const href = $(this).attr('href');    if (!href) return;
```

```
// build the absolute url    const absoluteUrl = resolve(url, href);    allUrl$.next(absoluteUrl);  });});
```

在上面的代码中，我们抓取了页面中的所有链接，并将其发送到`allUrl$`以便稍后抓取。

#### 刮下来保存我们想要的电影！

```
const  fs  =  require('fs');//...
```

```
const isMovie = $ =>  $(`[property='og:type']`).attr('content') === 'video.movie';const isComedy = $ =>  $(`.title_wrapper .subtext`)    .text()    .includes('Comedy');const isHighlyRated = $ => +$(`[itemprop="ratingValue"]`).text() > 7;
```

```
urlAndDOM$  .pipe(    filter(({ $ }) => isMovie($)),    filter(({ $ }) => isComedy($)),    filter(({ $ }) => isHighlyRated($))  )  .subscribe(({ url, $ }) => {    // append the data we want to a file named "comedy.txt"    fs.appendFile('comedy.txt', `${url}, ${$('title').text()}\n`);  });
```

### 是的，我们刚刚创建了一个网页抓取器

在大约 70 行代码中，我们创建了一个 web scraper

*   自动抓取的网址，没有不必要的重复
*   自动抓取并保存我们想要的信息到文本文件中

你可以在[https://github . com/jacobgoh 101/web-scraping-with-rxjs/blob/86ff 05 e 893 dec 5 f1 b 39647350 CB 0 f 74 EFE 258 c 86/index . js](https://github.com/jacobgoh101/web-scraping-with-rxjs/blob/86ff05e893dec5f1b39647350cb0f74efe258c86/index.js)中看到到目前为止的代码

如果您曾经尝试过从头开始编写 web scraper，那么现在您应该能够看到用 RxJS 编写一个 web scraper 是多么优雅。

### 但是我们还没有完成…

在理想的情况下，上面的代码应该可以永远正常工作，不会出现任何问题。

但在现实中，愚蠢的错误时有发生。

### 处理错误

#### 限制活动并发连接数

如果我们在短时间内向服务器发送太多请求，很可能我们的 IP 会被暂时阻止发出任何进一步的请求，特别是对于像 IMDB 这样的已建立的网站。

同时发送如此多的请求也被认为是粗鲁/不道德的，因为这会增加服务器的负载，在某些情况下，**会使服务器崩溃。**

[mergeMap](https://www.learnrxjs.io/operators/transformation/mergemap.html) 内置了控制并发的功能。只需在第三个函数参数中添加一个数字，它就会自动限制活动的并发连接。优雅！

```
const maxConcurrentReq = 10;//...const urlAndDOM$ = uniqueUrl$.pipe(  mergeMap(    //...    null,    maxConcurrentReq  ));
```

代码差异:[https://github . com/jacobgoh 101/web-scraping-with-rxjs/commit/6 aaed 6 da e230 D2 de 1493 f1 b6d 78282 ce 2 E8 f 316](https://github.com/jacobgoh101/web-scraping-with-rxjs/commit/6aaed6dae230d2dde1493f1b6d78282ce2e8f316)

#### 处理并重试失败的请求

由于死链接或服务器端速率限制，请求可能会随机失败。这对网页抓取器至关重要。

我们可以使用 [catchError](https://www.learnrxjs.io/operators/error_handling/catch.html) 、 [retry](https://www.learnrxjs.io/operators/error_handling/retry.html) 操作符来处理这个问题。

```
const { BehaviorSubject, from, of } =  require('rxjs');const {  // ...  retry,  catchError} = require('rxjs/operators');//...
```

```
const maxRetries = 5;// ...
```

```
const urlAndDOM$ = uniqueUrl$.pipe(  mergeMap(    url => {      return from(rp(url)).pipe(        retry(maxRetries),        catchError(error => {          const { uri } = error.options;          console.log(`Error requesting ${uri} after ${maxRetries} retries.`);          // return null on error          return of(null);        }),        // filter out errors        filter(v => v),        // ...      );    },
```

代码差异:[https://github . com/jacobgoh 101/web-scraping-with-rxjs/commit/3098 b48 ca 91 a 59 aa 5171 BC 2a a9 c 17801 e 769 fcbb](https://github.com/jacobgoh101/web-scraping-with-rxjs/commit/3098b48ca91a59aa5171bc2aa9c17801e769fcbb)

#### 改进的重试失败请求

使用 retry 操作符，请求失败后会立即重试。这并不理想。

最好延迟一段时间再重试。

我们可以使用[learn rjs](https://www.learnrxjs.io/operators/error_handling/retrywhen.html)中建议的`genericRetryStrategy`来实现这一点。

代码差异:[https://github . com/jacobgoh 101/web-scraping-with-rxjs/commit/e 194 F4 ff 128 a 573241055 ffc 0d 1969d 54 c8 c 270](https://github.com/jacobgoh101/web-scraping-with-rxjs/commit/e194f4ff128a573241055ffc0d1969d54ca8c270)

### 结论

概括地说，在这篇文章中，我们讨论了:

*   如何使用 Cheerio 抓取网页
*   如何使用像 filter，distinct 这样的 RxJS 操作符避免重复抓取
*   如何使用 mergeMap 创建请求响应的可观察对象
*   如何在 mergeMap 中限制并发性
*   如何处理错误
*   如何处理重试

希望这对你有所帮助，加深了你对 RxJs 和 web 抓取的理解。

*最初发布于[开发至](https://dev.to/jacobgoh101/simple--customizable-web-scraper-using-rxjs-and-node-1on7)。*