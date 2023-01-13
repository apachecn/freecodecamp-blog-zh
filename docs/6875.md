# 了解更多关于 JavaScript 的承诺:在 25 次测试中从零到英雄

> 原文：<https://www.freecodecamp.org/news/learn-more-about-javascripts-promises-from-zero-to-hero-in-25-tests-ddaaf4d3c928/>

作者:安德里亚·库提法里斯

# 了解更多关于 JavaScript 的承诺:在 25 次测试中从零到英雄

![1*JZlPOGIMUKYlwvBnmPomEg](img/95d6a276f5f8699c608bfd2c927ee2e4.png)

一份试卷胜过千言万语…或者是一张图片…？

我认为解释 JavaScript 承诺的最好方式是通过例子。什么是一个好的、独立的、简短的写例子的方法？一个测试！

对于没见过茉莉测试服的人，`it('...', (done) => {..`。})是测试 a `nd d` one 是异步测试完成时必须执行的函数。

这里的规则是:

*   每个测试都以用英语断言某事开始。您必须推导出为什么测试代码暗示测试断言为真。
*   有些测试是有预期的。如果测试通过，期望是真实的。
*   其他测试依赖于被调用的回调`done()`。如果没有调用`done()`，测试失败。

每个测试都在[这个 JSFiddle](https://jsfiddle.net/kouty79/e52qkkmu/) 里，可以边看边随意摆弄。尤其是如果你对任何一个测试有疑问，一定要修改测试代码并研究发生了什么。

### 测试

让我们从承诺基础开始:

```
it('Promise executor is run SYNCHRONOUSLY', () => {  let executorRun = false;  new Promise(function executor() {    executorRun = true;  });  expect(executorRun).toBe(true);});it('you can resolve a promise', (done) => {  new Promise((resolve) => setTimeout(resolve, 1))    .then(done);});it('... or you can reject a promise', (done) =&gt; {  new Promise((resolve, reject) => setTimeout(reject, 1))    .then(undefined, done);});it('An error inside the executor, rejects the promise', (done) => {  new Promise(function executor() {    throw 'Error';  }).catch(done);});
```

好像你调用`resolve()`的时候，运行的是第一个`then(...)`回调。如果你调用`reject()` 或者抛出一个错误，`catch()`或者`then(...)`的第二次回调被运行。

同样，**承诺执行者同步运行****。这意味着承诺是处理异步代码的一种方式，而不是在异步线程中执行任务。如果你想在主线程之外执行一些 JavaScript 代码，使用 [Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers) 。**

**让我们更详细地看看这些`then(...)`和`catch()`函数是什么，以及“链接承诺”是什么意思:**

```
`// Chaining promises`
```

```
`it('you can chain promise because .then(...) returns a promise'  , (done) => {  fetch('https://jsonplaceholder.typicode.com/posts/1')    .then(response => response.json())    .then(json => expect(json.userId).toBe(1))    .then(done);});it('you can use the fail callback of .then(success, fail) to ' +  'handle rejected promises', (done) => {  Promise.reject()    .then(function success() {      throw 'I must not be executed';    }, function fail() {      done();    });});it('... or you can use .catch() to handle rejected promises'  , (done) => {  Promise.reject()    .then(function success() {      throw 'I must not be executed';    })    .catch(done);});it('also .catch() returns a promise, allowing promise chaining'  , (done) => {  Promise.reject()    .catch(() => undefined)    .then(done);});it('you must return a rejected promise if you want to ' +  'execute the next fail callback', (done) => {  function someApiCall() {    return Promise.reject('Error');  }  someApiCall()    .catch((err) => {      console.error(err);      // Without the line below, .catch gets not called      return Promise.reject(err);    })    .catch(done);});it('... or you can throw an error if you want to ' +  'execute the next fail callback', (done) => {  function someApiCall() {    return Promise.reject('Error');  }  someApiCall()    .catch((err) => {      console.error(err);      throw err; // Without this line, .catch gets not called    })    .catch(done);});it('values returned inside .then()/.catch() callbacks ' +  'are provided to the next callback', (done) => {  Promise.resolve(1)    .then(value => value + 1)    .then(value => expect(value).toBe(2));  Promise.reject(1)    .catch(value => value + 1)    .then(value => expect(value).toBe(2));  setTimeout(() => {    done();  }, 1);});`
```

**好吧，但是什么是`Promise.resolve()`和`Promise.reject()`？让我们来了解一下！**

```
`it('you can use Promise.resolve() to wrap values or promises'  , (done) => {  function iMayReturnAPromise() {    return Math.random() >= 0.5 ? Promise.resolve() : 5;  }`
```

```
 `Promise.resolve(iMayReturnAPromise()).then(done);});`
```

```
`it('you can use Promise.resolve() to execute something just after'  , (done) => {  let arr = [];  Promise.resolve().then(() => arr.push(2));  arr.push(1);`
```

```
 `setTimeout(() => {    expect(arr).toEqual([1, 2]);    done();  }, 1);});`
```

```
`/** @seehttps://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules **/it('Promise.resolve() is normally executed before setTimeout(.., 0)'  , (done) => {  let arr = [];  setTimeout(() => arr.push('timeOut'), 0);  Promise.resolve().then(() => {    arr.push('resolve');  });`
```

```
 `setTimeout(() => {    expect(arr).toEqual(['resolve', 'timeOut']);    done();  }, 1);});`
```

```
`it('you can create rejected promises', (done) => {  Promise.reject('reason').catch(done);});`
```

```
`it('pay attention to "Uncaught (in promise) ..."', () => {  Promise.reject('The error');  // Outputs in the console Uncaught (in promise) The error});`
```

#### **连锁承诺与创造新的承诺**

**虽然`new Promise(...)`是创造承诺的一种方式，但你应该避免使用它。大多数情况下，函数/库会返回一个承诺，因此您应该将承诺链接起来，而不是创建新的承诺:**

```
`it("Don't use new Promise(...), prefer chaining", (done) => {  const url = 'https://jsonplaceholder.typicode.com/posts/1';  function badlyDesignedCustomFetch() {    return new Promise((resolve, reject) => {      fetch(url).then((response) => {        if (response.ok) {          resolve(response);        } else {          reject('Fetch failed');        }      });    });  }  function wellDesignedCustomFetch() {    return fetch(url).then((response) =>; {      if (!response.ok) {        return Promise.reject('Fetch failed');      }      return (response);    });  }  Promise.all([    badlyDesignedCustomFetch(),    wellDesignedCustomFetch()  ]).then(done);});`
```

**但是，什么时候应该使用`new Promise(...)`？当你想从回调接口转移到承诺接口时。见下文:**

```
`function imgOnLoad(img) {  return new Promise((resolve, reject) => {    img.onload = resolve;    img.onerror = reject;  });}`
```

#### **并行执行**

**承诺链很好，但是并行执行异步操作呢？以下是你需要知道的全部内容:**

```
`// Parallel execution of promises`
```

```
`it('you can use Promise.all([...]) to execute promises in parallel'  , (done) => {  const url = 'https://jsonplaceholder.typicode.com/posts';  const p1 = fetch(`${url}/1`);  const p2 = fetch(`${url}/2`);  Promise.all([p1, p2])    .then(([res1, res2]) => {      return Promise.all([res1.json(), res2.json()])    })    .then(([post1, post2]) => {      expect(post1.id).toBe(1);      expect(post2.id).toBe(2);    })    .then(done);});it('Promise.all([...]) will fail if any of the promises fails'  , (done) =&gt; {  const p1 = Promise.resolve(1);  const p2 = Promise.reject('Error');  Promise.all([p1, p2])    .then(() => {      fail('I will not be executed')    })    .catch(done);});it("if you don't want Promise.all() to fail, wrap the promises " +  "in a promise that will not fail", (done) => {  function iMayFail(val) {    return Math.random() >= 0.5 ?      Promise.resolve(val) :      Promise.reject(val);  }  function promiseOr(p, value) {    return p.then(res => res, () => value);  }  const p1 = iMayFail(10);  const p2 = iMayFail(9);  Promise.all([promiseOr(p1, null), promiseOr(p2, null)])    .then(([val1, val2]) => {      expect(val1 === 10 || val1 === null).toBe(true);      expect(val2 === 9 || val2 === null).toBe(true);    })    .catch(() => {      fail('I will not be executed')    })    .then(done);});it('Promise.race([...]) will resolve as soon as ' +  'one of the promises resolves o rejects', (done) => {  const timeout =    new Promise((resolve, reject) => setTimeout(reject, 100));  const data =    fetch('https://jsonplaceholder.typicode.com/posts/1');  Promise.race([data, timeout])    .then(() => console.log('Fetch OK'))    .catch(() => console.log('Fetch timeout'))    .then(done);});`
```

#### **句法**

**与同步代码的典型语法相比，Promise 语法有点复杂。的确，通过链接承诺，代码保持了良好的可读性，但它还可以更好。新的 **await/async 语法**使得使用承诺像编写同步代码一样简单。**

```
`// New await/async syntax`
```

```
`it('you can use the new await/async syntax', async () => {  function timeout(ms) {    return new Promise((resolve) => setTimeout(resolve, ms));  }  const start = Date.now();  const delay = 200;  await timeout(delay + 2); // Just some ms tolerance  expect(Date.now() - start).toBeGreaterThanOrEqual(delay);});it('an async function returns a promise', (done) => {  async function iAmAsync() {    return 1;  }  iAmAsync()    .then((val) => expect(val).toBe(1))    .then(done);});it('await just awaits a promise resolution', async (done) => {  await Promise.resolve();  done();});it('await will throw an error if the promise fail', async(done) =&gt; {  try {    await Promise.reject();    fail('I will not be executed');  } catch (err) {    done();  }});`
```

#### **同步功能**

**最后一点:当你设计一个函数时，你必须决定它是否是同步的。不要因为“你永远不知道”而拒绝承诺。尽可能使用“正常的”同步功能。**

**所有的测试都是[这里](https://jsfiddle.net/kouty79/e52qkkmu/)，在 JSFiddle 上。**

**仅此而已！希望你喜欢这篇文章。**