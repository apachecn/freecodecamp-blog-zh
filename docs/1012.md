# 如何通过认证 Kubernetes 安全专家考试–备忘单和学习指南

> 原文：<https://www.freecodecamp.org/news/how-to-pass-the-certified-kubernetes-security-specialist-exam/>

本文基于我学习和通过认证 Kubernetes 安全专家考试的经历。我在 2021 年 9 月的第一次尝试中通过了考试。

我在 2020 年 2 月通过了认证 Kubernetes 应用程序开发员考试，随后在 2020 年 3 月通过了认证 Kubernetes 管理员考试。

认证 Kubernetes 安全专家或 CKS 考试于 2020 年 11 月左右发布，但我没有机会在 2021 年 9 月之前参加考试。

作为一点背景信息，在过去的 3 年里，我几乎每天都与 Kubernetes 一起工作，这种经历对我通过 CKS 考试是一个额外的优势。

在这篇文章中，我将分享一些可以帮助你准备和通过考试的资源，以及一张你可以在准备时使用的有用的小抄。我还会分享一些建议，这些建议应该会对你有所帮助。

### What is Kubernetes?

Kubernetes 是目前最先进、功能最丰富的容器编排系统，并且还在不断改进。

它有一个庞大的社区来支持，并且它总是在构建新的功能和解决问题。Kubernetes 无疑在以极快的速度发展，要跟上它的发展速度是一个挑战。这使得它成为容器编排解决方案的最佳选择。

* * *

## 目录:

