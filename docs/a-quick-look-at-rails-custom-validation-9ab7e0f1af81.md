# 快速浏览 Rails 定制验证

> 原文：<https://www.freecodecamp.org/news/a-quick-look-at-rails-custom-validation-9ab7e0f1af81/>

雷蒙德·福佑

# 快速浏览 Rails 定制验证

![pK-gMzy3meGp1rdP21xe1TUB9Dde408OFq4S](img/55031a5e7b098d0b8d120a234af6e555.png)

validation

我最近开始使用 Ruby(差不多 2 个月了)和 Ruby on Rails(3 周多一点)。使用 Rails 的活动记录框架是我最喜欢 Ruby on Rails 的地方之一。在本帖中，我们将关注活动记录中的验证，特别是自定义记录。在我们进入正题之前，先简单介绍一下活动记录。

活动记录是构成 Ruby on Rails 的核心宝石之一。它是框架中处理数据库的部分。

它是一个 ORM(对象关系映射)框架，允许你使用纯 ruby 为数据库构建一个模式，它基于由 Martin Fowler 描述的活动记录设计模式。因此，有了活动记录，您就可以使用 ruby 创建数据库、创建表、存储、检索和删除数据，ruby 可以在幕后转换为 SQL。

### 快速介绍

假设我们有一个学生模型，其属性为 first name 和 last name。要使用活动记录，我们只需扩展**应用记录**，当我们运行`rails db:migrate` 时，它会给出它的 SQL 语句。

为了与数据库交互，我们使用从 ApplicationRecord 超类继承的方法。

它还支持关联和其他数据库内容。

关于活动记录的详细介绍，请查看官方的 ruby on rails 指南。

### 确认

因为我们为用户而不是我们自己编写 web 应用程序，所以我们不能保证用户总是将有效的数据输入到数据库中。为了加强这一点，活动记录为我们提供了一个小型验证框架，确保数据的存在、某些字段的唯一性等等。

让我们看看上面的学生表。我们不希望创建一个没有名字或姓氏的用户，目前这是可能的。为了减轻这种情况，我们只需要修改我们的学生类，如下所示:

通过这种修改，当您创建一个没有名字或姓氏属性的学生实例时，它是一个无效的学生，活动记录不会将其保存到数据库中。

活动记录还为我们提供了检查数据有效或无效的方法:

有了它，我们甚至不必尝试保存数据。

除了防止数据被持久化之外，Active Record 还提供了一个错误列表，其中保存了验证失败的属性和向用户显示的用户友好消息。可以访问这些错误，如下面的代码片段所示。

关于验证还有很多内容，但这不是本文的主题。要深入了解，您可以从 ruby on rails 指南关于验证的章节中获得深入的解释。

### 自定义验证

有时，我们可能希望使用某些验证，而不仅仅是确保属性、长度、惟一性或活动记录提供的任何帮助。幸运的是，活动记录允许我们定义自己的定制验证，这是本文的重点。

所以，让我们说一下我们的学生模型，我们有一个强制性的学生注册号列。它必须从注册表中填写(我知道这可以自动生成)，应该总是从注册年份开始。现在，活动记录不提供这种现成的验证，但它使我们有可能定义和使用它。

主要有两种方法来定义您自己的验证逻辑:

*   自定义验证程序
*   自定义方法

#### **自定义验证器**

要使用自定义验证器进行验证，您只需在一个类中定义您的验证逻辑，该类扩展了 **ActiveModel::Validator** 并实现了 validate 方法，该方法将待验证的记录作为其参数。

如果验证失败，它会将属性及其错误信息添加到错误数组中。因此，在我们的例子中，我们将使用 RegNumValidator，如下所示:

为了在学生模型中使用这个验证器，我们使用了`validates_with`方法:

这样，当用户试图用错误的注册号创建学生时，记录创建会失败，并且会显示一条错误消息。

#### **自定义方法**

要使用定制方法进行验证，您只需要在模型类中定义一个用于验证的方法，并像调用任何内置验证一样调用它——使用`validate`。使用与上面相同的逻辑，我们的模型将如下所示:

### 结论

我希望这篇文章已经为您提供了开始探索活动记录验证和定制验证的必要知识。只要您有一个不属于现有活动记录验证 API 的验证规则，您就可以自己编写一个。

[**活动记录验证— Ruby on Rails 指南**](https://guides.rubyonrails.org/active_record_validations.html)
[*验证用于确保只有有效的数据保存到您的数据库中。例如，重要的是……*guides.rubyonrails.org](https://guides.rubyonrails.org/active_record_validations.html)