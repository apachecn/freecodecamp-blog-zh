# 如何管理你的 Ruby 版本

> 原文：<https://www.freecodecamp.org/news/how-to-manage-your-ruby-versions/>

## **Ruby 随着时间的推移发生了变化**

自 20 世纪 90 年代以来，Ruby 一直在不断发展。像许多语言一样，不同版本之间也有语法变化。这意味着清楚你的代码期望哪个 Ruby 版本是很重要的。

可能最明显的变化来自 Ruby 1.9。以前，我们像这样编写散列:

```
 { :one => 1, :two => 2, :three => 3 }
```

“hashrocket”操作符(`=>`)的这种用法如此普遍，以至于 Ruby 1.9 提供了一种简写方式:

```
 { one: 1, two: 2, three: 3 }
```

这个旧代码可以在任何版本上运行，但是新语法只能在 Ruby 1.9+上运行。

## 这是如何引起问题的？

例如，您可能已经决定使用一个内部依赖于 Ruby 1.9 特性的 Gem。这意味着您的项目现在也依赖于 Ruby 1.9 的特性。

如果你不指定你的项目需要哪个版本的 Ruby，当代码在一台机器上工作，而在另一台机器上不工作的时候，会非常混乱。

和大多数语言一样，指定代码期望的 Ruby 版本被认为是一个好的做法。这使得在开发机器上管理多个项目变得更加容易，每个项目都需要不同版本的 Ruby。

## 如何指定我的 Ruby 版本？

有几个工具在这方面很流行，但是两者都同意共享一个公共文件。许多 Ruby(或 Rails)项目会包含一个简单的`.ruby-version`文件，它简单地指定了一个版本号，例如:

```
2.4.2
```

帮助您管理 Ruby 版本的流行工具有:

*   [Ruby 版本管理器(RVM)](https://rvm.io/)
*   [rbenv](https://github.com/rbenv/rbenv)

让我们看看 RVM。

### **使用 RVM**

RVM 通常安装在 Linux、Unix 或 MacOS 机器上([链接](https://rvm.io/))。这非常方便，因为它与`cd`(`c`hange`d`I 目录)命令挂钩。因此，当您转移到一个新项目时，您的`.ruby-version`会被自动读取，并且在您开始工作之前，您会自动切换到正确的 Ruby 版本。

例如，您可能有这样的序列:

```
% cd ~/projects/older-project
% ruby --version

ruby 2.3.5p376 (2017-09-14 revision 59905) [x86_64-darwin16]

% cd ~/projects/newer-project
% ruby --version

ruby 2.4.2p198 (2017-09-14 revision 59899) [x86_64-darwin16]
```

(这些例子来自 MacOS 机器)。

## 关于 Ruby 的其他信息:

*   【Ruby 面向对象编程简介
*   [你应该知道的最常见的 Ruby 数组方法](https://www.freecodecamp.org/news/p/62edc7d6-1ec8-4e6b-ab42-51136a3b7073/)