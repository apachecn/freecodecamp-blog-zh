# 测试驱动开发简介

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-test-driven-development-c4de6dce5c/>

我已经编程五年了，老实说，我一直避免测试驱动的开发。我没有回避，因为我觉得这不重要。事实上，这似乎非常重要——而是因为我觉得不做这件事太舒服了。这已经改变了。

### 什么是测试？

测试是确保程序接收正确的输入并产生正确的输出和预期的副作用的过程。我们用*规范*定义这些正确的输入、输出和副作用。您可能已经看到了命名约定为`filename.spec.js`的测试文件。`spec`代表规格。在这个文件中，我们指定或*断言*我们的代码应该做什么，然后测试它以验证它是否做到了。

谈到测试，您有两种选择:手工测试和自动化测试。

#### 人工测试

手动测试是从用户的角度检查应用程序或代码的过程。打开浏览器或程序并四处导航，试图测试功能并查找错误。

#### 自动化测试

另一方面，自动化测试是编写代码来检查其他代码是否工作。与手工测试相反，规格在不同的测试中保持不变。最大的优势是能够更快地测试许多东西。

这两种测试技术的结合将尽可能多地清除错误和意想不到的副作用，并确保您的程序做您所说的事情。本文的重点是自动化测试，尤其是单元测试。

> 自动化测试主要有两种类型:单元测试和端到端测试(E2E)。E2E 测试将应用程序作为一个整体来测试。单元测试测试最小的代码片段或单元。什么是单位？我们定义了什么是单元，但一般来说，它是应用程序功能中相对较小的一部分。

#### 概述:

1.  测试就是验证我们的应用程序做了它应该做的事情。
2.  有两种类型的测试:手动和自动
3.  测试*断言*你的程序会以某种方式运行。然后测试本身证明或否定这一主张。

### 测试驱动开发

测试驱动开发是这样的行为:首先决定你希望你的程序做什么(规范)，制定一个失败的测试，*然后*编写代码使测试通过。它通常与自动化测试联系在一起。尽管您也可以将这些原则应用到手动测试中。

让我们看一个简单的例子:建造一个木制的桌子。传统上，我们会做一个表，然后一旦表做好了，测试它以确保它能做，嗯，表应该做什么。另一方面，TDD 会让我们首先定义表应该做什么。然后，当它不做这些事情时，添加最小量的“表”来使每个单元工作。

下面是一个构建木制桌子的 TDD 示例:

```
I expect the table to be four feet in diameter.

The test fails because I have no table.

I cut a circular piece of wood four feet in diameter.

The test passes.

__________

I expect the table to be three feet high.

The test fails because it is sitting on the ground.

I add one leg in the middle of the table.

The test passes.

__________

I expect the table to hold a 20-pound object.

The test fails because when I place the object on the edge, it makes the table fall over since there is only one leg in the middle.

I move the one leg to the outer edge of the table and add two more legs to create a tripod structure.

The test passes.
```

这将继续下去，直到表完成。

#### 概述

1.  使用 TDD，测试逻辑先于应用逻辑。

### 实际例子

想象一下，我们有一个管理用户及其博客文章的程序。我们需要一种方法来更精确地跟踪用户在我们的数据库中写的帖子。现在，用户是一个具有名称和电子邮件属性的对象:

```
user = { 
   name: 'John Smith', 
   email: 'js@somePretendEmail.com' 
}
```

我们将跟踪用户在同一个用户对象中创建的帖子。

```
user = { 
   name: 'John Smith', 
   email: 'js@someFakeEmailServer.com'
   posts: [Array Of Posts] // <-----
}
```

每篇文章都有标题和内容。我们不是存储每个用户的整篇文章，而是存储一些可以用来引用文章的独特内容。我们首先想到的是存储标题。但是，如果用户更改了标题，或者——尽管有点不太可能——两个标题完全相同，那么引用该博客文章就会有一些问题。相反，我们将为每篇博客文章创建一个惟一的 ID，并将它存储在`user`对象中。

```
user = { 
   name: 'John Smith', 
   email: 'js@someFakeEmailServer.com'
   posts: [Array Of Post IDs]
}
```

