# 如何使用 FormData 以简单的方式上传单个或多个文件

> 原文：<https://www.freecodecamp.org/news/formdata-explained/>

在这篇文章中，我们将了解作为 HTML5 规范一部分的现代网络浏览器中可用的表单数据接口。

我们将看到在 Ajax、Angular 7、Ionic 和 React 中使用 FormData 的例子。

## 什么是 FormData

FormData 只是一种可以用来存储键值对的数据结构。顾名思义，它是为保存表单数据而设计的，也就是说，你可以用它和 JavaScript 一起构建一个对应于 HTML 表单的对象。当您需要将表单数据发送到 RESTful API 端点时，它非常有用，例如使用`XMLHttpRequest`接口、`fetch()` API 或 Axios 上传单个或多个文件。

您可以通过使用`new`操作符实例化 FormData 接口来创建 FormData 对象，如下所示:

```
const formData = new FormData() 
```

`formData`引用是指 FormData 的一个实例。您可以在对象上调用许多方法来添加和处理数据对。每一对都有一个键和值。

以下是对 FormData 对象可用的方法:

*   `append()`:用于给对象追加一个键值对。如果该项已经存在，则将该值附加到该项的原始值上，
*   `delete()`:用于删除一个键值对。
*   `entries()`:返回一个迭代器对象，你可以用它来遍历对象中的键值对列表，
*   `get()`:用于返回键值。如果追加了多个值，它返回第一个值，
*   `getAll()`:用于返回指定键的所有值。
*   `has()`:用于检查是否有钥匙，
*   `keys()`:返回一个迭代器对象，你可以用它来列出对象中可用的键，
*   `set()`:用指定的键给对象加值。如果一个键已经存在，
*   `values()`:返回 FormData 对象的值的迭代器对象。

## 使用普通 JavaScript 的文件上传示例

现在让我们看一个使用普通 JavaScript、`XMLHttpRequest`和`FormData`上传文件的简单例子。

导航到您的工作文件夹，创建一个包含以下内容的`index.html`文件:

```
<!DOCTYPE html>
<html>

<head>
	<title>Parcel Sandbox</title>
	<meta charset="UTF-8" />
</head>

<body>
	<div id="app"></div>

	<script src="index.js">
	</script>
</body>

</html> 
```

我们简单地创建一个 HTML 文档，它带有一个由`app` ID 标识的`<div>`。接下来，我们使用一个`<script>`标签来包含`index.js`文件。

接下来，创建`index.js`文件并添加以下代码:

```
document.getElementById("app").innerHTML = `
<h1>File Upload & FormData Example</h1>
<div>
<input type="file" id="fileInput" />
</div>
`;

const fileInput = document.querySelector("#fileInput");

const uploadFile = file => {
  console.log("Uploading file...");
  const API_ENDPOINT = "https://file.io";
  const request = new XMLHttpRequest();
  const formData = new FormData();

  request.open("POST", API_ENDPOINT, true);
  request.onreadystatechange = () => {
    if (request.readyState === 4 && request.status === 200) {
      console.log(request.responseText);
    }
  };
  formData.append("file", file);
  request.send(formData);
};

fileInput.addEventListener("change", event => {
  const files = event.target.files;
  uploadFile(files[0]);
}); 
```

我们首先在 HTML 页面中插入一个`<input type="file" id="fileInput" />`元素。这将用于选择我们将要上传的文件。

接下来，我们使用`querySelector()`方法查询文件输入元素。

接下来，我们定义了`uploadFile()`方法，在该方法中，我们首先声明一个保存文件上传端点地址的`API_ENDPOINT`变量。接下来，我们创建一个`XMLHttpRequest`请求和一个空的`FormData`对象。

我们使用 FormData 的 append 方法将作为参数传递给`uploadFile()`方法的文件附加到`file`键。这将创建一个键-值对，以`file`作为键，以传递的文件内容作为值。

接下来，我们使用`XMLHttpRequest`的`send()`方法发送请求，并将`FormData`对象作为参数传入。

在定义了`uploadFile()`方法之后，我们监听`<input>`元素上的变更事件，并使用所选文件作为参数调用`uploadFile()`方法。从`event.target.files`数组中访问该文件。

您可以从这个代码沙箱中试验这个示例:

## 上传多个文件

你可以很容易地修改上面的代码来支持多文件上传。

首先，您需要将`multiple`属性添加到`<input>`元素中:

```
<input type="file" id="fileInput" multiple /> 
```

现在，您将能够从您的驱动器中选择多个文件。

接下来，更改`uploadFile()`方法以接受一个文件数组作为参数，并简单地遍历该数组并将文件追加到`FormData`对象:

```
const uploadFile = (files) => {
  console.log("Uploading file...");
  const API_ENDPOINT = "https://file.io";
  const request = new XMLHttpRequest();
  const formData = new FormData();

  request.open("POST", API_ENDPOINT, true);
  request.onreadystatechange = () => {
    if (request.readyState === 4 && request.status === 200) {
      console.log(request.responseText);
    }
  };

  for (let i = 0; i < files.length; i++) {
    formData.append(files[i].name, files[i])
  }
  request.send(formData);
}; 
```

最后，使用文件数组作为参数调用该方法:

```
fileInput.addEventListener("change", event => {
  const files = event.target.files;
  uploadFile(files);
}); 
```

接下来，您可以查看这些高级教程，了解如何将`FormData`与 Angular、Ionic 和 React 配合使用:

*   [如何发布带角度 7 的表单数据](https://www.techiediaries.com/angular-formdata/)
*   [反应& Axios 表单数据](https://www.techiediaries.com/react-formdata-file-upload-multipart-form-tutorial/)
*   [用 Ionic 4 上传多个文件&表单数据](https://www.techiediaries.com/ionic-formdata-multiple-file-upload-tutorial/)