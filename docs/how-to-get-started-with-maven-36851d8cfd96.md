# 如何开始使用 Maven

> 原文：<https://www.freecodecamp.org/news/how-to-get-started-with-maven-36851d8cfd96/>

Maven 在行业中使用得非常频繁，我觉得最好在本文中介绍一些基础知识，以便可以有效地使用它。

本文将涵盖 maven 基础知识、maven 插件、maven 依赖项和 maven 构建生命周期等内容。

### 什么是 Maven

Maven 的创建是为了提供一种构建项目的标准方式。它的一个强大特性是依赖管理。

Maven 通常用于依赖性管理，但这不是它唯一能做的事情。

如果你不知道依赖管理意味着什么，不要担心？。我也将在本文中讨论这个问题。

### 安装 Maven

你可以从 https://maven.apache.org/安装 Maven

还要确保在路径中设置了 Maven，以便`mvn`命令能够工作。

您可以使用命令验证它是否已安装并且可以访问

```
mvn -v
```

还要确保设置了 [JAVA_HOME](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) 。

默认情况下，Maven 将使用您在 JAVA_HOME 中提供的 jdk。这可以被覆盖，但是对于本文，我们将使用 JAVA_HOME 中提供的 jdk。

### 创建您的 Maven 项目

通常像 eclipse 这样的 IDE 可以用来轻松地创建 maven 项目。但是在本文中，我将从命令行运行这些命令，以便清楚地理解这些步骤。

运行以下命令创建项目。

```
mvn -B archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.first.app -DartifactId=first-maven-app 
```

上面命令中的原型只不过是一个示例项目模板。 **groupdId** 告诉你的项目属于哪个组， **artifactId** 是项目名称。

运行上面的命令后，maven 可能需要一分钟左右的时间来下载必要的插件并创建项目。

现在创建了一个名为 first-maven-app 的文件夹。打开文件夹，你会看到一个名为 **pom.xml** 的文件

### pom.xml

POM 代表项目对象模型。pom.xml 有关于项目的所有细节，这是您将告诉 maven 它应该做什么的地方。

该文件的内容如下所示:

```
 <project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.first.app</groupId>
  <artifactId>first-maven-app</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>first-maven-app</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

**groupdId** 和 **artifactId** 与我们在命令行中给出的值相同。

**打包**是神器的打包格式。默认值是**罐子**。它还可以有其他值，如 ear、war、tar 等。

**版本**表示工件的版本号。如果**快照**存在，则表明该版本仍在开发中，可能不稳定。如果版本号没有**快照，**则为实际发布版本。

**名称**是项目名称。

我将在下面解释 Maven 中的依赖和插件。

### 超级 POM

如您所见，pom.xml 非常小。原因是大量的配置存在于由 Maven 内部维护的超级 POM 中。

pom.xml 扩展了 super Pom 以获得 Super Pom 中的所有配置。

Super Pom 中的一个配置表明:

*   所有 java 源代码都存在于 **src/main/java** 中
*   所有 java 测试代码都存在于 **src/test/java** 中

我在这里只提到这个配置，因为我们将在本文中处理源代码和测试代码。

### 密码

这里讨论的全部代码都可以在这个回购:[https://github.com/aditya-sridhar/first-maven-app](https://github.com/aditya-sridhar/first-maven-app)

让我们添加一些简单的 Java 代码。创建以下文件夹结构:

**src/main/Java/com/test/app/app . Java**

App.java 是我们将要添加的 Java 代码。

将以下代码复制到 App.java 中:

```
package com.first.app;

import java.util.List;
import java.util.ArrayList;

public class App 
{
    public static void main( String[] args )
    {
        List<Integer> items = new ArrayList<Integer>();
        items.add(1);
        items.add(2);
        items.add(3);
        printVals(items);
        System.out.println("Sum: "+getSum(items));
    }

    public static void printVals(List<Integer> items){
        items.forEach( item ->{
            System.out.println(item);
        });
    }

    public static int getSum(List<Integer> items){
        int sum = 0;
        for(int item:items){
            sum += item;
        }
        return sum;
    }
} 
```

这是一个简单的代码，有两个功能。

但是要注意的一点是，代码在 **printVals** 函数的 forEach 循环中使用了 lambda 表达式。

Lambda 表达式至少需要 Java 8 才能运行。但是默认情况下，Maven 3.8.0 使用 Java 版运行。

所以我们需要告诉 maven 改用 Java 1.8。为了做到这一点，我们将使用 Maven 插件。

### 腹部插塞

我们将使用 Maven 编译器插件来指示使用哪个 Java 版本。将以下几行添加到 pom.xml 中:

```
<project>
...
<build>
  <plugins>
     <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.0</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
  <plugins>