#### 设置我们的测试环境

对于这个例子，我们将使用 Jest。Jest 是一个测试套件。通常，你需要一个测试库和一个独立的断言库，但是 Jest 是一个一体化的解决方案。

> 断言库允许我们对代码做出断言。所以在我们的木桌例子中，我们的断言是:“我期望桌子能装下一个 20 磅的物体。”换句话说，我是在断言这个表应该做什么。

#### 项目设置

1.  创建一个 NPM 项目:`npm init`。
2.  创建`id.js`并将其添加到项目的根目录。
3.  安装笑话:`npm install jest --D`
4.  更新 package.json `test`脚本

```
// package.json

{
   ...other package.json stuff
   "scripts": {   
     "test": "jest" // this will run jest with "npm run test"
   }
}
```

项目设置到此结束！我们不会有任何 HTML 或任何样式。我们纯粹从单元测试的角度来处理这个问题。信不信由你，我们现在有足够的时间来开玩笑。

在命令行中，运行我们的测试脚本:`npm run test`。

您应该会收到一个错误:

```
No tests found
In /****/
  3 files checked.
  testMatch: **/__tests__/**/*.js?(x),**/?(*.)+(spec|test).js?(x) - 0 matches
  testPathIgnorePatterns: /node_modules/ - 3 matches
```

Jest 正在寻找具有某些特定特征的文件名，例如文件名中包含的`.spec`或`.test`。

我们把`id.js`更新为`id.spec.js`。

再次运行测试

您应该会收到另一个错误:

```
FAIL  ./id.spec.js
  ● Test suite failed to run

Your test suite must contain at least one test.
```

稍微好一点的是，它找到了文件，但不是测试。那是有道理的；这是个空文件。

#### 我们如何写一个测试？

测试只是接收几个参数的函数。我们可以用`it()`或`test()`来调用我们的测试。

> `it()`是`test()`的别名。

让我们写一个非常基本的测试来确保 Jest 正常工作。

```
// id.spec.js

test('Jest is working', () => {
   expect(1).toBe(1);
});
```

再次运行测试。

```
PASS  ./id.spec.js
  ✓ Jest is working (3ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.254s
Ran all test suites.
```

我们通过了第一次测试！让我们分析测试和结果输出。

我们传递一个标题或描述作为第一个参数。

`test('Jest is Working')`

我们传递的第二个参数是一个函数，在这个函数中，我们实际上声明了一些关于代码的内容。虽然，在这种情况下，我们并没有断言关于我们代码的某些东西，而是一些总的来说将会通过的真理，一种健全性检查。

`...() => { expect(1).toBe(1)` })；

这个断言在数学上是正确的，所以这是一个简单的测试来确保我们已经正确地连接了 Jest。

结果告诉我们测试是通过还是失败。它还告诉我们测试和测试套件的数量。

#### 关于组织我们的测试的一个旁注

还有另一种方法来组织我们的代码。我们可以将每个测试包装在一个`describe`函数中。

```
describe('First group of tests', () => {
   test('Jest is working', () => {
      expect(1).toBe(1);
   });
});

describe('Another group of tests', () => {
   // ...more tests here
});
```

允许我们将测试分成几个部分:

```
PASS  ./id.spec.js
  First group of tests
    ✓ Jest is working(4ms)
    ✓ Some other test (1ms)
  Another group of tests
    ✓ And another test
    ✓ One more test (12ms)
    ✓ And yes, one more test
```

我们不会使用`describe`，但是*比看不到包装测试的`describe`函数更常见的是*。或者甚至几个`describes`——也许我们测试的每个文件都有一个。出于我们的目的，我们将只关注`test`，并保持文件相当简单。

#### 基于规格的测试

尽管坐下来开始输入应用程序逻辑很诱人，但是一个精心制定的计划将使开发变得更容易。我们需要定义我们的程序将做什么。我们用规范来定义这些目标。

我们对这个项目的高级规范是创建一个惟一的 ID，尽管我们应该把它分解成更小的单元来测试。对于我们的小项目，我们将使用以下规范:

