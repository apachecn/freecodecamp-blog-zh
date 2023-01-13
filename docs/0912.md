# 如何在 JavaScript 中将回调函数传递给 String.replace()

> 原文：<https://www.freecodecamp.org/news/how-to-pass-callback-functions-to-string-replace-javascript/>

你知道字符串`.replace()`方法接受回调函数吗？我今天才发现，所以想和大家分享一下。

你需要这个功能做什么？为什么会存在？当你阅读这篇文章时，我会回答所有这些问题。

## `replace()`法

字符串方法替换字符串中的文本字符。它有两个参数:要替换的字符串，以及要替换的值。

使用这种方法，您可以替换字符串字符(如“hello”)或与正则表达式模式(如`/hi/`)匹配的[字符。](https://www.freecodecamp.org/news/javascript-string-replace-example-with-regex/)

这种方法的语法如下:

```
String.replace(string/pattern, replacer) 
```

以下是一些展示如何使用这种方法的示例:

```
const sentence = "Hi my name is Dillion"

const replaced1 = sentence.replace("Dillion", "JavaScript")
console.log(replaced1)
// "Hi my name is JavaScript"

const replaced2 = sentence.replace(/\s/g, "-")
console.log(replaced2)
// "Hi-my-name-is-Dillion" 
```

但是，`replacer`参数也可以是一个函数。

## 为什么需要使用函数作为 replacer 方法？

原因是有时候，你想对那些符合指定模式的字符做些什么。

下面是语法:

```
String.replace(/pattern/, function(matched){
    // do something with matched and return
    // the replace value
}) 
```

如果您使用像“Dillion”这样的文字字符串模式，您不需要回调函数，因为您已经知道您在句子中匹配的只是“Dillion”。

但是使用正则表达式模式，它可以匹配多种东西。这里有一个例子:

```
const sentence = "I am a good guy and you too"
const replaced = sentence.replace(/g\S*/g, "😂")

console.log(replaced)
// I am a 😂 😂 and you too 
```

regex 模式匹配所有以“g”开头的单词，两个单词匹配；“好”和“家伙”。在这种情况下，如果我们想对匹配的值做些什么，我们需要回调。

这是另一个例子:

```
const sentence = "I am a good guy and you too"
const replaced = sentence.replace(/g\S*/g, function(matched){
    console.log("matched", matched)
    return "😂"
})

console.log(replaced)
// matched good
// matched guy
// I am a 😂 😂 and you too 
```

我们可以用匹配值做些什么呢？有这么多的场景，但是我将使用引导我发现这一点的一个用例。

## 如何用正则表达式查找和替换文本中的 URL

在 WhatsApp 和 Twitter 这样的平台上，你会发现，当你发布一个带有链接的帖子或消息时，链接的颜色与其他文本不同，行为也像一个链接。然后当它被点击时，它将用户导航到一个单独的页面。

他们是如何做到这一点的？这个想法是用一个元素替换文本中的链接，这个元素有一些样式，也可以作为一个链接。

下面是我如何用 JavaScript 做到这一点:

```
const text = "My website is https://dillionmegida.com and I write on http://freecodecamp.org/"

const regex = /https?:\/\/\S*/gi

const modifiedText = text.replace(regex, (url) => {
    return `<a class="text--link" href="${url}">${url}</a>`
})

console.log(modifiedText)
// My website is <a class="text--link" href="https://dillionmegida.com">https://dillionmegida.com</a> and I write on <a class="text--link" href="http://freecodecamp.org/">http://freecodecamp.org/</a> 
```

正则表达式匹配带有“https://...”的模式(s 是可选的)。使用回调，我可以获得与正则表达式匹配的`url`,并使用它创建一个带有“text - link”类的锚标记字符串。

有了这个返回的字符串，我可以把它注入 DOM。在我的例子中，我使用 React，所以我使用 [dangerouslySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) 将其注入到一个段落中。我可以在样式表中为“文本链接”类指定一种颜色。

## 结论

我们每天都在学习新的东西，我希望你今天已经在 JavaScript 中学到了一些东西——`String.replace()`中的回调函数。

此外，在本文中，我展示了一个很好的利用这个函数的用例。

如果你觉得有帮助，请分享这个。