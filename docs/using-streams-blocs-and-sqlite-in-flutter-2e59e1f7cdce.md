# 如何在 Flutter 中使用流、块和 SQLite

> 原文：<https://www.freecodecamp.org/news/using-streams-blocs-and-sqlite-in-flutter-2e59e1f7cdce/>

最近，我一直在使用 Flutter 中的流和块从 SQLite 数据库中检索和显示数据。不可否认，我花了很长时间才理解它们。也就是说，我想回顾一下这一切，希望你在离开的时候能够自信地在自己的应用中使用它们。我将尽可能深入，尽可能简单地解释一切。

在这篇文章中，我们将从头到尾制作一个简单的应用程序，它利用了流、BLoCs 和 SQLite 数据库。这个应用程序将允许我们创建、修改和删除笔记。如果你还没有这样做，使用`flutter create APPNAME`创建一个新的准系统 Flutter 应用程序。如果你从头开始，理解这一切会容易得多。然后，稍后，将你学到的东西应用到你现有的应用中。

首要任务是创建一个类来处理表的创建和数据库查询。为了正确地做到这一点，我们需要在我们的`pubspec.yaml`文件中添加`sqflite`和`path_provider`作为依赖项。