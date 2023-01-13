# 如何将开源 Java 库上传到 Maven Central

> 原文：<https://www.freecodecamp.org/news/how-to-upload-an-open-source-java-library-to-maven-central-cac7ce2f57c/>

by Seun Matt

# 如何将开源 Java 库上传到 Maven Central

![vkpmwy8oSVeopuE86popOytHNCIbpsoiWUla](img/bbd2a10668dbf57cdcb9635b0b441a5b.png)

本文从头到尾是一个全面的指南，介绍如何将 Java 库部署到 Maven Central，这样每个人都可以通过将依赖项包含到他们的项目中来使用它。

不言而喻，必须有一个 Java 库才能上传。因此，第一件事是创建一个独特的、高质量代码标准的 Java 库，这对开发人员社区是有益的。当然，你已经得到了——这也是你为什么在最后读这篇文章的原因*(或者也许你正计划创造一个)*。

总之，要上传一个闪亮的新 Java 库到 Maven Central，我们必须保留我们的组 ID，在项目的 *pom.xml* 中提供所需的细节，用 GnuPG 签署生成的工件，最后部署到 Sonatype 的 Nexus Repository Manager。

### 第一步:

我们的第一步是确保我们的开源库可以在像 Github 这样的公共代码库中使用。然后，我们可以继续保留我们想要的组 ID。理想情况下，组 ID 对于个人或组织来说应该是唯一的，就像在一个域中一样。群组 ID 的例子包括 ***com.smattme*** 和***org . Apache . commons****。*