1.  创建一个随机数
2.  该数字是一个整数。
3.  创建的数字在指定的范围内。
4.  号码是唯一的。

#### 概述

1.  Jest 是一个测试套件，有一个内置的断言库。
2.  测试只是一个函数，它的参数定义了测试。
3.  规格说明定义了我们的代码应该做什么，并且最终是我们测试的内容。

### 规范 1:创建一个随机数

JavaScript 有一个创建随机数的内置函数—`Math.random()`。我们的第一个单元测试将查看是否创建并返回了一个随机数。我们想要做的是使用`math.random()`创建一个数字，然后确保返回的是这个数字。

因此，您可能会认为我们会做如下事情:

`expect(our-functions-output).toBe(some-expected-value)`。我们的返回值是随机的，问题是我们无法知道会发生什么。我们需要将`Math.random()`函数重新赋值给某个常量值。这样，当我们的函数运行时，Jest 用常量替换了`Math.random()`。这个过程叫做*嘲讽。*所以，我们真正测试的是`Math.random()`被调用并返回一些我们可以计划的期望值。

现在，Jest 也提供了一种证明函数被调用的方法。然而，在我们的例子中，这个断言仅仅保证了我们在代码中的某个地方调用了`Math.random()`。它不会告诉我们`Math.random()`的结果也是返回值。

> 你为什么想要模仿一个函数？重点不就是测试真正的代码吗？是也不是。许多函数包含我们无法控制的东西，例如 HTTP 请求。我们并不想测试这段代码。我们假设这些依赖项会做它们应该做的事情，或者创建模拟它们行为的虚拟函数。如果这些是我们编写的依赖项，我们可能会为它们编写单独的测试。

将以下测试添加到`id.spec.js`

```
test('returns a random number', () => {
   const mockMath = Object.create(global.Math);
   mockMath.random = jest.fn(() => 0.75);
   global.Math = mockMath;
   const id = getNewId();
   expect(id).toBe(0.75);
});
```

#### 分解上面的测试

首先，我们复制全局数学对象。然后我们改变`random`方法来返回一个常量值，我们可以*期望*。最后，我们用模拟的`Math`对象替换全局的`Math`对象。

我们应该从一个函数中获得一个 ID(我们还没有创建这个函数——记住这个 TDD)。然后，我们期望 ID 等于 0.75——我们模拟的返回值。

> 注意，我选择使用 Jest 为模仿函数提供的内置方法:`jest.fn()`。我们也可以传入一个匿名函数。但是，我想向您展示这种方法，因为在我们的测试中，有时需要一个 Jest-mock 函数来实现其他功能。

运行测试:`npm run test`

```
FAIL  ./id.spec.js
✕ returns a random number (4ms)
● returns a random number
   ReferenceError: getNewId is not defined
```

注意，我们得到了一个引用错误，就像我们应该得到的那样。我们的测试找不到我们的`getNewId()`。

在测试上方添加以下代码。

```
function getNewId() {
   Math.random()
}
```

> 为了简单起见，我将代码和测试放在同一个文件中。正常情况下，测试将被写在一个单独的文件中，任何依赖项在需要时被导入。

```
FAIL  ./id.spec.js
   ✕ returns a random number (4ms)
   ● returns a random number

   expect(received).toBe(expected) // Object.is equality
   Expected: 0.75
   Received: undefined
```

我们又失败了，原因是所谓的*断言错误*。我们的第一个错误是引用错误。第二个错误告诉我们它收到了`undefined`。但是我们调用了`Math.random()`那么发生了什么？记住，不显式返回的函数将隐式返回`undefined`。这个错误是一个很好的提示，说明有些东西没有被定义，比如变量，或者，就像我们的例子一样，我们的函数没有返回任何东西。

将代码更新为以下内容:

```
function getNewId() {
   return Math.random()
}
```

运行测试

```
PASS  ./id.spec.js
✓ returns a random number (1ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
```

恭喜你！我们通过了第一次测试。

