# 边缘云微服务——如何使用 WasmEdge 和 Rust 构建高性能和安全的应用

> 原文：<https://www.freecodecamp.org/news/edge-cloud-microservices-with-wasmedge-and-rust/>

边缘云允许开发人员在靠近用户的地方部署微服务(即细粒度的 web 服务)。这为他们提供了更好的用户体验(和非常快的响应时间)、安全性和高可用性。

它还利用本地甚至私有数据中心、CDN 网络和电信数据中心(例如 5G MECs)来提供计算服务。

边缘云的成功例子包括 Cloudflare、Fastly、Akamai、fly.io、Vercel、Netlify 等等。

但与大型公共云相比，边缘云也是一个资源受限的环境。如果边缘微服务本身很慢或臃肿或不安全，它们将挫败在边缘云上部署的整个目的。

在本文中，我将向您展示如何在 [WebAssembly 沙箱](https://github.com/WasmEdge)中创建轻量级和高性能的 web 服务，然后在边缘云提供商 [fly.io](http://fly.io) 上免费部署它们。

[Fly.io](http://Fly.io) 是边缘云上虚拟机服务的领先提供商。它在世界各地都有边缘数据中心。 [fly.io](http://Fly.io) 虚拟机支持应用服务器、数据库，在我们的例子中，还支持微服务的轻量级运行时。

我将使用 [WasmEdge 运行时](https://github.com/WasmEdge/WasmEdge)作为那些微服务的安全沙箱。WasmEdge 是专门为云原生服务优化的 WebAssembly 运行时。

我们将用 Rust 或 JavaScript 编写的微服务应用程序封装在基于 WasmEdge 的 Docker 映像中。

这种方法有几个引人注目的优点:

*   WasmEdge 以接近本机的速度运行沙盒应用程序。根据一项同行评议的研究，WasmEdge 运行 Rust 程序的速度几乎与 Linux 运行本机代码的速度相同。
*   WasmEdge 是一个高度安全的运行时。它保护您的应用免受外部和内部威胁。
*   WasmEdge 运行时的攻击面比常规的 Linux OS 运行时显著减少。
*   软件供应链攻击的风险大大降低，因为 WebAssembly 沙箱只能访问显式声明的功能。
*   WasmEdge 提供了一个完整的可移植的应用程序运行时环境，其内存占用仅为标准 Linux 操作系统运行时映像的 1/10。
*   WasmEdge 运行时是跨平台的。这意味着机器的开发和部署不必相同。一旦创建了 WasmEdge 应用程序，您就可以将其部署到任何支持 WasmEdge 的地方，包括 [fly.io](http://fly.io) 基础设施。

如果应用程序很复杂，那么性能优势会被放大。例如，WasmEdge AI 推理应用程序不需要安装 Python。WasmEdge node.js 应用程序不需要安装 node.js 和 v8。

在本文的其余部分，我将演示如何运行:

*   异步 HTTP 服务器(在 Rust 中)
*   一个非常快速的图像分类网络服务(Rust ),以及
*   一个节点。JS web 服务器
*   具有数据库连接的有状态微服务

它们都在 WasmEdge 中快速安全地运行，而消耗的资源只有普通 Linux 容器的 1/10。

### 先决条件

首先，如果您的系统上已经安装了 Docker 工具，那就太好了。如果没有，现在请按照本手册的第一部分安装 Docker。然后我们将使用在线安装程序来安装 WasmEdge、Rust 以及用于 [fly.io](http://fly.io) 的`flyctl`工具。

安装 WasmEdge。[详见此处](https://wasmedge.org/book/en/start/install.html)。

```
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash -s -- -e all
```

安装铁锈。[详见此处](https://www.rust-lang.org/tools/install)。

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

安装[飞轮](http://fly.io)的`flyctl`工具。[详见此处](https://fly.io/docs/hands-on/install-flyctl/)。

```
curl -L https://fly.io/install.sh | sh
```

一旦你安装了`flyctl`，按照[的指示在](https://fly.io/docs/hands-on/sign-up/) [fly.io](http://fly.io) 注册一个免费账户。您现在已经准备好在边缘云上部署 web 服务了！

## 生锈的简单微服务

我们的第一个例子是用 Rust 编写的一个简单的 HTTP 服务。它展示了一个现代的 web 应用程序，可以扩展该应用程序以支持任意复杂的业务逻辑。

基于流行的 tokio 和 hyper crates，这种微服务是快速的、异步的(非阻塞的)，并且非常易于开发人员创建。

完全静态链接的 WasmEdge 映像只有 4MB，而基本 Linux 映像有 40MB。这足以运行用 Rust 的 tokio 和 hyper 框架编写的异步 HTTP 服务。

运行以下两个 CLI 命令，从 WasmEdge 的 slim Docker 映像创建并部署一个 [fly.io](http://fly.io) 应用程序。

```
$ flyctl launch --image juntaoyuan/flyio-echo
$ flyctl deploy
```

就是这样！您可以使用 curl 命令来测试部署的 web 服务是否实际工作。无论你向它发送什么数据，它都会回显。

```
$ curl https://proud-sunset-3795.fly.dev/echo -d "Hello WasmEdge on fly.io!"
Hello WasmEdge on fly.io!
```

`juntaoyuan/flyio-echo` Docker 映像的 Docker 文件包含 WasmEdge 运行时和定制 web 应用程序`wasmedge_hyper_server.wasm`的完整包。

```
FROM wasmedge/slim-runtime:0.11.0
ADD wasmedge_hyper_server.wasm /
CMD ["wasmedge", "--dir", ".:/", "/wasmedge_hyper_server.wasm"]
```

构建`wasmedge_hyper_server.wasm`应用[的 Rust 源代码项目可以在 GitHub](https://github.com/WasmEdge/wasmedge_hyper_demo/tree/main/server) 上获得。它使用 tokio API 来启动 HTTP 服务器。

当服务器收到请求时，它委托给`echo()`函数来异步处理请求。这允许微服务接受和处理多个并发的 HTTP 请求。

```
#[tokio::main(flavor = "current_thread")]
async fn main() -> Result<(), Box<dyn std::error::Error + Send + Sync>> {
    let addr = SocketAddr::from(([0, 0, 0, 0], 8080));

    let listener = TcpListener::bind(addr).await?;
    println!("Listening on http://{}", addr);
    loop {
        let (stream, _) = listener.accept().await?;

        tokio::task::spawn(async move {
            if let Err(err) = Http::new().serve_connection(stream, service_fn(echo)).await {
                println!("Error serving connection: {:?}", err);
            }
        });
    }
}
```

异步`echo()`功能如下。它利用 hyper 提供的 HTTP API 来解析请求并生成响应。这里，响应只是请求数据体。

```
async fn echo(req: Request<Body>) -> Result<Response<Body>, hyper::Error> {
    match (req.method(), req.uri().path()) {
        ... ...
        (&Method::POST, "/echo") => Ok(Response::new(req.into_body())),
        ... ...

        // Return the 404 Not Found for other routes.
        _ => {
            let mut not_found = Response::default();
            *not_found.status_mut() = StatusCode::NOT_FOUND;
            Ok(not_found)
        }
    }
}
```

现在让我们添加到基本的微服务来做一些令人印象深刻的事情！

## Rust 中的一个人工智能推理微服务

在这个例子中，我们将为图像分类创建一个 web 服务。它通过 Tensorflow Lite 模型处理上传的图像。

我们将使用 WasmEdge 的 Rust API 来访问 Tensorflow，而不是创建一个复杂(臃肿)的 Python 程序，tensor flow 以全本机代码速度运行推理任务(例如，如果可用，利用 GPU 硬件)。

通过 WASI-NN 标准，WasmEdge 的 Rust API 可以与 Tensorflow、PyTorch、OpenVINO 和其他人工智能框架中的人工智能模型一起工作。

对于包含完整 Tensorflow Lite 依赖项的 AI 推理应用程序，WasmEdge 占用空间小于 115MB。相比之下，标准 Tensorflow Linux 映像超过 400MB。

运行以下两个 CLI 命令，从 WasmEdge + Tensorflow 的 slim Docker 映像创建并部署一个 [fly.io](http://fly.io) 应用程序。

```
$ flyctl launch --image juntaoyuan/flyio-classify
$ flyctl deploy
```

就是这样！您可以使用 curl 命令来测试部署的 web 服务是否实际工作。它返回带有置信度的图像分类结果。

```
$ curl https://silent-glade-6853.fly.dev/classify -X POST --data-binary "@grace_hopper.jpg"
military uniform is detected with 206/255 confidence
```

`juntaoyuan/flyio-classify` Docker 映像的 Docker 文件包含 WasmEdge 运行时的完整包、整个 Tensorflow 库及其依赖项，以及自定义 web 应用程序`wasmedge_hyper_server_tflite.wasm`。

```
FROM wasmedge/slim-tf:0.11.0
ADD wasmedge_hyper_server_tflite.wasm /
CMD ["wasmedge-tensorflow-lite", "--dir", ".:/", "/wasmedge_hyper_server_tflite.wasm"]
```

构建`wasmedge_hyper_server_tflite.wasm`应用[的 Rust 源代码项目可以在 GitHub](https://github.com/WasmEdge/wasmedge_hyper_demo/tree/main/server-tflite) 上获得。与前面的例子一样，基于 tokio 的异步 HTTP 服务器在异步`main()`函数中。

`classify()`函数处理请求中的图像数据，将图像转换成张量，运行 Tensorflow 模型，然后将返回值(以张量形式)转换成文本标签和可能分类的概率。

```
async fn classify(req: Request<Body>) -> Result<Response<Body>, hyper::Error> {
    let model_data: &[u8] = include_bytes!("models/mobilenet_v1_1.0_224/mobilenet_v1_1.0_224_quant.tflite");
    let labels = include_str!("models/mobilenet_v1_1.0_224/labels_mobilenet_quant_v1_224.txt");
    match (req.method(), req.uri().path()) {

        (&Method::POST, "/classify") => {
            let buf = hyper::body::to_bytes(req.into_body()).await?;
            let flat_img = wasmedge_tensorflow_interface::load_jpg_image_to_rgb8(&buf, 224, 224);

            let mut session = wasmedge_tensorflow_interface::Session::new(&model_data, wasmedge_tensorflow_interface::ModelType::TensorFlowLite);
            session.add_input("input", &flat_img, &[1, 224, 224, 3])
                .run();
            let res_vec: Vec<u8> = session.get_output("MobilenetV1/Predictions/Reshape_1");
            ... ...

            let mut label_lines = labels.lines();
            for _i in 0..max_index {
              label_lines.next();
            }
            let class_name = label_lines.next().unwrap().to_string();

            Ok(Response::new(Body::from(format!("{} is detected with {}/255 confidence", class_name, max_value))))
        }

        // Return the 404 Not Found for other routes.
        _ => {
            let mut not_found = Response::default();
            *not_found.status_mut() = StatusCode::NOT_FOUND;
            Ok(not_found)
        }
    }
}
```

在本文的最后一部分，我们将讨论如何向 Rust 微服务添加更多的功能，比如数据库客户端和 web 服务客户端。

## Node.js 中的一个简单微服务

虽然基于 Rust 的微服务又轻又快，但并不是每个人都是 Rust 开发者。

如果您对 JavaScript 更熟悉，您仍然可以在 edge cloud 中利用 WasmEdge 的安全性、性能、占用空间小和可移植性。具体来说，可以使用 Node.js APIs 为 WasmEdge 创建微服务！

对于 Node.js 应用程序，WasmEdge 占用的内存少于 15MB。相比之下，标准 Node.js Linux 映像的内存超过 150MB。

运行以下两个 CLI 命令，从 WasmEdge + Node.js 的 slim Docker 映像创建并部署一个 [fly.io](http://fly.io) 应用程序。

```
$ flyctl launch --image juntaoyuan/flyio-nodejs-echo
$ flyctl deploy
```

就是这样！您可以使用 curl 命令来测试部署的 web 服务是否实际工作。无论你向它发送什么数据，它都会回显。

```
$ curl https://solitary-snowflake-1159.fly.dev -d "Hello WasmEdge for Node.js on fly.io!"
Hello WasmEdge for Node.js on fly.io!
```

`juntaoyuan/flyio-nodejs-echo` Docker 镜像的 Docker 文件包含 WasmEdge 运行时的完整包，QuickJS 运行时`wasmedge_quickjs.wasm`，Node.js [模块](https://wasmedge.org/book/en/dev/js/nodejs.html#the-javascript-modules)，以及 web 服务应用`node_echo.js`。

```
FROM wasmedge/slim-runtime:0.11.0
ADD wasmedge_quickjs.wasm /
ADD node_echo.js /
ADD modules /modules
CMD ["wasmedge", "--dir", ".:/", "/wasmedge_quickjs.wasm", "node_echo.js"]
```

`node_echo.js`应用程序的完整 JavaScript 源代码如下。正如您可以清楚地看到的，它只使用标准的 Node.js APIs 来创建一个异步 HTTP 服务器，该服务器回显 HTTP 请求体。

```
import { createServer, request, fetch } from 'http';

createServer((req, resp) => {
  req.on('data', (body) => {
    resp.end(body)
  })
}).listen(8080, () => {
  print('listen 8080 ...\n');
})
```

WasmEdge 的 QuickJS 引擎不仅提供 Node.js 支持，还提供 Tensorflow 推理支持。我们将 Rust Tensorflow 和 WASI-NN SDK 包装到 JavaScript APIs 中，以便 JavaScript 开发人员能够[轻松创建人工智能推理应用](https://wasmedge.org/book/en/dev/js/tensorflow.html)。

## 边缘的有状态微服务

使用 WasmEdge，还可以创建由数据库支持的有状态微服务。[这个 GitHub repo](https://github.com/WasmEdge/wasmedge-db-examples) 包含 WasmEdge 应用中基于 tokio 的非阻塞数据库客户端的例子。

*   MySQL 客户端允许 WasmEdge 应用程序访问大多数云数据库。
*   [anna-rs 项目](https://github.com/essa-project/anna-rs)是一个边缘本地 KV 存储，在边缘节点上具有可调的同步和一致性级别。WasmEdge 应用[可以使用 anna-rs](https://github.com/WasmEdge/wasmedge-db-examples/tree/main/anna) 作为边缘缓存或数据库。

现在，您可以使用 WasmEdge SDKs 和运行时在 edge cloud 上构建各种各样的 web 服务。迫不及待地想看看你的作品！