因此，我们将在这里创建一个 JIRA 帐户[，然后](https://issues.sonatype.org/secure/Signup!default.jspa)[登录](https://issues.sonatype.org/login.jsp)以创建一个新的项目票证。点击网站顶部的**创建**按钮将加载一个模态，我们将在其中提供组 ID(如 com.smattme)、SCM URL(如[*【https://github.com/SeunMatt/mysql-backup4j.git*](https://github.com/SeunMatt/mysql-backup4j.git)*)、*项目 URL(如[*【https://github.com/SeunMatt/mysql-backup4j*](https://github.com/SeunMatt/mysql-backup4j)*)、*摘要(可以是库的名称，如 *mysql-backup4j 【T19*

**注意:**在创建新项目票证的模式中，确保**项目**字段设置为*社区支持—开源项目存储库托管(OSSRH)* ，并且**问题类型**为*新项目* **。**

新票证项目的创建将触发在 Sonatype 的 OSS Repository Hosting (OSSRH)上创建存储库，这些存储库将在部署我们的工件后同步到 Maven Central。

在收到确认问题已经解决的电子邮件之前，不要进行部署，这一点很重要。如果有任何问题，我们可以随时对问题发表评论，以获得帮助和/或解释。

### 第二步:

既然我们已经成功注册了我们的组 ID，接下来要做的事情就是用必要的信息更新项目的 ***pom.xml*** 。让我们首先提供项目的名称、描述和 URL，以及坐标和打包信息:

```
<groupId>com.smattme</groupId>
```

```
<artifactId>mysql-backup4j</artifactId>
```

```
<version>1.0.1</version>
```

```
<packaging>jar</packaging> 
```

```
<name>${project.groupId}:${project.artifactId}&lt;/name> 
```

```
<description> This is a simple library for backing up mysql databases and sending to emails, cloud storage and so on. It also provide a method for programmatically, importing SQL queries generated during the export process</description> 
```

```
<url>https://github.com/SeunMatt/mysql-backup4j</url>
```

接下来是许可和开发者信息。在这种情况下，我们将使用麻省理工学院的许可证。如果使用了任何其他许可证，那么所需要的就是该许可证的相应 URL。如果是开源许可，很有可能会在*opensource.org*发布:

```
<licenses> <license>  <name>MIT License</name>              <url>http://www.opensource.org/licenses/mit-license.php</url>       <;/license> </licenses> <developers> 
```

```
 <developer>   <name>Seun Matt</name>   <email>smatt382@gmail.com</email>      <organization>SmattMe</organization> <organizationUrl>https://smattme.com</organizationUrl></developer> 
```

```
</developers>
```

另一个需要的重要信息是源代码管理(SCM)细节:

```
<scm>   <connection>scm:git:git://github.com/SeunMatt/mysql-backup4j.git</connection>   <developerConnection>scm:git:ssh://github.com:SeunMatt/mysql-backup4j.git</developerConnection>   <url>https://github.com/SeunMatt/mysql-backup4j/tree/master</url> </scm>
```

在这种情况下，项目托管在 Github 上，因此提供值的原因。其他 SCM 的示例配置可在[这里](https://central.sonatype.org/pages/requirements.html#scm-information)找到。

### 第三步:

在这一步中，我们将使用 Apache Maven 插件配置我们的项目以部署到 OSSRH。插件要求我们在我们的 ***pom.xml*** 中添加一个***distribution management***部分:

```
<distributionManagement> 
```

```
 <snapshotRepository>   <id>ossrh</id>        <url>https://oss.sonatype.org/content/repositories/snapshots</url> </snapshotRepository> 
```

```
<repository> <id>ossrh</id> <url>https://oss.sonatype.org/service/local/staging/deploy/maven2/</url> </repository> 
```

```
</distributionManagement>
```

我们将为上述已配置的存储库提供用户凭证，方法是向 settingGS . XML 添加一个***<serve***RS>ent***ry，该文件可以在用户的主目录*中找到，例如/Users/sma* tt/.m2。请注意，id 与快照报告*** sitor **y 和报告的 id 相同**

```
<settings>   <servers>    <server>     <id>ossrh</id>    <username>your-jira-id</username>    <password>your-jira-pwd&lt;/password>  </server>   </servers></settings>
```

接下来我们要添加一些 Maven 构建插件:源代码、Javadoc、nexus staging 和 GPG 插件。这些插件中的每一个都将被放置在***<plugi****n*s>标签内，即 insid***e&***lt；构建>标签。

将库部署到 OSSRH 需要同时部署库的源代码和 JavaDoc。因此，我们将添加 Maven 插件来无缝实现这一点:

```
<plugin>   <groupId>org.apache.maven.plugins</groupId>  <artifactId>maven-javadoc-plugin</artifactId>         <version>3.0.0</version> <executions>   <execution>    <id>attach-javadocs</id>   <goals>     <goal>jar</goal>   </goals>   </execution>  </executions> </plugin> 
```

```
<plugin>  <groupId>org.apache.maven.plugins</groupId>  <artifactId>maven-source-plugin</artifactId>    <version>3.0.1</version> <executions>   <execution>    <id>attach-sources</id>   <goals>      <goal>jar</goal>   </goals>   </execution> </executions> </plugin>
```

最新版本的 Javadoc 和源代码插件可以分别在[这里](https://search.maven.org/classic/#search%7Cga%7C1%7Cg%3Aorg.apache.maven.plugins%20a%3Amaven-javadoc-plugin)和[这里](https://search.maven.org/classic/#search%7Cga%7C1%7Cg%3Aorg.apache.maven.plugins%20a%3Amaven-source-plugin)找到。

我们需要满足的另一个需求是用 GPG/PGP 程序签署我们的工件。为此，我们必须在我们的系统上安装 GnuPG。下载安装后，我们随时可以运行 ***gpg —版本*** 来验证安装。请注意，在某些系统上，将使用 ***gpg2 —版本*** 。此外，我们应该确保 GnuPG 安装的 bin 文件夹在系统路径中。

现在，我们可以通过运行命令 ***gpg — gen-key*** 并遵循提示来为我们的系统生成一个密钥对。我们可以使用***gpg-list-keys 列出可用的按键。*** 务必遵循本[指南](https://central.sonatype.org/pages/working-with-pgp-signatures.html#delete-a-sub-key)以确保生成的主密钥用于签署我们的文件。

让我们回到我们的 ***pom.xml*** 文件，并为 GnuPG 程序添加 Maven 插件，这样我们的文件就可以用我们在程序构建期间生成的默认密钥自动签名了:

```
<plugin>  <groupId>org.apache.maven.plugins</groupId>  <artifactId>maven-gpg-plugin</artifactId>  <version>1.6</version>  <executions>  <execution>    <id>sign-artifacts</id>   <phase>verify</phase>   <goals>     <goal>sign</goal>   </goals> </execution> </executions> </plugin>
```

Maven GPG 插件的最新版本可以在这里找到。在生成我们的密钥对时，我们为它提供了一个密码短语；这个密码短语将被配置在我们的 ***.m2/settings.xml*** 文件中。我们还可以为 GnuPG 程序指定可执行文件——或者是 ***gpg*** 或者是 ***gpg2。*** 所以在 *settings.xml* 文件中，我们会添加一个*&**lt；profil*** es >段刚结束 ta ***g 为<*** /servers >:

```
<profiles>  <profile>   <id>ossrh</id>  <activation>   <activeByDefault>true</activeByDefault>  </activation>  <properties>    <gpg.executable>gpg</gpg.executable>   <gpg.passphrase>pass-phrase</gpg.passphrase> </properties> </profile> </profiles>
```

最后，我们将添加 Nexus Staging Maven 插件，以便部署我们的库——包括源代码、Javadoc 和*。asc 文件到 OSSRH，使用简单的命令:

```
<plugin>  <groupId>org.sonatype.plugins</groupId> <artifactId>nexus-staging-maven-plugin</artifactId>          <version>1.6.8</version> <extensions>true</extensions>
```

```
 <configuration>    <serverId>ossrh</serverId>    <nexusUrl>https://oss.sonatype.org/</nexusUrl> <autoReleaseAfterClose>false</autoReleaseAfterClose> </configuration>
```

```
</plugin>
```

插件的最新版本可以在[这里](https://search.maven.org/classic/#search%7Cga%7C1%7Ca%3Anexus-staging-maven-plugin%20g%3Aorg.sonatype.plugins)找到。记下**<serverId>ossrh</se**rverId>；您会注意到在 s ettings.xml 文件中使用的**相同的**值 ossrh 是**。这对于插件能够定位我们在 settings.xml 的 t*h***e<serve***RS>section***中配置的凭证很重要

### 第四步:

在本节中，我们将运行一些 Maven CLI 命令来进行实际部署。在这个阶段之前，我们应该仔细检查和测试库，以确保没有错误。为什么？我们要开始直播了！

首先运行: ***mvn 清理部署***

如果一切顺利，我们将在控制台输出中看到为项目创建的临时存储库 id，如下所示:

****创建了 ID 为“comsmattme-1001”的暂存库。***

注意:“ **comsmattme** ”是我们在本文中作为例子使用的组 ID。

让我们使用先前创建的 JIRA 帐户的相同凭证登录到[*【https://oss.sonatype.org】*](https://oss.sonatype.org/)*，以检查我们已经部署到暂存的工件。登录后，我们将单击左侧菜单中**构建推广**子菜单下的**暂存库**，并使用页面右上角的搜索栏，我们将搜索创建的暂存库 ID，例如本例中的***comsmattme-1001***。*

*我们应该能够看到和检查 nexus staging Maven 插件上传的工件。如果对一切都满意，那么我们可以通过运行以下 maven 命令进行发布:*

****mvn nexus-staging:发布****

*该库可能需要 2 个小时或更长时间才能显示在 [Maven Central Search](https://search.maven.org/) 上。为了搜索我们的工件，我们将向搜索框提供以下查询:***g:com . smattme a:MySQL-backup 4j***其中 **g** 是组 ID，而 **a** 是工件名称。*

### *结论*

*在这篇全面的文章中，我们已经完成了将 Java 库部署到 Maven Central 的过程。本文中使用的示例项目的完整的 ***pom.xml*** 可以在[这里](https://github.com/SeunMatt/mysql-backup4j/blob/master/pom.xml)找到，并且 ***settings.xml*** 也可以在[这里](https://gist.github.com/SeunMatt/79abd3c3a7d110fefe01d8eaea730001#file-settings-xml)找到。编码快乐！*

**最初发表于[smattme.com](https://smattme.com/blog/technology/comprehensive-step-by-step-guide-on-how-to-upload-open-source-java-library-to-maven-central)。**