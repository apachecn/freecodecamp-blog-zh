# 介绍可观察的事物以及它们与承诺的不同之处

> 原文：<https://www.freecodecamp.org/news/what-are-observables-how-they-are-different-from-promises/>

**可观测量**、**可观测量**、**可观测量**...是啊！今天，我们就来谈谈这个经常被市场讨论的词。我们还将了解它们与承诺有何不同(没听说过承诺？不要担心！你很快就会知道更多)。开始吧！

我第一次遇到术语**可观测的**是在我开始学习 Angular 的时候。虽然这不是一个特定于 angle 的特性，但这是一种处理**异步** 请求的新方式。异步请求？你知道的，对吧？不要！没关系。那么让我们先来理解什么是**异步**请求。

## 异步请求

嗯！您一定读过 JavaScript 世界中的异步特性。在计算机世界中，异步意味着程序的流程是独立发生的。它不会等待任务完成。它移动到下一个任务。

现在，你可能会想——没有完成的任务会怎么样？员工处理那些未完成的任务。是啊！在后台，一个同事工作并处理那些未完成的任务，一旦完成，它就发回数据。

这可能会带来另一个问题，即我们如何处理返回的数据。答案是**承诺**、**观察点**、**试听点**等等。

我们知道这些异步操作会返回响应，要么是成功后的一些数据，要么是一个错误。为了解决这个问题，像**承诺**、**回调**、**可观测量**这样的概念进入了市场。嗯！我现在不会进入它们，因为我们已经偏离了我们的子主题，即' **async** '请求。(不用担心！这些话题马上就要讨论了)。

在讨论了以上几点之后，您可能已经对什么是**异步**请求有了一个大致的了解。让我们把它弄清楚。一个**异步**请求是客户端不等待响应的请求。什么都不堵。让我们通过一个非常常见的场景来理解这个概念。

在 web 世界中，攻击服务器来获取用户的详细信息、列表等数据是很常见的。我们知道这需要时间，任何事情都有可能发生(成功/失败)。

在这种情况下，我们不是等待数据到来，而是异步处理它(不等待)，这样我们的应用程序就不会被阻塞。这种请求是异步请求。我想现在我们都清楚了。因此，让我们看看我们实际上如何处理这些异步请求。

正如我已经告诉你的，Observables 给了我们一种处理异步请求的新方法。其他方式是承诺、回调和异步/等待。这些是流行的方式。让我们来看看其中的两个，分别是回调和承诺。

## 回收

回调是很常见的一种。回调函数(顾名思义)在后面调用。也就是说，当请求完成并返回数据或错误时，就会调用这些函数。为了更好地理解，请看一下代码:

```
const request = require(‘request’);
request('https://www.example.com', function (err, response, body) {
  if(error){
    // Error handling
  }
  else {
    // Success 
  }
});
```

这是处理异步请求的一种方式。但是，当我们在第一次请求成功后想要再次向服务器请求数据时，会发生什么呢？如果我们想在成功的第二次请求之后提出第三次请求呢？恐怖！

在这一点上，我们的代码将变得混乱，可读性更差。这叫做'**回调** **地狱**'。为了克服它，承诺出现了。它们提供了一种更好的处理异步请求的方式，提高了代码的可读性。让我们多了解一点。

## 承诺

承诺是承诺它们在不久的将来会有价值的对象——无论是成功还是失败。承诺都有自己的方法，分别是**然后**和**接住**。**。然后当成功到来时调用()**，否则调用 **catch()** 方法。**承诺** 是使用**承诺**构造函数创建的。看看代码可以更好地理解。

```
function myAsyncFunction(name){
     return new Promise(function(resolve, reject){
          if(name == ‘Anchal’){
               resolve(‘Here is Anchal’)
         }
         else{
              reject(‘Oops! This is not Anchal’)
        }

     }
} 

myAsyncFunction(‘Anchal’)
.then(function(val){
      // Logic after success
      console.log(val)     // output -  ‘Here is Anchal’
})
.catch(function(val){
    //Logic after failure
     console.log(val)     // output - ‘Oops! This is not Anchal’
})
```

