# 不要用 Mac 系统的 Ruby——用这个代替

> 原文：<https://www.freecodecamp.org/news/do-not-use-mac-system-ruby-do-this-instead/>

可能有人曾经跟你说过:“不要用系统 Ruby。”这是个好建议，但为什么呢？让我们找出答案。

## 你有哪颗红宝石？

MacOS 预装了一个“系统 Ruby”。

使用`which`命令查看 Ruby 的安装位置:

```
$ which ruby
/usr/bin/ruby 
```

如果看到`/usr/bin/ruby`，是预装的 macOS 系统 Ruby。

使用系统 Ruby 运行 sysadmin 脚本是没问题的，只要你不试图通过更新或者添加 gem 来改变系统 Ruby。

但是当你用 Ruby 开发项目的时候，你不想用它。

## 用于开发的 Ruby

对于用 Ruby 开发项目，你应该[安装 Ruby with Homebrew](https://mac.install.guide/ruby/12.html) 或者使用一个版本管理器，比如 asdf、chruby、rbenv 或者 rvm。

如果您正在处理多个项目，并且不能一次更新所有项目，版本管理器会有所帮助。关于版本管理器的比较和安装 Ruby 的最佳方法，请参阅我的文章[在 Mac 上安装 Ruby](https://mac.install.guide/ruby/index.html)。

但是为什么不用 macOS 默认的 Ruby 呢？让我们来看看为什么使用 Mac 默认的 Ruby 进行开发不是一个好主意。

### 宝石安装麻烦

RubyGems 是现成的软件库，使得用 Ruby 进行开发变得简单而有趣。大多数 Ruby 项目至少使用一些 gem。

如果使用 Mac 系统 Ruby，运行`gem install`会尝试将 gems 保存到系统 Ruby 目录`/Library/Ruby/Gems/2.6.0`。该目录归系统超级用户`root`所有。普通用户不允许写入它(你真的不应该改变这个文件夹)。

如果你试图安装一个 gem，例如`gem install rails`，你会得到一个权限错误:

```
ERROR: While executing gem ... (Gem::FilePermissionError)
You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory 
```

### 它违反了系统安全

基于 Unix 的系统功能强大，所以有一个变通办法。您可以作为超级用户安装 gems 来覆盖权限限制。但是不要这样！

```
$ sudo gem install rails 
```

任何时候你要跑的时候，你都应该停下来问问自己是否会搬起石头砸自己的脚。

在这种情况下，您需要`sudo`,因为您正在修改由操作系统管理的系统文件。不要这样做！您可能会使系统处于崩溃或受损状态。更糟糕的是，gem 可能包含恶意代码，篡改你的计算机。

### 宝石管理

有经验的开发人员使用 [Bundler](https://bundler.io/) 来安装 gem 并管理它们的依赖关系。

假设您的项目使用不同版本的 gem(也许在您的项目之间有一个新的 gem 版本)。或者你项目中的两个不同的 gem 依赖于一个依赖 gem 的不同版本。

Bundler 使用项目目录中的 gem 文件来跟踪您需要的 gem。如果你使用`sudo`来安装 gems 和系统 Ruby，你会在系统 Ruby 目录中得到一堆不兼容的 gems。

您可以通过使用一个命令安装 Bundler 来解决系统权限问题，该命令使用您的 gems 主目录。但是安装 Ruby with Homebrew 或者使用版本管理器并使用安装的 Bundler 更容易，这将正确地设置您的本地开发环境。

### 使用最新的红宝石

当您开始一个项目时，使用最新的 Ruby 版本(在撰写本文时是 3.0)。

macOS Catalina 或者 Big Sur 的系统 Ruby 是 Ruby 2.6.3，比较老。如果你刚开始使用 Ruby，用 Homebrew 安装，用 Ruby 3.0 做一个项目。当你开始构建另一个项目时，可能是时候安装一个版本管理器了，这样你就可以用不同的 Ruby 版本来处理项目了。

## 大苏尔后的马科斯

MacOS Big Sur 现在是当前版本。[苹果表示](https://developer.apple.com/documentation/macos-release-notes/macos-catalina-10_15-release-notes):

> macOS 中包含 Python、Ruby 和 Perl 等脚本语言运行时，以便与传统软件兼容。默认情况下，未来版本的 macOS 将不包含脚本语言运行时，可能会要求您安装额外的软件包。”

如果你在 2021 年底读到这篇文章，系统 Ruby 可能已经不在了。如果没有，请准备好安装 Ruby with Homebrew 或一个版本管理器。

## 享受 Ruby

对于计划用 Rails 构建 web 应用程序的开发人员，我写了一个指南，[在 Mac 上安装 Rails](https://learn-rails.com/install-rails-mac/index.html)，它超出了[在 Mac 上安装 Ruby](https://mac.install.guide/ruby/index.html)的范围，展示了如何选择一个既能与 Node 一起工作又能与 Ruby 一起工作的版本管理器。

享受用 Ruby 编码的乐趣吧！毕竟，它被认为是一种致力于程序员快乐的语言。但是要记住，系统 Ruby 是为 macOS 准备的，不是为你准备的。