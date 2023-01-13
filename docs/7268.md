# 这里有一些你可以从头开始写的函数装饰器

> 原文：<https://www.freecodecamp.org/news/here-are-a-few-function-decorators-you-can-write-from-scratch-488549fe8f86/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

> **function decorator** 是一个高阶函数，它将一个函数作为参数，返回另一个函数，返回的函数是参数函数的变体——[Javascript allongé](https://leanpub.com/javascript-allonge/read#decorators)

让我们写一些在库中常见的函数装饰器，比如[underscript . js](http://underscorejs.org/#functions)、 [lodash.js](https://lodash.com/docs/4.17.5) 或 [ramda.js](http://ramdajs.com/docs/) 。

### 一次()

*   [once(fn)](https://jsfiddle.net/cristi_salcescu/zpLeLp0v/) :创建一个只执行一次的函数版本。这对于初始化函数很有用，我们希望确保它只运行一次，不管它从不同的地方被调用了多少次。

```
function once(fn){
  let returnValue;
  let canRun = true;
  return function runOnce(){
      if(canRun) {
          returnValue = fn.apply(this, arguments);
          canRun = false;
      }
      return returnValue;
  }
}

var processonce = once(process);
processonce(); //process
processonce(); //
```

`once()`是返回另一个函数的函数。返回的函数`runOnce()`是一个[闭包](https://medium.freecodecamp.org/why-you-should-give-the-closure-function-another-chance-31253e44cfa0)。同样重要的是要注意原始函数是如何被调用的——通过传入`this`和所有`arguments` : `fn.apply(this, arguments)`的当前值。

> 如果你想更好地理解闭包，看看[为什么你应该再给闭包函数一次机会](https://medium.freecodecamp.org/why-you-should-give-the-closure-function-another-chance-31253e44cfa0)。

### 在()之后

*   [after(count，fn)](https://jsfiddle.net/cristi_salcescu/4evuoxe6/) :创建一个仅在多次调用后执行的函数版本。例如，当我们希望确保函数只在所有异步任务完成后运行时，这是很有用的。

```
function after(count, fn){
   let runCount = 0;
   return function runAfter(){
      runCount = runCount + 1;
      if (runCount >= count) {
         return fn.apply(this, arguments);        
      }
   }
}

function logResult() { console.log("calls have finished"); }

let logResultAfter2Calls = after(2, logResult);
setTimeout(function logFirstCall() { 
      console.log("1st call has finished"); 
      logResultAfter2Calls(); 
}, 3000);

setTimeout(function logSecondCall() { 
      console.log("2nd call has finished"); 
      logResultAfter2Calls(); 
}, 4000);
```

请注意我是如何使用`after()`来构建一个新函数`logResultAfter2Calls()`的，它将只在第二次调用后执行`logResult()`的原始代码。

### 油门()

*   [throttle(fn，wait)](https://jsfiddle.net/cristi_salcescu/5tdv0eq6/) :创建一个版本的函数，当被重复调用时，每隔`wait`毫秒调用一次原始函数。这对于限制更快发生的事件很有用。

```
function throttle(fn, interval) {
    let lastTime;
    return function throttled() {
        let timeSinceLastExecution = Date.now() - lastTime;
        if(!lastTime || (timeSinceLastExecution >= interval)) {
            fn.apply(this, arguments);
            lastTime = Date.now();
        }
    };
}

let throttledProcess = throttle(process, 1000);
$(window).mousemove(throttledProcess);
```

在这个例子中，移动鼠标会产生很多`mousemove`事件，但是对原始函数`process()`的调用只会每秒发生一次。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****