# Docker 是什么？学习如何使用容器——用例子解释

> 原文：<https://www.freecodecamp.org/news/what-is-docker-learn-how-to-use-containers-with-examples/>

容器是当今软件开发的基本工具。当您利用容器时，在任何环境中运行应用程序都变得很容易。

运行容器最流行的技术是 [Docker](https://www.docker.com/) ，它可以在任何操作系统上运行。

在这篇博客文章中，你将学习使用 Docker 来处理 3 个最重要的用例。您将学习如何:

*   使用 Docker 在本地运行数据库，
*   使用文档化的数据库运行自动化测试，
*   使用 Docker 在本地和生产中运行您的应用程序。

您将使用 Java [Spring Boot](https://spring.io/projects/spring-boot) 应用程序，但是所有的知识都适用于您选择的其他编程语言。

要运行所有示例，您需要:

*   [安装对接器](https://docs.docker.com/engine/install/)
*   [安装 Java](https://www.java.com/de/download/)

## 使用 Docker 运行独立的应用程序

> Docker 消除了重复、平凡的配置任务，并在整个开发生命周期中用于快速、简单和可移植的应用程序开发-桌面和云。(来源:[https://www.docker.com/use-cases/](https://www.docker.com/use-cases/))

Docker 的超能力的核心是利用所谓的 [cgroups](https://en.wikipedia.org/wiki/Cgroups) 来创建轻量级的、隔离的、可移植的和高性能的环境，你可以在几秒钟内开始。

让我们看看如何使用 Docker 来提高工作效率。

## 数据库容器

使用 Docker，您可以在几秒钟内启动多种类型的数据库。这很简单，并且不会因为运行数据库所需的其他需求而污染您的本地系统。一切都打包在 Docker 容器中。

通过搜索[hub.docker.com](https://hub.docker.com/)，你可以为许多数据库找到现成的容器。

使用`docker run`命令，你可以启动一个 [MySQL Docker 容器](https://hub.docker.com/_/mysql/)。

```
docker run --rm -v "$PWD/data":/var/lib/mysql --name mysql -e MYSQL_ROOT_PASSWORD=admin-password -e MYSQL_DATABASE=my-database -p 3306:3306 mysql:8.0.28-debian
```

该命令使用运行 Docker 容器的高级功能:

*   `-v "$PWD/data"`将您的本地目录`./data`映射到 docker 容器，这允许您启动 Docker 容器而不会丢失数据，
*   `-p 3306:3306`将容器端口`3306`映射到我们的机器，以便其他应用程序可以使用它，
*   `-e MYSQL_DATABASE=my-database`设置一个环境变量，自动创建一个名为`my-database`的新数据库。
*   `-e MYSQL_ROOT_PASSWORD=admin-password`设置一个环境变量来设置管理员密码，
*   `--rm`停止时移除容器。

这些环境变量以及更多内容都记录在 [Docker 映像的页面](https://hub.docker.com/_/mysql/?tab=description)上。

### 如何使用数据库容器进行开发

你将使用一个流行的技术栈来构建一个基于 Java 和 T2 Spring Boot 的 web 应用程序。为了关注 Docker 部分，您可以从官方的[克隆一个简单的演示应用程序，使用 Rest 指南](https://spring.io/guides/gs/accessing-data-rest/)访问 JPA 数据。

```
# Download the sample application
git clone https://github.com/spring-guides/gs-accessing-data-rest.git

# Open the final application folder
cd complete
```

该应用程序带有一个内存数据库，这对于生产来说没有价值，因为它不允许多个服务访问和改变单个数据库。一个 [MySQL](https://www.mysql.com/) 数据库更适合将你的应用程序扩展到更多的读写。

因此，将 MySQL 驱动程序添加到您的`pom.xml`:

```
 <!-- Disable in memory database -->
       <!--
       <dependency>
           <groupId>com.h2database</groupId>
           <artifactId>h2</artifactId>
           <scope>runtime</scope>
       </dependency>
       -->

       <!-- MySQL driver -->
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <scope>runtime</scope>
       </dependency> 
```

现在，您需要通过添加配置文件`src/main/resources/application.properties`来添加连接到数据库的配置。

```
# Database configuration
spring.datasource.url=jdbc:mysql://localhost:3306/my-database
spring.datasource.username=root
spring.datasource.password=admin-password

# Create table and database automatically
spring.jpa.hibernate.ddl-auto=update
```

现在，您可以启动应用程序并调用现有端点:

```
# Get all people
curl http://localhost:8080/people

# Add a person
curl -i -H "Content-Type:application/json" -d '{"firstName": "Frodo", "lastName": "Baggins"}' http://localhost:8080/people

# Get all people again, which now returns the created person
curl http://localhost:8080/people
```

您成功地使用了基本的应用程序，该应用程序读写数据库中的数据。使用 MySQL Docker 数据库可以在几秒钟内为您提供一个健壮的数据库，并且您可以从任何应用程序中使用它。

### 如何使用数据库容器进行集成测试

应用程序已经有了需要运行数据库的测试。但是，因为您用实际的 MySQL 数据库替换了内存中的数据库，所以如果您停止数据库，测试将不会成功运行。

```
# Stop database
docker rm -f mysql

# Run tests
./mvnw clean test

... skipped output ...
[ERROR] Tests run: 7, Failures: 0, Errors: 7, Skipped: 0
... skipped output ...
```

为了快速启动和停止运行测试的容器，有一个方便的工具叫做 [testcontainers](https://github.com/testcontainers) 。在那里你可以找到许多编程语言的库，包括 Java。

首先，您需要向您的`pom.xml`添加一些依赖项:

```
 <!-- testcontainer -->
       <dependency>
           <groupId>org.testcontainers</groupId>
           <artifactId>testcontainers</artifactId>
           <version>1.16.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.testcontainers</groupId>
           <artifactId>mysql</artifactId>
           <version>1.16.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.testcontainers</groupId>
           <artifactId>junit-jupiter</artifactId>
           <version>1.16.3</version>
           <scope>test</scope>
       </dependency> 
```

您需要更新测试以利用 testcontainers，它会在每次测试运行时启动数据库。向测试添加一个注释和一个字段以使用它:

```
//added imports
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.DynamicPropertyRegistry;
import org.springframework.test.context.DynamicPropertySource;
import org.testcontainers.containers.MySQLContainer;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;

@SpringBootTest
@AutoConfigureMockMvc
@Testcontainers // Annotation to enable testcontainers
public class AccessingDataRestApplicationTests {

   // Field to access the started database
   @Container
   private static MySQLContainer database = new MySQLContainer<>("mysql:5.7.34");

   //Set database configuration using the started database
   @DynamicPropertySource
   static void databaseProperties(DynamicPropertyRegistry registry) {
       registry.add("spring.datasource.url", database::getJdbcUrl);
       registry.add("spring.datasource.username", database::getUsername);
       registry.add("spring.datasource.password", database::getPassword);
   } 
```

对于每次测试执行，数据库都会为您启动，这允许您在执行测试时使用实际的数据库。所有的布线、设置、启动和清理都为您完成了。

## 将你的申请归档

使用简单的 Docker 工具对你的应用程序进行 Docker 化是可能的，但是不推荐。

您可以构建您的应用程序，使用包含 Java 的基本容器，并复制和运行您的应用程序。但是有很多陷阱，每种语言和框架都是如此。所以总是寻找让你生活更轻松的工具。

在这个例子中，您将使用 [Jib](https://github.com/GoogleContainerTools/jib) 和[distroles 容器](https://github.com/GoogleContainerTools/distroless)轻松构建 Docker 容器。结合使用这两者可以得到一个最小的、安全的、可复制的容器，它在本地和生产中的工作方式是一样的。

要使用 Jib，您需要将它作为 maven 插件添加到您的`pom.xml`:

```
<build>
       <plugins>
           <plugin>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-maven-plugin</artifactId>
           </plugin>

        <!-- Jib plugin -->
           <plugin>
               <groupId>com.google.cloud.tools</groupId>
               <artifactId>jib-maven-plugin</artifactId>
               <version>3.2.1</version>
               <configuration>
                   <from>
                       <image>gcr.io/distroless/java17:nonroot</image>
                   </from>
                   <to>
                       <image>my-docker-image</image>
                   </to>
               </configuration>
           </plugin>
       </plugins>
   </build>
```

现在，您可以构建映像并运行应用程序:

```
# build the docker container
./mvnw compile jib:dockerBuild

# find your build image
docker images

# start the database
docker run --rm -v "$PWD/data":/var/lib/mysql --name mysql -e MYSQL_ROOT_PASSWORD=admin-password -e MYSQL_DATABASE=my-database -p 3306:3306 mysql:8.0.28-debian

# start the docker container which contains your application
docker run --net=host my-docker-image

… skipped output…
2022-04-15 17:43:51.509  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2022-04-15 17:43:51.521  INFO 1 --- [           main] c.e.a.AccessingDataRestApplication       : Started AccessingDataRestApplication in 6.146 seconds (JVM running for 6.568)
```

应用程序使用网络模式主机`--net=host`启动，这使得连接到您启动的数据库变得很容易。或者，您可以创建一个`docker network`并在同一个网络中启动两者。

您可以将容器推送到容器注册表中，并从任何容器编排工具中引用它，以便在生产环境中运行您的应用程序。

## 摘要

在本教程中，您学习了如何利用 Docker 来创建、测试。和运行应用程序，而不会污染您的系统。

一切都在您隔离的 Docker 环境中，并且在本地工作，就像在持续集成系统和生产系统上一样，在那里您可能会启动数百个应用程序。

您可以在[my GitHub Docker For Development 示例应用程序库](https://github.com/sesigl/docker-for-development-example-application)中找到现成的示例应用程序。

我希望你喜欢这篇文章。

如果你喜欢它，觉得有必要给我一点掌声，或者只是想和我保持联系，[在 Twitter 上关注我](https://twitter.com/sesigl)。

我在易贝·克莱南泽根公司工作，这是世界上最大的机密公司之一。顺便说一下，[我们正在招聘](https://www.ebay-kleinanzeigen.de/careers)！

## 参考

*   [坞站枢纽:MySQL 图像](https://hub.docker.com/_/mysql/)
*   [Docker 文档:运行命令](https://docs.docker.com/engine/reference/commandline/run/)
*   [Visual Studio 代码:开发容器](https://code.visualstudio.com/docs/remote/containers)
*   [学习 Java——免费 Java 课程](https://www.freecodecamp.org/news/learn-java-free-java-courses-for-beginners/)
*   Youtube:用 Jib 更快地构建容器