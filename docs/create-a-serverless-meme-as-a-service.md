# 如何创建无服务器的 Meme 即服务

> 原文：<https://www.freecodecamp.org/news/create-a-serverless-meme-as-a-service/>

“迷因经济”显然是下一件大事。是互联网“注意力经济”的自然延伸。在许多其他人当中，甚至 T2 的埃隆·马斯克也在做这件事。据估计，模因经济已经价值[2 . 5 亿美元](https://news.bitcoin.com/blockchain-backed-nft-market-value-grew-299-in-2020/)。

> 迷因经济是 FOMO 和 YOLO 相遇的地方。AXIOS 的费利克斯·萨蒙

然而，创造一个迷因需要时间。一个允许任何人定制一个迷因并生成一个新迷因的 web 应用程序怎么样？那是一种文化基因即服务(MaaS)！

## 为什么没有服务器？

作为开发人员，创建一个为图像添加文本标题的 web 应用程序可能并不难。然而，迷因即服务有一些额外的要求。

*   成像处理任务通常是计算密集型的，并且需要高性能。
*   迷因服务可能用处不大，或者会大受欢迎。换句话说，它需要可扩展，开发者只为实际使用付费。

以上问题都有解决方案。首先，我们将使用现代高性能编程语言来编写图像和文本操作函数。为此，我们将使用 Rust，它提供了本机性能，但具有内存安全性。

接下来，我们可以通过公共云中的无服务器功能来最好地满足可扩展性需求。无服务器功能在不使用时是免费的，可以迅速扩展到数百万用户。

虽然可以将从 Rust 编译的本机程序作为无服务器功能运行，但更好的方法是在 WebAssembly VM 中将 Rust 程序作为无服务器功能运行。

WebAssembly VM 充当本机应用程序和无服务器主机环境之间的兼容层和安全沙箱。它允许 Rust 程序具有更高的可移植性，因为 WebAssembly VM 被预先配置为在公共云无服务器运行时所需的各种操作系统和容器映像中运行。

此外，WebAssembly 还能让 Rust 程序轻松安全地访问用 C/C++编写的软件库。一个例子是从 Rust 访问遗留操作系统中的 Tensorflow 库。

## 快速启动

在本教程中，我们将使用 GitHub 上的模板项目[在腾讯云上部署我们的 Rust meme-as-a-service。该模板基于开源的无服务器框架。](https://github.com/second-state/tencent-meme-scf)

Rust 程序编译并运行在第二个状态虚拟机(SSVM)上，这是一个针对在基于云的主机环境中运行而优化的 WebAssembly 虚拟机。

> 虽然我们在这个例子中使用腾讯云，但无服务器框架对所有领先的公共云都是不可知的。只需稍作改动，就可以轻松部署到 AWS 或 Azure。

首先，你需要[安装无服务器框架](https://www.serverless.com/framework/docs/getting-started/)并在[腾讯云](https://cloud.tencent.com/)上创建一个免费账户。接下来，派生或克隆[模板 GitHub repo](https://github.com/second-state/tencent-meme-scf) ，然后将 cd 放入其目录。

```
$ git clone https://github.com/second-state/tencent-meme-scf
$ cd tencent-meme-scf
```

现在可以使用无服务器框架来部署云函数、函数服务的 API 网关以及使用函数服务的静态 HTML 页面。在`.env`文件中配置部署到腾讯云。

只需按照屏幕上的说明登录腾讯云并进行部署。

```
$ sls deploy
... ...
  website:       https://sls-website-ap-hongkong-kfdilz-1302315972.cos-website.ap-hongkong.myqcloud.com
  vendorMessage: null

63s › tencent-meme-scf › "deploy" ran for 3 apps successfully.
```

在浏览器中加载`website` URL，查看您部署的 [meme 即服务在运行](https://sls-website-ap-hongkong-hmtn9c-1302315972.cos-website.ap-hongkong.myqcloud.com/)！它允许你为 meme 图片上的每个标题/水印定制文本、位置和大小。

## 应用程序如何工作

该应用程序的整体架构是一个典型的 JAMStack 应用程序。用于图像处理的后端逻辑被部署为无服务器功能，并且可通过 API 获得。

前端 UI 是一个静态的 HTML 和 JavaScript 网页。前端通过 JavaScript Ajax 调用与后端 API 交互。

在 [main.rs](https://github.com/second-state/tencent-meme-scf/blob/main/src/main.rs) 文件中用 Rust 编写了为 meme 背景图片添加字幕(或水印)的后端无服务器函数。它首先读取水印文本的字体文件和背景图像文件。

然后，它从指定每个水印的文本、位置和大小的用户应用程序中读取输入。输入是 JSON 文本的形式，使用`serde_json`库将它解析成一个 Rust 结构数组。

`_watermark()`函数将每个水印添加到图像中。该函数输出最终图像，并对其进行 base64 编码。无服务器运行时的 API 网关将 base64 编码的图像返回给要在网页上显示的调用 JavaScript。

```
const FONT_FILE : &[u8] = include_bytes!("PingFang-Bold.ttf") as &[u8];
const TEMPLATE_BUF : &[u8] = include_bytes!("bg.png") as &[u8];

fn main() {
  let mut buffer = String::new();
  io::stdin().read_to_string(&mut buffer).expect("Error reading from STDIN");
  let obj: FaasInput = serde_json::from_str(&buffer).unwrap();

  let mut img = image::load_from_memory(TEMPLATE_BUF).unwrap();

  let memes: Vec<Watermark> = serde_json::from_str(&(obj.body)).unwrap();
  for m in memes {
    _watermark(m, &mut img);
  }

  let mut buf = vec![];
  img.write_to(&mut buf, image::ImageOutputFormat::Png).unwrap();
  println!("{}", base64::encode_config(buf, base64::STANDARD));
}
```

`_watermark()`功能为图像添加一段水印文本。它利用标准的 Rust 图像处理库(称为 crates)来处理迷因图像。

```
fn _watermark(w: Watermark, img: &mut image::DynamicImage) {
  let font_size = w.font_size;

  let font = Vec::from(FONT_FILE);
  let font = Font::try_from_vec(font).unwrap();

  let scale = Scale {
    x: font_size,
    y: font_size,
  };
  drawing::draw_text_mut(img, image::Rgba([0u8, 0u8, 0u8, 255u8]), w.left, w.top, scale, &font, &w.text);
}
```

前端 JavaScript 从 HTML 表单中获取用户输入的文本、位置和字体大小，将 JSON 中的输入数据提交给云函数，然后显示返回的 base64 图像。

```
var memes = [];
memes[0] = {};
memes[0].text = $('#top-says').val();
memes[0].left = parseInt($('#top-left').val());
memes[0].top = parseInt($('#top-top').val());
memes[0].font_size = parseInt($('#top-font').val());
... ...

$.ajax({
  url: window.env.API_URL,
  type: "post",
  data : JSON.stringify(memes),
  dataType: "text",
  success: function (data) {
    const img_url = "data:image/png;base64," + data;
    $('#wm_img').prop('src', img_url);
  },
  error: function(jqXHR, exception){
    console.log("Error Status: " + jqXHR.statusText);
  }
});
```

## 构建你自己的“迷因即服务”

有了源代码模板，你可以创建自己的 meme 即服务。您可以更改 meme 背景图像，并更改添加文本水印的用户界面。

为此，首先确保您已经安装了 [Rust](https://www.rust-lang.org/tools/install) 编译器和 [ssvmup](https://www.secondstate.io/articles/ssvmup/) 构建工具。

对 Rust 代码和 HTML 文档进行更改。将 Rust 函数编译到 WebAssembly，并将结果复制到`scf/`文件夹。

```
$ ssvmup build --enable-aot
$ cp pkg/scf.so scf/
```

运行无服务器框架命令来部署应用程序。

```
$ sls deploy
... ...
  website:       https://sls-website-ap-hongkong-kfdilz-1302315972.cos-website.ap-hongkong.myqcloud.com
  vendorMessage: null

63s › tencent-meme-scf › "deploy" ran for 3 apps successfully.
```

## 下一步是什么

现在你已经创建并发布了你自己的“迷因即服务”,恭喜你！

使用 JAMStack 应用程序和无服务器功能，您可以做更多的事情。例如，您可以为 Tensorflow 推理创建无服务器函数，以将高级图像识别和处理集成到您的应用程序中。