> 理想情况下，我们希望尽可能快地发现断言错误。断言错误——特别是像这样的*值断言错误*,尽管我们稍后会谈到*布尔断言错误*——给我们提示什么是错误的。

### 规范 2:我们返回的数字是一个整数。

`Math.random()`生成一个介于 0 和 1 之间的数字(不含 0 和 1)。我们现有的代码永远不会生成这样的整数。不过没关系，这是 TDD。我们将检查一个整数，然后编写将我们的数字转换成整数的逻辑。

那么，我们如何检查一个数是否是整数呢？我们有几个选择。回想一下，我们嘲笑了上面的`Math.random()`，我们返回的是一个常量值。事实上，我们也在创建一个实值，因为我们返回的是一个介于 0 和 1 之间的数字(不包括 0 和 1)。例如，如果我们返回一个字符串，我们就不能通过这个测试。或者另一方面，如果我们为模拟值返回一个整数，测试将总是(错误地)通过。

因此，一个关键的要点是，如果您要使用模拟的返回值，它们应该是真实的，这样我们的测试就可以使用这些值返回有意义的信息。

另一种选择是使用`Number.isInteger()`，将我们的 ID 作为参数传递，并查看它是否返回 true。

最后，在不使用模拟值的情况下，我们可以将得到的 ID 与其整数版本进行比较。

让我们看看选项 2 和 3。

**选项 2:使用 Number.isInteger()**

```
test('returns an integer', () => {
   const id = getRandomId();
   expect(Number.isInteger(id)).toBe(true);
});
```

测试失败是理所当然的。

```
FAIL  ./id.spec.js
✓ returns a random number (1ms)
✕ returns an integer (3ms)

● returns an integer
expect(received).toBe(expected) // Object.is equality

Expected: true
Received: false
```

测试失败，出现*布尔断言错误*。回想一下，测试失败有多种方式。我们希望它们因断言错误而失败。换句话说，我们的主张并不像我们所说的那样。但是更重要的是，我们希望我们的测试因为*值断言错误*而失败。

布尔断言错误(真/假错误)不能给我们很多信息，但是值断言错误可以。

让我们回到我们的木制桌子的例子。现在请原谅我，下面的两个陈述可能看起来有些尴尬和难以理解，但它们在这里是为了强调一点:

首先，你可能断言**桌子是蓝色的**是真的。在另一个断言中，你可以断言**桌子的颜色是蓝色的**。我知道，这很难说出口，甚至看起来像是相同的断言，但它们不是。看看这个:

`expect(table.isBlue).toBe(true)`

相对

`expect(table.color).toBe(blue)`

假设表不是蓝色的，第一个示例错误将告诉我们它预期为真，但收到了假。你不知道桌子是什么颜色。我们很可能已经完全忘记去画它了。然而，第二个示例错误可能告诉我们它期望蓝色，但收到红色。第二个例子提供了更多的信息。它能更快地指出问题的根源。

让我们使用选项 2 重写测试，以接收值断言错误。

```
test('returns an integer', () => {
   const id = getRandomId();
   expect(id).toBe(Math.floor(id));
});
```

我们希望从函数中得到的 ID 等于这个 ID 的底值。换句话说，如果我们得到的是一个整数，那么这个整数的底等于这个整数本身。

```
FAIL  ./id.spec.js
✓ returns a random number (1ms)
✕ returns an integer (4ms)
● returns an integer
expect(received).toBe(expected) // Object.is equality

Expected: 0
Received: 0.75
```

哇，这个函数碰巧返回模拟值的几率有多大！嗯，实际上是 100%。尽管我们的模拟值似乎只限于第一个测试，但我们实际上是在重新分配全局值。因此，无论重新赋值是如何嵌套的，我们都在改变全局`Math`对象。

如果我们想在每次测试前改变一些东西，有一个更好的地方放它。Jest 为我们提供了一种`beforeEach()`方法。我们传入一个函数，它在每次测试之前运行我们想要运行的任何代码。例如:

```
beforeEach(() => {
   someVariable = someNewValue;
});

test(...)
```

