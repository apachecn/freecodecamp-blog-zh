# 如何处理嵌套回调，避免“回调地狱”

> 原文：<https://www.freecodecamp.org/news/how-to-deal-with-nested-callbacks-and-avoid-callback-hell-1bc8dc4a2012/>

JavaScript 是一种奇怪的语言。偶尔，你必须处理一个回调，这个回调在另一个回调中。

人们亲切地称这种模式为*回调地狱*。

看起来有点像这样:

```
firstFunction(args, function() {
  secondFunction(args, function() {
    thirdFunction(args, function() {
      // And so on…
    });
  });
});
```

这是给你的 JavaScript。看到嵌套回调是令人难以置信的，但我不认为这是一个“地狱”。如果你知道如何处理，这个“地狱”是可以控制的。

### 回拨时

如果你正在阅读这篇文章，我假设你知道什么是回调。如果你不知道，请在继续之前阅读[这篇文章](https://zellwk.com/blog/callbacks/)中关于回调的介绍。在那里，我们讨论什么是回调，以及为什么在 JavaScript 中使用回调。

### 回调地狱的解决方案

回调地狱有四种解决方案:

1.  写评论
2.  将函数拆分成更小的函数
3.  利用承诺
4.  使用异步/等待

在我们深入研究解决方案之前，让我们一起构建一个回调地狱。为什么？因为太抽象了，看不到`firstFunction`、`secondFunction`、`thirdFunction`。我们想把它具体化。

### 构建回调地狱

让我们想象我们正在做一个汉堡。要做一个汉堡，我们需要经历以下几个步骤:

1.  获取配料(我们假设这是一个牛肉汉堡)
2.  煮牛肉
3.  买汉堡面包
4.  把熟牛肉放在小圆面包之间
5.  上汉堡

如果这些步骤是同步的，您将会看到一个类似如下的函数:

```
const makeBurger = () => {
  const beef = getBeef();
  const patty = cookBeef(beef);
  const buns = getBuns();
  const burger = putBeefBetweenBuns(buns, beef);
  return burger;
};

const burger = makeBurger();
serve(burger);
```

然而，在我们的场景中，假设我们不能自己制作汉堡。我们必须指导助手做汉堡的步骤。在我们指示助手之后，我们必须*等待*助手完成，然后才能开始下一步。

如果我们想在 JavaScript 中等待什么，我们需要使用回调。要做汉堡，我们必须先得到牛肉。我们只有拿到牛肉后才能煮牛肉。

```
const makeBurger = () => {
  getBeef(function(beef) {
    // We can only cook beef after we get it.
  });
};
```

为了烹饪牛肉，我们需要将`beef`传递给`cookBeef`函数。不然没什么好煮的！然后，我们必须等牛肉熟了。

一旦牛肉熟了，我们就有小面包了。

```
const makeBurger = () => {
  getBeef(function(beef) {
    cookBeef(beef, function(cookedBeef) {
      getBuns(function(buns) {
        // Put patty in bun
      });
    });
  });
};
```

拿到小面包后，我们需要把馅饼放在小面包中间。这就是汉堡形成的地方。

```
const makeBurger = () => {
  getBeef(function(beef) {
    cookBeef(beef, function(cookedBeef) {
      getBuns(function(buns) {
        putBeefBetweenBuns(buns, beef, function(burger) {
            // Serve the burger
        });
      });
    });
  });
};
```

终于可以上汉堡了！但是我们不能从`makeBurger`返回`burger`，因为它是异步的。我们需要接受一个回调来提供汉堡。

```
const makeBurger = nextStep => {
  getBeef(function (beef) {
    cookBeef(beef, function (cookedBeef) {
      getBuns(function (buns) {
        putBeefBetweenBuns(buns, beef, function(burger) {
          nextStep(burger)
        })
      })
    })
  })
}

// Make and serve the burger
makeBurger(function (burger) => {
  serve(burger)
})
```

(我做这个回调地狱的例子很有意思？).

### 回调地狱的第一个解决方案:写评论

回调地狱很容易理解。我们可以读它。只是…看起来不太好。

如果你是第一次阅读`makeBurger`，你可能会想“为什么我们做一个汉堡需要这么多回调？没道理啊！”。

在这种情况下，你应该留下注释来解释你的代码。

```
// Makes a burger
// makeBurger contains four steps:
//   1\. Get beef
//   2\. Cook the beef
//   3\. Get buns for the burger
//   4\. Put the cooked beef between the buns
//   5\. Serve the burger (from the callback)
// We use callbacks here because each step is asynchronous.
//   We have to wait for the helper to complete the one step
//   before we can start the next step

const makeBurger = nextStep => {
  getBeef(function(beef) {
    cookBeef(beef, function(cookedBeef) {
      getBuns(function(buns) {
        putBeefBetweenBuns(buns, beef, function(burger) {
          nextStep(burger);
        });
      });
    });
  });
};
```

现在，与其想“wtf？!"当你看到回调地狱的时候，你就明白为什么要这样写了。

### 回调地狱的第二个解决方案:将回调分成不同的函数

我们的回调地狱例子已经是这样的例子。让我向您展示一步一步的命令式代码，您就会明白为什么了。

对于我们的第一次回访，我们必须去冰箱拿牛肉。厨房里有两台冰箱。我们需要去正确的冰箱。

```
const getBeef = nextStep => {
  const fridge = leftFright;
  const beef = getBeefFromFridge(fridge);
  nextStep(beef);
};
```

要煮牛肉，我们需要把牛肉放入烤箱；把烤箱调到 200 度，等 20 分钟。

```
const cookBeef = (beef, nextStep) => {
  const workInProgress = putBeefinOven(beef);
  setTimeout(function() {
    nextStep(workInProgress);
  }, 1000 * 60 * 20);
};
```

现在想象一下，如果你不得不在`makeBurger`中写下这些步骤中的每一步……你可能会被庞大的代码数量吓晕！

关于将回调分解成更小函数的具体例子，你可以阅读我的回调文章中的这一小部分。

### 回拨地狱的第三个解决方案:使用承诺

我假设你知道什么是承诺。如果没有，请[看这篇文章](https://zellwk.com/blog/js-promises/)。

承诺可以让回拨变得更容易管理。您将看到以下代码，而不是上面看到的嵌套代码:

```
const makeBurger = () => {
  return getBeef()
    .then(beef => cookBeef(beef))
    .then(cookedBeef => getBuns(beef))
    .then(bunsAndBeef => putBeefBetweenBuns(bunsAndBeef));
};

// Make and serve burger
makeBurger().then(burger => serve(burger));
```

如果您利用单参数风格的承诺，您可以将上面的调整为:

```
const makeBurger = () => {
  return getBeef()
    .then(cookBeef)
    .then(getBuns)
    .then(putBeefBetweenBuns);
};

// Make and serve burger
makeBurger().then(serve);
```

更容易阅读和管理。

但问题是如何将基于回调的代码转换成基于承诺的代码。

#### 将回电转化为承诺

为了将回调转换为承诺，我们需要为每个回调创建一个新的承诺。当回拨成功时，我们可以兑现承诺。或者我们可以`reject`承诺如果回调失败。

```
const getBeefPromise = _ => {
  const fridge = leftFright;
  const beef = getBeefFromFridge(fridge);

  return new Promise((resolve, reject) => {
    if (beef) {
      resolve(beef);
    } else {
      reject(new Error(“No more beef!”));
    }
  });
};

const cookBeefPromise = beef => {
  const workInProgress = putBeefinOven(beef);

  return new Promise((resolve, reject) => {
    setTimeout(function() {
      resolve(workInProgress);
    }, 1000 * 60 * 20);
  });
};
```

实际上，复试可能已经为你准备好了。如果使用 Node，每个包含回调的函数将具有相同的语法:

1.  回调将是最后一个参数
2.  回调总是有两个参数。这些论点也是同样的顺序。(首先是错误，然后是您感兴趣的内容)。

```
// The function that’s defined for you
const functionName = (arg1, arg2, callback) => {
  // Do stuff here
  callback(err, stuff);
};

// How you use the function
functionName(arg1, arg2, (err, stuff) => {
  if (err) {
  console.error(err);
  }
  // Do stuff
});
```

如果你的回调有相同的语法，你可以使用类似于 [ES6 Promisify](https://www.npmjs.com/package/es6-promisify) 或 [Denodeify](https://www.npmjs.com/package/denodeify) (de-node-ify)的库来回调承诺。如果使用 Node v8.0 及以上版本，可以使用 [util.promisify](https://nodejs.org/dist/latest-v8.x/docs/api/util.html#util_util_promisify_original) 。

他们三个都工作。您可以选择使用任何库。不过，每种方法之间都有细微的差别。我会让你去检查他们的文档。

### 回调地狱的第四个解决方案:使用异步函数

要使用异步函数，首先需要知道两件事:

1.  如何将回电转化为承诺(阅读上文)
2.  如何使用异步函数([需要帮助就看这个](https://zellwk.com/blog/async-await))。

有了异步函数，你可以写`makeBurger`就好像它又是同步的一样！

```
const makeBurger = async () => {
  const beef = await getBeef();
  const cookedBeef = await cookBeef(beef);
  const buns = await getBuns();
  const burger = await putBeefBetweenBuns(cookedBeef, buns);
  return burger;
};

// Make and serve burger
makeBurger().then(serve);
```

这里我们可以对`makeBurger`做一个改进。你大概可以同时给`getBuns`和`getBeef`找两个帮手。这意味着你可以用`Promise.all`来`await`他们俩。

```
const makeBurger = async () => {
  const [beef, buns] = await Promise.all(getBeef, getBuns);
  const cookedBeef = await cookBeef(beef);
  const burger = await putBeefBetweenBuns(cookedBeef, buns);
  return burger;
};

// Make and serve burger
makeBurger().then(serve);
```

(注意:您可以对承诺做同样的事情…但是语法不如 async/await 函数那么好和清晰)。

### 包扎

回调地狱没有你想的那么地狱。有四种简单的方法来管理回调地狱:

1.  写评论
2.  将函数拆分成更小的函数
3.  利用承诺
4.  使用异步/等待

本文最初发布在 *[我的博客](https://zellwk.com/blog/nested-callbacks)上。*
如果你想要更多的文章来帮助你成为更好的前端开发人员，请注册我的[时事通讯](https://zellwk.com/)。