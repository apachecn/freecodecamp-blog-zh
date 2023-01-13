# JavaScript 中的基本表单验证

> 原文：<https://www.freecodecamp.org/news/basic-form-validation-in-javascript/>

过去，表单验证发生在服务器上，在用户已经输入了所有信息并按下提交按钮之后。

如果信息不正确或丢失，服务器将不得不发送回所有信息，告诉用户在再次提交表单之前更正表单。

这是一个漫长的过程，会给服务器带来很大的负担。

现在，JavaScript 提供了多种方法在浏览器中验证表单数据，然后再将其发送到服务器。

以下是我们将在以下示例中使用的 HTML 代码:

```
<html>
<head>
  <title>Form Validation</title>
  <script type="text/javascript">
    // Form validation will go here
  </script>
</head>
<body>
  <form id="form">
    <table cellspacing="2" cellpadding="2" border="1">
      <tr>
        <td align="right">Username</td>
        <td><input type="text" id="username" /></td>
      </tr>
      <tr>
        <td align="right">Email Address</td>
        <td><input type="text" id="email-address" /></td>
      </tr>
      <tr>
        <td></td>
        <td><input type="submit" value="Submit" id="submit-btn" /></td>
      </tr>
    </table>
  </form>
</body>
</html>
```

## 基本验证

这种类型的验证包括检查所有必需字段，并确保它们被正确填写。

下面是一个函数`validate`的基本示例，如果用户名和电子邮件地址输入为空，它会显示警告，否则返回 true:

```
const submitBtn = document.getElementById('submit-btn');

const validate = (e) => {
  e.preventDefault();
  const username = document.getElementById('username');
  const emailAddress = document.getElementById('email-address');
  if (username.value === "") {
    alert("Please enter your username.");
    username.focus();
    return false;
  }
  if (emailAddress.value === "") {
    alert("Please enter your email address.");
    emailAddress.focus();
    return false;
  }

  return true;
}

submitBtn.addEventListener('click', validate); 
```

但是如果有人输入随机文本作为他们的电子邮件地址呢？目前，`validate`函数仍将返回 true。可以想象，向服务器发送坏数据会导致问题。

这就是数据格式验证的用武之地。

## 数据格式验证

这种类型的验证实际上是查看表单中的值，并验证它们是否正确。

众所周知，验证电子邮件地址非常困难——有大量合法的电子邮件地址和主机，不可能猜出所有有效的字符组合。

也就是说，所有有效的电子邮件地址都有几个共同的关键因素:

*   地址必须包含一个@和至少一个点(。)字符
*   @字符不能是地址中的第一个字符
*   的。@字符后必须至少有一个字符

考虑到这一点，开发人员可能会使用 regex 来确定电子邮件地址是否有效。泰勒·麦金尼斯在他的博客上推荐了一个功能:

```
const emailIsValid = email => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

emailIsValid('free@code@camp.org') // false
emailIsValid('quincy@freecodecamp.org') // true
```

添加到上一个示例的代码中，它将如下所示:

```
const submitBtn = document.getElementById('submit-btn');

const validate = (e) => {
  e.preventDefault();
  const username = document.getElementById('username');
  const emailAddress = document.getElementById('email-address');
  if (username.value === "") {
    alert("Please enter your username.");
    username.focus();
    return false;
  }

  if (emailAddress.value === "") {
    alert("Please enter your email address.");
    emailAddress.focus();
    return false;
  }

  if (!emailIsValid(emailAddress.value)) {
    alert("Please enter a valid email address.");
    emailAddress.focus();
    return false;
  }

  return true; // Can submit the form data to the server
}

const emailIsValid = email => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

submitBtn.addEventListener('click', validate); 
```

## HTML5 表单约束

一些常用于`<input>`的 HTML5 约束是`type`属性(例如`type="password"`)、`maxlength`、`required`和`disabled`。

一个不太常用的约束是采用 JavaScript 正则表达式的`pattern`属性。

## 更多信息

*   [表单数据验证| MDN](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Form_validation)
*   [约束验证| MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/Constraint_validation)