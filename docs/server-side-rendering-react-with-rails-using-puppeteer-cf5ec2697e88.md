# 如何使用木偶师在 React with Rails 中设置服务器端渲染

> 原文：<https://www.freecodecamp.org/news/server-side-rendering-react-with-rails-using-puppeteer-cf5ec2697e88/>

作者:西塔拉姆·谢尔克

# 如何使用木偶师在 React with Rails 中设置服务器端渲染

这篇文章由我在 [Altizon Systems](http://www.altizon.com) 的同事 [Hricha Kabir](https://in.linkedin.com/in/hrichakabir) 合著。她主要从事这项工作。我有机会一路学习。

![SFMTJgWJQqSDFXU-rUJrn4HkgCccugY1GpYh](img/2a01c281cd7893bcd1b371538e24c227.png)

Photo by [Robert V. Ruggiero](https://unsplash.com/@rvruggiero?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们致力于专注于制造业的物联网产品，并构建分析报告。大多数情况下，报表的设计和信息会根据使用它的用户的角色而变化。

例如:主管级人员对一周的综合报告感兴趣，而部门主管对一天的统计数据感兴趣，质量经理对轮班数据感兴趣。如果我们进一步深入这个层次结构，那么生产经理将是希望查看实时生产数据的人。

对于实时数据，生产经理可以打开实时仪表板并对其进行监控，但希望查看合并报告的用户不会选择登录产品并查看报告。相反，他们更喜欢预定的电子邮件通信，这种通信将根据预定的时间发送报告，并一直重复，直到用户停止该预定的作业。

因此，我们的任务是编写一个实用程序，每天或在规定的时间定期通过电子邮件向用户发送特定的报告。对于调度，我们使用 [Sidekiq](https://sidekiq.org/) ，但我们的主要挑战是:如何在服务器端执行/计算报告(不打开浏览器)并通过电子邮件发送。

在我们所有的产品中，我们使用 **React** 作为前端，它负责在浏览器上呈现报告。为了在电子邮件中附加相同的报告，我们必须为每个报告设计一个 HTML 模板。这意味着我们对每个报告都有两个视图，一个用 HTML 编写，另一个用 ReactJS 编写。

这具有以下缺点:

**1。**产品代码库会有重复代码，这违反了软件开发的 DRY 原则。

**2。**增加设计影响产品交付的报告的时间。

我们决定寻找其他可能的解决方案，而不是重复编写相同的代码。我们遇到了以下选择，并逐一探索，直到我们成功实现目标。

**1。打印屏幕:**使用打印屏幕打印报告，并将其作为附件发送。

**2。前端打印按钮:**在报表界面，增加一个打印按钮，将 react 组件转换成 HTML/PDF 页面。

**3。Ruby Gem:** 有一个 Ruby Gem，它可以在 Rails 中的服务器端呈现一个报表组件，生成它的 HTML 并作为邮件正文发送。

**4。使用节点服务器的服务器端渲染:**使用服务器端渲染使用 ReactDOMServer 和 [headless](https://developers.google.com/web/updates/2017/04/headless-chrome) 浏览器协议渲染 HTML 和 JS 并生成 pdf。

现在，我们将讨论每个解决方案的细节。

#### **打印屏幕:**

这是我们最简单的解决方案，需要用户输入。用户需要有一个系统，该系统会将屏幕截图作为图像，并将其附加在邮件上。但是这个没有规模。另外，如果用户希望在预定时间或定期发送报告，该怎么办？

例 **:** 每周晚 8 点。一个人不可能做到这一点。这也受到无法完全捕获可滚动页面的影响，所以我们不得不放弃这个想法。

#### **前端打印按钮:**

在做了一些搜索后，我们发现有一些可用的浏览器扩展可以生成一个打开页面的完整图像。例子:Chrome 中的全页面截屏。将这个扩展添加到浏览器后，我们将能够以图像(png/jpg)格式捕获任何屏幕。

同样，这种解决方案不需要开发，但会遇到一些以前的缩放和调度问题。同时，我们仍然需要一个登录到浏览器的用户来执行这个操作，这违背了电子邮件传递的目的。

#### **RubyGem:**

很快我们意识到我们的解决方案不需要浏览器交互。我们需要使用[服务器端渲染](https://alligator.io/react/server-side-rendering/)。所以我们开始探索支持 React 服务器端渲染的 Ruby Gems。我们探索了以下宝石

**3.1[rails _ react _ stdio](https://github.com/aaronvb/rails_react_stdio):**

它基于 [react-stdio](https://github.com/ReactTraining/react-stdio) ，支持服务器端渲染，与服务器端技术无关。它作为一个二进制文件来完成渲染 react 组件的工作。为了在服务器端渲染 react 组件，我们需要传递 React 组件和 props 的文件路径(如果需要的话)。它将返回一个 JSON 响应，其中包含报告的 HTML 代码。此外，我们可以通过电子邮件发送 HTML 内容。

在内部，这个 gem 使用 *popen3* 来执行渲染命令。但这意味着 *react-stdio* 二进制文件需要存在于运行我们的 rails 应用程序的 docker 容器中。

从可维护性和可再现性的角度来看，这并不太好。此外，带有图表的大 HTML 内容加载起来会很慢，所以我们更喜欢 pdf 附件。然而我们试了一下。

示例 **:** 用于呈现包含组件 *TestComponent* 和文件路径*app/assets/JavaScript s/components/test component . jsx*的报告

首先，将宝石包含在 Gemfile 中:

```
gem ‘rails_react_stdio’, ‘~> 0.1.0’
```

现在，从电子邮件调度程序调用 gem 方法来呈现报告，并从响应中获取 HTML。然后将此回复发送至电子邮件。

```
email_body = RailsReactStdio::React.render(‘app/assets/javascripts/components/TestComponent.jsx’, {city: “Pune”})
```

我们尝试使用上述方法，但没有成功。此外，GitHub repo 没有得到积极的维护，所有的测试案例都失败了。所以我们决定不用这个继续前进。

**3.2 反应轨道:**

ReactJS 社区建造了这个宝石。它使用 [ExecJS](https://github.com/rails/execjs) 在服务器端执行 react 组件的渲染动作。我们只需要通过一个标志*‘先决条件’:真*。

```
<%= react_component(‘Dashboard’, {name: ‘Example’}, {prerender: true}) %>
```

这个预渲染过程不能访问窗口或文档，所以它不加载运行时 JavaScript 或 CSS。我们也在一些事情上使用 JQuery，所以它不能很好地工作。

还有一个替代方案:这个 gem 有另一个用于服务器端呈现的类， *ExecJSRenderer* ，它帮助 JavaScript 在服务器端使用组件。

*ExecJSRenderer* 类有两个方法: *before_render* 和 *after_render，*，用于访问组件渲染前后所需的 JavaScript。但是这需要对现有的代码库进行大量的修改，以支持服务器渲染，在每个控制器中。除此之外，ExecJS 不提供沙箱以及运行时错误信息。我们仍在寻找更好的东西。

#### **使用节点服务器的服务器端渲染:**

我们在内部探索的大多数 ruby gems 都创建了节点服务器，并在服务器端呈现 react。所以我们没有使用 Ruby，而是决定直接使用节点服务器，自己完成这个任务。

**4.1 基于 ReactDOMServer 的解决方案:**

这里我们使用 [ReactDOMServer](https://reactjs.org/docs/react-dom-server.html) 。这是 React 团队对服务器端渲染的首选解决方案。我们创建了一个节点服务器，它使用 react 组件调用 *renderToString()* 方法。它返回我们与 HTML 结合并通过电子邮件发送的渲染内容。

示例 **:**

```
server.get(‘/’, (req, res, next) => {
  /**
  * renderToString() will take our React app and turn it into a      string
  * to be inserted into our Html template function.
  */
  console.log(‘started’)
  const body = renderToString(<App />);
  const title = ‘Server side Rendering React Components’;
  var result = Html({ body, title })
}
```

方法返回一个字符串响应。我们将这个响应传递给一个 HTML 模板，并通过邮件发送该模板。

当我们对此进行测试时，正如预期的那样收到了一封电子邮件，只是报告中使用的所有图像都被破坏了。电子邮件无法解析图像源的相对路径。

为了在电子邮件中获得正确的图像，我们需要

*   在 S3 存储图像并在报告中使用源 URL:所以现在我们将在电子邮件中添加 S3 图像 URL，并且电子邮件服务器将直接从 S3 服务器加载图像。它需要额外的云空间来存储图像，从目的地下载需要从电子邮件收件箱进行另一次网络呼叫。

或者

*   在邮件中发送图像的 base64 代码:我们可以发送图像的 base64 代码，而不是图像 URL。虽然它增加了网络负载，但许多邮件服务器(如 Outlook 和 Gmail)会阻止 base64 图像。

所以我们仍然需要做些什么。

4.2 [木偶师](https://github.com/GoogleChrome/puppeteer) :

在探索了上述方法之后，我们发现了木偶师无头 Chrome 服务。Puppeteer 是 Google Chrome 团队的一个 NodeJS 库，用于端到端测试。默认情况下，它使用 Chrome/Chromium 浏览器。本质上，它模拟了用户可以在浏览器中执行的所有操作。例如:键盘输入、鼠标事件、表单提交等。

木偶请求的结果可以是 HTML 页面、屏幕截图或 PDF。对于 HTML，它在服务器端呈现整个页面以及所有图像、CSS 和 JavaScript。或者用户可以在服务器端呈现页面，如果需要，他们可以生成截图或 pdf。

如果我们将这个库与一个节点服务器一起使用，我们可以在 Sidekiq 中调度一个任务，这个任务将向这个服务器发出请求，呈现一个报告，并通过电子邮件发送它。Puppeteer 还有一组丰富的 API，支持在请求中发送定制的报头，帮助我们认证后台请求。

这正是我们需要的！

**因此，puppeteer 中的整个请求生命周期应该是这样的:**

**1。**启动浏览器

**2。**在浏览器上创建页面

**3。**通过报表应用服务器进行身份验证

**4。**打开用户想要的报表 URL 并返回呈现的页面内容

**5。**根据用户的需求，存储渲染页面的 HTML 或者截图或者生成 PDF。

我们可以使用默认的浏览器(当我们安装木偶师时下载)或者我们可以给一个特定的浏览器版本。如果我们给定一个特定的浏览器版本，那么我们必须确保 puppeteer APIs 与给定的浏览器兼容。

以上所有都可以通过以下步骤实现。

**1。安装木偶师:**

它下载默认的 Chrome/Chromium 浏览器。因此，如果我们想启动默认浏览器并安装木偶师:

```
npm install puppeteer
```

**2。服务请求:**

构建一个服务器，它将接收请求，实现请求生命周期并返回期望的结果。

下面是我们用来生成给定页面 URL 的 pdf 的代码片段。

```
const puppeteer = require(“puppeteer”);
const browser = await puppeteer.launch();
const page = await browser.newPage();
await page.emulateMedia(“screen”);
await page.goto(‘https://www.google.com', {
      timeout: 30 * 1000, 
      waitUntil: “networkidle0”
});
await page.pdf(pdfOptions);
return page;
```

正如我们在上面的生命周期步骤中提到的，它首先启动浏览器。然后，它在启动的浏览器中创建和打开一个页面。在这里，除了 URL，我们还传递了*超时*和*等待直到*参数，这些参数是基于以下原因给出的:

*   *超时*:如果我们想要限制请求时间，那么我们可以将它传递给超时变量
*   *waitUntil* :如果给定了 networkidle0，那么直到或者除非当前请求完成，否则下一个请求不会被服务。

最后，呈现的内容将被传递给 pdf 方法，它将生成一个 pdf。我们还可以提供 PDF 格式选项，如页面高度、页面宽度、页眉、页脚和页边距。

此外，我们还调用了 page . emulate media(“screen”)；它将 CSS 应用于页面。如果我们不添加 emaulateMedia，我们的 PDF 就不会加载 CSS。

除了我们已经使用的，还有许多不同的配置选项。请访问 [API 文档](https://github.com/GoogleChrome/puppeteer/blob/v1.12.2/docs/api.md#)了解更多信息。

如果你觉得这很有帮助，或者你有任何建议，请随时写在评论中。

这是所有的乡亲。