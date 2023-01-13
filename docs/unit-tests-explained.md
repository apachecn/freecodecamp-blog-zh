# 单元测试解释

> 原文：<https://www.freecodecamp.org/news/unit-tests-explained/>

单元测试是一种位于软件测试金字塔底部的测试。它包括将代码库分解成更小的部分(或单元)并单独测试。

根据编程语言(或范式)的类型，这些可以与您定义为一个单元的任何东西相冲突，尽管最常见的做法是与函数相冲突。

### 为什么要这么做？

*   ****保护****——单元测试防止引入新的或旧的防御性编程错误
*   **——你可以添加变更，或者重用或重构代码(这两种情况都很常见)，并确保你没有添加 bug**
*   ******文档****——单元测试记录了代码的行为和流程，因此对于代码新手来说很容易理解**
*   ******隔离**** -它将一个模块从整个特征中隔离出来。这种方法迫使你去思考一个模块本身，并问它的工作是什么？**
*   ******质量****——当单元测试迫使你思考和使用你自己的 API 时，它强制执行好的/可扩展的接口和模式。它可以指出应该解决的任何紧密耦合或过于复杂的问题。糟糕的代码通常更难测试**
*   ****——单元测试如今是一个常见的规程，并且是大部分软件公司的要求****
*   ********更少的 bug****——大量研究表明，对应用程序进行测试可以减少 40% — 80%的生产 bug 密度。****

### ****示例(在 JavaScript 中)****

**假设有一个函数写在文件 ****add.js******

```
`var add = function(number1, number2){
  return number1 + number2;
}`
```

**现在，为了编写这个特定函数的单元测试，我们可以使用像 [mocha](http://mochajs.org/) 这样的测试工具:**

```
`const mocha = require('mocha')
const chai = require('chai')  // It is an assertion library
describe('Test to check add function', function(){
  it('should add two numbers', function(){
    (add(2,3)).should.equal(5)  //Checking that 2+3 should equal 5 using the given add function
  });
});`
```

### **测试驱动开发**

**单元测试是软件开发的测试驱动开发(TDD)方法的一个关键特征。**

**在这种方法中，通过重复使用非常短的周期来编写特定特性或功能的代码。**

**首先，开发人员编写一组自动化单元测试，并确保它们最初会失败。接下来，开发人员实现通过测试用例所需的最少量的代码。**

**一旦验证了代码的行为符合预期，开发人员就可以返回并重构代码，以符合任何相关的编码标准。**

## **关于单元测试的更多信息:**

*   **[Python 中单元测试的介绍](https://www.freecodecamp.org/news/an-introduction-to-testing-in-python/)**
*   **[Jasmine 单元测试简介](https://www.freecodecamp.org/news/jasmine-unit-testing-tutorial-4e757c2cbf42/)**
*   **[如何对你的第一个 Vue.js 组件进行单元测试](https://www.freecodecamp.org/news/how-to-unit-test-your-first-vue-js-component-14db6e1e360d/)**

## **关于测试驱动开发的更多信息:**

*   **测试驱动开发的实用介绍**
*   **[TDD:是什么，不是什么](https://www.freecodecamp.org/news/test-driven-development-what-it-is-and-what-it-is-not-41fa6bca02a2/)**
*   **[用 Jest 快速介绍 TDD](https://www.freecodecamp.org/news/a-quick-introduction-to-test-driven-development-with-jest-cac71cb94e50/)**
*   **[为什么 TDD 值得你花时间](https://www.freecodecamp.org/news/test-driven-development-i-hated-it-now-i-cant-live-without-it-4a10b7ce7ed6/)**