# JavaScript Rest vs Spread Operator——有什么区别？

> 原文：<https://www.freecodecamp.org/news/javascript-rest-vs-spread-operators/>

JavaScript 对 rest 和 spread 操作符都使用了三个点(`...`)。但是这两个运算符并不相同。

rest 和 spread 的主要区别在于，rest 操作符将一些特定的用户提供的值放入一个 JavaScript 数组中。但是 spread 语法将 iterables 扩展成单独的元素。

例如，考虑下面这段代码，它使用 rest 将一些值封装到一个数组中:

```
// Use rest to enclose the rest of specific user-supplied values into an array:
function myBio(firstName, lastName, ...otherInfo) { 
  return otherInfo;
}

// Invoke myBio function while passing five arguments to its parameters:
myBio("Oluwatobi", "Sofela", "CodeSweetly", "Web Developer", "Male");

// The invocation above will return:
["CodeSweetly", "Web Developer", "Male"]
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-t3kcyw?file=script.js)

在上面的代码片段中，我们使用了`...otherInfo` rest 参数将`"CodeSweetly"`、`"Web Developer"`和`"Male"`放入一个数组中。

现在，考虑这个扩展操作符的例子:

```
// Define a function with three parameters:
function myBio(firstName, lastName, company) { 
  return `${firstName} ${lastName} runs ${company}`;
}

// Use spread to expand an array’s items into individual arguments:
myBio(...["Oluwatobi", "Sofela", "CodeSweetly"]);

// The invocation above will return:
“Oluwatobi Sofela runs CodeSweetly”
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-ppjslx?file=script.js)

在上面的代码片段中，我们使用了扩展操作符(`...`)将`["Oluwatobi", "Sofela", "CodeSweetly"]`的内容扩展到`myBio()`的参数中。

如果您还不了解其余部分或传播运算符，请不要担心。这篇文章已经让你懂了！

在接下来的小节中，我们将讨论 rest 和 spread 在 JavaScript 中是如何工作的。

所以，事不宜迟，让我们从 rest 操作符开始。

## Rest 算子到底是什么？

**rest 操作符**用于将用户提供的其他特定值放入一个 JavaScript 数组中。

例如，以下是 rest 语法:

```
...yourValues
```

上面代码片段中的三个点(`...`)代表 rest 操作符。

rest 操作符后面的文本引用了希望包含在数组中的值。您只能在函数定义中的最后一个参数之前使用它。

为了更好地理解语法，让我们看看 rest 是如何处理 JavaScript 函数的。

### Rest 运算符如何在函数中工作？

在 JavaScript 函数中，rest 被用作函数最后一个参数的前缀。

**这里有一个例子:**

```
// Define a function with two regular parameters and one rest parameter:
function myBio(firstName, lastName, ...otherInfo) { 
  return otherInfo;
}
```

rest 操作符(`...`)指示计算机将用户提供的任何`otherInfo`(参数)添加到数组中。然后，将该数组赋给`otherInfo`参数。

因此，我们称`...otherInfo`为 rest 参数。

