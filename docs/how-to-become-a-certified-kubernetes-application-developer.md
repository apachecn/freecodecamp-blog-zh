# 如何成为经过认证的 Kubernetes 应用程序开发人员

> 原文：<https://www.freecodecamp.org/news/how-to-become-a-certified-kubernetes-application-developer/>

本指南是我最近通过的认证 Kubernetes 应用程序开发人员(CKAD)考试的学习笔记的摘要。

即使你对认证不感兴趣，也可以把这当成你在 Kubernetes 的**一站式商店**:你可以在一个地方得到所有主要技术概念的解释以及无数的例子。

此外，它还有一些额外的东西，来自我准备和参加考试的经历。

在撰写本文时，CKAD 课程(学习领域和每个领域的权重)如下:

*   13%–核心概念
*   18%–配置
*   10%–多容器容器
*   18%–可观察性
*   20%–吊舱设计
*   13%–服务和网络
*   8%–状态持久性

本指南涵盖了所有课程，只是顺序不同。

我假设你已经了解了 Kubernetes 的基础知识(基本上是容器和豆荚),并希望将你的技能提升到一个新的水平。通过这次考试将使你的简历脱颖而出，因为这是一个非常受欢迎的认证。

## 内容

*   [立方结构简介](#intro-to-kubernetes)
*   [如何在 Kubernetes 中管理集群](#how-to-manage-clusters-in-kubernetes)
*   [超越豆荚和部署](#beyond-pods-and-deployments)
*   [如何配置 pod 和容器](#how-to-configure-pods-and-containers)
*   [如何在 Kubernetes 中安排 Pods】](#how-to-schedule-pods-in-kubernetes)
*   [在 Kubernetes 的存储](#storage-in-kubernetes)
*   [Kubernetes 的网络和安全](#network-and-security-in-kubernetes)
*   [在 Kubernetes 中的可观察性和调试](#observability-and-debugging-in-kubernetes)
*   [提示和技巧](#tips-and-tricks)
*   [练习时间](#practice-time)
*   [结论](#conclusions)

## Kubernetes 简介

Kubernetes 是一种允许跨多个节点轻松部署和管理容器化应用程序的技术。它的一些最突出的特点是:

*   容器配置和部署
*   系统监控
*   持久存储
*   自动化调度
*   自动扩展和自动修复

还有更多。

Kubernetes 以一种**声明式**的方式工作:您定义您希望集群处于的状态，Kubernetes 确保集群始终处于该状态。

要定义或修改这个状态，您需要与 API 服务器进行交互。这可以通过以下方式实现:

*   休息电话
*   命令行工具`kubectl`。你可以在这里找到在你的机器[上安装它的说明。](https://kubernetes.io/docs/tasks/tools/)

如果您没有访问 Kubernetes 集群的权限，我建议您在本地机器上安装 [minikube](https://minikube.sigs.k8s.io/docs/start/) 来跟进。安装并启动后，运行以下命令创建您的第一个 pod。

```
kubectl run --image=busybox --restart=Never --rm -it -- echo "Welcome to Kubernetes!!" 
```

一旦打印出欢迎信息，此窗格将被自动删除。

## 如何在 Kubernetes 中管理集群

集群管理不是成为 CKAD 课程的一部分。出于考试目的，您不需要知道如何创建集群、管理节点等等。

除非你打算成为一名集群管理员，否则你很有可能只是一个托管版 Kubernetes 的用户，比如 GCP 的 GKE 或者 AWS 的 EKS。

但是，您必须熟悉名称空间、标签和注释的概念。

### 名称空间

名称空间允许您创建*虚拟集群*，也就是说，在同一个物理集群的不同部分隔离资源。例如:

*   分离不同的环境，如开发、阶段、QA 和生产
*   将一个复杂的系统分解成更小的子系统。您可以为前端组件创建一个命名空间，为后端组件创建另一个命名空间，依此类推。
*   为了避免名称冲突:可以在不同的名称空间中用相同的名称创建相同的资源。这使得创建看起来完全相同不同环境(想想 stage 和 prod)变得很容易。

您可以通过运行以下命令来创建命名空间:

```
kubectl create ns my-namespace 
```

### 资源配额

如果您想限制开发人员可以在名称空间中创建的资源数量(物理资源和 Kubernetes 对象，如 pods)，您可以使用[资源配额](https://kubernetes.io/docs/concepts/policy/resource-quotas/)。

例如，您可以通过定义以下`ResourceQuota`来限制用户可以在您的集群上创建的 Kubernetes *秘密*的数量:

```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-quota
spec:
  hard:
    secrets: "2" 
```

在一个文件中——姑且称之为`my-quota.yaml`——然后运行`kubectl apply -f my-quota.yaml`或简单地运行

```
kubectl create quota my-quota --hard="secrets=2" 
```

更多关于*的秘密*稍后在指南中。

### 标签

标签允许您在 Kubernetes 集群中组织资源。标签是在创建资源时或通过标记现有资源而附加到资源上的键值对。然后，您可以使用标签来筛选资源。

例如:

*   您想只显示您的`backend`窗格。您可以将标签`tier=backed`附加到 pod 上(键和值都是任意的，您可以使用任何想要的值)，然后运行`kubectl get pods -l tier=backend`来检索所需的 pod。
*   您想要定义与某些 pod 相关联的部署或服务。告诉部署/服务需要关注哪些 pod 的方法是使用标签来选择它们。

以下是一些常见的与标签相关的操作:

```
# Labeling a pod, but similar syntax for nodes, etc
kubectl label pods pod-name key=value
# Removing a label by podname
kubectl label pods pod-name label_key-

# Removing a pod using the label as selector
kubectl label po -l app app-

# Display labels
kubectl get pods --show-labels

# Select pods based on their labels (assume we've defined)
kubect get pods -l 'tier in (frontend,backend)'
kubect get pods -l tier=frontend,deployer=coolest-team 
```

注释与标签的相似之处在于它们是键值对，但是它们不能用于选择资源。他们有不同的目的。

注释通常被添加到其他工具中进行处理。例如，如果您正在运行 Prometheus 来收集指标，您可以将此配置添加到您的描述符中:

```
metadata:
	labels:
		name: fluentd-elasticsearch
	annotations:
        prometheus.io/scrape: 'true'
        prometheus.io/port: '9102' 
```

它告诉 Prometheus 分别用`true`和`9102`覆盖*刮削*和*端口*的默认值。

## 超越机架和部署

正如你现在所知道的，一个 **pod** 是 Kubernetes 的基本单位。在大多数情况下，您可以将其视为一个容器，但是一个 pod 可以由多个容器组成。由于豆荚本质上是短暂的，我们需要一种机制来确保当我们现有的豆荚死亡时新的豆荚被创造出来。

通过部署，您可以定义所需的状态，例如让应用程序的 3 个副本始终运行。Kubernetes 将努力在集群中始终实现并保持这种状态。

部署还可以用于方便地管理随时运行的副本数量，执行滚动更新，回滚到以前的版本，等等。

但是，还有更多工作负载。

### 多容器容器

一个 pod 可以运行多个容器。容器可以无缝地相互对话，因为它们在同一个网络中，并且它们可以使用*卷*来共享数据。

现在，让我们深入探讨一些多容器容器的常见设计模式。在本指南的后面部分，我们将详细介绍卷以及如何部署其中的一些模式。

#### 边车

在这个模式中，我们有一个运行主应用程序的容器和另一个运行辅助任务的容器来增强主容器。

一个典型的例子是运行一个 web 服务器(主容器)和一个处理日志、监控、刷新 pod 卷中的数据、终止 TLS 等任务的副容器。

#### 适配器

类似地，您可以有一个主容器和一个辅助容器，**转换主容器**的输出。

例如，假设您的主容器运行一个输出大量复杂日志的服务。在将输出发送到日志记录服务之前，您可以使用适配器容器来简化输出，从而将主容器(以及服务的开发人员)从这项任务中卸载下来。

#### 大使

另一个常见的选择是使用辅助容器**作为主容器和外部世界之间的代理**。

例如，对于不同的环境，比如测试和生产，您可能有不同的数据库。当您的主容器需要连接到数据库时，它可以将根据环境确定合适的数据库的任务交给大使容器。

### 服务

pod 可以使用它们的 IP 地址连接其他 pod。然而，豆荚本质上是短暂的。当它们死亡时，假设您有一个复制控制器，将创建一个具有新 IP 地址的新 pod。这使得很难使用 IPs 直接连接到 pod。

Kubernetes 提供了一个名为 Service 的抽象，它创建了一个具有固定 IP 地址的资源，并将请求发送到合适的 pods。

不用直接连接到 pods，你可以通过它的 IP 地址访问他们的服务，或者更好地使用它的全限定名称，这要归功于 DNS 服务。此外，某些类型的服务也将您的应用程序暴露在集群之外。

#### 服务类型

您可以创建的主要服务类型有:

*   **cluster IP–**在集群内部公开您的应用程序。如果不指定类型，这是 Kubernetes 创建的默认服务。
*   **节点端口–**它在集群中的每个节点上打开一个端口，向外界展示您的应用程序。
*   **负载平衡器—**使用云提供商负载平衡器对外公开服务。

#### 如何在集群内部公开您的应用程序

假设您想要在端口 80 上向集群中的其他节点公开您的应用程序`my-app`。您可以创建一个*部署*，用这个命令部署您的应用程序:

```
kubectl create deploy my-app --image=my-app --port=80 
```

这将创建只能通过其 IP 从群集中的其他资源访问的 pod，如果 pod 重新启动，IP 将会改变。

下一步是为这些 pod 创建一个 ClusterIP 服务。以下命令创建一个可以在端口 80 访问的服务，并将请求转发到您的应用程序(也在端口 80)。

```
kubectl expose deploy my-app --port=80 
```

#### 如何在集群之外公开您的应用程序

如果您想向外界公开您的应用程序，您可以使用这个配置来创建一个节点端口服务:

```
kind: Service
apiVersion: v1
metadata:
	name: my-svc
spec:
    type: NodePort
    selector:
	    app: nginx
    ports:
    - protocol: TCP
    	port: 80 # The port where you can reach this service
    	targetPort: 80 # The port that you opened in the pod 
```

### [入口](https://kubernetes.io/docs/concepts/services-networking/ingress/)

服务并不是向外界公开您的应用程序的唯一方式。您还可以使用一个**入口对象**作为集群的入口点。

除此之外，你还需要一个**入口控制器**，比如 Nginx，来实现入口对象中定义的规则。

由于进入超出了 CKAD 的范围，我们将转移到其他主题。

### [工作岗位](https://kubernetes.io/docs/concepts/workloads/controllers/job/)

作业将创建一个或多个 pod，如果它们成功完成，则**不会重新启动。您可以使用它们来执行批处理作业，即**一次性任务**，如执行计算、备份一些文件、转换和导出一些数据等等。**

除非另有说明，否则 pod 将运行一次。您可以使用参数`completions`指定作业需要运行多少次才能被视为完成。默认情况下，pod 将按顺序运行，但您可以设置作业，使它们并行运行。

如果它们没有成功完成，您可以将它们配置为在 Kubernetes 认为作业失败之前`Never`重启(重试)或`OnFailure`重启`backoffLimit`次。

以下是如何从命令行创建一个简单的作业:

```
 kubectl create job my-job --image=busybox -- echo "Hello World" 
```

如果作业运行成功，其状态将为“已完成”,并且不会删除其 pod，以便您可以访问其日志。您可以使用以下方式查看它们:

```
kubectll get pods --show-all 
```

因为默认情况下，他们不会在 2 分钟后出现在默认窗格列表中。

### [克朗乔布斯](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)

Kubernetes 提供了 Conjobs 来创建需要定期运行或在未来特定时间运行的作业:定期清理和备份任务、TLS 证书的更新等等。

Kubernetes 将尽力只运行一个作业来执行您在 Cronjob 配置中指定的任务。但是，有 3 个常见问题需要注意:

*   不能保证作业会在期望的时间准确运行**。假设您需要您的作业在 09:00:00 运行。您可以将`startingDeadlineSeconds`属性设置为 X 秒，这样，如果在 09:00:00 + X 秒之前作业还没有启动，它将被标记为失败并且不运行。**
*   **可以同时调度两个作业**来执行任务。在这种情况下，你需要确保任务是*幂等的*，也就是说，如果任务执行多次，执行任务的结果不会改变。
*   **可能不会安排任何作业**。为了解决这个问题，请确保 Cronjob 能够完成上一次运行没有完成的工作。

这就是如何创建一个每分钟打印“Hello World”的简单 cronjob:

```
kubectl create cronjob my-job --image=busybox --schedule="*/1 * * * *" -- echo "Hello World" 
```

我推荐这个[网站](https://crontab.guru/)让你的 cron 计划表达正确。

接下来的 3 个资源不是 CKAD 考试的一部分，但是我认为你至少应该对它们有一个基本的了解。

### [守护进程设置](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)

守护进程集确保在集群的每个**节点**中调度一个 pod。除此之外，pod 始终处于启动和运行状态:如果它死亡或者有人删除了它，pod 将被重新创建。

守护进程集的一个常见用例是收集来自每个节点的日志和指标。但是，即使您没有创建任何守护进程，它们已经在您的集群中运行了:Kubernetes 创建了一个守护进程集，在每个节点中运行一个名为`kube-proxy`的组件！

如果从集群中删除一个节点，Kubernetes 不会在另一个节点中重新创建它的守护进程，因为这个节点已经在运行守护进程集了。如果您向群集中添加新节点，它将自动运行守护程序集。

### [有状态集](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)

到目前为止，我们已经看到了部署无状态应用程序或 pod 的工具，这些应用程序或 pod 拥有自己的持久存储，将比它们更长寿。要部署有状态的应用程序，您可以使用有状态集。

由于这不是 CKAD 考试的一部分，我们不会进入更多的细节。

### [静态吊舱](https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/)

静态 pod 是由`kubelet`管理的 pod，而不是由 Kubernetes API 管理的。

要创建它们，您只需要创建一个常规的 pod 配置文件，并配置 kubelet 来扫描该目录。重启`kubelet`后，它将创建 pod，如果失败则重启。

Kubernetes 将创建一个镜像 pod，这是 Kubernetes API 服务器中 pod 的副本。当您运行`kubectl get pods`时，您可以看到这个 pod 出现了，但是如果您尝试使用`kubectl delete podName`删除它，它将立即出现在 pod 列表中，这是由在您创建静态 pod 的节点中运行的`kubelet`创建的。

## 如何配置 pod 和容器

我们刚刚看到了您可以在 Kubernetes 集群上部署的不同工作负载。现在，让我们花一些时间来学习如何配置运行这些工作负载的 pod 和容器。

### [初始化容器](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-initialization/)

您可以使用 *init containers* 来设置 pod 的初始状态:通过将一些数据写入 pod 的卷，下载一些文件，等等。

您可以为同一个 pod 定义一个或多个 init 容器。它们将按顺序执行，pod 仅在所有容器完成后才开始运行。因此，init 容器也可以用来让 pod 在执行之前等待某个条件。

例如，您可以让 pod 在启动之前等待另一个服务启动并运行。

您可以通过在 pod 描述的`spec`部分添加如下内容来定义一个 init 容器:

```
spec:
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup myservice.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for myservice; sleep 2; done"]
 ... 
```

### 配置图

将您的应用程序配置与源代码分开是您应该遵循的实践。配置映射允许您在 Kubernetes 中这样做。

配置映射用于**存储非机密数据**的键值对。我们将在下一节看到如何在 *Secrets* 中存储机密数据(例如，密码)。

您可以从命令行创建配置映射:

```
# Passing the values as arguments
kubectl create configmap my-map --from-literal=db_url=my-url --from-literal=username=username

# Passing the values from a file
kubectl create configmap another-map --from-file=my-file 
```

创建后，您的应用程序可以通过多种方式在同一命名空间中的 pod 中使用它:

*   作为命令行参数
*   作为环境变量
*   从只读卷中的文件

让我们来看一个使用以下方法从配置图中读取值的 pod 定义:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  restartPolicy: Never
  containers:
    - name: busybox
      image: busybox
      args:
        - /bin/sh
        - -c
        - "echo $MY_VARIABLE" # This value comes from the configmap
      env:
      - name: MY_VARIABLE
        valueFrom:
          configMapKeyRef:
            name: another-map           
            key: my-key # To load individual keys from a map
      envFrom:
      - configMapRef:
          name: another-map # To import all values from a map as env variables
      volumeMounts:
      - name: config
        mountPath: "/config" # This will contain files with the data stored in my-map
        readOnly: true
  volumes:
    # Name to refer to this volume in the pod
    - name: config
      configMap:
        # Name of the ConfigMap
        name: my-map 
```

### [秘密](https://kubernetes.io/docs/concepts/configuration/secret/)

Secrets 与 ConfigMaps 非常相似，但是您使用它们来存储**机密数据**。创建和管理秘密是一个敏感的话题。请务必阅读文档。在这里，我们将看到基本的。

创建秘密的最简单方法是:

```
#To create a secret from a literal
kubectl create secret generic secret-name --from-literal=password=password
#To create a secret from the content of a file
kubectl create secret generic secret-name --from-file=path-to-file 
```

然后，您可以将您的秘密作为环境变量或文件安装到 pod 中:

```
apiVersion: v1
kind: Pod
metadata:
  name: another-pod
spec:
  restartPolicy: Never
  containers:
    - name: busybox
      image: busybox
      args:
        - /bin/sh
        - -c
        - "echo $MY_VARIABLE"
      env:
      - name: MY_VARIABLE
        valueFrom:
          secretKeyRef:
            name: my-secret2
            key: username # To load individual keys from a map
      envFrom:
      - secretRef:
          name: yet-another-secret # To import all values from a map as env variables
      volumeMounts:
      - name: secret-volume
        mountPath: "/secrets" # This will contain files with the data stored in my-map
        readOnly: true
  volumes:
    # Name to refer to this volume in the pod
    - name: secret-volume
      secret:
        # Name of the Secret
        secretName: my-secret 
```

### [探针](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

探测器允许您设置规则来决定一个 pod 是否正常、是否准备好为流量提供服务以及是否准备好启动。在这一节的最后，我将介绍一些探针的示例描述符。

#### 活性探针

Kubernetes 会自动重启*崩溃的容器*。然而，这并没有考虑到诸如 bug(假设您的应用程序中有一个无限循环)、死锁等情况。您可以定义活跃度探测器来**检测您的应用程序是否健康**。Kubernetes 使用这个探测来决定是否需要重启容器。

您可以定义三种类型的活动探测器:

*   HTTP Get，您可以在应用程序中定义一个端点(例如/health ), Kubernetes 可以点击这个端点来确定应用程序是否健康。
*   TCP 套接字探测，尝试建立到特定端口的 TCP 连接。如果无法建立连接，应用程序将重新启动。
*   Exec 探测器，它在容器内部运行一个命令，如果退出状态代码不为 0，则重新启动该命令。

#### 就绪探测

假设一个服务在启动时需要加载一个大的配置文件。容器本身可能是健康的(这可以使用上面描述的活跃度探测器来检查)，但是还没有准备好开始接受请求。

Kubernetes 使用就绪探测来确定您的应用程序是否准备好开始提供流量服务。

许多关于活性探测的想法也适用于就绪性探测:

*   它们可以配置初始延迟、超时、阈值、周期等
*   它们可以基于 HTTP get 调用、TCP 套接字连接或容器内命令的执行

然而，虽然活性探测告诉 Kubernetes 如果容器不健康就重新启动容器，但是就绪探测用于从可以接受请求的容器池中删除容器。一旦容器准备好了，它就可以开始服务请求了。

#### 启动探测器

启动探测器仅在启动期间使用，例如用于启动缓慢的应用程序。如果启动探测器成功，活动和就绪探测器(如果配置了)将开始定期运行。

### 例子

该 pod 将执行以下命令:

*   睡眠 20
*   触摸/tmp/健康
*   睡眠 30
*   RM-RF/tmp/健康
*   睡眠 600

配置探头时，您应该知道的最基本参数是:

*   开始探测前的**初始延迟秒**
*   **周期秒**定义探测频率
*   **超时几秒钟**,之后探针超时
*   **失败阈值**确定 Kubernetes 放弃的尝试次数

根据我们的配置，探测将在 pod 创建后 10 秒开始。在前 20 秒内，文件`/tmp/healthy`不存在。因此，就绪探测将会失败。然后，我们将创建该文件，在接下来的 30 秒内，就绪探测将会成功，直到我们再次删除该文件。

活性探测是一个简单的`echo "I'm healthy"`命令。如果可以运行，则 pod 是健康的。否则，需要重新启动。

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - "sleep 20; touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
    image: busybox
    name: tmp
    livenessProbe:
     exec: # If this command can be run, the pod is healthy
       command:
       - echo
       - "I'm healthy"
     initialDelaySeconds: 5 # Only start probing after 5 seconds
     periodSeconds: 5 # Probe every 5 seconds
    readinessProbe:
     exec: # If this command can be run, the pod is ready to serve traffic
       command:
       - cat
       - /tmp/healthy
     initialDelaySeconds: 5 # Only start probing after 5 seconds
     periodSeconds: 10 # Probe every 10 seconds
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {} 
```

执行此命令查看 pod 经历的这些状态:

```
kubectl get pods --watch 
```

看看`Ready`栏是如何从`0/1`到`1/1`再回到`0/.1`的？

如我们所料，运行这个命令来获取事件列表，并查看探测是如何失败的。

```
kubectl describe pods my-pod 
```

### [集装箱资源](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)

创建 pod 时，您可以定义:

*   它需要运行的最小资源量，称为*请求*。
*   pod 应该使用的最大资源量，称为*限制*。

Kubernetes 在尝试安排 pod 时会考虑您请求的资源。无论当前的资源消耗如何，它都不会在没有足够容量的节点中调度 pod。

例如，如果 pod A 和 B 在节点 N 中运行，Kubernetes 将通过如下方式计算新的 pod C 是否能适合 N:

`Total capacity of N - (resources requested by A + resources requested by B) <= resources requested by C`

即使 pod A 和 B 没有使用它们请求的所有资源，Kubernetes **承诺**它们会在那个 pod 上提供这些资源。这就是为什么当前的资源使用量不是前面公式的一部分。

如果 pod 超出其资源限制，会发生两种情况:

*   如果它超过了它的 CPU 限制，它将被节流
*   如果它超过了内存限制，它将被重新启动

现在让我们创建一个 pod，它请求一些内存和 CPU，并限制允许它使用的资源:

```
apiVersion: v1
kind: Pod
metadata:
  name: resource-limited-pod
spec:
  restartPolicy: Never
  containers:
    - name: busybox
      image: busybox
      args:
        - /bin/sh
        - -c
        - "echo Hello Kubernetes; sleep 3600"
      resources:
        requests:
          memory: "300Mi"
          cpu: 0.2
        limits:
          memory: "1Gi"
          cpu: 0.5 
```

作为练习，为请求的内存(和/或 CPU)设置一些高得离谱的值，然后*尝试*来创建 pod。你会看到分离舱永远不会准备好。的产量

```
kubectl describe pods resource-limited-pod 
```

会告诉你为什么。

## [如何在 Kubernetes 中安排 Pods】](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node)

这部分不是 CKAD 考试课程的一部分。但是我相信您至少应该对我在这里公开的概念有一些基本的理解，因为在您使用 Kubernetes 时，它们很可能会出现。

Kubernetes 允许您指定希望通过多种机制在哪些节点上调度您的 pods。

最简单的方法是使用 **nodeSelector** 字段根据标签选择一个节点。然而，引入了其他特性以允许更复杂的设置。

### [污点和宽容](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)

您可以**污染一个节点以防止 pods 被安排在其中**，而无需修改 pods 本身。当你污染一个节点时，Kubernetes 将在该节点中安排的唯一的 pod 将是**容忍**污染的 pod。

如果您运行以下命令，就可以看到这一点:

```
kubectl describe node master.Kubernetes 
```

在上一个命令的输出中，您会看到这样一行:

```
Taints: node-role.kubernetes.io/master:NoSchedule 
```

这个污点阻止了 pods 在主节点中被调度(除非 pods 容忍这个污点)。

有三种类型的污点:

*   NoSchedule: Kubernetes 不会在一个节点中调度 pods，如果它们不能容忍污染的话。
*   PreferNoSchedule:不能容忍污染的 pod 不会在一个节点中被调度，除非它们不能在其他地方被调度。
*   NoExecute:如果已经在节点中运行的 pods *不能容忍污染，将它们从节点中驱逐出去。*

**污点不保证 pod 将被调度到特定的节点**。为了实现这一点，您需要我们现在将要探讨的*节点关联性*的概念。

### 节点关联性

节点关联告诉 Kubernetes**将 pods 调度到特定的节点**。

假设您有一个需要特殊硬件才能运行的服务。您希望将硬件专用于此服务，并且只希望来自 S 的 pods 在其上运行。你如何实现这一点？

您可以污染具有这种类型设备的节点，以便在这些节点中只能调度来自服务 S 的 pod。然而，Kubernetes 仍然可以在没有所需硬件的其他节点上部署这些 pod。

我们可以看到污点和节点相似性的结合是如何确保只有来自服务的 pod 运行在我们的专用节点中的:

*   节点关联将 pod 从调度到专用节点(而不是其他节点)。
*   污点确保在专用节点中不会调度任何其他的 pod，只调度来自服务 s 的 pod。

## 库伯内特斯的存储

默认情况下，当 pod 中的容器重新启动时，容器中的所有数据都会丢失。这是故意的。

如果您希望数据比容器、pod 甚至节点的寿命更长，您需要使用**卷**。此外，如果一个单元由多个容器组成，则该单元的容量可以被所有容器使用。

### [卷](https://kubernetes.io/docs/concepts/storage/volumes/)

在 pod 级别定义卷之后，您需要将它装入需要访问它的每个容器中。

有许多类型的卷可用于不同的用例(通常，这取决于您希望在 *pod* 被销毁时发生什么)。一些常见的卷类型有:

*   emptyDir:创建一个最初为空的目录。在 pod 中的不同容器之间共享文件的简单方法。
*   hostPath:将文件或目录从节点的文件系统挂载到 pod 中。删除 pod 后，文件将保留在主机中。
*   位于自动气象站、GCP 或 Azure 云上的卷。

有关这些卷和其他类型卷的更多信息，请参考文档。

要在容器中挂载一个卷，您的 pod 描述符(我们称这个文件为`with-mounted-volume.yaml`)应该如下所示:

```
apiVersion: v1
kind: Pod
metadata:
  name: with-mounted-volume
spec:
  volumes: # Volumes are defined at the pod level
  - name: my-volume # We'll use this name to mount the volume inside the pod
    hostPath: # Here we define the type of volume we want
      path: /var/my-k8s-volume
  containers:
  - args:
    - /bin/sh
    - -c
    - "sleep 3600"
    image: busybox
    name: bb
    volumeMounts:
    - name: my-volume
      mountPath: /tmp/my-volume-path
  restartPolicy: Never 
```

因为我们已经创建了一个`hostPath`卷，所以它的内容将比 pod 更持久。让我们来测试一下:

```
# Create a pod
kubectl apply -f with-mounted-volume.yaml
# Create a file at the mounted location
kubectl exec -it with-mounted-volume -- sh -c "echo 'Inside the pod' > /tmp/my-volume-path/newfile"
# Try to read the file
kubectl exec -it with-mounted-volume -- cat /tmp/my-volume-path/newfile
# Delete the pod
kubectl delete pods with-mounted-volume
# Create a new pod
kubectl apply -f with-mounted-volume.yaml
# Explore the content of `/tmp/my-volume-path`  to see if it persisted
kubectl exec -it with-mounted-volume -- cat /tmp/my-volume-path/newfile 
```

#### 重新审视多容器容器

现在我们已经熟悉了卷，让我们创建一个多容器 pod，它将使用一个挂载的卷在容器之间共享数据。

*   我们的 pod 描述符将被称为`communicating-containers.yaml`
*   我们会有一个有两个容器的容器。
*   其中一个会每秒钟将`Hello World`追加到一个文件中。
*   另一个容器可以访问这个文件的内容(以及放在这个共享卷中的任何内容)。我们将跟踪共享文件，看看另一个容器如何将`Hello World`附加到它上面。

这是我们 pod 的定义:

```
apiVersion: v1
kind: Pod
metadata:
  name: communicating-containers
spec:
  volumes:
  - name: vol
    emptyDir: {}
  containers:
  - args:
    - sh
    - -c
    - "while true;do echo 'Hello World' >> /etc/shared/log; sleep 1; done"
    image: busybox
    name: container-1
    volumeMounts:
    - name: vol
      mountPath: /etc/shared/ # The container can access the volume here
  - args:
    - sh
    - -c
    - "sleep 3600"
    image: busybox
    name: container-2
    volumeMounts:
    - name: vol
      mountPath: /etc/a-different-location # The volume can be mounted at different locations on each containers
  restartPolicy: Never 
```

让我们创建 pod:

```
kubectl apply -f communicating-containers.yaml 
```

一旦 pod 运行，我们可以从`container-2`开始跟踪文件:

```
kubectl exec -it communicating-containers -c container-2 -- tail -f /etc/a-different-location/log 
```

尽管是`container-1`向`log`写入数据，但由于它在共享卷中，`container-2`也能看到这个文件。

### [持久卷](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

持久卷(PVs)和持久卷声明(PVC)将 pod 与存储技术分离开来。PV 由集群管理员创建，或者基于[存储类](https://kubernetes.io/docs/concepts/storage/storage-classes/)动态创建。

与我们之前创建的在单元级别定义的卷相反，PVs 有自己的生命周期，独立于任何单元。

创建 PVs 后，*用户*可以创建永久卷声明来获得他们需要的存储，*，而无需关心支持他们的存储的实际基础架构*。

#### 例子

首先，我们在 pv.yaml 中定义一个持久卷:

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: myvolume
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
    - ReadWriteMany
  storageClassName: normal
  hostPath:
    path: /etc/foo 
```

对于持久卷声明，这是描述符(在 pvc.yaml 中):

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mypvc
spec:
  storageClassName: normal
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 4Gi 
```

现在，让我们在集群中创建资源

```
# First, the Persistent Volume
kubectl apply -f pv.yaml
# Then, the Persistent Volume Claim
kubectl apply -f pvc.yaml 
```

我们可以采用上一个示例中的 pod 定义，稍微调整一下，开始使用这个持久卷，而不是我们已经定义的 hostPath。这是我们唯一需要改变的事情:

```
volumes:
  - name: mypd # To refer to this volume when we configure the pod
    persistentVolumeClaim: # Instead of hostPath
      claimName: mypvc # The name of the pvc we just created 
```

## Kubernetes 的网络和安全

关于安全性，这些概念是您应该熟悉的最低要求，以便能够通过 CKAD 考试。

### [网络策略](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

默认情况下，允许所有入站流量(即流入应用程序的流量)和出站流量(即来自应用程序的流量)。任何 pod 都可以连接到任何其他 pod，即使它们在不同的名称空间中。

您可以根据不同的标准定义网络策略来控制入站和出站流量。

为了说明这一点，让我们创建一个网络策略，只允许来自标签为`access: allowed`的 pod 的流量进入我们的数据库:

```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: access-db-policy # pick a name
spec:
  podSelector:
    matchLabels:
      app: db # selector for the databse pods
  ingress: # allow ingress traffic
  - from:
    - podSelector: 
        matchLabels: 
          access: allowed # Only the pods with this label will be able to send traffic to the database 
```

### [安全上下文](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

在配置安全上下文时，您可以启用安全功能，例如防止容器以 root 用户身份运行，选择 pod 以什么用户身份运行，等等。这里有一个例子:

```
spec:
  securityContext:
    runAsUser: 1000 #This sets the user for every container in this pod, but can be overriden
  containers:
  - name: my-container
     image: alpine
     securityContext:
     	runAsUser: 1001 # This overrides the user set at the pod level     	
  - name: another-container
	.... 
```

### [服务账户](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

您的应用程序可以连接到 API 服务器并与之交互。例如，检索关于名称空间中的窗格的信息。服务帐户允许应用程序通过 API 服务器对进行认证，这样它们就可以在上面运行。

有一个*默认服务帐户*，但是让所有的 pods 都使用它并不是一个好的做法，因为并不是每个应用程序都需要能够在 API 服务器上执行相同的操作。

创建服务帐户最简单的方法是通过命令行:

```
kubectl create serviceaccount my-sa 
```

创建后，您可以使用 pod 描述符中的`serviceAccountName`字段将其分配给 pod:

```
spec:
  serviceAccountName: my-sa
  containers:
  - name: web-server
  ... 
```

一旦应用程序通过了身份验证，下一步就是查看它是否有适当的权限来执行它试图完成的操作，也就是说，查看应用程序是否得到了*授权*。

### [基于角色的访问控制](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

管理授权的常用方法是**基于角色的访问控制(RBAC)** 。服务帐户链接到一个或多个角色。每个角色都有权限对特定资源执行特定操作。

RBAC 授权分两步定义:

*   使用 Role 和 ClusterRole 资源创建角色，以指定可以在某些资源上执行的操作
*   使用 RoleBinding 和 ClusterRoleBinding 资源将角色绑定到帐户。

可以想象，包含前缀 Cluster 的资源在集群范围内是可用的，而其他资源只在特定的名称空间中定义。

因为这超出了 CKAD 考试的范围，所以我们不会详细讨论如何在集群中创建和配置 RBAC。

## Kubernetes 中的可观察性和调试

一旦部署了应用程序，您如何知道集群中正在发生什么呢？

我们已经引入了探针作为一种机制来决定是否需要重启 pods，以及 pods 是否准备好为流量提供服务。在这里，我们将看到更多的机制，以更好地了解我们的集群的状态，并解决任何问题。

#### 获取基本 Pod 信息

要访问 pod 的当前状态，有两个基本选项。这是第一个:

```
kubect get pods 
```

这将向您显示箱的`STATUS`、`AGE`、`RESTARTS`的数量以及箱内有多少个集装箱`READY`。您可以向该命令传递标志，以便它显示 pod 的 IP 地址、标签等。

第二个选项提供了更详细的信息:

```
kubectl describe pods my-pod 
```

在该命令输出的最底部，您会发现一个事件列表，该列表会在出现问题时向您提供提示:

*   活动/就绪探测失败
*   提取图像时出错
*   内存不足，无法安排 pod

诸如此类。

### 如何获取容器的日志

如果 pod 正在运行，您可以使用以下命令访问其日志:

```
kubectl logs pod-name [-c container-name] [-n namespace] 
```

如果您的 pod 中有多个容器，您只需要传递容器名称。类似地，只有从不同名称空间中的 pod 检索日志时才需要名称空间标志。

您甚至可以使用`-f`标志来跟踪日志:

```
kubectl logs -f pod-name [-c container-name] [-n namespace] 
```

这对认证来说就足够了。然而，在真实的生产环境中，手动检查日志既麻烦又低效。如果你在 GCP，你会希望使用其他技术来管理你的日志或服务，比如 StackDriver。如果你想从总体上了解更多关于 StackDriver 和 GCP 的信息，一定要看看我的 [GCP 指南](https://www.freecodecamp.org/news/google-cloud-platform-from-zero-to-hero/)。

### 故障排除提示

虽然在故障诊断方面没有通用的解决方案，但是从 pod 级别开始了解问题的根本原因通常是一个好主意。

您的基本工具应该是我们上面讨论过的命令。除此之外，您还可以随时打开 pod 内部的终端:

```
# Assuming you container image uses sh
kubectl exec -it my-pod -c container -n namespace -- sh 
```

不建议用这种方式解决 pod 内部的问题，因为根据设计，pod 是短暂的，重启后，您的更改将会丢失，问题会再次出现。然而，深入了解问题是个好主意。

要检索 pod 或节点指标，您可以运行以下命令:

```
kubectl top pod pod-name 
```

这些工具应该可以帮助您解决日常操作中最常见的问题。有太多的事情可能会出错，无法在这里涵盖。如果你面临一些你无法解决的问题，我建议你访问这个条目来获得调试你的应用程序的帮助。

## 提示和技巧

这是一些“窍门”，我发现在我与 Kubernetes 的日常工作中很有用，尤其是通过 CKAD 考试。**考试讲的是速度和效率**。考虑到这一点，让我们看看我们可以做些什么来表现得更好。

### 设置别名

您将一遍又一遍地输入相同的命令。一旦开始考试，请创建以下别名:

```
# Essential aliases. I strongly recommend setting them
alias k='kubectl'
alias kg='k get'
alias kgpo='k get pods'
alias kdpo='k describe pod'
alias kd='k describe'

# I also found this one very useful
alias kap='k apply -f'

# I have this for work only
alias kgpoa='kgpo --all-namespaces'
alias kgpol='kgpo --show-labels'
alias kgpow='kgpo -o wide'
alias kgd='kg deploy'
alias kgs='kg svc'
alias kdd='kd deploy'
alias kds='kd svc' 
```

### 更快地搜索您的命令历史记录

我见过很多工程师，其中很多是前辈，笨拙地敲了 20 次向上箭头键，以便在他们的命令历史中找到一些东西。

相反，要确保你能舒服地使用`Ctrl + r`。只要按下它，并开始键入你正在寻找的命令的一部分。然后，一直按`Ctrl + r`直到找到为止。现在你可以按下`Enter`来运行它或者简单地开始输入来修改它。

这是一个巨大的时间节省，但不是每个人都知道。

### 熟悉文档

考试的时候可以查阅文档。然而，现在不是学习新东西的时候。访问文档以获取 yaml 的模板，检查特定的参数，等等。

确保你对文档的**结构以及在哪里可以找到东西有一个很好的想法。使用**搜索功能**更快。**

最后，这张 [Kubectl 小抄](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)是金色的，在考试期间**将会提供给你。**

### 你不需要记住所有的东西

除了文档之外，`kubectl`对大多数命令都有一个`--help`标志(`-h`是简短的版本)。大多数时候你会在那里找到问题的答案。

事实上，我建议您在阅读文档之前这样做，因为这样更快。

假设您想要创建一个资源配额对象。您不记得需要编写的 yaml 或创建它的命令。但是，你知道`kubectl command`和`--help`标志。然后，你应该在去看医生之前试试这个:

```
# From this, we can get what we need to create our resource quota object
kubeclt create quota -h
# For example
kubeclt create quota my-quota --hard="secrets=2" 
```

您有大量的信息来源，因此您不需要记住许多命令或任何描述符。

然而，鉴于考试期间时间非常有限，我认为有必要记住以下一些命令以及如何利用它们。

### 快速生成基本 pod 规格

**这是极其重要的**。不要每次需要 pod 时都从文档中复制和粘贴，而是使用以下命令来获取描述符，您可以在以后修改该描述符以满足您的要求:

```
# Run pod nginx and write its spec into a file called pod.yaml
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml
# Some of these flags can be useful, depending on the problem
kubectl run nginx --image=nginx --restart=Never  --port=80 --labels=key=val --dry-run=client -o yaml > pod.yaml 
```

其他有用的参数:

```
--rm # to remove the pod as soon as it's finished
-it # Enter the interactive terminal, to run commands directly inside the pod 
```

例如，您可以使用它来创建一个临时窗格，并使用它来验证您的工作:

```
kubectl run tmp --image=busybox --restart=Never --rm -it -- /bin/sh 
```

现在你可以`wget -O- svc:port`查看你的服务是否在运行，网络策略是否在工作，等等。

##### 注意:

`--dry-run=client -o yaml`不仅仅是为了豆荚，而是为了许多资源。如果我们回到上一节，在那里我们创建了一个资源配额，我们可以得到如下描述符:

```
kubeclt create quota my-quota --hard="secrets=2" --dry-run=client -o yaml 
```

### 使用 grep

不需要深入了解正则表达式。只传递关键词。例如，从冗长的`kubectl describe pods my-pod`中快速检索信息:

```
# -i makes the search case-insensitive. It's a bit slower but for very short texts it won't make a difference
# -C 2 will display the matched line as well as the 2 lines before and after the match (the "context")
kubectl describe pods my-pod | grep -i -C 2 labels
kubectl describe pods my-pod | grep -i -C 2 ip
... 
```

### 观察窗格的状态变化

不要每隔几秒手动运行`kubeclt get pods`来查看变化，而是传递`watch`标志来查看您的 pod 运行情况:

```
kubectl get pods --watch 
```

### 快速删除窗格

犯错是学习过程的一部分。考试时你也会犯这些错误。由于时间紧迫，我们需要确保在删除资源时不必等待太久。添加这些标志以立即删除窗格:

```
k delete pods my-pod --force --grace-period=0 
```

### 使用特定命令运行 Pod

我发现学习如何从命令行向 pods、jobs、cronjobs 等传递命令非常有用，如下例所示:

```
kubectl run loop --image=busybox -o yaml --dry-run=client --restart=Never \
-- /bin/sh -c 'for i in {1..10}; do echo "Counting: $i"; done' \
> pod.yaml 
```

这将生成文件 pod.yaml:

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: loop
  name: loop
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - 'for i in {1..10}; do echo "Counting: $i"; done'
    image: busybox
    name: loop
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {} 
```

您可以运行单个命令或链接多个命令，就像一个迷你 bash 脚本:

```
# Run a particular executable
kubectl run busybox --image=busybox -it --restart=Never -- echo 'hello world'
# Run commands inside a shell (useful to run multiple commands)
kubectl run busybox --image=busybox -it --restart=Never -- /bin/sh -c 'echo hello world' 
```

这不是一个很大的区别，但是您不需要记住如何在 pod 描述符中编写它，也不需要浪费时间打开文档。你只需要运用你已经知道的东西。

### 铺开

熟悉`rollout`命令，以获得关于部署状态的信息。

我总是从这面`--help`旗开始，来记住如何做我想做的事情。

```
kubectl rollout -h 
```

### 奖金

这些最后的提示不是为了认证，而是为了日常工作:

*   这个 [kube-ps1 模块](https://github.com/jonmosco/kube-ps1)使得总是知道您在哪个集群和名称空间中操作变得容易，以防止错误，例如在您不应该的时候弄乱 prod 资源。
*   另外，我推荐看一看[掌舵人](https://helm.sh/)。Helm 是一个包管理器，可以用来轻松部署应用程序(可以把它想象成`npm`)。Helm 还允许您编写模板，您可以重用这些模板来创建基于不同值(名称、资源请求和限制等)的对象。

## 练习时间

你从实践中学到的总是比从阅读中学到的多。所以我在这里留下了一些我在备考过程中解决的问题，供大家练习。查看[本回购](https://github.com/dgkanatsios/CKAD-exercises)和[本](https://medium.com/bb-tutorials-and-thoughts/practice-enough-with-these-questions-for-the-ckad-exam-2f42d1228552)。

我建议在检查建议的解决方案之前，自己解决所有这些问题。此外，验证您的工作:检查日志，创建一个 pod 来连接到您的服务，等等。这也将有助于为考试建立你的肌肉记忆。

试着按照我的建议来解决这些练习，就像你真的在考试一样:没有干扰，只有一个标签打开——用官方的 Kubernetes 文档。

## 结论

本指南包含了你需要的一切，让你的技能更上一层楼，通过 CKAD 考试，成为一名高效的 Kubernetes 开发者。一切都在你的掌握之中。你只需要投入一些工作。祝你好运！

您可以访问我的博客[www.yourdevopsguy.com](https://www.yourdevopsguy.com/)和[在 Twitter 上关注我](https://twitter.com/CodingLanguages)获取更多高质量的技术内容。

如果你喜欢这篇文章，请分享它，因为你可以帮助别人通过考试或找到工作。