出于我们的目的，我们不会使用这个。但是让我们稍微改变一下我们的代码，以便将全局`Math`对象重置回默认值。回到第一个测试，按如下方式更新代码:

```
test('returns a random number', () => {
   const originalMath = Object.create(global.Math);
   const mockMath = Object.create(global.Math);
   mockMath.random = () => 0.75;
   global.Math = mockMath;
   const id = getNewId();
   expect(id).toBe(0.75);
   global.Math = originalMath;
});
```

我们在这里做的是在覆盖任何对象之前保存默认的`Math`对象，然后在测试完成后重新分配它。

让我们再次运行我们的测试，特别关注我们的第二个测试。

```
✓ returns a random number (1ms)
✕ returns an integer (3ms)
● returns an integer
expect(received).toBe(expected) // Object.is equality

Expected: 0
Received: 0.9080890805713182
```

因为我们已经更新了我们的第一个测试，回到默认的`Math`对象，我们现在真正得到了一个随机数。就像之前的测试一样，我们期望得到一个整数，或者换句话说，生成的数字的下限。

更新我们的应用逻辑。

```
function getRandomId() {
   return Math.floor(Math.random()); // convert to integer
}

FAIL  ./id.spec.js
✕ returns a random number (5ms)
✓ returns an integer
● returns a random number
expect(received).toBe(expected) // Object.is equality
Expected: 0.75
Received: 0
```

哦，我们的第一次测试失败了。发生了什么事？

因为我们在嘲笑我们的返回值。无论如何，我们的第一个测试返回 0.75。然而，我们期望得到 0(0.75 的底数)。也许检查一下`Math.random()`是否被调用会更好。虽然，这有点无意义，因为我们可以在代码中的任何地方调用`Math.random()`，但永远不要使用它，测试仍然通过。也许，我们应该测试我们的函数是否返回一个数字。毕竟我们的 ID 必须是一个数字。同样，我们已经在测试我们是否接收到一个整数。而且所有的整数都是数字；那个测试是多余的。但是还有一个测试我们可以试试。

当该说的都说了，该做的都做了，我们期望得到一个整数。我们知道我们将使用`Math.floor()`来这样做。因此，也许我们可以检查一下`Math.floor()`是否以`Math.random()`作为参数被调用。

```
test('returns a random number', () => {
   jest.spyOn(Math, 'floor'); // <--------------------changed
   const mockMath = Object.create(global.Math); 
   const globalMath = Object.create(global.Math);
   mockMath.random = () => 0.75;
   global.Math = mockMath;
   const id = getNewId();
   getNewId(); //<------------------------------------changed
   expect(Math.floor).toHaveBeenCalledWith(0.75); //<-changed
   global.Math = globalMath;
});
```

我已经注释了我们更改的行。首先，将您的注意力转移到片段的结尾。我们断言调用了一个函数。现在，回到第一个变化:`jest.spyOn()`。为了观察一个函数是否被调用，jest 要求我们要么模仿这个函数，要么监视它。我们已经看到了如何模仿一个函数，所以在这里我们窥探一下`Math.floor()`。最后，我们做的另一个改变是简单地调用`getNewId()`,而不将其返回值赋给变量。我们没有使用 ID，我们只是断言它调用了一些带有参数的函数。

运行我们的测试

```
PASS  ./id.spec.js
✓ returns a random number (1ms)
✓ returns an integer

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
```

祝贺第二次成功测试。

### 规格 3:数量在规定范围内。

我们知道`Math.random()`返回一个介于 0 和 1(不含)之间的随机数。如果开发者想返回一个介于 3 和 10 之间的数字，她能做什么？

下面是答案:

`Math.floor(Math.random() * (max — min + 1))) + min;`

上述代码将产生一个范围内的随机数。让我们看两个例子来说明它是如何工作的。我将模拟创建两个随机数，然后应用公式的其余部分。

**例子:**3 到 10 之间的数字。我们的随机数是. 001 和. 999。我选择了极值作为随机数，所以你可以看到最终结果在范围内。

`0.001 * (10-3+1) + 3 = 3.008`那一层是`3`

`0.999 * (10-3+1) + 3 = 10.992`那一层是`10`

