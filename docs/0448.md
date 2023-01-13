# JavaScript 中的回调函数是什么？JS 回调示例教程

> 原文：<https://www.freecodecamp.org/news/what-is-a-callback-function-in-javascript-js-callbacks-example-tutorial/>

在 JavaScript 中，有更高阶的方法和函数接受函数作为参数。这些用作其他函数参数的函数称为回调函数。

## JavaScript 中什么是回调？

回调是作为另一个函数的参数传递的函数。

这意味着父函数通常可以使用任何类型的函数。但是，另一方面，回调函数是指在使用父函数的特定情况下(或有限的情况下)使用。

## 如何用 JavaScript 创建回调函数？

创建回调函数就像 JavaScript 中的其他函数一样:

```
function callbackFunction () {

}
```

回调函数和其他函数的区别在于它的使用方式。

回调函数是专门用来作为另一个函数的参数的。

```
function anyFunction(fun) {
    // ...
    fun(a, b, c);
    //...
}

anyFunction(callbackFunction);
```

因此，要创建一个`callbackFunction`，你需要知道父函数如何使用回调函数，它传递什么参数，以及它传递参数的顺序。

### 回调函数的例子是什么？

我们现在要写我们自己的回调函数，因为这是你要做很多次的事情。所以，我们开始吧！

JavaScript 语言中已经集成的一个高阶函数是`every`方法。

`every`方法是一个数组方法，使用回调来检查数组中的所有元素是否通过了某个测试。

查看关于`every`方法的[文档，可以看到回调被传递了三个参数:数组的一个元素、该元素的索引和整个数组。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every)

所以回调函数的签名应该是这样的:

```
function callbackFunction(element, index, array) {
    // do something
}
```

回调函数可以根据您的需要简单或复杂。为了创建一个例子，我们需要一些背景。

### 如何用 JavaScript 编写回调函数

所以，假设你正在处理字符串数组。您需要检查数组是否只包含正好三个字符长的字符串，是大写的，包含所有不同的字母，并且它们在数组中不重复。

这是一个非常复杂的情况，但是也许你最终需要做类似的事情或者同等复杂的事情，所以这都是很好的实践。

当你构建一个函数的时候，你可以一次处理一个条件，因为你需要检查很多东西。

第一个条件是元素是一个字符串，所以，让我们添加它:

```
function callbackFunction(element, index, array) {

    // check that element is a string
	const isNotString = typeof element !== "string";
    // if it's not, end function
    if (isNotString) {return;}

}
```

接下来，字符串必须全部大写，只包含字母，长度为 3 个字符。

您可以分别检查这三个条件，也可以用一个正则表达式一起检查这三个条件。

这样的正则表达式应该是这样的:`/^[A-Z]{3}$/`。

让我们看看这个正则表达式由哪些部分组成:

*   开头的字符`^`和结尾的字符`$`是锚点。这些说明字符串必须以这种方式开始和结束。如果两者都使用，它们会限制字符串只包含正则表达式中的模式。
*   `[A-Z]`是一个字符类，匹配从`A`到`Z`的任意字符，所以都是大写字母。
*   `{3}`是一个计数器。这表示前一个东西必须连续三次精确匹配。

上面解释的正则表达式就是这个正则表达式的等价物:`/^[A-Z][A-Z][A-Z]$/`。

在这种情况下，我们写了三次类`[A-Z]`，而不是计数器`{3}`。

让我们把这个添加到代码中。

```
function callbackFunction(element, index, array) {

    // check that element is a string
    const isNotString = typeof element !== "string";
    // if it's not, end function
    if (isNotString) {
        return;
    }

    // check that string is 3 characters long and only uppercase letters
    const isItThreeUpperCaseLetters = /^[A-Z]{3}$/.test(element);
    // otherwise, end function
    if (!isItThreeUpperCaseLetters) {
        return;
    }

}
```

