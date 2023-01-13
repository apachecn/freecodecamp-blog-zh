# 用免费的 19 小时 Python 课程创建 API

> 原文：<https://www.freecodecamp.org/news/creating-apis-with-python-free-19-hour-course/>

很多 API 教程只是教绝对最小值。但是生产就绪的 API 比大多数教程所教授的要复杂得多。

我们刚刚在 freeCodeCamp.org YouTube 频道上发布了一个 19 小时的大型课程，将教你如何使用 Python 和 FastAPI 库构建一个成熟的 API

Sanjeev Thiyagarajan 开发了这个课程。Sanjeev 是一个很棒的老师，他真的知道如何给初学者分解东西。

本课程中构建的 API 适用于社交媒体类型的应用程序，用户可以创建/阅读/删除/更新帖子，就像其他用户的帖子一样。它包括用户注册和认证。

首先，您将学习 API 设计的基础知识，包括路由、序列化/反序列化、模式验证和模型。您还将了解如何设置和使用 SQL 数据库。

然后，您将学习如何使用原始 SQL 查询和 SqlAlchemy ORM 将 API 与数据库集成。Postgres 被用作数据库，但你学到的一切将适用于几乎任何其他 SQL 数据库。

接下来，您将学习如何使用 pytest 库为应用程序设置测试。您将建立一个测试数据库，并执行大量的集成测试。

创建 API 之后，您将学习如何使用两种不同的方法部署 API。第一个是部署在 Ubuntu 机器上，第二个是部署到 Heroku。您甚至将学习如何对应用程序进行分类。

最后，您将学习如何使用 GitHub 操作构建 CI/CD 管道。

以下是本综合课程涵盖的完整主题列表:

### 第 1 部分:简介

*   课程项目
*   课程介绍
*   课程项目概述

### 第 2 部分:设置和安装

*   Mac Python 安装
*   Mac 与代码安装和设置
*   Windows Python 安装
*   Windows VS 代码安装和设置
*   Python 虚拟环境基础
*   windows 上的虚拟环境
*   Mac 上的虚拟环境

### 第 3 部分:FastAPI

*   安装带有 pip 的依赖项
*   启动快速 API
*   路径操作
*   路径操作顺序(是的，这很重要)
*   邮递员介绍
*   HTTP 发布请求
*   用 Pydantic 进行模式验证
*   CRUD 操作
*   将帖子存储在数组中
*   创建帖子
*   邮递员收集和保存请求
*   检索一个帖子
*   路径顺序很重要
*   更改响应状态代码
*   删除帖子
*   更新帖子
*   自动文档
*   Python 包

### 第 4 部分:数据库

*   数据库介绍
*   Postgres Windows 安装
*   Postgres Mac 安装
*   数据库模式和表
*   使用 PgAdmin GUI 管理 Postgres
*   您的第一个 SQL 查询
*   使用“where”关键字过滤结果
*   SQL 运算符
*   在关键字中
*   LIKE 关键字的模式匹配
*   订购结果
*   极限和偏移
*   插入数据
*   删除数据
*   更新数据

### 第 5 部分:Python +原始 SQL

*   设置应用程序数据库
*   使用 Python 连接到数据库
*   正在检索帖子
*   正在创建帖子
*   获得一个职位
*   删除帖子
*   更新帖子

### 第六节:ORMs

*   ORM 简介
*   SQLALCHEMY 设置
*   添加 CreatedAt 列
*   获取所有帖子
*   创建帖子
*   按 ID 获取帖子
*   删除帖子
*   更新帖子

### 第 7 节:Pydantic 模型

*   Pydantic 与 ORM 模型
*   学究式模型深度潜水
*   响应模型

### 第 8 部分:身份验证和用户

*   创建用户表
*   用户注册路径操作
*   哈希用户密码
*   折射镜散列逻辑
*   按 ID 获取用户
*   FastAPI 路由器
*   路由器前缀
*   路由器标签
*   JWT 代币基础
*   登录过程
*   创建令牌
*   OAuth2 PasswordRequestForm
*   验证用户已登录
*   修复 bug
*   保护路线
*   测试过期令牌
*   在受保护的路由中提取用户
*   邮递员高级功能

### 第 9 节:关系

*   SQL 关系基础
*   Postgres 外键
*   SQLAlchemy 外键
*   更新发布模式以包括用户
*   创建新帖子时分配所有者 id
*   仅删除和更新您自己的帖子
*   仅检索登录用户的帖子
*   Sqlalchemy 关系
*   查询参数
*   清理我们的 main.py 文件
*   环境变量

### 第 10 部分:投票/类似系统

*   投票/喜欢理论
*   投票表
*   投票 Sqlalchemy
*   投票路线
*   sql 连接
*   SqlAlchemy 中的联接
*   获得一个带有连接的帖子

### 第 11 节:使用 Alembic 进行数据库迁移

*   什么是数据库迁移工具
*   Alembic 设置
*   阿拉伯语第一次修订
*   Alembic 回滚数据库模式
*   Alembic 完成了模式的其余部分
*   禁用 SqlAlchemy 创建引擎

### 第 12 部分:部署前清单

*   什么是 CORS？？？？？
*   Git 先决条件
*   Git Install
*   开源代码库

### 第 13 节:部署 Heroku

*   Heroku 简介
*   创建 Heroku 应用程序
*   英雄库配置文件
*   添加 Postgres 数据库
*   Heroku 的环境变量
*   Heroku Postgres 实例上的 Alembic 迁移
*   推动变更为生产

### 第 14 节:部署 Ubuntu

*   创建一个 Ubuntu 虚拟机
*   更新包
*   安装 Python
*   安装 Postgres &设置密码
*   Postgres 配置
*   创建新用户并设置 python 环境
*   环境变量
*   生产数据库上的迁移
*   格尼科恩
*   创建 Systemd 服务
*   NGINX
*   设置域名
*   SSL/HTTPS
*   NGINX 使能
*   防火墙
*   将代码变更推向生产

### 第 15 节:码头工人

*   Dockerfile
*   复合坞站
*   邮政集装箱
*   绑定安装
*   Dockerhub
*   生产与开发

### 第 16 节:测试

*   测试简介
*   编写您的第一个测试
*   -s & -v 标志
*   测试更多功能
*   用参数表示
*   测试类
*   固定装置
*   组合夹具+参数化
*   测试异常
*   FastAPI 测试客户端
*   Pytest flags
*   测试创建用户
*   设置测试数据库
*   每次测试后创建和销毁数据库
*   更多设备来处理数据库交互
*   路径中的尾随斜线
*   夹具范围
*   测试用户夹具
*   测试/验证令牌
*   Conftest.py
*   登录测试失败
*   获取所有帖子测试
*   用于创建测试立柱的立柱夹具
*   未经授权的获取帖子测试
*   进行一次后期测试
*   创建后期测试
*   删除后测试
*   更新帖子
*   投票测试

### 第 17 节:CI/CD 管道

*   CI/CD 简介
*   Github 操作
*   创造就业机会
*   设置 python/dependencies/pytest
*   环境变量
*   秘密 Github
*   测试数据库
*   构建 Docker 图像
*   部署到 Heroku
*   管道测试失败
*   部署到 Ubuntu

观看 freeCodeCamp.org YouTube 频道的全部课程(19 小时观看)。

[https://www.youtube.com/embed/0sOvCWFmrtA?feature=oembed](https://www.youtube.com/embed/0sOvCWFmrtA?feature=oembed)