让我们写一个测试

```
test('generates a number within a specified range', () => {
   const id = getRandomId(10, 100);
   expect(id).toBeLessThanOrEqual(100);
   expect(id).toBeGreaterThanOrEqual(10);
});

FAIL  ./id.spec.js
✓ returns a random number (1ms)
✓ returns an integer (1ms)
✕ generates a number within a specified range (19ms)

● generates a number within a specified range
expect(received).toBeGreaterThanOrEqual(expected)

Expected: 10
Received: 0
```

在我们更新代码之前,`Math.random()`的下限将始终为 0。更新代码。

```
function getRandomId(min, max) {
   return Math.floor(Math.random() * (max - min + 1) + min);
}

FAIL  ./id.spec.js
✕ returns a random number (5ms)
✓ returns an integer (1ms)
✓ generates a number within a specified range (1ms)

● returns a random number

expect(jest.fn()).toHaveBeenCalledWith(expected)

Expected mock function to have been called with:

0.75 as argument 1, but it was called with NaN.
```

哦不，我们的第一次测试又失败了！发生了什么事？

很简单，我们的测试断言我们正在用`0.75`调用`Math.floor()`。然而，我们实际上用 0.75 加上和减去一个还没有定义的最大和最小值来调用它。在这里，我们将重新编写第一个测试，以包括我们的一些新知识。

```
test('returns a random number', () => {
   jest.spyOn(Math, 'floor');
   const mockMath = Object.create(global.Math);
   const originalMath = Object.create(global.Math);
   mockMath.random = () => 0.75;
   global.Math = mockMath;
   const id = getNewId(10, 100);
   expect(id).toBe(78);
   global.Math = originalMath;
});

PASS  ./id.spec.js
✓ returns a random number (1ms)
✓ returns an integer
✓ generates a number within a specified range (1ms)

Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total
```

我们做了一些相当大的改变。我们已经向我们的函数传递了一些样本数(10 和 100 作为最小值和最大值)，并且我们再次更改了我们的断言来检查某个返回值。我们可以这样做，因为我们知道如果调用了`Math.random()`，该值将被设置为 0.75。并且，当我们将我们的最小和最大计算应用于`0.75`时，我们每次都会得到相同的数字，在我们的例子中是 78。

现在我们不得不开始怀疑这是否是一个好的测试。我们不得不回去重新塑造我们的测试来适应我们的代码。这有点违背 TDD 的精神。TDD 说改变你的代码以使测试通过，而不是改变测试以使测试通过。如果你发现自己试图修改测试以使它们通过，这可能是一个糟糕的测试的迹象。然而，我想把测试留在这里，因为有几个好的概念。然而，我强烈建议你考虑这样一个测试的功效，以及一个更好的写测试的方法，或者是否有必要包含它。

让我们回到第三个测试，它生成一个范围内的数字。

我们看到它已经过去，但我们有一个问题。你能想到吗？

我想知道的问题是我们是否只是运气好？我们只生成了一个随机数。这个数字刚好在范围内并通过测试的可能性有多大？

幸运的是，在这里，我们可以从数学上证明我们的代码是有效的。然而，为了好玩(如果你能称之为好玩的话)，我们将把我们的代码包装在一个运行 100 次的`for loop`中。

```
test('generates a number within a defined range', () => {
   for (let i = 0; i < 100; i ++) {
      const id = getRandomId(10, 100);    

      expect(id).toBeLessThanOrEqual(100);
      expect(id).toBeGreaterThanOrEqual(10);
      expect(id).not.toBeLessThan(10);
      expect(id).not.toBeGreaterThan(100);
   }
});
```

我添加了一些新的断言。我使用`.not`只是为了演示其他可用的 Jest API。

```
PASS  ./id.spec.js
  ✓ is working (2ms)
  ✓ Math.random() is called within the function (3ms)
  ✓ receives an integer from our function (1ms)
  ✓ generates a number within a defined range (24ms)

Test Suites: 1 passed, 1 total
Tests:       4 passed, 4 total
Snapshots:   0 total
Time:        1.806s
```