**注意:** [参数](https://www.codesweetly.com/javascript-arguments)是可选值，可以通过调用程序传递给函数的参数。

这里还有一个例子:

```
// Define a function with two regular parameters and one rest parameter:
function myBio(firstName, lastName, ...otherInfo) { 
  return otherInfo;
}

// Invoke myBio function while passing five arguments to its parameters:
myBio("Oluwatobi", "Sofela", "CodeSweetly", "Web Developer", "Male");

// The invocation above will return:
["CodeSweetly", "Web Developer", "Male"]
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-t3kcyw?file=script.js)

在上面的代码片段中，注意到`myBio`的调用向函数传递了五个参数。

换句话说，`"Oluwatobi"`和`"Sofela"`被分配给`firstName`和`lastName`参数。

同时，rest 操作符将剩余的参数(`"CodeSweetly"`、`"Web Developer"`和`"Male"`)添加到一个数组中，并将该数组赋给`otherInfo`参数。

因此，`myBio()`函数正确地返回了`["CodeSweetly", "Web Developer", "Male"]`作为`otherInfo`休息参数的内容。

### 当心！不能在包含 Rest 参数的函数中使用`“use strict”`

请记住，您*不能*在任何包含 rest 参数、默认参数或[析构参数](https://www.codesweetly.com/destructuring-object#how-to-use-a-destructuring-object-to-copy-values-from-properties-to-a-functions-parameters)的函数中使用`“use strict”`指令。否则，计算机会抛出一个[语法错误](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Errors/Strict_Non_Simple_Params)。

例如，考虑下面的例子:

```
// Define a function with one rest parameter:
function printMyName(...value) {
  "use strict";
  return value;
}

// The definition above will return:
"Uncaught SyntaxError: Illegal 'use strict' directive in function with non-simple parameter list"
```

[**在 CodeSandbox 上试试**](https://codesandbox.io/s/you-cannot-use-use-strict-inside-a-function-containing-a-spread-parameter-cvis3)

`printMyName()`返回了一个语法错误，因为我们在带有 rest 参数的函数中使用了`“use strict”`指令。

但是假设您需要函数处于严格模式，同时还使用 rest 参数。在这种情况下，您可以在函数外部编写`“use strict”`指令。

**这里有一个例子:**

```
// Define a “use strict” directive outside your function:
"use strict";

// Define a function with one rest parameter:
function printMyName(...value) {
  return value;
}

// Invoke the printMyName function while passing two arguments to its parameters:
printMyName("Oluwatobi", "Sofela");

// The invocation above will return:
["Oluwatobi", "Sofela"]
```

[**在 CodeSandbox 上试试**](https://codesandbox.io/s/you-can-use-use-strict-outside-a-function-containing-a-spread-parameter-spbmh)

**注意:**只有在整个脚本或者[封闭范围](https://www.codesweetly.com/javascript-lexical-scope)处于严格模式时，才可以将`“use strict”`指令放在函数之外。

现在我们知道了 rest 在函数中是如何工作的，我们可以谈谈它在[析构赋值](https://www.codesweetly.com/destructuring-array)中是如何工作的。

### Rest 操作符如何在析构赋值中工作

rest 操作符通常用作析构赋值的最后一个变量的前缀。

**这里有一个例子:**

```
// Define a destructuring array with two regular variables and one rest variable:
const [firstName, lastName, ...otherInfo] = [
  "Oluwatobi", "Sofela", "CodeSweetly", "Web Developer", "Male"
];

// Invoke the otherInfo variable:
console.log(otherInfo); 

// The invocation above will return:
["CodeSweetly", "Web Developer", "Male"]
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-tckdt8?file=script.js)

rest 运算符(`...`)指示计算机将用户提供的其余值添加到一个数组中。然后，它将该数组赋给`otherInfo`变量。

因此，您可以将`...otherInfo`称为 rest 变量。

这里还有一个例子:

```
// Define a destructuring object with two regular variables and one rest variable:
const { firstName, lastName, ...otherInfo } = {
  firstName: "Oluwatobi",
  lastName: "Sofela", 
  companyName: "CodeSweetly",
  profession: "Web Developer",
  gender: "Male"
}

// Invoke the otherInfo variable:
console.log(otherInfo);

// The invocation above will return:
{companyName: "CodeSweetly", profession: "Web Developer", gender: "Male"}
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-fmr3dr?file=script.js)

在上面的代码片段中，注意 rest 操作符将一个 properties 对象——而不是一个数组——赋给了`otherInfo`变量。

换句话说，每当你在一个[析构对象](https://www.codesweetly.com/destructuring-object)中使用 rest 时，rest 操作符将产生一个 properties 对象。

然而，如果你在一个[析构数组](https://www.codesweetly.com/destructuring-array)或函数中使用 rest，操作符将产生一个数组文字。

在我们结束关于 rest 的讨论之前，您应该知道 JavaScript [参数](https://www.codesweetly.com/javascript-arguments)和 rest 参数之间的一些差异。那么，下面就来说说那个吧。

### Arguments vs Rest 参数:有什么区别？

以下是 JavaScript 参数和 rest 参数之间的一些差异:

#### 区别 1:`arguments`对象是一个类似数组的对象——不是一个真正的数组！

请记住，JavaScript arguments 对象不是一个真正的数组。相反，它是一个类似于[数组的](https://www.codesweetly.com/javascript-arguments#what-is-an-arraylike-object)对象，不具备常规 JavaScript 数组的全面特性。

然而，rest 参数是一个真正的数组对象。因此，您可以在其上使用所有的数组方法。

例如，您可以对 rest 参数调用`sort()`、`map()`、`forEach()`或`pop()`方法。但是您不能在 arguments 对象上做同样的事情。

#### 区别 2:不能在箭头函数中使用`arguments`对象

箭头函数中没有`arguments`对象，所以不能在那里使用。但是您可以在所有函数中使用 rest 参数，包括 arrow 函数。

#### 区别 3:让休息成为你的首选

最好使用 rest 参数而不是`arguments`对象——尤其是在编写 ES6 兼容代码时。

现在我们知道了 rest 是如何工作的，让我们来讨论一下`spread`操作符，这样我们就可以看到它们的区别了。

## 什么是 Spread 操作符，`spread`在 JavaScript 中是如何工作的？

**spread 操作符** ( `...`)帮助你将 iterables 展开成单个的元素。

spread 语法在数组文字、函数调用和初始化的属性对象中工作，将 [iterable 对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators#Iterables)的值分散到单独的项中。所以实际上，它做了与 rest 操作符相反的事情。

**注意:**扩展操作符只有在数组文字、函数调用或初始化的属性对象中使用时才有效。

那么，这到底意味着什么呢？让我们看一些例子。

### Spread 示例 1:Spread 如何在数组文本中工作

```
const myName = ["Sofela", "is", "my"];
const aboutMe = ["Oluwatobi", ...myName, "name."];

