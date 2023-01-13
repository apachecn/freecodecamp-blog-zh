# 如何在 PostgreSQL 中调度作业

> 原文：<https://www.freecodecamp.org/news/how-to-schedule-a-job-in-postgresql/>

日程安排允许你自动完成事情，这样你就不必实时去做了。

在本文中，我们将了解如何在 PostgreSQL 中调度作业。我们将使用 pgAgent，这是 PostgreSQL 的一个作业调度代理。

# 如何安装 PostgreSQL 和堆栈生成器

您可以使用 Stack Builder 安装 pgAgent。

从[官网](https://www.postgresql.org/download/)安装 PostreSQL。这将与安装程序一起下载堆栈生成器。

如果您已经安装了 PostgreSQL，您可以下载安装程序并运行 Stack Builder(如果您还没有安装的话)。

PostgreSQL 安装完成后，Stack Builder 就会运行。我用的是 PostgreSQL14 和 pgAdmin4。

# 如何安装 pgAgent

当您运行 Stack Builder 时，它将首先打开一个欢迎向导。

![Screenshot-2022-07-10-163841_auto_x2_auto_x2_colored_toned_light_ai](img/682ecc9cd99846a0570baf66e86072ca.png)

如果您安装了多个 PostgreSQL 版本，您将选择一个来安装 pgAgent。

![Screenshot-2022-07-10-163907_auto_x2_colored_toned_light_ai_auto_x2_colored_toned_light_ai](img/b580969f64c25942732b887dceca38dd.png)

在*附件、工具和实用程序*下，您会找到 pgAgent。选中复选框进行安装。

![Screenshot-2022-07-10-163926_auto_x2_colored_toned_light_ai--1-_auto_x2_colored_toned_light_ai](img/7c8f0c6842ff709e332dacea7fa339c4.png)

接下来，它将要求您选择要安装 pgAgent 的目录。

![Screenshot-2022-07-10-163956](img/f4b9e538972333e72216ada517931f4c.png)

然后，Stack Builder 将打开 pgAgent 安装向导。

![Screenshot-2022-07-10-164018](img/a2d0b25430472296075c20748e93d385.png)

您可以在这里选择是否要在升级模式下安装。如果您不想在升级时自动更改脚本，可以选中该框。

![Screenshot-2022-07-10-164038](img/6ee64d534212c67acf886f5aaa0b3bb9.png)

在 *PostgreSQL 安装详细信息*向导中，提供您在安装 PostgreSQL 时输入的用户名和密码。

![Screenshot-2022-07-10-164125](img/8bf49e927923c402d8f541654d4b641c.png)

如果您输入不正确的细节，它将抛出一个连接错误。所以一定要记住这些细节。

> ***注意:*** 使用您在此阶段提供的用户名和密码登录 PostgreSQL 查看 *pgAgent 作业*。

![Screenshot-2022-07-10-164233](img/e0f0cf23b5f1bb6c66dfeb24b6cf2c87.png)

添加这些细节后，设置开始:

![Screenshot-2022-07-10-164250](img/5102683d34b833d5e4159eaaf476946a.png)

需要几秒钟才能完成。

![Screenshot-2022-07-10-164304](img/a2f8ff5eab8b6a9bb1f03a5eb616397e.png)

点击最后的*完成*按钮。

![Screenshot-2022-07-10-164320](img/510a1c263f654ace2eaa5520d2d50722.png)

Stack Builder 还会显示一个*安装完成*向导。它有安装和卸载实用程序的说明。

一旦安装了 Stack Builder，您只需运行它来安装其他实用程序。要卸载它们，您需要使用控制面板。

![Screenshot-2022-07-10-190229](img/a68a9ff22925ac94a8098e50be52ad77.png)

*pgAgent 作业*将在仪表板左侧的浏览器树中显示。

![Screenshot-2022-07-10-152307-1](img/c1cd73e658be734f834f160aeddff0dd.png)

上面你可以看到浏览器树的特写视图。

# 如何在 pgAgent 中创建作业

要创建一个新任务，右击 *pgAgent 任务*按钮并点击*创建*。

![Screenshot-2022-07-10-152329-1](img/7a845a46a6a4004b572d184c77db8bec.png)

你会看到一个菜单，点击*创建* > *pgAgent 作业*。

![Screenshot-2022-07-10-191902](img/8e76bbf8889d7248f59ccf489ada558b.png)

*创建 pgAgent 对话框*有四个选项卡。

第一个是*通用*标签。您可以在这里输入作业的名称并选择一个类别。

*类别*仅用于内部分类，这不会影响您的工作运行。您可以根据工作职能选择一个。因为我想将数据导出到 CSV，所以我将选择*数据导出*类别。

![Screenshot-2022-07-10-152531-1](img/8a51067a1e7b02bbe7b941c0b459054e.png)

接下来，我们点击*创建 pgAgent* 对话框中的*步骤*选项卡。在方框的右上角，您会看到一个+号。单击它以添加新行。

![Screenshot-2022-07-10-153345-1](img/f87fad31a3b28de79747eea023f8cd55.png)

*步骤*页签有两个部分:*通用*和*代码*。

在*常规*选项卡中:

1.  添加步骤的名称。
2.  接下来，你*启用*或*禁用*该步骤。只有启用了该步骤，您的作业才会运行。
3.  根据你的工作是*本地*还是*远程*，你可以选择*连接类型*。我将选择远程连接。
4.  远程连接允许您手动添加连接字符串。语法应该类似于 [libq 连接字符串](https://www.postgresql.org/docs/current/libpq-connect.html#LIBPQ-CONNSTRING)。我将以相同的格式添加我的连接详细信息:`host=localhost port=5432 dbname=postgres`
5.  在*出错时*选择框中，您可以选择出错时应该发生的情况。我选择了失败的工作。
6.  最后可以评论一下步骤。然后保存更改。

接下来是*步骤*选项卡中的*代码*部分。
![Screenshot-2022-07-10-155158-1](img/050aace57b4c7229afcab284551bf8da.png)

因为我想从一个视图中导出数据，所以我将调用这个视图并请求它导出文件。代码将:
`COPY (select * from acc_view) TO E'C:\\test-data\\try.csv';`

添加代码后，我将保存更改。

![Screenshot-2022-07-10-153614-1](img/01a1d2ed21f1cdf5f0bbb782160e6064.png)

我们现在准备安排作业。在*计划*选项卡中，我们添加了*开始日期时间*和*结束日期时间*以开始和结束作业。

![Screenshot-2022-07-10-163332-1](img/b69ffbda4c991d5841bc6984ff6dc0e7.png)

*SQL* 是最后一个标签页。它显示了 GUI 生成的代码。如果您想要动态调度作业，您必须执行此处显示的过程代码。

# 如何在 pgAgent 中查看已创建的作业

一旦创建了新作业，它将显示在浏览器树中的 *pgAgent 作业*下。

![Screenshot-2022-07-10-163417-1](img/182de356a2180e0103db8a1dd3ef1c20.png)

它的*时间表*和*步骤*将在您延长作业时显示。

![Screenshot-2022-07-10-163540-1](img/57a60a9f9ce9546d0a30872a5923087b.png)

要查看作业是否被执行(是失败还是成功)，您可以通过作业的名称选择作业，然后单击仪表板中的*统计信息*选项卡。在这里，您可以查看作业的执行次数、开始和结束时间、状态和 id。在*状态*栏中 *s* 表示成功 *f* 表示失败。

![Screenshot-2022-07-10-163639-1](img/f387297cd78549bb3f6fc28674a82b4d.png)

要调试作业失败的原因，您只需在浏览器树中点击*步骤*下的步骤名称，然后点击仪表板上的*统计信息*。在*输出*栏中，您可以看到作业失败的原因。

在我的例子中，它无法访问我试图将数据复制到的目录。一旦我更改了路径，我的作业就成功执行了(注意第一行)。

# 如何在 pgAgent 中编辑作业

要在 pgAgent 中编辑作业，您可以选择该作业并点击仪表板上的*属性*选项卡。

![Screenshot-2022-07-10-201542](img/432dbf68333177ce40ecd17a9a14a021.png)

单击左上角的铅笔图标，将会打开一个向导，您可以在其中编辑所有细节。

# 结论

在您的代码中创建调度器并不总是可行的，但是当它是一个选项时，它会非常有用。

PostgreSQL 的一个强大功能是任务调度和导出 CSV 格式的数据。我将在下一个教程中尝试解释如何动态创建作业。快乐学习。