通过 100 次迭代，我们可以相当自信地认为我们的代码将我们的 ID 保持在指定的范围内。你也可以为了更多的确认而故意让测试失败。例如，您可以将其中一个断言更改为 *not* expect 一个大于 50 的值，但仍然将 100 作为最大参数传入。

#### 在一个测试中使用多个断言可以吗？

是的。这并不是说您不应该尝试将这些多重断言简化为一个更健壮的断言。例如，我们可以重写我们的测试，使之更加健壮，并将我们的断言减少到只有一个。

```
test('generates a number within a defined range', () => {
   const min = 10;
   const max = 100;
   const range = [];
   for (let i = min; i < max+1; i ++) {
     range.push(i);
   }
   for (let i = 0; i < 100; i ++) {
      const id = getRandomId(min, max);
      expect(range).toContain(id);
   }
});
```

这里，我们创建了一个数组，包含我们范围内的所有数字。然后我们检查 ID 是否在数组中。

### 规范 4:编号是唯一的

我们如何检查一个数字是否是唯一的？首先，我们需要定义独特对我们意味着什么。最有可能的是，在我们应用程序的某个地方，我们可以访问所有正在使用的 ID。我们的测试应该断言生成的数字不在当前 id 列表中。有几种不同的方法可以解决这个问题。我们可以使用之前看到的`.not.toContain()`，或者使用带有`index`的东西。

#### **indexOf()**

```
test('generates a unique number', () => {
   const id = getRandomId();
   const index = currentIds.indexOf(id);
   expect(index).toBe(-1);
});
```

`array.indexOf()`返回传入元素在数组中的位置。如果数组不包含元素，它返回`-1`。

```
FAIL  ./id.spec.js
✓ returns a random number (1ms)
✓ returns an integer
✓ generates a number within a defined range (25ms)
✕ generates a unique number (10ms)

● generates a unique number

ReferenceError: currentIds is not defined
```

测试失败，出现引用错误。`currentIds`未定义。让我们添加一个数组来模拟一些可能已经存在的 ID。

```
const currentIds = [1, 3, 2, 4];
```

重新运行测试。

```
PASS  ./id.spec.js
✓ returns a random number (1ms)
✓ returns an integer
✓ generates a number within a defined range (27ms)
✓ generates a unique number

Test Suites: 1 passed, 1 total

Tests:       4 passed, 4 total
```

当测试通过时，这应该会再次引发一个危险信号。我们绝对没有任何东西可以确保这个号码是唯一的。发生了什么事？

再说一次，我们越来越幸运。事实上，*你的*测试可能失败了。尽管如果你一遍又一遍地运行它，由于`currentIds`的大小，你可能会得到两者的混合，通过次数远远多于失败次数。

我们可以尝试用一个`for loop`包起来。一个足够大的`for loop`可能会导致我们失败，尽管它们都有可能通过。我们能做的是检查我们的`getNewId()`函数是否能在某个数字是唯一的或不唯一的时候有自我意识。

比如说。我们可以设置`currentIds = [1, 2, 3, 4, 5]`。然后调用`getRandomId(1, 5)`。我们的函数应该意识到，由于约束，它不能生成任何值，并传回某种错误消息。我们可以测试错误信息。

```
test('generates a unique number', () => {
   mockIds = [1, 2, 3, 4, 5];
   let id = getRandomId(1, 5, mockIds);
   expect(id).toBe('failed');

   id = getRandomId(1, 6, mockIds);
   expect(id).toBe(6);
});
```

有几件事需要注意。有两种说法。在第一个断言中，我们预计我们的函数会失败，因为我们以一种不应该返回任何数字的方式约束了它。在第二个例子中，我们对它进行了约束，使它只能返回`6`。

```
FAIL  ./id.spec.js
✓ returns a random number (1ms)
✓ returns an integer (1ms)
✓ generates a number within a defined range (24ms)
✕ generates a unique number (6ms)

● generates a unique number

expect(received).toBe(expected) // Object.is equality

Expected: "failed"
Received: 1
```

