# Ruby on Rails 解释

> 原文：<https://www.freecodecamp.org/news/ruby-on-rails-explained/>

[Ruby on Rails](http://rubyonrails.org/) 是一个基于 Ruby 语言构建的服务器端框架(gem)来制作网站。它包括构建 web 应用程序所需的一切，并且有一个很大的社区。

Ruby on Rails 是一个固执己见的框架，强调使用约定胜于配置(CoC)，并且不重复你自己(干)的实践。Rails 可以被最好地描述为模型-视图-控制器(MVC)框架，并为快速应用程序开发提供合理的缺省值和结构。最近，Rails 集成了一个 API 模块，使得 web 服务的创建更快更容易。

Ruby on Rails 是由 David Heinemeir Hansson 创建的，目前是它的第 6 个版本。

## **如何安装导轨**

Rails 的下载方式和其他 Ruby gem 一样:使用`gem install`命令。在我们下载它之前，我们需要[下载 Ruby](https://www.ruby-lang.org/) 。之后，我们离使用 Ruby on Rails 只差三个字了:

```
$ gem install rails
```

Rails 将 sqlite3 作为默认数据库，它是磁盘上的一个简单文件。如果你想使用更强大的东西，你需要安装 MySQL 或 PostgreSQL。

## **如何创建一个 Rails 应用程序**

1.  安装 Ruby on Rails 之后，创建一个全新的应用程序就非常简单了，我们只需要三个词:

```
$ rails new your_application_name
```

2.如果你想使用 MySQL:

```
$ rails new <application_name> -d mysql
```

3.如果你想使用 Postgres:

```
$ rails new <application_name> -d postgresql
```

4.该命令将创建一个文件夹，其名称为您在上一个命令中通知的*您的*应用程序*名称*。下一步是转到您刚刚创建的新目录:

```
$ cd your_application_name
```

5.在运行应用程序之前，获取必要的 gem 和软件包:

```
$ bundle install
```

6.运行 rails 服务器并查看是否一切正常也很快:

```
$ rails server
```

这再简单不过了！嗯，这实际上并不是 100%正确，我们可以通过将`rails server`命令简化为:

```
$ rails s
```

7.现在用你喜欢的浏览器，进入`http://localhost:3000`，你会看到:“耶！你在铁轨上！”

### **创建 Rails 应用程序的替代方法**

1.  创建新目录:

```
$ mkdir <application_name>
```

2.进入新目录:

```
$ cd <application_name>
```

3.使用 Unix 点符号创建 Rails 应用程序。这导致将目录的名称分配给新应用程序:

```
$ rails new .
```

4.开始探索您刚刚创建的应用程序的框架。要查看有用的文件夹结构表，请查看【Rails 入门。

## **约定优于配置**

*约定优于配置*意味着开发者只需要指定应用程序的非常规方面。比如模型中有一个类`Sale`，数据库中对应的表默认叫做`sales`。只有当一个人背离了这个约定，比如称表为“售出的产品”，开发人员才需要编写关于这些名称的代码。一般来说，Ruby on Rails 的约定会导致更少的代码和重复。

## **什么是 MVC？**

模型(活动记录)包含业务逻辑，并与数据库交互。视图(动作视图)所有的 HTML 文件和结构。控制器(动作控制器)与视图和模型交互，以指导应用程序的动作。

## **干——不要重复自己**

*不要重复自己*表示信息位于一个单一的、明确的位置。例如，使用 Rails 的 ActiveRecord 模块，开发人员不需要在类定义中指定数据库列名。相反，Ruby on Rails 可以根据类名从数据库中检索这些信息。

## **Ruby on Rails 是开源的**

它不仅可以免费使用，你还可以帮助它变得更好。超过 4500 人已经为 [Rails](https://github.com/rails/rails) 贡献了代码。成为他们中的一员比你想象的要容易。