如你所见， **myAsyncFunction** 实际上很有希望在不久的将来会有一些价值。**。然后()**或者**。catch()** 根据承诺的状态调用。

**承诺**提高**代码**可读性**。通过使用 promises 可以看出代码的可读性有多强。使用 Promises 可以更好地处理异步操作。这是一个什么是承诺，他们如何处理数据和美丽的承诺进行简单介绍。**

现在，是时候学习我们的主题了:可观测量。

## 什么是可观测量？

可观察性也类似于回调和承诺——负责处理异步请求。可观测量是 ***RXJS*** 库的一部分。这个库引入了可观测量。

在了解一个可观察对象实际上是什么之前，你必须了解两种沟通模式:**拉**和**推**。这两个概念是数据生产者如何与数据消费者通信的协议。

### 推拉模式

正如我已经告诉你的，推和拉是数据生产者和消费者之间的通信协议。让我们逐一了解这两者。

**拉模型:**在这个模型中，数据的**消费者**就是**国王**。这意味着数据的消费者决定何时需要来自生产者的数据。生产者并不决定数据何时交付。如果你把**功能**和它联系起来，你会更好理解。

众所周知，函数负责完成某些任务。例如， **dataProducer** 是一个简单返回字符串的函数，比如“**嗨** **可观察**”。

```
function dataProducer(){
   return ‘Hi Observable’;
}
```

现在，你可以看到上面的函数并不决定它什么时候发送“Hi Observable”字符串。它将由用户决定，即调用该函数的代码。消费者是王道。之所以称之为拉模式，是因为**拉**任务决定了通信。这是****拉** **型号**。现在，让我们进入**推** **模式** *。***

****推送模型:**在这个模型中，数据的**生产者**就是**国王**。生产者决定何时向消费者发送数据。消费者不知道数据何时会到来。我们举个例子来理解一下:**

**我希望你记得**的承诺** *。*是的，**承诺**跟随**推**型号 。承诺(生产者)将数据交付给回调(*)。然后()*——消费)。回调不知道数据何时到来。在这里，**承诺**(制片人)才是王道。它决定了交流。所以才叫**推** **模式**因为制片人说了算。**

**像承诺一样，可观察的事物也遵循推动模型。怎么会？一旦我详细阐述了可观测量，你就会得到答案。让我们回到可观的事情上来。**

## **作为函数的可观察量**

**为了简单理解，你可以把可观测量看作函数。让我们看看下面的例子:**

```
**`function dataProducer(){
    return ‘Hi Observable’
}

