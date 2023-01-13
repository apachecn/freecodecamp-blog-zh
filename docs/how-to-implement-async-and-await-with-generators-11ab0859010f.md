# 用生成器实现异步和等待

> 原文：<https://www.freecodecamp.org/news/how-to-implement-async-and-await-with-generators-11ab0859010f/>

如今，由于有了 **async** 和 **await** 关键字，我们可以用同步方式编写异步代码。这样更容易阅读和理解。然而，最近我想知道，如果不使用这些关键字，如何能达到同样的效果。

事实证明这非常简单，因为使用生成器可以很容易地模拟**异步**和**等待**的行为。让我们来看看！

继续，克隆[库](https://github.com/maciejcieslar/asynq)，让我们开始吧。

### 发电机

我假设你对生成器没有什么经验，因为，老实说，大多数时候它们不是特别有用，没有它们你也能轻松应付。所以不要担心，我们将从快速提醒开始。

发生器是由[发生器函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)创建的对象，这些函数的名称旁边有一个 *** (星号)。

这些生成器有一种惊人的能力，可以让我们通过使用关键字 **yield** 来停止代码的执行——只要我们愿意。

考虑这个例子:

```
const generator = (function*() {
  // waiting for .next()
  const a = yield 5;
  // waiting for .next()
  console.log(a); // => 15
})();

console.log(generator.next()); // => { done: false, value: 5 }
console.log(generator.next(15)); // => { done: true, value: undefined }
```

鉴于这些都是绝对的基础知识，我建议你在继续阅读之前，先阅读一下这篇文章，了解一下这里到底发生了什么。

如果你觉得你对潜在的想法有很强的理解，我们可以继续。

### 坚持住，等一分钟

你有没有想过**等待**到底是怎么运作的？

不知何故，它只是等待我们的承诺，返回一个值，并继续执行。对我来说，这似乎是一个发电机在稍加调整后就能做的事情。

我们所能做的就是获取每一个产出值，把它放入一个承诺中，然后等待这个承诺被解决。之后，我们只需通过调用*generator . next(resolved value)将其返回给生成器。*

听起来像个计划。但是首先，让我们写一些测试来确保一切都按预期运行。

我们的 **asynq** 函数应该做什么:

*   在继续执行之前等待异步代码
*   用函数的返回值返回一个**承诺**
*   让 **try/catch** 处理异步代码

注意:因为我们使用发电机，我们的**等待**变成**产出**。

```
import { asynq } from '../src';

describe('asynq core', () => {
  test('Waits for values (like await does)', () => {
    return asynq(function*() {
      const a = yield Promise.resolve('a');
      expect(a).toBe('a');
    });
  });

  test('Catches the errors', () => {
    return asynq(function*() {
      const err = new Error('Hello there');

      try {
        const a = yield Promise.resolve('a');
        expect(a).toBe('a');

        const b = yield Promise.resolve('b');
        expect(b).toBe('b');

        const c = yield Promise.reject(err);
      } catch (error) {
        expect(error).toBe(err);
      }

      const a = yield Promise.resolve(123);
      expect(a).toBe(123);
    });
  });

  test('Ends the function if the error is not captured', () => {
    const err = new Error('General Kenobi!');

    return asynq(function*() {
      const a = yield Promise.reject(err);
      const b = yield Promise.resolve('b');
    }).catch((error) => {
      expect(error).toBe(err);
    });
  });

  test('Returns a promise with the returned value', () => {
    return asynq(function*() {
      const value = yield Promise.resolve(5);
      expect(value).toBe(5);

      return value;
    }).then((value) => {
      expect(value).toBe(5);
    });
  });
});
```

好吧，太好了！现在我们可以谈谈实现了。

我们的 **asynq** 函数将函数生成器作为参数——通过调用它，我们创建了一个生成器。

为了确保万无一失，我们将 [*称为 isGeneratorLike*](https://github.com/maciejcieslar/asynq/blob/master/src/utils.ts) ，它检查接收到的值是否是一个对象，并具有方法 **next** 和 **throw** 。

然后，我们递归地通过调用 *generator.next(ensuredValue)来消耗每个 **yield** 关键字。*我们等待返回的承诺被结算，然后通过重复整个过程将其结果返回给生成器。

我们还必须附加**catch** 处理程序，这样，如果函数抛出异常，我们可以通过调用 *generator.throw(error)* 来捕捉它并在函数内部返回异常。

现在，任何潜在的错误都将由 **catch** 来处理。如果没有一个 **try/catch** 块，一个错误将简单地完全停止执行——就像任何未处理的异常一样——我们的函数将返回一个被拒绝的承诺。

当生成器完成时，我们在一个承诺中返回生成器的返回值。

```
import { isGeneratorLike } from './utils';

type GeneratorFactory = () => IterableIterator<any>;

function asynq(generatorFactory: GeneratorFactory): Promise<any> {
  const generator = generatorFactory();

  if (!isGeneratorLike(generator)) {
    return Promise.reject(
      new Error('Provided function must return a generator.'),
    );
  }

  return (function resolve(result) {
    if (result.done) {
      return Promise.resolve(result.value);
    }

    return Promise.resolve(result.value)
      .then((ensuredValue) => resolve(generator.next(ensuredValue)))
      .catch((error) => resolve(generator.throw(error)));
  })(generator.next());
}
```

现在，在运行了我们的测试之后，我们可以看到一切都在按预期工作。

### 包扎

虽然这个实现可能不是在 JavaScript 引擎中使用的，但是能够自己做这样的事情确实感觉很好。

请随意再看一遍代码。你对潜在思想的理解越好，你就越能欣赏到**异步**和**等待**关键词的创造者的才华。

非常感谢您的阅读！我希望你发现这篇文章信息丰富。我还希望它能帮助你明白 **async** 和 **await** 关键字并没有什么神奇之处，它们可以很容易地被生成器替换。

如果您有任何问题或意见，请随时在下面的评论区提出，或者给我发[消息](https://www.mcieslar.com/contact)。

查看我的[社交媒体](https://www.maciejcieslar.com/about/)！

[加入我的简讯](http://eepurl.com/dAKhxb)！

*最初发布于 2018 年 8 月 6 日[www.mcieslar.com](https://www.mcieslar.com/implementing-async-and-await-with-generators)。*