console.log(aboutMe);

// The invocation above will return:
[ "Oluwatobi", "Sofela", "is", "my", "name." ]
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-rd1npd?file=script.js)

上面的代码片段使用 spread ( `...`)将`myName`数组复制到`aboutMe`。

**注:**

*   对`myName`的修改不会反映在`aboutMe`中，因为`myName`中的所有值都是[原语](https://www.codesweetly.com/web-tech-glossary#primitive-data-js)。因此，spread 操作符简单地将`myName`的内容复制并粘贴到`aboutMe`中，而没有创建任何对原始数组的引用。
*   正如 [@nombrekeff](https://dev.to/oluwatobiss/spread-operator-how-spread-works-in-javascript-4fdn#comment-node-767546) 在这里的一个评论[中提到的，spread 运算符只做浅层复制。因此，请记住，假设`myName`包含任何](https://dev.to/oluwatobiss/spread-operator-how-spread-works-in-javascript-4fdn)[非原语](https://www.codesweetly.com/web-tech-glossary#non-primitive-data-js)值，计算机会在`myName`和`aboutMe`之间创建一个引用。参见 [info 3](#info-3-beware-of-how-spread-works-when-used-on-objects-containing-non-primitives-) 了解更多关于 spread 运算符如何处理原始值和非原始值的信息。
*   假设我们没有使用 spread 语法来复制`myName`的内容。例如，如果我们写了`const aboutMe = ["Oluwatobi", myName, "name."]`。在这种情况下，计算机会将一个引用分配回`myName`。因此，原始数组中的任何更改都会反映在复制的数组中。

### Spread 示例 2:如何使用 Spread 将字符串转换为单个数组项

```
const myName = "Oluwatobi Sofela";

console.log([...myName]);

// The invocation above will return:
[ "O", "l", "u", "w", "a", "t", "o", "b", "i", " ", "S", "o", "f", "e", "l", "a" ]
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-axvtye?file=script.js)

在上面的代码片段中，我们在一个数组文字对象(`[...]`)中使用了 spread 语法(`...`)来将`myName`的字符串值扩展到各个项中。

就这样，`"Oluwatobi Sofela"`展开成了`[ "O", "l", "u", "w", "a", "t", "o", "b", "i", " ", "S", "o", "f", "e", "l", "a" ]`。

### Spread 示例 3:Spread 运算符如何在函数调用中工作

```
const numbers = [1, 3, 5, 7];

function addNumbers(a, b, c, d) {
  return a + b + c + d;
}

console.log(addNumbers(...numbers));

// The invocation above will return:
16
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-nrn8f3?file=script.js)

在上面的代码片段中，我们使用 spread 语法将`numbers`数组的内容分布到`addNumbers()`的参数中。

假设`numbers`数组有四个以上的条目。在这种情况下，计算机将只使用前四项作为`addNumbers()`参数，而忽略其余的。

**这里有一个例子:**

```
const numbers = [1, 3, 5, 7, 10, 200, 90, 59];

function addNumbers(a, b, c, d) {
  return a + b + c + d;
}

console.log(addNumbers(...numbers));

// The invocation above will return:
16
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-ef3ncm?file=script.js)

这里还有一个例子:

```
const myName = "Oluwatobi Sofela";

function spellName(a, b, c) {
  return a + b + c;
}

console.log(spellName(...myName));      // returns: "Olu"

console.log(spellName(...myName[3]));   // returns: "wundefinedundefined"

console.log(spellName([...myName]));    // returns: "O,l,u,w,a,t,o,b,i, ,S,o,f,e,l,aundefinedundefined"

console.log(spellName({...myName}));    // returns: "[object Object]undefinedundefined"
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-pkrxjd?file=script.js)

