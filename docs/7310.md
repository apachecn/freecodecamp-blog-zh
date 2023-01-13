# 如何使用 mysql-backup4j 以编程方式备份 MySQL 数据库

> 原文：<https://www.freecodecamp.org/news/how-to-backup-mysql-database-programmatically-using-mysql-backup4j-2b53a1cbf9b2/>

by Seun Matt

# 如何使用 mysql-backup4j 以编程方式备份 MySQL 数据库

![S-HbFqVU37hxEprLvEnqQoF1ddrV9tNUsbvN](img/b97723dd61c5d9d1273c72cc4acfdc42.png)

在本文中，我们将关注 [mysql-backup4j](https://github.com/SeunMatt/mysql-backup4j) ，这是一个非常灵活的 Java 库，我们可以用它来定期备份我们的数据库。

一旦我们的应用程序投入生产，我们不能没有一个及时的备份以防万一。通常，如果我们不得不一直手动触发这个过程，那么这个过程就会变得有些困难。

想象一下这样一个场景，我们有自动化和手动的数据库备份流程，这就是我们将要做的。

### 2.依赖安装

让我们将依赖项添加到项目的 *pom.xml* 中:

```
<dependency>   <groupId>com.smattme</groupId>   <artifactId>mysql-backup4j</artifactId>   <version>1.0.1</version></dependency>
```

最新版本可以在这里找到[。](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.smattme%22%20a%3A%22mysql-backup4j%22)

### 3.以编程方式导出 MySQL 数据库

用 mysql-backup4j 以编程方式导出 MySQL 数据库非常简单。我们只需要实例化它并传递给它一个 Java *Properties* 对象，该对象具有正确的配置属性集:

```
//required properties for exporting of db Properties properties = new Properties(); properties.setProperty(MysqlExportService.DB_NAME, "database-name"); properties.setProperty(MysqlExportService.DB_USERNAME, "root"); properties.setProperty(MysqlExportService.DB_PASSWORD, "root"); 
```

```
//properties relating to email config properties.setProperty(MysqlExportService.EMAIL_HOST, "smtp.mailtrap.io"); properties.setProperty(MysqlExportService.EMAIL_PORT, "25"); properties.setProperty(MysqlExportService.EMAIL_USERNAME, "mailtrap-username"); properties.setProperty(MysqlExportService.EMAIL_PASSWORD, "mailtrap-password");properties.setProperty(MysqlExportService.EMAIL_FROM, "test@smattme.com"); properties.setProperty(MysqlExportService.EMAIL_TO, "backup@smattme.com"); 
```

```
//set the outputs temp dir properties.setProperty(MysqlExportService.TEMP_DIR, new File("external").getPath()); 
```

```
MysqlExportService mysqlExportService = new MysqlExportService(properties); mysqlExportService.export();
```

根据上面的代码片段，我们创建了一个新的 *Properties* 对象，然后添加了数据库连接所需的属性，它们是:数据库名称、用户名和密码。

**只提供这些属性将使 *mysql-backup4j* 假定数据库运行在端口 *3306*** *的 *localhost* 上。因此，它将尝试使用这些值以及提供的用户名和密码进行连接。*

此时，库可以导出我们的数据库并生成一个包含 SQL 转储文件的 zip 文件。该文件以以下格式命名:

```
randomstring_day_month_year_hour_minute_seconds_database_name_dump.zip
```

由于我们已经提供了完整的电子邮件凭据作为用于配置它的属性的一部分，压缩的数据库转储将通过电子邮件发送到配置的地址。**如果未设置电子邮件配置，则备份后不会发生任何事情。**

我们设置的另一个重要配置是*TEMP _ DIR；*这是库在处理过程中临时存储生成文件的目录。**这个*目录*应该可以被正在运行的程序**写。

一旦备份操作完成，*临时目录*将被自动删除。甜蜜而简单对吗？是啊。

### 4.将生成的压缩文件发送到任何云存储

虽然该库可以将备份发送到预先配置的电子邮件地址，但它也为我们提供了一种方法，以 Java *File* 对象的形式获取生成的文件，因此我们可以对它做任何我们想做的事情。

为了实现这一点，我们必须添加以下配置属性:

```
//... properties.setProperty(MysqlExportService.PRESERVE_GENERATED_ZIP, "true");
```

该属性指示 *mysql-backup4j* 保存生成的 zip 文件，以便我们可以访问它:

```
File file = mysqlExportService.getGeneratedZipFile();
```

现在我们有了一个文件对象，我们可以使用适当的 SDK 和库将它上传到我们选择的任何云存储中。

**一旦完成，我们必须通过调用**手动清除*临时目录*中的 zip 文件

```
mysqlExportService.clearTempFiles(false);
```

这一点非常重要，因此我们不会在本地存储中有多余的文件。如果我们想以字符串的形式获取原始导出的 SQL 转储，我们只需要调用这个方法:

```
String generatedSql = mysqlExportService.getGeneratedSql();
```

我喜欢这个库的灵活性。可以设置的其他属性有:

```
properties.setProperty(MysqlExportService.DELETE_EXISTING_DATA, "true"); properties.setProperty(MysqlExportService.DROP_TABLES, "true");properties.setProperty(MysqlExportService.ADD_IF_NOT_EXISTS, "true"); properties.setProperty(MysqlExportService.JDBC_DRIVER_NAME, "root.ss");properties.setProperty(MysqlExportService.JDBC_CONNECTION_STRING, "jdbc:mysql://localhost:3306/database-name");
```

*DELETE_EXISTING_DATA* 将在 *INSERT INTO table* SQL 语句之前添加一条 *DELETE * FROM table* SQL 语句。

*DROP_TABLES* 将在 *CREATE TABLE IF NOT EXISTS* 语句之前添加一条 *DROP TABLE IF EXISTS* SQL 语句。

*ADD_IF_NOT_EXISTS* 默认为 *true* 将添加一个 *IF NOT EXISTS* 子句到 *CREATE TABLE* 语句中。

我们也可以通过属性指定*JDBC _ 司机 _ 名字*和*JDBC _ 连接 _ 字符串*。

如果我们的数据库碰巧运行在另一个主机或端口上，而不是在 *localhost:3306* 上，那么我们可以使用 JDBC _ 连接 _ 字符串属性来配置连接。DB_NAME 将从提供的连接字符串中提取。

我们可以通过使用 Java 作业调度器，如 [quartz](http://www.quartz-scheduler.org/) 或其他方法来自动化这个过程。此外，在一个典型的 web 应用程序中，我们可以为它创建一个路径，它将在一个*服务*或一个*控制器*中触发备份过程。

我们甚至可以将它集成到 web 应用程序中，这样当数据库有重大记录更新时，就会触发备份。可能性只受到我们创造力的限制。

### 5.导入数据库转储

没错。我们已经能够备份我们的数据库，并把它锁在一个安全的金库里。但是我们如何导入数据库并进行恢复呢？

首先，我们必须解压缩生成的 zip 文件，并将 SQL 转储文件提取到一个文件夹中。然后我们可以使用数据库客户端，如 [HeidiSQL](https://www.heidisql.com/) 和 [Adminer](https://www.adminer.org/) 来导入数据库。使用数据库管理器客户机将会提供一个可视化的辅助工具和其他强大的工具。

然而，假设我们发现自己需要在应用程序中以编程方式恢复数据库，而它仍然在运行。

我们需要做的就是以字符串形式读取生成的 SQL 转储的内容，并将其传递给库的 *MySqlImportService* ,只需进行最少的配置:

```
String sql = new String(Files.readAllBytes(Paths.get("path/to/sql/dump/file.sql")));
```

```
boolean res = MysqlImportService.builder() .setDatabase("database-name") .setSqlString(sql).setUsername("root") .setPassword("root") .setDeleteExisting(true).setDropExisting(true) .importDatabase(); 
```

```
assertTrue(res);
```

在上面的代码片段中，我们从文件系统中读取 SQL，然后使用 *MySqlImportService* 来执行导入操作。

我们将 *MySqlImportService* 配置为删除表中的任何现有内容，并丢弃现有的表。我们总是可以微调这些参数来满足我们的需要。如果操作成功，服务将返回 true，否则返回 false。

如果我们的数据库运行在另一个服务器和端口上，而不是 localhost:3306 上，该怎么办？我们也可以使用 *setJdbcConnString()* 方法进行配置。

虽然我们从本地文件系统中读取 SQL 文件，但是如果我们在 web 界面中，我们实际上可以提供一个允许从文件系统中选择文件的界面。然后可以读取内容，并作为 HTTP POST 请求发送到服务器。

### 6.结论

哇哦！这是我们刚刚看到的一些生产力工具。如果你喜欢 mysql-backup4j，记得在 Github 上启动它。

现在，在你的项目中使用它。有问题吗？捐款？升值？请把它们放在下面的评论区。

**从[https://smattme.com](https://smattme.com)T3 看我其他的技术帖和生活看法**

> 如果你觉得这篇文章有帮助，学到了什么，为这篇文章鼓掌，并在脸书和推特上与你的朋友分享。为你阅读的高质量内容感到自豪。

*最初发表于[smattme.com](https://smattme.com/blog/technology/how-to-backup-mysql-database-programmatically-using-mysql-backup4j)。*