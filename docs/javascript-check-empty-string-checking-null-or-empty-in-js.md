# JavaScript 检查空字符串——检查 JS 中的 Null 或空

> 原文：<https://www.freecodecamp.org/news/javascript-check-empty-string-checking-null-or-empty-in-js/>

您可能需要检查字符串是否为空有很多原因。最重要的原因之一是当您从数据库、API 或输入字段中检索数据时。

在本文中，您将学习如何在 JavaScript 中检查一个字符串是否为空。我们会看到许多你可以使用的例子和方法，这样你就可以理解它们，并决定在什么时候使用哪一个。

## Null 和空有什么区别？

在我们开始之前，您需要理解术语 Null 和 Empty 的含义，并且理解它们不是同义词。

例如，如果我们声明一个变量并给它赋一个空字符串，然后声明另一个变量并给它赋空值，我们可以通过查看它们的数据类型来区分它们:

```
let myStr1 = "";
let myStr2 = null;

console.log(typeof myStr1); // "string"
console.log(typeof myStr2); // "object" 
```

看上面的代码，我们可以看到编译器/计算机对每个值的解释都不一样。所以当需要检查时，我们必须为这两种类型的值传递条件，因为我们人类经常将`null`称为空。

## 如何在 JavaScript 中检查为空或 Null

我们现在知道空字符串是不包含任何字符的字符串。检查字符串是否为空非常简单。我们可以使用两个有些相似的主要方法，因为我们将使用严格的等式运算符(`==`)。

### 如何用`length`属性检查 JavaScript 中的空字符串

在第一个方法中，我们将通过添加 length 属性来检查字符串的长度。我们将检查长度是否等于`0`。如果它等于零，就意味着字符串是空的，如下所示:

```
let myStr = "";

if (myStr.length === 0) {
  console.log("This is an empty string!");
} 
```

上面将返回这个:

```
"This is an empty string!" 
```

但不幸的是，这种方法可能并不适用于所有情况。例如，如果我们有一个包含空格的字符串，如下所示:

```
let myStr = "  ";

if (myStr.length === 0) {
  console.log("This is an empty string!");
}else{
  console.log("This is NOT an empty string!");
} 
```

这将返回:

```
"This is NOT an empty string!" 
```

我们可以很容易地修复这个错误，首先使用`trim()`方法删除空格，然后检查该字符串的长度，看它是否为空，如下所示:

```
let myStr = "  ";

if (myStr.trim().length === 0) {
  console.log("This is an empty string!");
}else{
  console.log("This is NOT an empty string!");
} 
```

这将返回以下内容:

```
"This is an empty string!" 
```

注意:如果值为 null，这将抛出一个错误，因为`length`属性对 null 无效。

要解决这个问题，我们可以添加一个参数来检查值的类型是否是字符串，如果不是，则跳过这个检查:

```
let myStr = null;

if (typeof myStr === "string" && myStr.trim().length === 0) {
  console.log("This is an empty string!");
} 
```

### 如何通过字符串比较检查 JavaScript 中的空字符串

检查字符串是否为空的另一种方法是将字符串与空字符串进行比较。

例如:

```
let myStr = "";

if (myStr === "") {
  console.log("This is an empty string!");
} 
```

和前面的方法一样，如果我们有空格，这将不会把字符串读为空。所以我们必须首先使用`trim()`方法来删除所有形式的空格:

```
let myStr = "   ";

if (myStr.trim() === "") {
  console.log("This is an empty string!");
} else {
  console.log("This is NOT an empty string!");
} 
```

正如我们对`length`方法所做的一样，我们也可以检查值的类型，这样只有当值是字符串时才会运行:

```
let myStr = null;

if (typeof myStr === "string" && myStr.trim() === "") {
  console.log("This is an empty string!");
} 
```

## 如何在 JavaScript 中检查 Null

到目前为止，我们已经看到了如何使用长度和比较方法来检查字符串是否为空。

现在，我们来看看如何检查是否是`null`，然后检查两者。为了检查`null`，我们简单地将该变量与 null 本身进行比较，如下所示:

```
let myStr = null;

if (myStr === null) {
  console.log("This is a null string!");
} 
```

这将返回:

```
"This is a null string!" 
```

## 如何在 JavaScript 中检查 Null 或空字符串

至此，我们已经了解了如何检查空字符串，以及变量是否设置为 null。现在让我们以这种方式检查两者:

```
let myStr = null;

if (myStr === null || myStr.trim() === "") {
  console.log("This is an empty string!");
} else {
  console.log("This is not an empty string!");
} 
```

这将返回:

```
"This is an empty string!" 
```

## 结论

在本文中，我们学习了如何检查空字符串或 null，以及为什么它们不是一回事。

祝编码愉快！