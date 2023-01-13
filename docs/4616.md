# 出色的静态网站插件，让你手舞足蹈

> 原文：<https://www.freecodecamp.org/news/how-to-setup-worry-free-blog-comments-in-20-simple-steps/>

**本帖最初来自[www.jaredwolff.com](https://www.jaredwolff.com/how-to-setup-worry-free-blog-comments-in-less-than-20-simple-steps/)**

隐私。

性能。

光彩照人的容貌。

你能三者兼得吗？

(当然！)

拥有一个静态生成的博客很棒。许多人使用像 Disqus 和 Google Analytics 这样的服务来使他们变得更好。如果你是他们中的一员就不足为奇了！隐私问题是每个人关注的焦点。所以，与其保持现状，不如是时候做点什么了！

如果你一直在寻求保护你的网站访问者的隐私和提高性能，这篇博文就是为你准备的。

在本文中，我们将使用 DigitalOcean 的 Docker droplet。它允许您在一台(虚拟)机器上托管多个不同的应用程序/服务。结束时，你将知道如何使用 Commento 运行你自己的评论服务器。另外，我会分享一些我在这个过程中学到的技巧，让你更容易掌握。

李 eets 走！

## 反向代理

这个设置最重要的一个方面是反向代理。反向代理的作用类似于路由器。对某个域的请求进来。该请求然后被路由到与该域相关联的服务。

这是 Nginx 反向代理的图表+让我们加密助手文档。这将有助于说明这个想法。

![Nginx Reverse Proxy with Let's Encrypt](img/f94451adeccb2fb6b7746ff0454dd054.png)

另一个好处是对外界多了一层保护。您的网站运行在一个私有网络中，唯一的访问是通过 Nginx 反向代理。将您的 DNS 指向服务器，Nginx 会处理所有的魔法。

以下是如何设置它:

1.  继续设置你的数字海洋水滴。你需要的所有信息都在这里。5 美元的版本已经足够了。

2.  [转到这里克隆存储库。](https://github.com/evertramos/docker-compose-letsencrypt-nginx-proxy-companion)您也可以在您的终端上运行。确保你首先进入你的数字海洋水滴！

    ```
     git clone git@github.com:evertramos/docker-compose-letsencrypt-nginx-proxy-companion.git 
    ```

3.  将目录更改为克隆的存储库。

4.  将`.env.sample`复制到`.env`并更新里面的值。我不得不把`IP`的值改成我的数字海洋水滴的 IP。我把其他的都留下了。

5.  运行`docker-compose up -d`开始一切。(您可以在没有`-d`选项的情况下运行，以确保一切正常开始。或者您可以使用`docker container logs -f <container name`附加日志输出

将子域指向此服务器时，请确保使用 A 记录。这是我的一个例子:

![NS1 A Record Configuration](img/176ad35d62b78677dc7240e1bca539b3.png)

根据您的 DNS 提供商，您必须弄清楚如何设置 A 记录。但是这超出了本文的目的！

## 使用 Docker Compose 设置注释

![Commento Logo with Docker Logo](img/2bb1b50640d1bd18976b25a10f0fc753.png)

这是我目前用于 Commento 的 docker 撰写文件。它还包括一些用于配置 Github、Gitlab 和 Google 的环境变量。它还包括用于设置 SMTP 设置的环境变量。这些参数很重要。否则，您将无法收到密码重置或审核电子邮件！

```
version: '3'

services:
  commento:
    image: registry.gitlab.com/commento/commento
    container_name: commento
    restart: always
    environment:
      COMMENTO_ORIGIN: https://${COMMENTS_URL}
      COMMENTO_PORT: 8080
      COMMENTO_POSTGRES: postgres://postgres:postgres@postgres:5432/commento?sslmode=disable
      COMMENTO_SMTP_HOST: ${SMTP_HOST}
      COMMENTO_SMTP_PORT: ${SMTP_PORT}
      COMMENTO_SMTP_USERNAME: ${SMTP_USERNAME}
      COMMENTO_SMTP_PASSWORD: ${SMTP_PASSWORD}
      COMMENTO_SMTP_FROM_ADDRESS: ${SMTP_FROM_ADDRESS}
      COMMENTO_GITHUB_KEY: ${COMMENTO_GITHUB_KEY}
      COMMENTO_GITHUB_SECRET: ${COMMENTO_GITHUB_SECRET}
      COMMENTO_GITLAB_KEY: ${COMMENTO_GITLAB_KEY}
      COMMENTO_GITLAB_SECRET: ${COMMENTO_GITLAB_SECRET}
      COMMENTO_GOOGLE_KEY: ${COMMENTO_GOOGLE_KEY}
      COMMENTO_GOOGLE_SECRET: ${COMMENTO_GOOGLE_SECRET}
      COMMENTO_TWITTER_KEY: ${COMMENTO_TWITTER_KEY}
      COMMENTO_TWITTER_SECRET: ${COMMENTO_TWITTER_SECRET}
      VIRTUAL_HOST: ${COMMENTS_URL}
      VIRTUAL_PORT: 8080
      LETSENCRYPT_HOST: ${COMMENTS_URL}
      LETSENCRYPT_EMAIL: ${EMAIL}
    depends_on:
      - postgres
    networks:
      - db_network
      - webproxy

  postgres:
    image: postgres
    container_name: postgres
    environment:
      POSTGRES_DB: commento
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    networks:
      - db_network
    volumes:
      - postgres_data_volume:/var/lib/postgresql/data

networks:
  db_network:
  webproxy:
    external: true

volumes:
  postgres_data_volume: 
```

要设置环境变量，将它们放在一个`.env`文件中。确保`.env`文件和`docker-compose.yml`在同一个目录下。当你运行`docker-compose up`时，它将应用在`.env`文件中设置的变量。如果它们留空，则什么都不会设置。

设置所需的`COMMENTS_URL`和`EMAIL`，否则可能会遇到问题。设置这些的最好方法是在`.env`文件中调整它们。这里有一个例子:

```
COMMENTS_URL=comments.your.url
EMAIL=you@your.url 
```

## 获取 OAuth 密钥和机密

Commento 与大多数流行的 OAuth 提供商合作。因此，访问者可以留下评论，而无需创建帐户。

每个的说明都是相似的。我在下面列出了所有这些方法的步骤。

### 推特

1.  登录[Twitter.com](http://twitter.com)，申请开发者账号:[https://developer.twitter.com/en/application/use-case](https://developer.twitter.com/en/application/use-case)

    ![Twitter API Access](img/3e906c410bb2139a3d87fcda3d45f28b.png)

2.  描述你将如何使用 API。你可以用我写的。

    ![How will you use the API?](img/b49fbb09074bf61d4f813a172a03ea9d.png)

3.  仔细检查你的输入，点击**看起来不错！**

    ![Is everything correct?](img/7ae688227c7504706223bf8844f03c2c.png)

4.  同意服务条款。

    ![Agree to Developer Agreement](img/913451d2c14a2f47e0bef8940a48f15d.png)

    ![You did it!](img/246f795908333f6f4d8f54d7d72f380c.png)

5.  他们会告诉你检查你的电子邮件进行确认。确认您的电子邮件，您应该能够创建您的第一个应用程序！

6.  一旦被批准进入**开始**点击**创建 app** 。

    ![Create an app](img/33fc69ccc46ab09ef315d9ef52e5c0f7.png)

7.  下一屏，再次点击**创建应用**

    ![Create an app](img/e0c017699d0eb18e4286474a183d7089.png)

8.  输入所有适当的详细信息。对于回调 URL，使用 [`https://<your URL>/api/oauth/github/callback`](https://comments.jaredwolff.com/api/oauth/google/callback) ，其中 [`<your URL>`](https://comments.jaredwolff.com/api/oauth/google/callback) 是您的评论子域。

    ![App details](img/01644ae9f893b5962f3a0a4f6587de20.png)

9.  最后，一旦你完成填写信息去**钥匙和令牌**的区域。保存密钥和令牌。将它们输入到`.env`文件中。可以用`COMMENTO_TWITTER_KEY`和`COMMENTO_TWITTER_SECRET`

    ![Get oauth key and secret](img/6455ec7138d17119919a039d27f475f4.png)

### Gitlab

1.  登录[Gitlab.com](http://gitlab.com)并转到右上角点击**设置**

2.  然后点击**应用**

    ![Gitlab profile](img/f1ddb37e39da784e89374bac54be82ea.png)

3.  输入应用的名称。我把**注释到**。

4.  将重定向 URI 设置为 [`https://<your URL>/api/oauth/gitlab/callback`](https://comments.jaredwolff.com/api/oauth/google/callback)

5.  选择 **read_user** 范围。

    ![Gitlab add application](img/6970acfab8530c881184ef5a3dfb440a.png)

6.  点击绿色的**保存应用**按钮

7.  使用`COMMENTO_GITLAB_KEY`和`COMMENTO_GITLAB_SECRET`复制**应用 ID** 和**秘密**并将其放入您的`.env`文件中

    ![Application key and secret](img/f3f17433f4787dbdc62dcf70f36220f7.png)

### 开源代码库

1.  要获得 OAuth 密钥和秘密，您需要访问以下网址:[https://github.com/settings/developers](https://github.com/settings/developers)

2.  在那里，点击**新 OAuth 应用**

    ![Add OAuth application](img/e740409b275948e761bd9c69f1454d77.png)

3.  输入您的详细信息。对于回调 URL，使用 [`https://<your URL>/api/oauth/github/callback`](https://comments.jaredwolff.com/api/oauth/google/callback) ，其中 [`<your URL>`](https://comments.jaredwolff.com/api/oauth/google/callback) 是您的评论子域。

    ![Register new OAuth application](img/a24587fb9459612a1632d82bba4ef8c5.png)

    *注意:确保在你的 URL 中包含`https`。*

4.  使用`COMMENTO_GITHUB_KEY`和`COMMENTO_GITHUB_SECRET`获取**客户端 ID** 和**客户端秘密**，并将其放入您的`.env`文件中

    ![Application created successfully](img/4c3f5e9c084c3776a8589f2870d64027.png)

### 谷歌

设置谷歌和设置推特一样乏味。不管我刚才说的有多可怕，这是完全可行的。以下是步骤。

1.  转到这个网址:[谷歌开发者控制台](https://console.developers.google.com/cloud-resource-manager?previousPage=%2Fapi)

2.  创建新项目

    ![Create a new project](img/0969c8cb47351105e8e4f1d2739b8c51.png)

3.  一旦你有了一个项目，点击左上角的 **GoogleAPIs 标志**返回。(确保 **GoogleAPIs 徽标**旁边的下拉菜单与您的新项目相同！)

4.  然后，点击左侧的**凭证**。

5.  在 **OAuth 同意界面**中更新**应用名称**和**授权域**

    ![Setup application](img/457c8402816e74a698dc5186543d4947.png)

6.  点击**创建凭证**，然后点击 **OAuth 客户端 ID**

    ![Setup credentials](img/2b4d0cf71ec0a1807bc5c2fd280eb846.png)

7.  在 **Create OAuth client ID** 上输入与 Commento 关联的子域到**授权的 Javascript origins。**然后，输入完整的回拨网址。比如 [`https://comments.jaredwolff.com/api/oauth/google/callback`](https://comments.jaredwolff.com/api/oauth/google/callback) 。将`comments.jaredwolff.com`替换为您的 URL，让它成为您的。

    ![Create OAuth Client ID](img/fd87b82b85d289f492ae11aa62e8ee33.png)

    一旦进入，点击**创建**按钮。

8.  抓取**客户端 ID** 和**客户端秘密**

    ![OAuth Credentials](img/b40ef8b9fbaa1f98fb325ee7e8ac4d17.png)

9.  使用`COMMENTO_GOOGLE_KEY`和`COMMENTO_GOOGLE_SECRET`更新你的`.env`文件

## 安装应用程序

您已经输入了 OAuth 凭据电子邮件、域和 SMTP 凭据。是时候结束这场演出了！

1.  一旦你编辑完你的`.env`文件。运行`docker-compose up`(对于未命名为`docker-compose.yml`的文件，使用`-f`标志。例子:`docker-compose -f commento.yml up`

2.  观察输出中的错误。如果它看起来不错，你可能想杀死它( **CTRL+C** )并带着`-d`标志运行

3.  第一次启动时，Commento 会提示您一个登录屏幕。

    ![Commento Login](img/9b7fa808fe54d2ac8b7fe8989aa8e6b5.png)

4.  点击**创建新账户还没有账户？报名吧。**

5.  输入您的信息并点击**注册**

6.  查看您的电子邮件，然后点击包含的链接:

    ![Validation email with link](img/7db4efe8fe6f218abe48cc8487d9a923.png)

7.  使用您新创建的帐户登录。

8.  然后，点击**添加一个新域。**

    ![Add new domain](img/bd3a1596122362692328d75d82af2470.png)

9.  创建完成后，转到**安装指南。**复制这个片段，然后把它放在你想要你评论的地方。在我的例子中，我把代码片段放在了我的`<article>`标签后面的区域。

    ![Code snippet](img/054941abaaabfc12222632d27ac6f069.png)

10.  重新编译你的网站并检查是否成功！

    ![Blog comment section with checkmarks](img/8ee817a3f51ecb99c81153661349de69.png)

    勾号！最后，我建议您尝试使用每个单独的 OAuth 配置登录。这样你就知道它为你的网站访问者工作。？

## 可供选择的事物

我花了很大一部分时间研究一些替代方案。这绝不是关于什么最适合你的网站的权威指南。以下是撰写本文时的一些最佳案例:

[https://utteranc.es/#configuration](https://utteranc.es/#configuration)

[https://github.com/netlify/gotell](https://github.com/netlify/gotell)

[https://github.com/eduardoboucas/staticman](https://github.com/eduardoboucas/staticman)

[https://posativ.org/isso/](https://posativ.org/isso/)

[https://www.remarkbox.com](https://www.remarkbox.com/)

[https://www.vis4.net/blog/2017/10/hello-schnack/](https://www.vis4.net/blog/2017/10/hello-schnack/)

[https://github.com/gka/schnack](https://github.com/gka/schnack)

Hugo 博客上也有一个巨大的主题，里面有很多链接和资源:

[https://discourse . go Hugo . io/t/alternative-to-disqus-needle-than-ever-by-less/5516](https://discourse.gohugo.io/t/alternative-to-disqus-needed-more-than-ever/5516)

## 结论

恭喜你。您现在拥有了自己的评论服务器！？

在本文中，您了解了如何利用 Docker 和 Nginx 反向代理的力量。另外，您知道如何设置 OAuth 凭证！这样，未来的设置将很容易。

顺便说一下，这只是冰山一角。您可以为分析、数据收集等设置相同的服务器。所有示例代码，包括其他应用程序的代码，都可以在这里找到。

最后，如果你正在寻找付费评论，请访问 www.commento.io 并注册这项服务。你将支持令人敬畏的开源软件！

如果你有意见和问题，让我们听听。开始下面的对话。？？？