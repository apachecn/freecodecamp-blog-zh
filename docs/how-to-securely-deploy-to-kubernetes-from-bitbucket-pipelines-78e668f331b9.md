# 如何从 Bitbucket 管道安全地部署到 Kubernetes

> 原文：<https://www.freecodecamp.org/news/how-to-securely-deploy-to-kubernetes-from-bitbucket-pipelines-78e668f331b9/>

![Locking it down](img/72ae86eb87e99fdc5202505b2bc54d73.png)

[超过 100，000 个 GitHub repos 泄露了 API 或密钥- ZDNet](https://www.zdnet.com/article/over-100000-github-repos-have-leaked-api-or-cryptographic-keys/)

如果这发生在你身上，请举手。你正在阅读一篇关于无数主题中的一个的写得很好的文章，你会读到这样一行:

```
// DO NOT DO THIS IN A PRODUCTION APP
const API_KEY = '<api-key-displayed-in-plain-text>' 
```

好吧，那你应该怎么做呢？不幸的是，没有一种万能的方法来保护你的秘密。部署在不同环境中的不同编程语言都以自己的方式处理秘密。

我只想说，永远不要在代码或存储库中存储秘密。秘密应该在最后时刻通过环境变量传递到你的应用程序中。

## 比特斗管道-连续输送

自从它出现在 Alpha 中，我就一直在使用[位桶管道](https://medium.com/r/?url=https%3A%2F%2Fbitbucket.org%2Fproduct%2Ffeatures%2Fpipelines%3Futm_source%3DMedium%26utm_medium%3DPost%26utm_campaign%3DTueri%26utm_content%3DKubernetes)，我不得不说，这太棒了。这必须是从您的回购开始设置连续交割的最快捷、最简单的方式。

管道使用 YAML 文件进行配置，根据您的需要，管道可以非常简单，也可以非常复杂。

### 管道配置

出于几个原因，我喜欢将我的构建工作分成几个步骤:

*   如果某个步骤失败，您可以重新运行各个步骤。
*   每一步都是相互独立的。只有您的基本回购和您声明的任何“工件”将被传递到下一步。

下面是一个 3 步 bitbucket-pipelines.yml 文件，它获取一个 create-react-app 站点，将其打包为 Docker 映像，并将其部署到 Kubernetes 集群:

```
options:
  # Enable docker for the Pipeline
  docker: true

pipelines:
  branches:
    master:
      - step:
          name: Build app for Production (create-react-app)
          image: mhart/alpine-node:10
          caches:
            - node
          script:
            # Install Dependencies
            - npm install
            # Run our Tests
            - npm run test
            # Package App for Production
            - npm run build
          artifacts:
            # Pass the "build" Directory to the Next Step
            - build/**
      - step:
          name: Build Docker Image
          script:
            # NOTE: Set $DOCKER_HUB_USERNAME and $DOCKER_HUB_PASSWORD as environment SECRETS in Bitbucket repository settings
            # Use $BITBUCKET_COMMIT to tag our docker image
            - export IMAGE_NAME=<docker-username>/<docker-image>:$BITBUCKET_COMMIT
            # Build the Docker image (this will use the Dockerfile in the root of the repo)
            - docker build -t $IMAGE_NAME .
            # Authenticate with the Docker Hub registry
            - docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD
            # Push the new Docker image to the Docker registry
            - docker push $IMAGE_NAME
      - step:
          # trigger: manual
          name: Deploy to Kubernetes
          image: atlassian/pipelines-kubectl
          script:
            # NOTE: $KUBECONFIG is secret stored as a base64 encoded string
            # Base64 decode our kubeconfig file into a temporary kubeconfig.yml file (this will be destroyed automatically after this step runs)
            - echo $KUBECONFIG | base64 -d > kubeconfig.yml
            # Tell our Kubernetes deployment to use the new Docker image tag
            - kubectl --kubeconfig=kubeconfig.yml --namespace=<namespace> set image deployment/<deployment-name> <deployment-name>=<docker-username>/<docker-image>:$BITBUCKET_COMMIT 
```

*bitbucket-pipelines.yml*

```
FROM mhart/alpine-node:10
WORKDIR /app
EXPOSE 5000

# Install http server
RUN yarn global add serve

# Bundle app source
COPY build /app/build

# Run serve
CMD [ "serve", "-n", "-s", "build" ] 
```

*Dockerfile*

这是最重要的部分:

```
- echo $KUBECONFIG | base64 -d > kubeconfig.yml 
```

我们的 kubeconfig 文件作为 Base64 编码的字符串存储在名为`$KUBECONFIG`的 Bitbucket secret 中。

位桶机密以加密方式存储，当您在管道中调用该变量时会解密。

我们解码`$KUBECONFIG`变量，并将其存储在一个名为 kubeconfig.yml 的临时文件中，该文件在该步骤完成后会被自动删除。

## 打破它

### 第一步

1.  安装依赖项
2.  运行测试
3.  建设
4.  将构建目录传递到步骤 2

### 第二步

1.  名称 Docker 图像
2.  构建 Docker 映像
3.  推入映像到坞站集线器

### 第三步

1.  解码 kubeconfig
2.  设置部署映像

## 构建性能

整个构建过程不到 1 分 40 秒，使用 Alpine Node，Docker 映像只有 29 MB。

## 结论

保护你的秘密并不难，但首先要知道去哪里找。

在不同的 Node.js 环境中保护机密的一些技巧:

*   Node.js(开发):使用。环境文件和。gitignore 保留。env 文件移出您的存储库。
*   Node.js (Production):使用 Kubernetes Secrets、Docker Secrets 并作为环境变量传递到容器中。

### 记住这条规则:

*   不要在你的代码、你的库或者你的 docker 映像中存储秘密。

编码快乐！

* * *

*最初发布于[tueri . io](https://tueri.io/blog/2019-04-04-how-to-securely-deploy-to-kubernetes-from-bitbucket-pipelines/?utm_source=FreeCodeCamp&utm_medium=Post&utm_campaign=Continuous%20Deployment)*