*   [CKS 考试资源](#resourcesfortheexam)
*   [别名](#aliases)
    *   [vi 默认为~/。vimrc](#videfaultsforvimrc)
    *   [kubectl 默认为~/。巴沙尔](#kubectldefaultsforbashrc)
*   [快捷键](#shortcuts)
*   [库柏叶](#kubernetescheatsheet)
    *   [kubectl 运行命令](#kubectlruncommand)
    *   [如何从现有 pod 生成 yaml 规范](#howtogenerateyamlspecfromanexistingpod)
    *   [kubectl pod commands](#kubectlpodcommands)
    *   [如何打印日志并导出](#howtoprintlogsandexportthem)
    *   [如何创建配置图和密码](#howtocreateconfigmapsandsecrets)
    *   [有助于调试的命令](#helpfulcommandsfordebugging)
    *   [滚动更新和推出](#rollingupdatesandrollouts)
    *   [缩放和自动缩放命令](#scaleandautoscalecommand)
    *   [网络策略](#networkpolicy)
    *   [使用 Kubesec 的静态分析](#staticanalysisusingkubesec)
    *   [使用 Trivvy 进行漏洞扫描](#vulnerabilityscanningusingtrivvy)
    *   [如何删除不需要的服务](#howtoremoveunwantedservices)
    *   [运行时类](#runtimeclasses)
    *   [RBAC 命令](#rbaccommands)
    *   [集群维护](#clustermaintenance)
*   [CKS 考试小贴士](#cksexamtips)
    *   [JSON 和 JSONPath](#jsonandjsonpath)
*   [CKS 考试题目](#cksexamtopics)
    *   [如何保护和强化容器图像](#howtosecureandhardencontainerimages)
    *   [如何最小化操作系统占用空间](#howtominimiseosfootprint)
        *   [容器层](#conatinerlayers)
        *   [多阶段构建](#multistagebuilds)
    *   [如何限制节点访问](#howtolimitnodeaccess)
    *   [SSH 强化](#sshhardening)
        *   [如何禁用 SSH](#howtodisablessh)
        *   [如何删除过时的包和服务](#howtoremoveobsoletepackagesandservices)
        *   [如何限制内核模块](#howtorestrictkernelmodules)
        *   [如何识别和禁用开放端口](#howtoidentifyanddisableopenports)
    *   [如何限制网络访问](#howtorestrictnetworkaccess)
        *   [如何识别运行在端口](#howtoidentityaservicerunningonport)上的服务
        *   [UFW 防火墙](#ufwfirewall)
    *   [Linux 系统调用](#linuxsyscalls)
        *   [如何使用 Strace 跟踪系统调用](#howtotracesyscallsusingstrace)
    *   [AquaSec tracee](#aquasectracee)
    *   [如何用 Seccomp 限制系统调用](#howtorestrictsyscallswithseccomp)
        *   [干馏在立方中](#seccompinkubernetes)
    *   [AppArmor](#apparmor)
        *   [如何在 Kubernetes 中使用 AppArmor](#howtouseapparmorinkubernetes)
    *   [Linux 功能](#linuxcapabilities)
*   [如何备考](#howtopreparefortheexam)
*   练习，练习，再练习！

* * *

## 考试资源

以下是一些通过 CKS 考试的有用资源:

1.  [killer . sh 认证的 Kubernetes 安全专家](https://www.udemy.com/course/certified-kubernetes-security-specialist/)
2.  [kode cloud 认证的 Kubernetes 安全专家(CKS)](https://kodekloud.com/courses/certified-kubernetes-security-specialist-cks/)
3.  [瓦利德·沙阿里为 CKS 考试收集了一些不可或缺的材料](https://github.com/walidshaari/Certified-Kubernetes-Security-Specialist)
4.  [Abdennour 对 CKS 考试目标的参考](https://github.com/abdennour/certified-kubernetes-security-specialist)
5.  [Ibrahim Jelliti 为准备 Kubernetes 安全专家(CKSS)认证考试而收集的资源](https://github.com/ibrahimjelliti/CKSS-Certified-Kubernetes-Security-Specialist)

KodeKloud 和 Killer.sh 的课程提供了模拟考试模拟器，这对准备考试非常有帮助，并提供了一个关于考试的很好的想法。我强烈建议报读一门或两门课程。

从 Linux Foundation 购买考试可以让您免费试用 killer.sh 的考试模拟器。这样，如果您精通课程内容，您可以跳过课程，直接使用考试提供的考试模拟器。

考试费用为 375 美元，但有优惠和交易，如果你寻找它们，你可能会得到更好的价格。考试时长为 2 小时，有效期为 2 年，而 CKA 和 CKAD 的有效期为 3 年。

## 别名

CKS 是一种基于表现的考试，你会得到一个考试模拟器，你必须在其中解决问题。除了考试选项卡之外，您只能打开一个选项卡。

因为这个考试需要你写很多命令，我很早就想到我必须依靠别名来减少击键次数以节省时间。

我在考试的时候用到了 **vi** 编辑器，所以在这里分享一些对这个编辑器有用的小技巧。

### ~/的 vi 默认值。vimrc:

```
vi ~/.vimrc
---
:set number
:set et
:set sw=2 ts=2 sts=2
---
^: Start of word in line
0: Start of line
$: End of line
w: End of word
GG: End of file 
```

### kubectl 默认为~/。bashrc:

```
vi ~/.bashrc
---
alias k='kubectl'
alias kg='k get'
alias kd='k describe'
alias kl='k logs'
alias ke='k explain'
alias kr='k replace'
alias kc='k create'
alias kgp='k get po'
alias kgn='k get no'
alias kge='k get ev'
alias kex='k exec -it'
alias kgc='k config get-contexts'
alias ksn='k config set-context --current --namespace'
alias kuc='k config use-context'
alias krun='k run'
export do='--dry-run=client -oyaml'
export force='--grace-period=0 --force'

source <(kubectl completion bash)
source <(kubectl completion bash | sed 's/kubectl/k/g' )
complete -F __start_kubectl k

alias krp='k run test --image=busybox --restart=Never'
alias kuc='k config use-context'
--- 
```

## 快捷指令

`kubectl get`命令为访问资源提供了简短易记的名字，比如`pvc`代表`persistentstorageclaim`。这些可以帮助在考试中节省大量的击键和宝贵的时间。

*   **po** 为`pods`
*   **rs** 为`replicasets`
*   **为`deployments`部署**
*   **svc** 为`services`
*   **ns** 为`namespace`
*   **netpol** 为`networkpolicy`
*   **pv** 为`persistentstorage`
*   **pvc** 为`persistentstorageclaim`
*   **萨**为`serviceaccounts`

## 库柏切叶刀

### kubectl 运行命令

`kubectl run`命令提供了一个标志`--restart`,允许您从部署到 CronJob 创建不同种类的 Kubernetes 对象。

下面的代码片段显示了可用于`--restart`标志的不同选项。

```
k run:
--restart=Always                             #Creates a deployment
--restart=Never                              #Creates a Pod
--restart=OnFailure                          #Creates a Job
--restart=OnFailure --schedule="*/1 * * * *" #Creates a CronJob 
```

### 如何从现有的 pod 生成 yaml 规范

有时，从现有的 pod 生成一个规范并对其进行更改比从头创建一个新的更容易。`kubectl get pod`命令为我们提供了所需的标志，以我们想要的格式输出 pod 规范。

```
kgp <pod-name> -o wide

# Generating YAML Pod spec
kgp <pod-name> -o yaml
kgp <pod-name> -o yaml > <pod-name>.yaml

# Get a pod's YAML spec without cluster specific information
kgp my-pod -o yaml --export > <pod-name>.yaml 
```

### kubectl pod commands

`kubectl run`命令提供了很多选项，比如指定请求和 pod 应该使用的限制，或者容器创建后应该运行的命令。

```
# Output YAML for a nginx pod running an echo command
krun nginx --image=nginx --restart=Never --dry-run -o yaml -- /bin/sh -c 'echo Hello World!'
# Output YAML for a busybox pod running a sleep command
krun busybox --image=busybox:1.28 --restart=Never --dry-run -o yaml -- /bin/sh -c 'while true; do echo sleep; sleep 10; done'
# Run a pod with set requests and limits
krun nginx --image=nginx --restart=Never --requests='cpu=100m,memory=512Mi' --limits='cpu=300m,memory=1Gi'
# Delete pod without delay
k delete po busybox --grace-period=0 --force 
```

### 如何打印日志并将其导出

在调试应用程序时，日志是信息的基本来源。`kubectl logs`命令提供了检查给定 pod 日志的功能。您可以使用以下命令来检查给定 pod 的日志。

```
kubectl logs deploy/<podname>
kubectl logs deployment/<podname>
#Follow logs
kubectl logs deploy/<podname> --tail 1 --follow 
```

除了查看日志之外，我们还可以将日志导出到一个文件中，以便进一步调试和任何人共享。

```
kubectl logs <podname> --namespace <ns> > /path/to/file.format 
```

### 如何创建配置映射和机密

`kubectl create`命令让我们从命令行创建配置图和密码。我们也可以使用 YAML 文件来创建相同的资源，并通过使用`kubectl apply -f <filename>`来应用命令。

```
kc cm my-cm --from-literal=APP_ENV=dev
kc cm my-cm --from-file=test.txt
kc cm my-cm --from-env-file=config.env

kc secret generic my-secret --from-literal=APP_SECRET=sdcdcsdcsdcsdc
kc secret generic my-secret --from-file=secret.txt
kc secret generic my-secret --from-env-file=secret.env 
```

### 用于调试的有用命令

当你在日常工作中面临问题和错误时，以及在解决 CKS 考试中的问题时，调试是一项非常重要的技能。

除了从容器中输出日志的能力之外，`kubectl exec`命令还允许您登录到正在运行的容器并调试问题。在容器内部，你也可以使用像`nc`和`nslookup`这样的工具来诊断网络相关的问题。

```
# Run busybox container
k run busybox --image=busybox:1.28 --rm --restart=Never -it sh
# Connect to a specific container in a Pod
k exec -it busybox -c busybox2 -- /bin/sh
# adding limits and requests in command
kubectl run nginx --image=nginx --restart=Never --requests='cpu=100m,memory=256Mi' --limits='cpu=200m,memory=512Mi'
# Create a Pod with a service
kubectl run nginx --image=nginx --restart=Never --port=80 --expose
# Check port
nc -z -v -w 2 <service-name> <port-name>
# NSLookup
nslookup <service-name>
nslookup 10-32-0-10.default.pod 
```

### 滚动更新和推出

`kubectl rollout`命令提供了检查更新状态的能力，如果需要，还可以回滚到以前的版本。

```
k set image deploy/nginx nginx=nginx:1.17.0 --record
k rollout status deploy/nginx
k rollout history deploy/nginx
# Rollback to previous version
k rollout undo deploy/nginx
# Rollback to revision number
k rollout undo deploy/nginx --to-revision=2
k rollout pause deploy/nginx
k rollout resume deploy/nginx
k rollout restart deploy/nginx
kubectl run nginx-deploy --image=nginx:1.16 --replias=1 --record 
```

### 缩放和自动缩放命令

`kubectl scale`命令提供了在给定部署中放大或缩小 pod 的功能。

使用`kubectl autoscale`命令，我们可以定义给定部署应该运行的最小单元数量，以及部署可以扩展到的最大单元数量，以及扩展标准，如 CPU 百分比。

```
k scale deploy/nginx --replicas=6
k autoscale deploy/nginx --min=3 --max=9 --cpu-percent=80 
```

### 网络策略

在 Kubernetes 集群中，默认情况下，所有的 pod 都可以与所有的 pod 通信，这在某些实现中可能是一个安全问题。

为了解决这个问题，Kubernetes 引入了网络策略，根据作为 pod 规范一部分的 pod 标签来允许或拒绝进出 pod 的流量。

以下示例拒绝在所有名称空间中运行的 pod 的入站和出站流量。

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: example
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Egress
  - Ingress 
```

以下示例拒绝在所有名称空间中运行的 pod 的入站和出站流量。但是它允许访问运行在端口 53 上的 DNS 解析服务。

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Egress
  - Ingress
  egress:
  - to:
    ports:
      - port: 53
        protocol: TCP
      - port: 53
        protocol: UDP 
```

以下示例拒绝对 AWS EC2 实例中 IP 地址`169.256.169.256`上运行的元数据服务器的出口访问。

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name:cloud-metadata-deny
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
      - ipBlock: 
          cidr: 0.0.0.0/0
          except:
          - 169.256.169.256/32 
```

以下示例允许对 AWS EC2 实例中 IP 地址`169.256.169.256`上运行的元数据服务器进行出口访问。

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: cloud-metadata-accessor
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: metadata-accessor
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 169.256.169.256/32 
```

### 使用 Kubesec 进行静态分析

Kubesec 是一个静态分析工具，用于分析 YAML 文件，找出文件中的问题。

```
kubesec scan pod.yaml

# Using online kubesec API
curl -sSX POST --data-binary @pod.yaml https://v2.kubesec.io/scan

# Running the API locally
kubesec http 8080 &

kubesec scan pod.yaml -o pod_report.json -o json 
```

### 使用 Trivvy 进行漏洞扫描

Trivvy 是一个漏洞扫描工具，它扫描容器图像以发现安全问题。

```
trivy image nginx:1.18.0
trivy image --severity CRITICAL nginx:1.18.0
trivy image --severity CRITICAL, HIGH nginx:1.18.0
trivy image --ignore-unfixed nginx:1.18.0

# Scanning image tarball
docker save nginx:1.18.0 > nginx.tar
trivy image --input archive.tar

# Scan and output results to file
trivy image --output python_alpine.txt python:3.10.0a4-alpine
trivy image --severity HIGH --output /root/python.txt python:3.10.0a4-alpine

# Scan image tarball
trivy image --input alpine.tar --format json --output /root/alpine.json 
```

### 如何删除不需要的服务

`systemctl`展示了启动、停止、启用、禁用和列出运行在 Linux 虚拟机上的服务的能力。

列出服务:

```
systemctl list-units --type service 
```

停止服务:

```
systemctl stop apache2 
```

禁用服务:

```
systemctl disable apache2 
```

移除服务:

```
apt remove apache2 
```

### 运行时类

Kubernetes 在版本`v1.12`中引入了 RuntimeClass 特性，用于选择容器运行时配置。容器运行时配置用于运行 pod 的底层容器。

大多数 Kubernetes 集群使用`dockershim`作为运行容器的运行时类，但是您可以使用不同的容器运行时。

在 Kubernetes 版本`v1.20`中`dockershim`已被弃用，并将在`v1.24`中移除。

如何创建运行时类:

```
apiversion: node.k8s.io/v1beta1
kind: RuntimeClass
metadata:
  name: gvisor
handler: runsc 
```

如何对任何给定的 pod 使用运行时类:

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  runtimeClassName: gvisor
  containers:
  - name: nginx
    image: nginx 
```

### RBAC 命令

在库伯内特斯，

> 基于角色的访问控制(RBAC)命令提供了一种基于单个用户或服务帐户的角色来管理对 Kubernetes 资源的访问的方法。([来源](https://kubernetes.io/docs/reference/access-authn-authz/rbac/))

以下是创建角色的方法:

```
kubectl create role developer --resource=pods --verb=create,list,get,update,delete --namespace=development 
```

如何创建角色绑定:

```
kubectl create rolebinding developer-role-binding --role=developer --user=faizan --namespace=development 
```

如何验证:

```
kubectl auth can-i update pods --namespace=development --as=faizan 
```

如何创建群集角色:

```
kubectl create clusterrole pvviewer-role --resource=persistentvolumes --verb=list 
```

以及如何创建与服务帐户的 Clusterrole 绑定关联:

```
kubectl create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer 
```

### 集群维护

您可以使用`kubectl drain`命令从给定的节点中删除所有正在运行的工作负载(pod)。

使用`kubectl cordon`命令圈出一个节点，将其标记为可调度。

并且使用`kubectl uncordon`命令将节点设置为可调度的，这意味着控制器管理器可以为给定的节点调度新的 pod。

如何排空所有 pod 的一个节点:

```
kubectl drain node-1 
```

如何清空节点并忽略 daemonsets:

```
kubectl drain node01 --ignore-daemonsets 
```

如何强制排水:

```
kubectl drain node02 --ignore-daemonsets --force 
```

如何将节点标记为不可调度，以便不能在此节点上调度新的单元:

```
kubectl cordon node-1 
```

将节点标记为可调度

```
kubectl uncordon node-1 
```

## CKS 考试小贴士

Kubernetes `kubectl get`命令为用户提供了一个输出标志，`-o`或`--output`，帮助我们以 JSON、yaml、wide 或 custom-columns 的形式格式化输出。

### JSON 和 JSONPath

如何以 JSON 对象的形式输出所有窗格的内容:

```
kubectl get pods -o json 
```

JSONPath 从 JSON 对象输出一个特定的键:

```
kubectl get pods -o=jsonpath='{@}'
kubectl get pods -o=jsonpath='{.items[0]}' 
```

`.items[*]`用于我们有多个对象的情况，例如有一个 pod 配置的多个容器:

```
# For list of items use .items[*]
k get pods -o 'jsonpath={.items[*].metadata.labels.version}'
# For single item
k get po busybox -o jsonpath='{.metadata}'
k get po busybox -o jsonpath="{['.metadata.name', '.metadata.namespace']}{'\n'}" 
```

该命令使用 JSONPath 返回节点的内部 IP:

```
kubectl get nodes -o=jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}' 
```

该命令检查特定键的相等性:

```
kubectl get pod api-stag-765797cf-lrd8q -o=jsonpath='{.spec.volumes[?(@.name=="api-data")].persistentVolumeClaim.claimName}'
kubectl get pod -o=jsonpath='{.items[*].spec.tolerations[?(@.effect=="NoSchedule")].key}' 
```

自定义列有助于输出特定字段:

```
kubectl get pods -o='custom-columns=PODS:.metadata.name,Images:.spec.containers[*].image' 
```

## CKS 考试题目

CKS 考试涵盖了与 Kubernetes 生态系统中的安全性相关的主题。Kubernetes 安全性是一篇文章要涵盖的庞大主题，因此这篇文章包含了考试中涉及的一些主题。

### 如何保护和强化容器映像

在设计运行代码的容器映像时，要特别注意安全和强化措施，以防止黑客和特权提升攻击。在构建容器映像时，请记住以下几点:

1.  使用特定的包版本，如`alpine:3.13`。
2.  不要以根用户身份运行——使用`USER <username>`阻止根用户访问。
3.  使用`readOnlyRootFilesystem: true`将`securityContext`中的文件系统设为只读
4.  使用`RUN rm -rf /bin/*`移除外壳访问

### 如何最小化操作系统占用空间

#### 容器层

指令`RUN`、`COPY`和`ADD`创建容器层。其他指令会创建临时中间映像，并且不会增加构件的大小。创建图层的指令会增加生成图像的大小。

典型的 docker 文件如下所示。它使用`RUN`指令添加一个层。

```
FROM ubuntu

RUN apt-get update && apt-get install -y golang-go

CMD ["sh"] 
```

#### 多阶段构建

多阶段构建利用 Dockerfile 文件中的多个`FROM`语句。`FROM`指令标志着构建的一个新阶段。它结合了多个`FROM`语句，允许利用以前的构建，以便有选择地将二进制文件复制到新的构建阶段，忽略不必要的二进制文件。最终的 Docker 图像在尺寸上要小得多，攻击面也大大减小。

```
FROM ubuntu:20.04 AS build
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN CGO_ENABLED=0 go build app.go

FROM alpine:3.13
RUN chmod a-w /etc
RUN addgroup -S appgroup && adduser -S appuser -G appgroup -h /home/appuser
RUN rm -rf /bin/*
COPY --from=build /app /home/appuser/
USER appuser
CMD ["/home/appuser/app"] 
```

### 如何限制节点访问

访问控制文件包含 Linux 操作系统中用户/组的敏感信息。

```
#Stores information about the UID/GID, user shell, and home directory for a user
/etc/passwd
#Stores the user password in a hashed format
/etc/shadow
#Stores information about the group a user belongs
/etc/group
#Stored information about the Sudoers present in the system
/etc/sudoers 
```

禁用用户帐户有助于通过禁止登录给定的用户帐户来保护对节点的访问。

```
usermod -s /bin/nologin <username> 
```

禁用`root`用户帐户具有特殊意义，因为 root 帐户拥有所有功能。

```
usermod -s /bin/nologin root 
```

下面是如何添加带有主目录和 shell 的用户:

```
adduser --home /opt/faizanbashir --shell /bin/bash --uid 2328 --ingroup admin faizanbashir
useradd -d /opt/faizanbashir -s /bin/bash -G admin -u 2328 faizanbashir 
```

如何删除用户帐户:

```
userdel <username> 
```

如何删除群组:

```
groupdel <groupname> 
```

如何将用户添加到组中:

```
adduser <username> <groupname> 
```

如何从组中删除用户:

```
#deluser faizanbashir admin
deluser <username> <groupname> 
```

如何为用户设置密码:

```
passwd <username> 
```

如何将用户提升到 sudoer:

```
vim /etc/sudoers
>>>
faizanbashir ALL=(ALL:ALL) ALL 
```

如何在没有密码的情况下启用 sudo:

```
vim /etc/sudoers
>>>
faizanbashir ALL=(ALL) NOPASSWD:ALL

visudo
usermod -aG sudo faizanbashir
usermod faizanbashir -G admin 
```

### SSH 强化

#### 如何禁用 SSH

可以利用`/etc/ssh/sshd_config`中给出的配置来保护对 Linux 节点的 SSH 访问。将`PermitRootLogin`设置为`no`会禁用节点上的 root 登录。

要强制使用密钥登录和禁止使用密码登录节点，可以将`PasswordAuthentication`设置为`no`。

```
vim /etc/ssh/sshd_config
>>
PermitRootLogin no
PasswordAuthentication no
<<
# Restart SSHD Service
systemctl restart sshd 
```

如何为 root 用户设置不登录:

```
usermod -s /bin/nologin root 
```

SSH 复制用户密钥/无密码 SSH:

```
ssh-copy-id -i ~/.ssh/id_rsa.pub faizanbashir@node01
ssh faizanbashir@node01 
```

### 如何删除过时的包和服务

以下是如何列出 Ubuntu 机器上运行的所有服务:

```
systemctl list-units --type service
systemctl list-units --type service --state running 
```

如何停止、禁用和删除服务:

```
systemctl stop apache2
systemctl disable apache2
apt remove apache2 
```

### 如何限制内核模块

在 Linux 中，内核模块是可以根据需要加载和卸载到内核中的代码片段。它们扩展了内核的功能，而不需要重启系统。模块可以配置为内置或可加载的。

如何列出所有内核模块:

```
lsmod 
```

如何手动将模块加载到内核中:

```
modprobe pcspkr 
```

如何将模块列入黑名单:(参考:CIS 基准-> 3.4 不常用网络协议)

```
cat /etc/modprobe.d/blacklist.conf
>>>
blacklist sctp
blacklist dccp

# Shutdown for changes to take effect
shutdown -r now

# Verify
lsmod | grep dccp 
```

### 如何识别和禁用开放端口

如何检查打开的端口:

```
netstat -an | grep -w LISTEN
netstat -natp | grep 9090

nc -zv <hostname|IP> 22
nc -zv <hostname|IP> 10-22

ufw deny 8080 
```

如何检查端口使用情况:

```
/etc/services | grep -w 53 
```

这是一个关于开放端口列表的参考文档。

### 如何限制网络访问

#### 如何识别端口上运行的服务:

```
systemctl status ssh
cat /etc/services | grep ssh
netstat -an | grep 22 | grep -w LISTEN 
```

#### UFW 防火墙

简单防火墙(UFW)是一个用于管理 Arch Linux、Debian 或 Ubuntu 中的防火墙规则的工具。UFW 允许您允许和阻止特定端口和特定来源的流量。

以下是安装 UFW 防火墙的方法:

```
apt-get update
apt-get install ufw
systemctl enable ufw
systemctl start ufw
ufw status
ufw status numbered 
```

如何允许所有出站和入站连接:

```
ufw default allow outgoing
ufw default allow incoming 
```

如何允许规则:

```
ufw allow 22
ufw allow 1000:2000/tcp
ufw allow from 172.16.238.5 to any port 22 proto tcp
ufw allow from 172.16.238.5 to any port 80 proto tcp
ufw allow from 172.16.100.0/28 to any port 80 proto tcp 
```

如何拒绝规则:

```
ufw deny 8080 
```

如何启用和激活防火墙:

```
ufw enable 
```

如何删除规则:

```
ufw delete deny 8080
ufw delete <rule-line> 
```

如何重置规则:

```
ufw reset 
```

### Linux 系统调用

Linux 系统调用用于从用户空间向 Linux 内核发出请求。例如，在创建文件时，用户空间向 Linux 内核发出创建文件的请求。

内核空间有以下内容:

*   内核代码
*   内核扩展
*   设备驱动程序

#### 如何使用 Strace 跟踪系统调用

下面是使用 strace 跟踪系统调用的方法:

```
which strace
strace touch /tmp/error.log 
```

如何获取服务的 PID:

```
pidof sshd
strace -p <pid> 
```

如何列出操作期间进行的所有系统调用:

```
strace -c touch /tmp/error.log 
```

如何合并列表系统调用:(计数并总结)

```
strace -cw ls / 
```

如何遵循 PID 并巩固:

```
strace -p 3502 -f -cw 
```

### AquaSec 追踪

AquaSec Tracee 由 Aqua Security 创建，它使用 eBPF 来跟踪容器中的事件。Tracee 在运行时直接在内核空间使用 eBPF(Extended Berkeley Packet Filter ),不会干扰内核源代码或加载任何内核模块。

*   二进制存储在`/tmp/tracee`
*   如果使用具有`--privileged`功能的容器运行，需要以只读模式访问以下内容:
    *   `/tmp/tracee` - >默认工作空间
    *   `/lib/modules` - >内核头
    *   `/usr/src` - >内核头

如何在 Docker 容器中找到 Tracee 的乐趣:

```
docker run --name tracee --rm --privileged --pid=host \
  -v /lib/modules/:/lib/modules/:ro -v /usr/src/:/usr/src/ro \
  -v /tmp/tracee:/tmp/tracee aquasec/tracee:0.4.0 --trace comm=ls

# List syscalls made by all the new process on the host
docker run --name tracee --rm --privileged --pid=host \
  -v /lib/modules/:/lib/modules/:ro -v /usr/src/:/usr/src/ro \
  -v /tmp/tracee:/tmp/tracee aquasec/tracee:0.4.0 --trace pid=new

# List syscalls made from any new container
docker run --name tracee --rm --privileged --pid=host \
  -v /lib/modules/:/lib/modules/:ro -v /usr/src/:/usr/src/ro \
  -v /tmp/tracee:/tmp/tracee aquasec/tracee:0.4.0 --trace container=new 
```

### 如何用 Seccomp 限制系统调用

安全计算模式是一个 Linux 内核级的特性，你可以用它来保护应用程序只使用它们需要的系统调用。

如何检查对 seccomp 的支持:

```
grep -i seccomp /boot/config-$(uname -r) 
```

如何测试以更改系统时间:

```
docker run -it --rm docker/whalesay /bin/sh
# date -s '19 APR 2013 22:00:00'

ps -ef 
```

如何检查任何 PID 的 seccomp 状态:

```
grep -i seccomp /proc/1/status 
```

Seccomp 模式:

*   模式 0:禁用
*   模式一:严格
*   模式 2:过滤

以下配置用于将系统调用列入白名单。白名单配置文件是安全的，但是必须有选择地启用系统调用，因为默认情况下它会阻止所有系统调用。

```
{
  "defaultAction": "SCMP_ACT_ERRNO",
  "architectures": [
    "SCMP_ARCH_X86_64",
    "SCMP_ARCH_X86",
    "SCMP_ARCH_X32"
  ],
  "syscalls": [
    {
      "names": [
        "<syscall-1>",
        "<syscall-2>",
        "<syscall-3>"
      ],
      "action": "SCMP_ACT_ALLOW"
    }
  ]
} 
```

以下配置用于将系统调用列入黑名单。黑名单配置文件比白名单具有更大的攻击面。

```
{
  "defaultAction": "SCMP_ACT_ALLOW",
  "architectures": [
    "SCMP_ARCH_X86_64",
    "SCMP_ARCH_X86",
    "SCMP_ARCH_X32"
  ],
  "syscalls": [
    {
      "names": [
        "<syscall-1>",
        "<syscall-2>",
        "<syscall-3>"
      ],
      "action": "SCMP_ACT_ERRNO"
    }
  ]
} 
```

Docker seccomp 配置文件阻塞了 x86 体系结构上 300 多个系统调用中的 60 个。

如何在 Docker 中使用 seccomp 配置文件:

```
docker run -it --rm --security-opt seccomp=/root/custom.json docker/whalesay /bin/sh 
```

如何允许容器的所有系统调用:

```
docker run -it --rm --security-opt seccomp=unconfined docker/whalesay /bin/sh

# Verify
grep -i seccomp /proc/1/status

# Output should be:
Seccomp:         0 
```

如何使用 Docker 容器获取容器运行时相关信息:

```
docker run r.j3ss.co/amicontained amicontained 
```

#### 库比涅斯的 Seccomp

安全计算模式(SECCOMP)是 Linux 内核的一个特性。您可以使用它来限制容器内可用的操作。 [Seccomp 文档](https://kubernetes.io/docs/tutorials/clusters/seccomp)

如何运行包含在 Kubernetes 中的:

```
kubectl run amicontained --image r.j3ss.co/amicontained amicontained -- amicontained 
```

从版本`v1.20`开始，Kubernetes 默认不实现 seccomp。

seccomp ' runtimedefault ' docker profile in kubricks:在库伯内的外挂程式设定档:

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: amicontained
  name: amicontained
spec:
  securityContext:
    seccompProfile:
      type: RuntimeDefault
  containers:
  - args:
    - amicontained
    image: r.j3ss.co/amicontained
    name: amicontained
    securityContext:
      allowPrivilegeEscalation: false 
```

kubelets 中的默认 seccomp 位置

```
/var/lib/kubelet/seccomp 
```

如何在节点中创建 seccomp 配置文件:

```
mkdir -p /var/lib/kubelet/seccomp/profiles

# Add a profile for audit
vim /var/lib/kubelet/seccomp/profiles/audit.json
>>>
{
  defaultAction: "SCMP_ACT_LOG"
}

# Add a profile for violations (Blocks all syscalls by default, will let nothing run)
vim /var/lib/kubelet/seccomp/profiles/violation.json
>>>
{
  defaultAction: "SCMP_ACT_ERRNO"
} 
```

本地 seccomp 配置文件–该文件应该存在于本地节点上，以便能够工作:

```
...
securityContext:
  seccompProfile:
    type: Localhost
    localhostProfile: profiles/audit.json
... 
```

上述配置文件将允许系统调用保存到一个文件中。

```
grep syscall /var/log/syslog 
```

如何将系统调用号映射到系统调用名:

```
grep -w 35 /usr/include/asm/unistd_64.h

# OR
grep -w 35 /usr/include/asm-generic/unistd.h 
```

### 表观摩尔

AppArmor 是一个 Linux 安全模块，用于将程序限制在一组有限的资源中。

如何安装设备:

```
apt-get install apparmor-utils 
```

如何检查 AppArmor 是否正在运行并已启用:

```
systemctl status apparmor

cat /sys/module/apparmor/parameters/enabled
Y 
```

AppArmor 配置文件存储在:

```
cat /etc/apparmor.d/root.add_data.sh 
```

如何列出 AppArmor 个人资料:

```
cat /sys/kernel/security/apparmor/profiles 
```

如何拒绝所有文件写入配置文件:

```
profile apparmor-deny-write flags=(attach_disconnected) {
  file,
  # Deny all file writes.
  deny /** w,
} 
```

如何拒绝写入`/proc`文件:

```
profile apparmor-deny-proc-write flags=(attach_disconnected) {
  file,
  # Deny all file writes.
  deny /proc/* w,
} 
```

如何拒绝重新装载根文件系统:

```
profile apparmor-deny-remount-root flags=(attach_disconnected) {

  # Deny all file writes.
  deny mount options=(ro, remount) -> /,
} 
```

如何检查配置文件状态:

```
aa-status 
```

配置文件加载模式

*   `Enforce`、监督和执行规则
*   `Complain`不会强制执行规则，但会将其记录为事件
*   `Unconfined`，不会强制或记录事件

如何检查配置文件是否有效:

```
apparmor_parser /etc/apparmor.d/root.add_data.sh 
```

如何禁用配置文件:

```
apparmor_parser -R /etc/apparmor.d/root.add_data.sh
ln -s /etc/apparmor.d/root.add_data.sh /etc/apparmor.d/disable/ 
```

如何生成个人资料并回答接下来的一系列问题:

```
aa-genprof /root/add_data.sh 
```

如何为命令生成配置文件:

```
aa-genprof curl 
```

如何从日志中禁用配置文件:

```
aa-logprof 
```

#### 如何在 Kubernetes 中使用 AppArmor

要将 AppArmor 与 Kubernetes 一起使用，必须满足以下先决条件:

*   Kubernetes 版本应该大于`1.4`
*   应该启用 AppArmor 内核模块
*   AppArmor 配置文件应该加载到内核中
*   应该支持容器运行时

立方结构中的范例使用:

```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper
  annotations:
    container.apparmor.security.beta.kubernetes.io/<container-name>: localhost/<profile-name>
spec:
  containers:
  - name: ubuntu-sleeper
    image: ubuntu
    command: ["sh", "-c", "echo 'Sleeping for an hour!' && sleep 1h"] 
```

**注意**:容器应该在包含 AppArmor 概要文件的节点中运行。

### Linux 功能

Linux 功能特性将作为`root`用户运行的进程可用的特权分成更小的特权组。这样，使用`root`特权运行的进程可以被限制为只获得执行其操作所需的最小权限。

Docker 支持 Linux 功能，作为 Docker run 命令的一部分:使用`--cap-add`和`--cap-drop`。默认情况下，容器启动时具有默认情况下允许的几个功能，并且可以删除。其他权限可以手动添加。

`--cap-add`和`--cap-drop`都支持 ALL 值，允许或删除所有功能。默认情况下，Docker 容器有 14 种功能。

*   内核< 2.2
    *   特权进程
    *   非特权过程
*   内核> = 2.2
    *   特权进程
        *   `CAP_CHOWN`
        *   `CAP_SYS_TIME`
        *   `CAP_SYS_BOOT`
        *   `CAP_NET_ADMIN`

[请参考本文档，了解 Linux 功能的完整列表](https://man7.org/linux/man-pages/man7/capabilities.7.html)。

如何检查命令需要哪些功能:

```
getcap /usr/bin/ping 
```

如何获得流程能力:

```
getpcaps <pid> 
```

如何添加安全功能:

```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper
spec:
  containers:
  - name: ubuntu-sleeper
    image: ubuntu
    command: ["sleep", "1000"]
    securityContext:
      capabilities:
        add: ["SYS_TIME"]
        drop: ["CHOWN"] 
```

## 如何准备考试

CKS 被认为是一个相当难的考试。但是根据我的经验，我认为，如果有足够好的练习，并且你理解了考试所涵盖的概念，在两个小时内是很容易应付的。

你肯定需要理解 Kubernetes 的基本概念。由于 CKS 的先决条件是通过 CKA 考试，所以在尝试 CKS 之前，你应该对 Kubernetes 及其功能有很好的了解。

此外，要通过 CKS，您需要理解容器编排带来的威胁和安全隐患。

CKS 考试的引入表明，集装箱的安全不容忽视。安全机制应该始终到位，以阻止对 Kubernetes 集群的攻击。

由于未受保护的 Kubernetes 仪表板，特斯拉加密货币黑客暴露了与 Kubernetes 或任何其他容器编排引擎相关的风险。 [Hackerone 有一个 Kubernetes bounty 页面](https://hackerone.com/kubernetes?type=team)，列出了标准 Kubernetes 集群中使用的源代码回购。

## 练习，练习，再练习！

实践是破解考试的关键，我个人发现 KodeKloud 和 Killer.sh 的考试模拟器对我帮助很大。

我没有像准备 CKA 考试那样多的时间来准备 CKS 的考试，但是我白天的工作是在 Kubernetes 上工作，所以我已经变得非常适应了。

实践是成功的关键。祝你考试顺利！

你可以在[faizanbashir . me](https://faizanbashir.me/rough-guide-to-qualifying-the-cks-exam-in-a-hurry)阅读这篇文章和其他文章