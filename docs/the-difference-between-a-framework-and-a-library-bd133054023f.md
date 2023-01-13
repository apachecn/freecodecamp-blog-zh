# 框架和库的区别

> 原文：<https://www.freecodecamp.org/news/the-difference-between-a-framework-and-a-library-bd133054023f/>

开发人员经常互换使用术语“库”和“框架”。但是还是有区别的。

框架和库都是别人写的代码，用来帮助解决常见问题。

例如，假设你有一个程序，你打算用字符串工作。你决定保持你的代码干燥(不要重复你自己),写一些可重用的函数，像这样:

```
function getWords(str) {
   const words = str.split(' ');
   return words;
}
function createSentence(words) {
   const sentence = words.join(' ');
   return sentence;
}
```

恭喜你。你创建了一个图书馆。

框架或库没有任何魔力。库和框架都是别人写的可重用代码。它们的目的是帮助你以更简单的方式解决常见问题。

我经常用房子来比喻 web 开发概念。

图书馆就像去宜家。你已经有了一个家，但你需要一些家具方面的帮助。你不想从头开始做自己的桌子。宜家允许你挑选不同的物品放在家里。你在控制中。

另一方面，框架就像建造一个模型房屋。在建筑和设计方面，你有一套蓝图和一些有限的选择。最终，承包商和蓝图处于控制之中。他们会让你知道何时何地你可以提供你的意见。

#### 技术差异

框架和库之间的技术差异在于一个叫做控制反转的术语。

当你使用一个库时，你负责应用程序的流程。您正在选择何时何地调用库。当你使用框架时，框架负责流程。它为您插入代码提供了一些位置，但是它会根据需要调用您插入的代码。

让我们看一个使用 jQuery(一个库)和 Vue.js(一个框架)的例子。

假设我们希望在出现错误时显示一条错误消息。在我们的示例中，我们将单击一个按钮，并假装出现错误。

#### 使用 jQuery:

```
// index.html
<html>
   <head>
      <script src="https://code.jquery.com/jquery-3.3.1.min.js"
      </script>
      <script src="./app.js"></script>
   </head>
   <body>
      <div id="app">
         <button id="myButton">Submit</button>
       </div>
   </body>
</html>
// app.js
// A bunch of our own code, 
// followed by calling the jQuery library
let error = false;
const errorMessage = 'An Error Occurred';
$('#myButton').on('click', () => {
  error = true; // pretend some error occurs and set error = true
  if (error) {
    $('#app')
       .append(`<p id="error">${errorMessage}</p>`);
  } else {
    $('#error').remove();
  }
});
```

注意我们是如何使用 jQuery 的。我们告诉我们的程序我们想在哪里调用它。这很像去一个实体图书馆，从书架上取下我们想要的书。

这并不是说 jQuery 函数不需要某些输入*一旦*我们调用它们，但是 jQuery 本身就是这些函数的库。我们是负责人。

#### 用 Vue.js

```
//index.html
<html>
   <head>
      <script src="https://cdn.jsdelivr.net/npm/vue"></script>
      <script src="./app.js"></script>
   </head>
   <body>
      <div id="app"></div>
   </body>
</html>
const vm = new Vue({
  template: `<div id="vue-example">
               <button @click="checkForErrors">Submit</button>
               <p v-if="error">{{ errorMessage }}</p>
             </div>`,
  el: '#vue-example',
  data: {
    error: null,
    errorMessage: 'An Error Occurred',
  },
  methods: {
    checkForErrors()  {
      this.error = !this.error;
    },
  },
});
```

有了 Vue，我们就要填空了。Vue 构造函数是一个具有特定属性的对象。它告诉我们它需要什么，然后在幕后，Vue 决定它何时需要它。Vue 反转程序的控制。我们把代码插入 Vue。Vue 负责。

不管是库还是框架，区别就在于有没有控制反转。

#### 关于“固执己见”的一点注记

您经常会听到框架和库被描述为“固执己见”或“不固执己见”这些术语是主观的。他们试图定义开发人员在构建代码时的自由度。

框架更固执己见，因为——根据定义——控制反转需要应用程序设计自由的让步。

还是那句话，坚持己见的程度是主观的。例如，我个人认为 Angular 是一个非常固执己见的框架，而 Vue.js 是一个不那么固执己见的框架。

### 概括起来

*   框架和库都是由他人编写的代码，帮助您以一种不太冗长的方式执行一些常见的任务。
*   框架颠倒了程序的控制。它告诉开发者他们需要什么。图书馆不会。程序员在他们需要的时候调用这个库。
*   一个库或框架给予开发者的自由度将决定它有多“固执己见”。

感谢阅读！