</build>
...
</project>
```

可以看到 Java **源**和**目标**版本都设置为 **1.8** 。

插件基本上在 maven 中完成一些动作。编译器插件编译源文件。

完整的 pom.xml 可从[这里](https://github.com/aditya-sridhar/first-maven-app/blob/master/pom.xml)获得。

有很多可用的 maven 插件。通过知道如何很好地使用插件，Maven 可以用来做令人惊讶的事情。？

### Maven 依赖性

通常在编写代码时，我们会使用很多现有的库。这些现有的库只不过是依赖关系。Maven 可以用来轻松管理依赖关系。

在我们项目的 pom.xml 中，您可以看到以下依赖关系:

```
 <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```

这种依赖性告诉我们，我们将需要 **junit** 。Junit 用于为 Java 代码编写单元测试。类似地，还可以添加许多其他依赖项。

假设您想在代码中处理 JSON。然后可以添加 **gson** 依赖关系，如下所示:

```
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.5</version>
</dependency>
```

你可以在[https://search.maven.org](https://search.maven.org/)中搜索 Maven 神器

### 传递依赖性

假设您向项目添加了一个依赖项 **A** 。现在 **A** 依赖于一个叫做 **B** 的依赖关系。 **B** 依赖于一个叫做 **C** 的依赖项。

既然你在项目中使用了 **A** ，那么你还需要 **B** 和 **C** 。

不过还好，如果在 pom.xml 中只添加 **A** 就足够了.因为 Maven 可以算出 A 依赖 B，B 依赖 C .所以内部 Maven 会自动下载 **B** 和 **C** 。

这里 **B** 和 **C** 是传递依赖关系。

### 自定义 Maven 资源库

所有这些依赖项都可以在一个公共的 Maven 中央存储库中找到[http://repo.maven.apache.org/maven2](http://repo.maven.apache.org/maven2)

可能有一些工件是您公司的私有产品。在这种情况下，您可以在您的组织内维护一个私有的 maven 存储库。我不会在本教程中讨论这一部分。

### 添加测试类

由于 junit 依赖关系存在于项目中，我们可以添加测试类。

创建以下文件夹结构:

**src/test/Java/com/test/app/app test . Java**

**AppTest.java**是试验班。

将以下代码复制到 AppTest.java 中:

```
package com.first.app;
import junit.framework.TestCase;
import java.util.List;
import java.util.ArrayList;

public class AppTest extends TestCase
{
    public AppTest( String testName )
    {
        super( testName );
    }

    public void testGetSum()
    {
        List<Integer> items = new ArrayList<Integer>();
        items.add(1);
        items.add(2);
        items.add(3);
        assertEquals( 6, App.getSum(items) );
    }
}
```

这个类测试 App 类中的 getSum()函数。

### Maven 构建生命周期和阶段

Maven 遵循构建生命周期来构建和分发工件。有三个主要的生命周期:

1.  **默认生命周期**:它处理构建和部署工件。
2.  清理生命周期:这处理项目清理
3.  站点生命周期:处理站点文档。**将在另一篇文章中讨论这个问题。**

生命周期由多个阶段组成。以下是**默认生命周期中的一些重要阶段:**

*   **验证**:检查项目的所有必要信息是否可用
*   **编译**:用于编译源文件。运行以下命令进行编译:

```
mvn compile
```

*   运行该命令后，将创建一个名为 target 的文件夹，其中包含所有编译过的文件。
*   **test** :用于运行项目中存在的所有单元测试。这就是为什么需要 Junit 依赖的原因。使用 Junit，可以编写单元测试。可以使用命令运行测试类

```
mvn test
```

*   **打包**:这将运行以上所有阶段，然后打包工件。这里它将把它打包成一个 **jar** 文件，因为 pom 指示需要一个 jar。为此，运行以下命令:

```
mvn package
```

*   在**目标**文件夹中创建 **jar** 文件
*   **验证**:这将确保项目符合质量标准
*   **install** :这将在本地存储库中安装软件包。本地存储库位置通常是**$ { user . home }/. m2/repository**。为此，请使用以下命令:

```
mvn install
```

*   **deploy** :用于将包部署到远程仓库

另一个常用的命令是 clean 命令，如下所示:

```
mvn clean
```

这个命令清除了目标文件夹中的所有内容

### 参考

Maven 官方指南:[https://maven.apache.org/guides/getting-started/](https://maven.apache.org/guides/getting-started/)

关于 POM 的更多信息:[https://maven . Apache . org/guides/introduction/introduction-to-the-POM . html](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)

关于构建生命周期的更多信息:[https://maven . Apache . org/guides/introduction/introduction-to-the-life cycle . html](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)

### 恭喜你。

你现在知道怎么用 Maven 了。这篇文章仅仅涵盖了 pom、插件、依赖项和构建生命周期的基础知识。要了解更多关于 Maven 的信息，请查看我上面给出的链接。

快乐编码？

### 关于作者

我热爱技术，关注该领域的进步。我也喜欢用我的技术知识帮助别人。

请随时通过我的 LinkedIn 账户与我联系[https://www.linkedin.com/in/aditya1811/](https://www.linkedin.com/in/aditya1811/)

你也可以在推特上关注我[https://twitter.com/adityasridhar18](https://twitter.com/adityasridhar18)

我的网站:[https://adityasridhar.com/](https://adityasridhar.com/)

*最初发表于[adityasridhar.com](https://adityasridhar.com/posts/how-to-get-started-with-maven)。*