### Spread 示例 4:Spread 如何在对象文本中工作

```
const myNames = ["Oluwatobi", "Sofela"];
const bio = { ...myNames, runs: "codesweetly.com" };

console.log(bio);

// The invocation above will return:
{ 0: "Oluwatobi", 1: "Sofela", runs: "codesweetly.com" }
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-qnmxsu?file=script.js)

在上面的代码片段中，我们在`bio`对象中使用 spread 将`myNames`值扩展为单独的属性。

### 关于 Spread 操作符需要了解什么

无论何时选择使用 spread 操作符，都要记住这三条基本信息。

#### 信息 1:扩展运算符不能扩展对象文字的值

由于 properties 对象不是 iterable 对象，因此不能使用 spread 运算符来扩展其值。

但是，您可以使用 spread 操作符将属性从一个对象克隆到另一个对象。

**这里有一个例子:**

```
const myName = { firstName: "Oluwatobi", lastName: "Sofela" };
const bio = { ...myName, website: "codesweetly.com" };

console.log(bio);

// The invocation above will return:
{ firstName: "Oluwatobi", lastName: "Sofela", website: "codesweetly.com" };
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-psnsa8?file=script.js)

上面的代码片段使用 spread 操作符将`myName`的内容克隆到`bio`对象中。

**注:**

*   spread 运算符只能扩展可迭代对象的值。
*   只有当一个对象(或其原型链中的任何对象)有一个带有 [@@iterator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterable_protocol) 键的属性时，它才是可迭代的。
*   [数组](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/@@iterator)、[类型数组](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray/@@iterator)、[字符串](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/@@iterator)、[映射](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/@@iterator)、[集合](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/@@iterator)都是内置的可迭代类型，因为它们默认具有`@@iterator`属性。
*   properties 对象不是可迭代数据类型，因为默认情况下它没有`@@iterator`属性。
*   你可以通过添加`@@iterator`到属性对象上，使属性对象成为可迭代的。

#### 信息 2:扩展运算符不克隆相同的属性

假设您使用 spread 运算符将属性从对象 A 克隆到对象 B 中，并且假设对象 B 包含与对象 A 中相同的属性，在这种情况下，B 的版本将覆盖 A 中的版本。

**这里有一个例子:**

```
const myName = { firstName: "Tobi", lastName: "Sofela" };
const bio = { ...myName, firstName: "Oluwatobi", website: "codesweetly.com" };

console.log(bio);

// The invocation above will return:
{ firstName: "Oluwatobi", lastName: "Sofela", website: "codesweetly.com" };
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-gjhjue?file=script.js)

注意到扩展操作符没有将`myName`的`firstName`属性复制到`bio`对象中，因为`bio`已经包含了一个`firstName`属性。

#### 信息 3:当用于包含非基本体的对象时，要小心 spread 是如何工作的！

假设您在一个只包含原始值的对象(或数组)上使用了 spread 运算符。计算机将而不是在原始对象和复制对象之间创建任何引用。

例如，考虑下面的代码:

```
const myName = ["Sofela", "is", "my"];
const aboutMe = ["Oluwatobi", ...myName, "name."];

console.log(aboutMe);

// The invocation above will return:
["Oluwatobi", "Sofela", "is", "my", "name."]
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-rd1npd?file=script.js)