var result = dataProducer();
console.log(result) // output -  ‘Hi Observable’`** 
```

**你可以使用一个可观察的:**

```
**`var observable = Rx.Observable.create((observer: any) =>{

   observer.next(‘Hi Observable’);

})

observable.subscribe((data)=>{
   console.log(data);    // output - ‘Hi Observable’
})`**
```

**从上面，你可以看到函数和可观测量表现出相同的行为。这可能会给你带来一个问题——可观测量和功能是一样的吗？不。我马上会解释为什么答案是不。请看上面例子的详细版本。**

```
**`function dataProducer(){
    return ‘Hi Observable’;
    return ‘Am I understandable?’ // not a executable code.
}

var observable = Rx.Observable.create((observer: any) =>{

   observer.next(‘Hi Observable’);
   observer.next( ‘Am I understandable?’ );

})

observable.subscribe((data)=>{
   console.log(data);    
})

Output :
‘Hi Observable’
‘Am I understandable?’`** 
```

**我希望你现在能明白我想说明的区别。从上图可以看出，**无论是功能还是可观测量都很懒**。我们需要调用(函数)或订阅(观察值)来获得结果。**

**订阅观察值与调用函数非常相似。但是可观测量的不同之处在于它们返回**多个**值的能力，这些值被称为**流**(流是一段时间内的数据序列)。**

**Observables 不仅可以同步返回值**，还可以异步**返回值**。****

```
**`var observable = Rx.Observable.create((observer: any) =>{
   observer.next(‘Hi Observable’);
    setTimeout(()=>{
        observer.next(‘Yes, somehow understandable!’)
    }, 1000)   

   observer.next( ‘Am I understandable?’ );
})

output :

‘Hi Observable’
‘Am I understandable?’ 
Yes, somehow understandable!’.`** 
```

**简而言之，你可以说**可观察值仅仅是一个函数，它能够随时间给出多个值，或者同步或者异步** ***。*****

**你现在有了一个关于可观测量的大纲。但是让我们通过观察不同阶段的可观测量来更好地理解它们。**

## **可观察的阶段**

**从上面的例子中，我们已经看到了 observables 是如何通过订阅来创建、执行和发挥作用的。因此，可观测量经过了四个阶段。它们是:**

1.  ****创作****
2.  ****订阅。****
3.  ****执行****
4.  ****毁灭。****

 ****创建一个可观察的**是使用**创建** **函数**完成的。**

```
**`var observable = Rx.Observable.create((observer: any) =>{
})`** 
```

**为了让一个**可观察的** **工作**，我们不得不**订阅**它。这可以使用 subscribe 方法来完成。**

```
**`observable.subscribe((data)=>{
   console.log(data);    
})`**
```

**可观察对象的执行是 create 块内部的事情。让我借助一个例子来说明:**

```
**`var observable = Rx.Observable.create((observer: any) =>{

   observer.next(‘Hi Observable’);        
    setTimeout(()=>{
        observer.next(‘Yes, somehow understandable!’)
    }, 1000)   

   observer.next( ‘Am I understandable?’ );

})`** 
```

**上面 create 函数中的代码是可观察的执行。一个可观察对象可以传递给订户的三种类型的**值**是:**

```
**`observer.next(‘hii’);//this can be multiple (more than one)

observer.error(‘error occurs’) // this call whenever any error occus.

Observer.complete(‘completion of delivery of all values’) // this tells the subscriptions to observable is completed. No delivery is going to take place after this statement.`**
```

**让我们看看下面的内容，了解这三种价值观:**

```
**`var observable = Rx.Observable.create((observer: any) =>{
try {
   observer.next(‘Hi Observable’);                                       
    setTimeout(()=>{
        observer.next(‘Yes, somehow understandable!’)
    }, 1000)   

   observer.next( ‘Am I understandable?’ );

   observer.complete();

   observer.next(‘lAST DELIVERY?’ );  
   // above block is not going to execute as completion notification is      already sent.
   }
catch(err){
     observer.error(err);	
  }

})`** 
```

**进入市场的最后一个阶段是破坏。出现错误或完整通知后，可观察对象会自动退订。但是有些情况下，我们必须手动**取消订阅**它。要手动执行此任务，只需使用:**

```
**`var subscription = observable.subscribe(x => console.log(x)); // Later: subscription.unsubscribe();`**
```

**这都是关于一个可观察的事物经过的不同阶段。**

**我想，现在，我们知道什么是可观测量了吧？但是另一个问题是什么——可观察到的东西和承诺有什么不同？让我们找到它的答案。**

## **承诺与观察**

**众所周知，承诺是用来处理异步请求的，observables 也可以做到这一点。但是它们有什么不同呢？**

### **可观察的人是懒惰的，而承诺不是**

**这是不言自明的:可观测量是懒惰的，也就是说我们必须订阅可观测量才能得到结果。在承诺的情况下，他们立即执行。**

### **与承诺不同，可观察值处理多个值**

**承诺只能提供一个单一的价值，而可观察的可以给你多个价值。**

### **可观测量是可取消的**

**你可以通过使用 **unsubscribe** 方法取消订阅来取消观察，而承诺没有这样的功能。**

### **可观测量提供了许多运算符**

**像**映射*****forEach*****滤镜**等运算符有很多。Observables 提供这些，而 promises 的桶中没有任何运算符。****

***这些特点使得可观察到的东西不同于承诺。***

***现在，是时候结束了。希望你对可观测量这个热门话题有更深入的了解！***

***感谢阅读！***