我们的测试失败了。由于我们的代码不检查任何东西或返回`failed`，这是意料之中的。虽然，你的代码可能收到了 2 到 6。

如何检查我们的函数*找不到*唯一的数字？

首先，我们需要进行某种循环，继续创建数字，直到找到一个有效的数字。但是在某些时候，如果没有有效的数字，我们需要退出循环，这样我们就避免了无限循环的情况。

我们要做的是跟踪我们创建的每个数字，当我们创建了所有可能的数字，并且没有一个数字通过我们的唯一检查时，我们将打破循环并提供一些反馈。

```
function getNewId(min = 0, max = 100, ids =[]) {
   let id;
   do {
      id = Math.floor(Math.random() * (max - min + 1)) + min;
   } while (ids.indexOf(id) > -1);
   return id;
}
```

首先，我们重构了`getNewId()`以包含一个当前 ID 列表的参数。此外，我们更新了参数，以便在没有指定参数的情况下提供默认值。

其次，我们使用一个`do-while`循环，因为我们不知道创建一个唯一的随机数需要多少次。例如，我们可以指定一个从 1 到 1000 的数字，只有*不可用的*数字是 7。换句话说，我们当前的 ID 只有一个 7。虽然我们的函数有 999 个其他数字可供选择，但理论上它可以一次又一次地产生数字 7。虽然这不太可能，但我们使用了一个`do-while`循环，因为我们不确定它会运行多少次。

此外，请注意，当我们的 ID *是*唯一的时候，我们就跳出了循环。我们用`indexOf()`来确定这个。

我们仍然有一个问题，目前的代码如何，如果没有可用的数字，循环将继续运行，我们将在一个无限循环。我们需要记录我们创建的所有数字，这样我们就知道我们什么时候用完了数字。

```
function getRandomId(min = 0, max = 0, ids =[]) {
   let id;
   let a = [];
   do {
      id = Math.floor(Math.random() * (max - min + 1)) + min;
      if (a.indexOf(id) === -1) {
         a.push(id);
      }
      if (a.length === max - min + 1) {
         if (ids.indexOf(id) > -1) {
            return 'failed';
         }
      }
   } while (ids.indexOf(id) > -1);
   return id;
}
```

这就是我们所做的。我们通过创建一个数组来解决这个问题。并且每当我们创建一个数时，就把它添加到数组中(除非它已经在那里了)。我们知道，当数组的长度等于我们选择的范围加 1 时，我们已经尝试了每个数字至少一次。如果我们到了那个点，我们已经创建了最后一个数字。然而，我们仍然希望确保我们创建的最后一个数字没有通过唯一测试。因为如果是这样，虽然我们希望循环结束，但我们还是想返回那个数。如果没有，我们返回“失败”。

```
PASS  ./id.spec.js
✓ returns a random number (1ms)
✓ returns an integer (1ms)
✓ generates a number within a defined range (24ms)
✓ generates a unique number (1ms)

Test Suites: 1 passed, 1 total

Tests:       4 passed, 4 total
```

恭喜你，我们可以运送我们的 ID 生成器，并使我们的数百万！

### 结论

我们做的一些事情是为了演示的目的。测试我们的数字是否在指定的范围内很有趣，但是这个公式可以用数学方法证明。因此，更好的测试可能是确保公式被调用。

此外，你可以用随机 ID 生成器变得更有创造性。例如，如果它找不到一个唯一的数字，该函数会自动将范围增加 1。

我们看到的另一件事是，当我们测试和重构时，我们的测试甚至规范可能会变得更加清晰。换句话说，认为在整个过程中什么都不会改变是愚蠢的。

最终，测试驱动开发为我们提供了一个框架，让我们在更细粒度的层次上思考我们的代码。这取决于你，开发人员，来决定你应该定义你的测试和断言的粒度。请记住，您的测试越多，您的测试越集中，它们与您的代码的耦合就越紧密。这可能会导致不愿意重构，因为现在您还必须更新您的测试。在测试的数量和粒度上肯定有一个平衡。这个平衡由你，开发者来决定。

感谢阅读！

沃兹