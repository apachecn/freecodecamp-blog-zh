# 如何删除 Docker 中的图像和容器

> 原文：<https://www.freecodecamp.org/news/how-to-remove-images-in-docker/>

## **RMI 坞站**

`docker rmi`按 ID 删除图像。

要删除图像，首先需要列出所有图像，以获得图像 id、图像名称和其他详细信息。通过运行简单的命令`docker images -a`或`docker images`。

在这之后，你确定哪个图像想要删除，执行这个简单的命令`docker rmi <your-image-id>`。然后，您可以通过列出所有图像并进行检查来确认图像是否已被删除。

### **删除多个图像**

当您想要删除多个特定图像时，有一种方法可以一次删除多个图像。为此，首先简单地通过列出图像来获取图像 id，然后执行简单命令。

`docker rmi <your-image-id> <your-image-id> ...`

在命令中写入图像 id，后跟图像 id 之间的空格。

### **一次删除所有图像**

要删除所有图像，有一个简单的命令。`docker rmi $(docker images -q)`

在上面的命令中，有两个命令，第一个在`$()`中执行，是 shell 语法，返回在该语法中执行的结果。所以在这个`-q- is a option is used to provide to return the unique IDs,`中，$()返回图像 id 的结果，然后`docker rmi`删除所有这些图像。

#### **更多信息:**

*   [坞站 CLI 文件:rmi](https://docs.docker.com/engine/reference/commandline/rm/)

## **码头工人室**

`docker rm`按名称或 ID 删除容器。

当 Docker 容器正在运行时，您首先需要在删除它们之前停止它们。

*   停止所有正在运行的容器:`docker stop $(docker ps -a -q)`
*   删除所有停止的容器:`docker rm $(docker ps -a -q)`

### **移除多个容器**

通过向命令传递要删除的容器列表，可以停止和删除多个容器。shell 语法`$()`返回括号内执行的任何内容的结果。因此，您可以在其中创建容器列表，并将其传递给`stop`和`rm`命令。

### 这是 docker ps -a -q 的细目分类

*   `docker ps`列出容器
*   列出所有集装箱的选项，甚至是停止的集装箱。如果没有这个，它默认只列出正在运行的容器
*   `-q`quiet 选项只提供容器数字 id，而不是关于容器的整个信息表

#### **更多信息:**

*   [坞站 CLI 文件:rm](https://docs.docker.com/engine/reference/commandline/rm/)

## 关于 Docker 中图像的更多信息:

*   [Docker 图像向导](https://www.freecodecamp.org/news/docker-image-guide-how-to-remove-and-delete-docker-images-stop-containers-and-remove-all-volumes/)
*   Docker 图像存储在哪里？

## 关于 Docker 中容器的更多信息:

*   [如何自动化 Docker 容器部署](https://www.freecodecamp.org/news/automate-docker-container-deployment-via-maven-53a855e26d3e/)
*   [如何修复 Docker 容器漏洞](https://www.freecodecamp.org/news/how-to-find-and-fix-docker-container-vulnerabilities-in-2020/)

## 关于 Docker 的更多信息:

*   [码头工人入门指南](https://www.freecodecamp.org/news/a-beginners-guide-to-docker-how-to-create-your-first-docker-application-cc03de9b639f/)
*   [Docker DevOps 课程(免费视频课程)](https://www.freecodecamp.org/news/docker-devops-course/)
*   [Docker 101:从创建到部署](https://www.freecodecamp.org/news/docker-101-creation-to-deployment/)