如果你不喜欢正则表达式，你可以阅读下面的[如何不使用正则表达式做同样的检查。](#and-if-we-didn-t-use-a-regular-expression)

然后，接下来，我们需要检查字符是否都不同。

有三个字符可以使用:`element[0] !== element[1] && element[0] !== element[2] && element[1] !== element[2]`。

但是，你也可以用一个循环来做这个——实际上是一个双循环。

```
// with the outer loop, you get j, the first index to compare
for (let j = 0; j++; j < element.length) {
    // with the inner loop you get k, the second index to compare
    for (let k = j+1; k++; k < element.length) {
        // you compare the element at index j with the element at index k
        if (element[j] === element[k]) {
            // if they are equal return to stop the function
            return;
        }
    }
}
```

这个循环可以在任何长度下运行，你不需要为不同的情况重写它。

和写三个对比完全一样吗？让我们跟随循环来检查。

在第一次迭代中，我们有`j=0`和`k=1`，所以第一个比较是`element[0] === element[1]`。然后`k`增加，所以是`j=0`和`k=2`，所以是`element[0] === element[2]`。

此时，内部循环停止，外部循环(带有`j`的循环)进入下一次迭代。这次`j=1`，内循环从`k=j+1`开始，所以在`k=2`——这里的比较是`element[1] === element[2]`。

内循环结束，外循环从`j=1`到`j=2`，由于`k = j+1 = 3`没有通过循环的`k < element.length`条件，内循环没有开始。

```
function callbackFunction(element, index, array) {

    // check that element is a string
    const isNotString = typeof element !== "string";
    // if it's not, end function
    if (isNotString) {
        return;
    }

    // check that string is 3 characters long and only uppercase letters
    const isItThreeUpperCaseLetters = /^[A-Z]{3}$/.test(element);
    // otherwise, end function
    if (!isItThreeUpperCaseLetters) {
        return;
    }

    // check if all characters are different
    const allDifferentCharacters = element[0] !== element[1] && element[0] !== element[2] && element[1] !== element[2];
    // if not, return to stop the function
    if (!allDifferentCharacters) {
        return;
    }

}
```

然后，我们需要检查的最后一件事是字符串在数组中没有重复。

我们可以用`indexOf`来检查当前的是`element`在数组里面的第一次出现。

为此，我们需要引用数组。我们有了它——它是传递给回调函数的参数之一,`array`参数。

如果这是字符串在数组中的第一次出现，`indexOf`的输出将与`index`相同。

如果`array.indexOf(element) === index`是`true`，这意味着`element`在`index`第一次出现在数组中。如果是`false`，一个相同的字符串出现在数组的前面。

让我们将这个检查添加到函数中。如果字符串通过了所有的检查，那么函数可以在最后返回`true`。

```
function callbackFunction(element, index, array) {

    // check that element is a string
    const isNotString = typeof element !== "string";
    // if it's not, end function
    if (isNotString) {
        return;
    }

    // check that string is 3 characters long and only uppercase letters
    const isItThreeUpperCaseLetters = /^[A-Z]{3}$/.test(element);
    // otherwise, end function
    if (!isItThreeUpperCaseLetters) {
        return;
    }

    // check if all characters are different
    const allDifferentCharacters = element[0] !== element[1] && element[0] !== element[2] && element[1] !== element[2];
    // if not, return to stop the function
    if (!allDifferentCharacters) {
        return;
    }

    // check if it's the first appearence of element inside the array
    const isItFirstAppearence = array.indexOf(element) === index;
    // if not, return to stop the function
    if (!isItFirstAppearence) {
        return;
    }

    return true;
}
```

#### 如果我们不使用正则表达式呢？

在上面的代码中，为了检查三种不同的东西，我们使用了一个正则表达式:`/^[A-Z]{3}$/`。

但是如果你不想使用 regex，你可以使用`length`属性来检查一个字符串是否有一定的长度。在这种情况下，`element.length === 3`检查字符串正好是三个字符长。

接下来，字符串必须全部大写，并且只包含字母。

你可以用`charCodeAt`来做这个。这个方法返回一个字符的 ASCII 码，知道大写字母有从 65 到 90 的 ASCII 码，就可以检查只有大写字母。

有三个数字需要检查:`element.charCodeAt(0)`、`element.charCodeAt(1)`、`element.charCodeAt(2)`。他们都需要在 65 到 90 岁之间。只有三个字符，但我们仍然可以使用循环。

因此，这将是如下:

```
for (let i = 0; i++; i < element.length) {
    // find the ASCII code of the character
    const code = element.charCodeAt(i);
    // check if it's outside of the range
    if (code < 65 || code > 90) {
        // if it is, return to stop the function
        return;
    }
}
```

让我们将它添加到函数中:

```
function callbackFunction(element, index, array) {

    // check that element is a string
    const isNotString = typeof element !== "string";
    // if it's not, end function
    if (isNotString) {return;}

    // check that element has length string
    const hasLengthThree = element.length === 3;
    // if it has a different length, end function
    if (!hasLengthThree) {return;}

    // loop over the characters
	for (let i = 0; i++; i < element.length) {
        // find the ASCII code of the character
        const code = element.charCodeAt(i);
        // check if it's outside of the range
        if (code < 65 || code > 90) {
            // if it's outside the range, return and stop the function
            return;
        }
    } 
}
```

如果你从上面的链接来到这里，你可以回到那里继续阅读如何完成功能，否则，请继续到最后。

### 如何使用示例回调函数

我们已经编写了回调函数。那么怎么用呢？

```
anArray.every(callbackFunction);
```

您也可以在回调中使用`every`方法——也许是对[方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)的回调。

随着程序变得越来越复杂，它可能会相应地使用更多的回调函数。

## 为什么我们在 JavaScript 中使用回调函数？

回调函数是 JavaScript 的一个优秀特性。这意味着我们可以有一个通用的函数来做一些事情(比如`every`检查数组中的每个元素是否都匹配某个条件`filter`，删除不匹配某个条件的元素`replace`，一个接受回调的字符串方法来描述如何替换字符串的一部分，等等)和一个回调函数来添加特定情况下的行为细节。

*   `filter`在那种情况下将删除回调指定的元素。
*   `every`将检查这种情况下的所有元素是否如回调函数所指定。
*   `replace`将在回调指定的情况下替换字符串的一部分。

高阶函数增加了代码的抽象层次。我们不知道(也不需要知道)，`every`如何检查数组的每个元素，并验证它们都通过了回调指定的测试。我们只需要知道这个方法接受一个回调函数。

## 结论

回调是作为其他函数的参数传递的函数。您已经看到了如何创建它的示例，以及关于它们为什么有用的一些考虑。

感谢您的阅读！