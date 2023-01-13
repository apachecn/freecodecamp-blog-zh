# 如何用 DBDesigner 快速直观地创建数据库模式

> 原文：<https://www.freecodecamp.org/news/how-to-create-database-schemas-quickly-and-intuitively-with-dbdesigner-2f4adf79a29d/>

开发一个项目最重要的部分之一就是在头脑中有一个清晰的最终目标。我们需要知道该项目的目标受众，以及它将包括的功能。这意味着我们需要尽可能了解业务逻辑，然后根据需要实现所有特性。

在为应用程序创建数据库模式时，DBDesigner 是一个很好的工具。它允许你创建任意数量的表格(就我而言)。您可以将任何数据类型属性添加到您创建的任何表中。您还可以将某些属性用作外键。这样，当您分别设置主键和外键时，您可以看到您试图创建的数据库的表之间的关系。

你可以使用你的电子邮件创建许多项目，并在任何你想的时候回到他们身边。您还可以通过电子邮件邀请您的同事，让他们与您合作准备该方案。

当您有了数据库模式的初始版本后，可以将其导出为以下数据库技术的 SQL 脚本:PostgreSQL、SQLite、MySQL、MSSql 和 Oracle。

### **演示**

让我们首先创建一个新的数据库模式来演示它在实践中是如何工作的。

我们可以从一个新的空白模板开始，也可以从现有的模板中选择一个。

我们将在这里演示一个空模板，以便我们可以看到其中包含的一些功能。否则，您可能不会注意到他们使用现有的模板。

首先，我们需要创建一个新的模式。我们的例子使用了“通用”数据库类型，我们称之为“库”。

所以，我们需要去*图式*&g*t；*新建然后我们会看到一个新的窗口会弹出来:

![dKGQLARUrZNYsvQzfxx65r0lfDNqFe2fTsiu](img/182c6408b8f605638eb5727420811879.png)

这是之后我们应该看到的图像:

![3evdVmxCEoqv6npUjobnLH1v-1RJA7qTrIS5](img/0cd26061a15e0f1b458c2fa02aeef8bd.png)

然后，我们需要向我们的模式中添加新的表，这可以通过右键单击网格上的任意位置，并选择“Table”选项来完成:

![JdF6W8rIb5S1G6s0mItsmb0rRpBhRJaZIGIS](img/b3192f12e3197913352d2922147465d1.png)

现在我们需要向表中添加字段。我们所要做的就是进入*添加字段*，之后会出现一个新窗口。您可以在其中选择类型，还可以为新表列设置一些约束:

![SiFU1HH-YjlF8zl2LL3Bo-Lwo-LIqAzImh76](img/2d89bf75cf9355a8a6154fdd3cba659d.png)

这里我们可以看到添加几列后的样子:

![8JJt351B9ZkdPyeWdRDBgf3rcykpCz1c8h9R](img/d8d303fce1ec0f833da74dd8b7e22a6c.png)

然后我们可以添加表之间的关系。我们将以在两个表之间创建一个*多对多*关系为例:*作者*和*书籍。*为此，我们首先需要创建一个名为 *AuthorBooks、*的新表，我们在其中添加外键，分别引用 *Authors* 表和 *Books* 表:

![9QbhYFaIoE9xEMDnINK5-MsqMlMP85NCEOAa](img/50b64601076e3184749352cf5bc17b11.png)

这里我们有与*书*表的联系:

![RUzIA5zXMBg8rQufWIb43FIkfiBp8E0HR0H-](img/7134a21d063755bc6dabbef24968f3a2.png)

完成后，我们应该会看到一个类似如下的模式:

![kx7-qxvknuSAt85PbjOU37BP-XiHDSEe9vev](img/237559aff619ec8cf17a67b82b9d5cc7.png)

*dbdesigner* 的一个非常棒的特性是它给了你在网格中移动表格的灵活性:

![EstgpMYYjO7hwLR7ug5pwniVvUV54uif2KQn](img/33684fa7d53dc37596903de087be26e9.png)

在免费版本中，我们还可以与多达五个合作者共享该模式。我们只需转到*模式> Sha* re，一个新窗口就会弹出，如下所示:

![yMNJVmGA6UCGGRUGNFMlAKqxdtG97Vhcb9r1](img/0dd246256129ebc8db1ee689d97d8d79.png)

我们可以通过进入: *Export > Im* age 将该模式保存为图像。

我们还可以通过以下方式生成相应的 SQL 脚本:

![pQz9K1zslWpL3MFBUwCqqmEpmOvU7MVMPEHZ](img/45ba7443dfa6be90a511f0caa695da4e.png)![lcpubCJYZNuusWp1YfBjkmUxu7dyMYviPN62](img/c3bea4b95cacf3f56823d6536c257699.png)![ftc3d8sSxITbljXkA7z4gjJ2Tv-UcuhKkEb9](img/2c3e1fd60bdaac9ad68a81f09458de15.png)

我们还可以将自己的 SQL 导入到模式中，并以图形方式显示出来:

![imO4ID01-K3ANkILlUQ-hSNHvIq9R9JYJueT](img/7cbbf8c09cf18154ab71e27f637622b8.png)

### 结论

我在和一位同事进行结对编程时听说了这个工具，并发现它非常有用。我希望你也能从中受益。

[DBDesigner](https://www.dbdesigner.net/designer/schema/187386) 还有其他功能，我绝对推荐你试试。