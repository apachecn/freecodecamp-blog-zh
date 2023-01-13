# 函数式编程基础介绍

> 原文：<https://www.freecodecamp.org/news/intro-to-functional-programming-basics/>

## **功能编程**

函数式编程是通过组合**，避免 ****【共享状态】********可变数据********副作用**** 来构建软件的过程。函数式编程是 ****声明式**** (告诉计算机你想做什么)而不是 ****命令式**** (告诉计算机具体怎么做)，应用程序状态流经纯函数。与面向对象的编程形成对比，在面向对象的编程中，应用程序状态通常是共享的，并与对象中的方法放在一起。**

### ****为什么要进行函数式编程？****

*   **一般来说更简洁**
*   **一般来说更容易预测**
*   **它比面向对象的代码更容易测试**

### ****编程语言采用****

**许多编程语言都支持函数式编程，如 Haskell、F#、Common Lisp、Clojure、Erlang、OCaml。JavaScript 还具有非类型化函数语言的属性。**

### ****用途****

**函数式编程在学术界已经流行很久了，但是很少有工业应用。然而，最近几个著名的函数式编程语言已经在商业或工业系统中使用。例如，瑞典爱立信公司在 20 世纪 80 年代末开发的 Erlang 编程语言被用于构建 T-Mobile、北电、脸书、法国电力和 WhatsApp 等公司的一系列应用程序。**

### ****高阶函数****

**高阶函数是函数式编程的一大部分。高阶函数是将函数作为参数或返回函数的函数。**

### ****地图****

**`map`是一个高阶函数，它对列表中的每个元素调用一个函数，并将结果作为一个*新的*列表返回。**

**这里有一个`map`的例子:**

```
`const myList = [6, 3, 5, 29];

let squares = myList.map(num => num * num); // [36, 9, 25, 841]`
```

### ****关于函数式编程的更多信息:****

*   **[学习 Java 中的函数式编程-全教程(视频)](https://www.freecodecamp.org/news/functional-programming-in-java-course/)**
*   **[学习函数式编程如何让你成为更好的开发人员](https://www.freecodecamp.org/news/learn-the-fundamentals-of-functional-programming/)**
*   **[如何在现代 JavaScript 中使用函数式编程](https://www.freecodecamp.org/news/how-and-why-to-use-functional-programming-in-modern-javascript-fda2df86ad1b/)**