# 如何在本地机器上构建和部署 AWS 应用程序

> 原文：<https://www.freecodecamp.org/news/how-to-build-and-deploy-aws-applications-on-local-machine/>

在我以前的文章中，我谈到了使用 [Chalice](https://www.freecodecamp.org/news/how-to-build-a-serverless-application-using-aws-chalice/) 和 [SAM](https://www.freecodecamp.org/news/how-to-build-a-serverless-application-using-aws-sam/) 在 AWS 上构建和部署无服务器应用程序。

这些是快速有趣的项目，利用了无服务器计算的能力，让我们在几分钟内就可以在 AWS 上部署无服务器应用程序。

但是，如果没有 AWS 帐户，许多人无法完全利用这些教程。设置 AWS 帐户和配置开发环境可能非常耗时。而且它还会导致不必要的费用(如果您没有正确配置的话)。

在本文中，我将带您完成构建和部署无服务器应用程序所需的步骤，而无需创建和设置实际的 AWS 帐户。

这一次，我们将使用 Amazon API Gateway、AWS Lambda 和 Amazon DynamoDB 创建一个样本 Pet Store 应用程序。这个应用程序将有 API 来添加一个新的宠物和获取可用宠物的列表。

## 先决条件

在本教程中，我们将使用 AWS SAM。您可以通过遵循上一篇[文章中的指导来安装和配置 SAM。](https://www.freecodecamp.org/news/how-to-build-a-serverless-application-using-aws-sam/)

## 如何创建项目

运行`sam-init`命令创建一个新项目。这将在当前目录下创建一个`pet-store`文件夹。

```
sam init -r java11 -d maven --app-template pet-store -n pet-store
```

关于传递的参数的更多细节，请参考之前的[文章](https://www.freecodecamp.org/news/how-to-build-a-serverless-application-using-aws-sam/)。

让我们更改`pom.xml`，将模块的名称更新为`PetStore`，并使用`Java 11`代替`Java 8`。