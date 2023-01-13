# HTML 属性解释

> 原文：<https://www.freecodecamp.org/news/html-attributes-explained/>

HTML 元素可以有属性，属性包含元素的附加信息。

HTML 属性通常以名称-值对的形式出现，并且总是出现在元素的开始标记中。属性名表示您提供的关于元素的信息类型，属性值是实际的信息。

例如，HTML 文档中的锚(`<a>`)元素创建到其他页面或页面其他部分的链接。在开始的`<a>`标签中使用`href`属性告诉浏览器链接将用户发送到哪里。

下面是一个将用户发送到 freeCodeCamp 主页的链接示例:

```
<a href="www.freecodecamp.org">Click here to go to freeCodeCamp!</a>
```

请注意，属性名(`href`)和值(“www.freeCodeCamp.org”)用等号分隔，值用引号括起来。

有许多不同的 HTML 属性，但大多数只对某些 HTML 元素有效。例如，`href`属性如果放在开始的`<h1>`标签中就不起作用。

在上面的例子中，提供给`href`属性的值可以是任何有效的链接。但是，有些属性只有一组可用的有效选项，或者值需要采用特定的格式。属性告诉浏览器 HTML 元素中内容的默认语言。属性`lang`的值应该使用标准语言或国家代码，比如`en`代表英语，或者`it`代表意大利语。

## **布尔属性**

有些 HTML 属性不需要值，因为它们只有一个选项。这些被称为布尔属性。标签中属性的存在将把它应用于 HTML 元素。然而，写出属性名并将其设置为值的一个选项也是可以的。在这种情况下，该值通常与属性名相同。

例如，表单中的`<input>`元素可以有一个`required`属性。这要求用户在提交表单之前填写该项目。

以下是做同样事情的例子:

```
<input type="text" required >
<input type="text" required="required" >
```

你可以在这里阅读更多关于 

## [< a href >属性](https://www.freecodecamp.org/news/the-a-href-attribute-explained/)

## **[<脚本> src 属性](https://www.freecodecamp.org/news/link-javascript-to-html-with-the-src/)**

## [滚动属性](https://www.freecodecamp.org/news/html-role-attribute/)

## [<一个目标>属性](https://www.freecodecamp.org/news/the-a-target-html-attribute-explained/)

现在，让我们了解一些其他的 HTML 属性:

## **P 对齐属性**

### **重要**

