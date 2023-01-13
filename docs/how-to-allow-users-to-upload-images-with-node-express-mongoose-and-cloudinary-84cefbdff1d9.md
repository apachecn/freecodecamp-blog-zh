# 如何允许用户使用 Node/Express、Mongoose 和 Cloudinary 上传图像

> 原文：<https://www.freecodecamp.org/news/how-to-allow-users-to-upload-images-with-node-express-mongoose-and-cloudinary-84cefbdff1d9/>

你是否正在构建一个全栈应用程序，并希望让用户上传图像，但你不知道如何上传？根据我的经验，这通常是通过让用户输入一个链接并将这个字符串保存到数据库中来实现的。这种方法很好，又快又简单，但也是一种欺骗。什么样的应用程序让你先去另一个网站上传你的图片，然后再回来链接？

那么，有什么解决办法呢？

允许用户输入一个文件，然后在你的服务器上，将这个文件上传到云服务，并保存在你的数据库中。Cloudinary 非常适合这个。它专用于媒体上传。它有很棒的文档。它允许转换。**和**有巨大的免费计划(10 GB 存储)。你可以在这里报名 [Cloudinary(我为此免费)。](https://cloudinary.com/invites/lpov9zyyucivvxsnalc5/yytj9stwvdsschwyccf8)

### 让我们从前端开始吧

```
<form action='/api/images' method="post" enctype="multipart/form-data">
  <input type='file' name='image' />
</form>
```

这个应该看着眼熟。您所需要的只是一个将信息提交给服务器的表单。向服务器提交文件需要 Enctype。

那就是前端解决了。

### 后端

现在，后端是所有奇迹发生的地方。你将需要使用 **Express** 和**mongose**的所有常用依赖项。此外，我们将利用 **Multer** 、 **Cloudinary** 和**Multer-storage-cloud inary**。Multer 将允许访问通过表单提交的文件。Cloudinary 用于配置和上传。多存储云将使结合这些的过程变得容易。

```
const multer = require("multer");
const cloudinary = require("cloudinary");
const cloudinaryStorage = require("multer-storage-cloudinary");
```

一旦需要依赖项，您就需要配置它们。当您注册 Cloudinary 时，您将获得 API 凭据。我建议将这些储存在一个。env "文件来保证它们的安全。

下面我们也是:

*   设置一个文件夹来保存这个项目在 Cloudinary 上组织的所有图像
*   仅确保”。jpg“和”。png”文件被上传
*   添加转换以保持高度和宽度一致，并管理文件大小。

关于转换，你还可以做更多的事情——如果你感兴趣，可以看看这里的。

```
cloudinary.config({
cloud_name: process.env.CLOUD_NAME,
api_key: process.env.API_KEY,
api_secret: process.env.API_SECRET
});
const storage = cloudinaryStorage({
cloudinary: cloudinary,
folder: "demo",
allowedFormats: ["jpg", "png"],
transformation: [{ width: 500, height: 500, crop: "limit" }]
});
const parser = multer({ storage: storage });
```

现在您的服务器已经设置好接收和处理这些图像，我们可以开始设置路线了。

在您的 post 路径中，您只需添加我们之前设置的解析器作为中间件。这将接收文件，将其上传到 Cloudinary，并返回一个包含文件信息的对象。您可以在请求对象中访问这些信息。

我喜欢从中提取我想要的信息，以保持我的数据库井然有序。至少，你会想要:

*   可用于在前端显示图像的 URL
*   public_id 将允许您从 Cloudinary 访问和删除映像。

```
app.post('/api/images', parser.single("image"), (req, res) => {
  console.log(req.file) // to see what is returned to you
  const image = {};
  image.url = req.file.url;
  image.id = req.file.public_id;
  Image.create(image) // save image information in database
    .then(newImage => res.json(newImage))
    .catch(err => console.log(err));
});
```

您的图像可能是数据库中更大对象的一部分。图像 URL 和 id 可以保存为字符串，作为其中的一部分。

**图像是数据库集合的占位符示例。用你自己的来代替。*

### 显示图像

当您想要在前端显示图像时，执行数据库查询，然后使用图像标签`<img src=imageURL` / >中的 URL。

我希望这能帮助你在你的网站上增加额外的东西。一旦你分解了这个过程中的每一步，事情就没那么难了。这会给你的网站带来专业的感觉，让它脱颖而出。

如果你有任何问题，请在评论中提问。