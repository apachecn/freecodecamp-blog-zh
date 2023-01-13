# 如何使用 Kubernetes、EKS 和 NGINX 为网站设置 DNS

> 原文：<https://www.freecodecamp.org/news/how-to-setup-dns-for-a-website-using-kubernetes-eks-and-nginx/>

作为网站质量监控平台 Foo 的创建者，我最近努力迁移到 Kubernetes 和 EKS(AWS 服务)。

Kubernetes 提供了强大的 DNS 支持。幸运的是，在集群中，我们可以按照规范中的定义通过主机名引用 pod。

但是如果我们要把一个 app 对外公开为一个静态域下的网站呢？我以为这将是一个常见的，有据可查的案件，但男孩是我错了。

> 假设在 Kubernetes 名称空间`bar`中有一个名为`foo`的服务。在名称空间`bar`中运行的 Pod 可以通过简单地对`foo`进行 DNS 查询来查找这个服务。在名称空间`quux`中运行的 Pod 可以通过查询服务和 Pod-Kubernetes 的`foo.bar` ~ [DNS 来查找该服务](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/)

是的，这是伟大的❤️，但这仍然导致许多未解之谜。让我们一步一步来，好吗？！这篇文章将解决以下问题。

1.  **如何定义服务**
2.  **如何在一个 NGINX 服务器下公开多个服务**。不需要花里胡哨的“[入口](https://kubernetes.io/docs/concepts/services-networking/ingress/)”**？**
3.  **如何创建一个外部域名系统并连接到一个域名**你已经通过任何合格的注册机构，如 GoDaddy 或谷歌域名，例如。我们将使用[53 路](https://aws.amazon.com/route53/)和[外部线路](https://github.com/kubernetes-sigs/external-dns)来完成繁重的工作。

这篇文章假设使用 EKS 和`eksctl`进行设置，如“[`eksctl`](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)入门”中所述，但是这篇文章中的许多概念和例子可以适用于各种配置。

## 步骤 1:定义服务

[用服务连接应用](https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/)解释了如何通过定义`Deployment`和`Service`来公开 NGINX 应用。让我们继续以同样的方式创建 3 个应用程序:一个面向用户的 web 应用程序、一个 API 和一个反向代理 NGINX 服务器，以在一个主机下公开这两个应用程序。

> web-deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web
        # etc, etc
```

> web-service.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: web
  labels:
    app: web
spec:
  ports:
  - name: "3000"
    port: 3000
    targetPort: 3000
  selector:
    app: web
```

> api-deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api
spec:
  replicas: 1
  selector:
    matchLabels:
      app: api
  template:
    metadata:
      labels:
        app: api
    spec:
      containers:
      - name: api
        # etc, etc
```

> api-service.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: api
  labels:
    app: api
spec:
  ports:
  - name: "3000"
    port: 3000
    targetPort: 3000
  selector:
    app: api
```

很公平，我们继续吧！

## 步骤 2:在一个 NGINX 服务器下公开多个服务

NGINX 是一个反向代理，它通过将请求发送到指定的源来代理请求，获取响应，然后将其发送回客户端。

回到集群中其他 pods 可以访问的服务名，我们可以设置一个 NGINX 配置，如下所示。

> 网站启用/www.example.com.conf

```
upstream api {
  server api:3000;
}

upstream web {
  server web:3000;
}

server {
  listen 80;

  server_name www.example.com;

  location / {
    proxy_pass http://web;
  }

  location /api {
    proxy_pass http://api;
  }
}
```

注意我们如何引用像`web:3000`和`api:300`这样的原始主机。尼利斯。

> nginx-deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80 
```

> nginx-service.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: nginx
  annotations:
    # this part will make more sense later
    external-dns.alpha.kubernetes.io/hostname: www.example.com
  labels:
    app: nginx
spec:
  type: LoadBalancer
  ports:
  - name: "80"
    port: 80
    targetPort: 80
  selector:
    app: nginx
```

...我们结束了！对吗？以我的经验，最初我是这么认为的。`[LoadBalancer](https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/)`提供外部可访问的 IP。您可以通过运行`kubectl get svc`来确认，果然您会在`EXTERNAL-IP`列中找到一个主机名。

假设您已经从一个提供管理 DNS 设置的接口的提供商那里获得了一个域名，您可以简单地添加这个 URL 作为`CNAME`就可以了，对吗？嗯，算是吧...但没有那么多。

Kubernetes 豆荚被认为是相对短暂的(而不是持久的)实体。请参阅“ [Pod 生命周期- Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/) ”了解更多信息。

也就是说，每当服务的生命周期发生重大变化时，在我们的 NGINX 应用程序中，我们将有一个不同的 IP 地址，这反过来会导致我们的应用程序长时间停机，这违背了 Kubernetes 的主要目的——帮助建立一个“高度可用”的应用程序。

好吧，别慌-我们会挺过去的？

## 步骤 3:创建一个外部 DNS 服务来动态指向 NGINX

在前面的步骤中，用我们的`LoadBalancer`规范加上 EKS，我们实际上创建了一个[弹性负载平衡器](https://aws.amazon.com/elasticloadbalancing/)(无论好坏)。

在本节中，我们将创建一个 DNS 服务，它通过“ALIAS 记录”指向我们的负载平衡器。这个别名记录本质上是动态的，因为每当我们的服务改变时，就会创建一个新的别名记录。稳定性建立在名称服务器记录中。

TL；其余部分的 dr 只需遵循[文件，将外部 DNS 与路线 53](https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/aws.md) 一起使用。53 路是“[云域名系统(DNS) web 服务](https://aws.amazon.com/route53/)”。

以下是我不得不做的事情，这些事情在文档中并不明显。抓牢你的马，这会变得有点乱。

*   `eksctl utils associate-iam-oidc-provider --cluster=your-cluster-name`根据 [`eksctl`服务账户文件](https://eksctl.io/usage/iamserviceaccounts/)。
*   当根据 [ExternalDNS 文档](https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/aws.md#iam-policy)创建 IAM 策略文档时，我实际上必须在我的帐户中通过 CLI 而不是 online 来完成。我一直得到这个错误:`WebIdentityErr: failed to retrieve credentials\ncaused by: AccessDenied: Not authorized to perform sts:AssumeRoleWithWebIdentity\n\tstatus code: 403`。当我通过 CLI 创建策略时，问题消失了。如果你安装了 [AWS CLI](https://aws.amazon.com/cli/) ，下面是你应该能够复制和执行的完整命令。

```
aws iam create-policy \
  --policy-name AllowExternalDNSUpdates \
  --policy-document '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Action":["route53:ChangeResourceRecordSets"],"Resource":["arn:aws:route53:::hostedzone/*"]},{"Effect":"Allow","Action":["route53:ListHostedZones","route53:ListResourceRecordSets"],"Resource":["*"]}]}'
```

*   使用上面的策略 ARN 输出，通过类似于`eksctl create iamserviceaccount --cluster=your-cluster-name --name=external-dns --namespace=default --attach-policy-arn=arn:aws:iam::123456789:policy/AllowExternalDNSUpdates`的命令创建一个绑定到 ExternalDNS 服务帐户的 IAM 角色。
*   我们现在应该有了一个新角色，我们可以在 [IAM 控制台](https://console.aws.amazon.com/iam)中看到，它的名字类似于`eksctl-foo-addon-iamserviceaccount-Role1-abcdefg`。单击列表中的角色，在下一个屏幕的顶部记下“角色 ARN”，类似于`arn:aws:iam::123456789:role/eksctl-foo-addon-iamserviceaccount-Role1-abcdefg`。
*   按照这些步骤在 53 号公路上创建一个“托管区”。
*   你可以在[53 路控制台](https://console.aws.amazon.com/route53)确认事情。
*   如果您的域提供商允许您管理 DNS 设置，请从您运行的命令的输出中添加 4 个名称服务器记录，以创建“托管区域”。
*   按照[的指示](https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/aws.md#deploy-externaldns)部署外部 DNS。之后，您可以使用`kubectl logs -f name-of-external-dns-pod`对日志进行结尾。你应该在最后看到这样一行:`time="2020-05-05T02:57:31Z" level=info msg="All records are already up to date"`

很简单，对吧？！好吧，也许不是...但至少你不用一个人去想这些？上面可能有一些差距，但希望它有助于指导你通过你的过程。

## 结论

虽然这篇文章可能有一些灰色地带，但是如果它帮助你建立动态 DNS 解析作为一个高度可用的应用程序的一部分，你就有了真正特别的东西？

如果我能帮助澄清任何事情或纠正我的术语，请添加评论！