HTML5 不支持此属性。建议使用 [`text-align` CSS 属性](https://guide.freecodecamp.org/css/text-align)。

为了对齐`<p>`标签中的文本，这个属性会有所帮助。

### **语法**

```
<p align="position">Lorem Ipsum...</p>
```

### **属性**

*   **-文字靠左对齐**
*   ******右**** -文本向右对齐**
*   ******居中**** -文本居中对齐**
*   ****-所有文本行宽度相等****

### ******例子******

```
**`<html>
<body>
<p align="center">Paragraph align attribute example</p>
</body>
</html>`**
```

## ******Img Src 属性******

****`<img src>`属性指的是您想要显示的图像的来源。标签不会显示没有 T2 属性的图像。但是，如果将源设置为图像的位置，则可以显示任何图像。****

****在`https://avatars0.githubusercontent.com/u/9892522?v=4&s=400`有一个 freeCodeCamp 徽标的图像****

****您可以使用`src`属性将其设置为图像。****

```
**`<html>
  <head>
    <title>Img Src Attribute Example</title>
  </head>
  <body>
    <img src="https://avatars0.githubusercontent.com/u/9892522?v=4&s=400">
  </body>
</html>`**
```

****上面的代码显示如下:****

****![The freeCodeCamp Avatar](img/33aed02337013adbe09e98057d829320.png)****

****所有浏览器都支持`src`属性。****

****您也可以将本地托管的文件作为您的映像。****

****例如，如果你有一个名为`images`的文件夹，里面有`freeCodeCamp.jpeg`，只要“图像”文件夹和`index.html`文件在同一个位置，`<img src="images/freeCodeCamp.jpeg>`就可以工作。****

****`../files/index.html`****

****`..files/images/freeCodeCamp.jpeg`****

## ******字体大小属性******

****该属性将字体大小指定为数值或相对值。数值范围从`1`到`7`，其中`1`最小，`3`为默认值。也可以使用一个相对值来定义它，比如`+2`或`-3`，相对于`<basefont>`元素的 size 属性值来设置它，或者相对于`3`，默认值，如果不存在的话。****

****语法:****

****`<font size="number">`****

****示例:****

```
**`<html>
  <body>
    <font size="6">This is some text!</font>
  </body>
</html>`**
```

****注意:`The size attribute of <font> is not supported in HTML5\. Use CSS instead.`****

## ******字体颜色属性******

****该属性用于为包含在`<font>`标签中的文本设置颜色。****

### ******语法:******

****超文本标记语言****

```
 **`### Important:
This attribute is not supported in HTML5\. Instead, this [freeCodeCamp article](https://guide.freecodecamp.org/css/colors) specifies a CSS method, which can be used.

### Note:
A color can also be specified using a 'hex code' or an 'rgb code', instead of using a name.

### Example:
1\. Color name attribute
```html
<html>
 <body>
  <font color="green">Font color example using color attribute</font>
</body>
</html>`**
```

****十六进制代码属性****

```
**`<html>
<body>
<font color="#00FF00">Font color example using color attribute</font>
</body>
</html>`**
```

****RGB 属性****

```
**`<html>
<body>
<font color="rgb(0,255,0)">Font color example using color attribute</font>
</body>
</html>`**
```

## ******自动对焦属性******

********自动对焦**** 属性是布尔属性。****

**如果存在，它指定该元素在页面加载时应自动获得输入焦点。**

**文档中只有一个表单元素可以有 ****自动聚焦**** 属性。它不能应用于`<input type="hidden">`。**

### ****适用于****

**元素属性`<button>`自动对焦`<input>`自动对焦`<select>`自动对焦`<textarea>`自动对焦**

### ****例子****

```
`<form>
    <input type="text" name="fname" autofocus>
    <input type="text" name="lname">
</form>`
```

### ****兼容性****

**这是一个 HTML5 属性。**

## ****Onclick 事件属性****

**单击元素时会触发事件。**

**它就像元素的 *onclick 方法*或`addEventListener('click')`一样工作。**

```
`<element onclick="event"></element>`
```

**`event`可以是一个 JavaScript 函数，也可以编写原始 JavaScript**

### ****例题****

**点击时改变`<p>`元素的颜色**

```
`<p id="text" onclick="redify()">Change my color</p>

<script>
function redify(){
  let text = document.querySelector('#text');
  text.style.color = "red";
}
</script>`
```

**使用原始 JavaScript onclick 属性:**

```
`<button onclick="alert('Hello')">Hello World</button>`
```

## ****Img 对齐属性****

**图像的 align 属性根据周围的元素指定图像的对齐位置。**

**属性值:
右对齐图像到右侧左对齐图像到左侧
顶部对齐图像到顶部
底部对齐图像到底部
中间对齐图像到中间**

**例如:**

```
`<!DOCTYPE html>
<html lang="en">
  <head>
   <title>Img Align Attribute</title>
 </head>
<body>
  <p>This is an example. <img src="image.png" alt="Image" align="middle"> More text right here
  <img src="image.png" alt="Image" width="100"/>
  </body>
</html>`
```

**如果我们愿意，我们也可以右对齐:**

```
`<p>This is another example<img src="image.png" alt="Image" align="right"></p>`
```

******请注意 HTML5 不支持 align 属性，应该使用 CSS 来代替。然而，它仍然受到所有主流浏览器的支持。******

## ****输入类型属性****

**输入类型属性指定用户应该在表单中输入的类型。**

### ****正文****

**一行文字。**

```
 `<form>
      <label for="login">Login:</label>
      <input type="text" name="login">
    </form>`
```

### ****密码****

**一行文字。文本自动显示为一系列点或星号(取决于浏览器和操作系统)。**

```
 `<form>
      <label for="password">Password:</label>
      <input type="password" name="password">
    </form>`
```

### ****电子邮件****

**HTML 检查输入是否匹配电子邮件地址格式(something@something)。**

```
 `<form>
      <label for="email">E-mail address:</label>
      <input type="email" name="email">
    </form>`
```

### ****号****

**只允许数字输入。您还可以指定允许的最小值和最大值。下面的示例检查输入是 1 到 120 之间的数字。**

```
 `<form>
      <label for="age">Age:</label>
      <input type="number" name="age" min="1" max="120">
    </form>`
```

### ****收音机****

**用户只能选择一个选项。单选按钮组需要具有相同的名称属性。您可以使用`checked`属性自动选择一个选项(在下面的示例中，选择了蓝色值)。**

```
 `<form>
      <label><input type="radio" name="color" value="red">Red</label>
      <label><input type="radio" name="color" value="green">Green</label>
      <label><input type="radio" name="color" value="blue" checked>Blue</label>
    </form>`
```

### ****复选框****

**用户可以从复选框组中选择零个或多个选项。您也可以在这里使用`checked`属性来选择一个或多个选项。**

```
 `<form>
      <label><input type="checkbox" name="lang" value="english">english</label>
      <label><input type="checkbox" name="lang" value="spanish">spanish</label>
      <label><input type="checkbox" name="lang" value="french">french</label>
    </form>`
```

### ****按钮****

**输入显示为按钮，按钮中应该显示的文本在值属性中。**

```
 `<form>
      <input type="button" value="click here">
    </form>`
```

### ****提交****

**显示提交按钮。应该在按钮中显示的文本在 value 属性中。点击按钮后，HTML 进行验证，如果通过，表单被提交。**

```
 `<form>
      <input type="submit" value="SUBMIT">
    </form>`
```

### ****复位****

**显示重置按钮。应该在按钮中显示的文本在 value 属性中。点击按钮后，表单中的所有值都将被删除。**

```
 `<form>
      <input type="reset" value="CANCEL">
    </form>`
```

**有更多类型元素。**

## **其他 HTML 属性:**

### **[<脚本> src 属性](https://www.freecodecamp.org/news/link-javascript-to-html-with-the-src/)**

### **[滚动属性](https://www.freecodecamp.org/news/html-role-attribute/)**

### **[< a href >属性](https://www.freecodecamp.org/news/the-a-href-attribute-explained/)**

### **[<一个目标>属性](https://www.freecodecamp.org/news/the-a-target-html-attribute-explained/)**