注意到`myName`中的每一项都是一个原始值。因此，当我们使用 spread 操作符将`myName`克隆到`aboutMe`时，计算机不会在两个数组之间创建任何引用。

因此，您对`myName`所做的任何更改都不会反映在`aboutMe`中，反之亦然。

举个例子，让我们给`myName`添加更多的内容:

```
myName.push("real");
```

现在，让我们检查一下`myName`和`aboutMe`的当前状态:

```
console.log(myName); // ["Sofela", "is", "my", "real"]

console.log(aboutMe); // ["Oluwatobi", "Sofela", "is", "my", "name."]
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-ujs6ny?file=script.js)

请注意，`myName`的更新内容没有反映在`aboutMe`中，因为 spread 没有在原始数组和复制数组之间创建引用。

##### 如果`myName`包含非原始物品怎么办？

假设`myName`包含非原语。在这种情况下，spread 将在原始非基本体和克隆体之间创建一个引用。

这里有一个例子:

```
const myName = [["Sofela", "is", "my"]];
const aboutMe = ["Oluwatobi", ...myName, "name."];

console.log(aboutMe);

// The invocation above will return:
[ "Oluwatobi", ["Sofela", "is", "my"], "name." ]
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-ombp5w?file=script.js)

注意到`myName`包含一个非原始值。

因此，使用 spread 操作符将`myName`的内容克隆到`aboutMe`中会导致计算机在两个数组之间创建一个引用。

因此，您对`myName`的副本所做的任何更改都会反映在`aboutMe`的版本中，反之亦然。

举个例子，让我们给`myName`添加更多的内容:

```
myName[0].push("real");
```

现在，让我们检查一下`myName`和`aboutMe`的当前状态:

```
console.log(myName); // [["Sofela", "is", "my", "real"]]

console.log(aboutMe); // ["Oluwatobi", ["Sofela", "is", "my", "real"], "name."]
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-qpyy8n?file=script.js)

注意，`myName`的更新内容反映在`aboutMe`中，因为 spread 在原始数组和复制数组之间创建了一个引用。

这里还有一个例子:

```
const myName = { firstName: "Oluwatobi", lastName: "Sofela" };
const bio = { ...myName };

myName.firstName = "Tobi";

console.log(myName); // { firstName: "Tobi", lastName: "Sofela" }

console.log(bio); // { firstName: "Oluwatobi", lastName: "Sofela" }
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-tbmtgm?file=script.js)

在上面的代码片段中，`myName`的更新没有反映在`bio`中，因为我们在一个只包含原始值的对象上使用了 spread 运算符。

**注意:**开发者会称`myName`为**浅对象**，因为它只包含原始项目。

**这里还有一个例子:**

```
const myName = { 
  fullName: { firstName: "Oluwatobi", lastName: "Sofela" }
};

const bio = { ...myName };

myName.fullName.firstName = "Tobi";

console.log(myName); // { fullName: { firstName: "Tobi", lastName: "Sofela" } }

console.log(bio); // { fullName: { firstName: "Tobi", lastName: "Sofela" } }
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/web-platform-9uce9g?file=script.js)

在上面的代码片段中，`myName`的更新反映在`bio`中，因为我们在包含非原始值的对象上使用了 spread 操作符。

**注:**

*   我们称`myName`为**深层对象**，因为它包含了一个非原始项。
*   当您在将一个对象克隆到另一个对象时创建引用时，您就做了**浅层复制**。例如，`...myName`产生了一个`myName`对象的浅层副本，因为你在其中一个对象上做的任何改变都会反映到另一个对象上。
*   当你克隆对象而不创建引用时，你做了**深度复制**。例如，我可以通过做`const bio = JSON.parse(JSON.stringify(myName))`将`myName`深度复制到`bio`中。通过这样做，计算机会将`myName`克隆到`bio`中，而不会创建任何引用。
*   通过用新对象替换`myName`或`bio`中的`fullName`对象，可以断开两个对象之间的引用。例如，做`myName.fullName = { firstName: "Tobi", lastName: "Sofela" }`会断开`myName`和`bio`之间的指针。

## 包装它

本文讨论了 rest 和 spread 操作符之间的区别。我们还用例子来看看每个操作符是如何工作的。

感谢阅读！