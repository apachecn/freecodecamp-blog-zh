# 如何使用 Bucket4J 和 Redis 创建速率限制器

> 原文：<https://www.freecodecamp.org/news/rate-limiting-with-bucket4j-and-redis/>

在本教程中，我们将学习如何在缩放服务中实现速率限制。我们将使用 [Bucket4J](https://github.com/vladimir-bukhtoyarov/bucket4j) 库来实现它，我们将使用 [Redis](https://redis.io/) 作为分布式缓存。

## 为什么使用速率限制？

让我们从一些基础知识开始，以确保我们理解速率限制的必要性，并介绍我们将在本教程中使用的工具。

### 无限制费率的问题

如果像 Twitter API 这样的公共 API 允许其用户每小时发出无限数量的请求，这可能会导致:

*   资源枯竭
*   服务质量下降
*   拒绝服务攻击

这可能导致**服务不可用或**变慢的情况。这也可能导致服务产生更多**意想不到的成本**。

### 限速有什么帮助

首先，速率限制可以防止拒绝服务攻击。当与重复数据删除机制或 API 密钥结合使用时，速率限制还有助于防止分布式拒绝服务攻击。

其次，它有助于估计交通。这对公共 API 非常重要。这也可以与自动化脚本结合使用，以监控和扩展服务。

第三，您可以使用它来实现基于层级的定价。这种定价模式意味着用户可以为更高的请求率付费。Twitter API 就是一个例子。

### 令牌桶算法

令牌桶是一种可以用来实现速率限制的算法。简言之，它的工作方式如下:

1.  创建一个具有一定容量(令牌数量)的桶。
2.  当一个请求进来时，桶被检查。如果有足够的容量，则允许请求继续进行。否则，请求被拒绝。
3.  当请求被允许时，容量被减少。
4.  经过一段时间后，容量会得到补充。

### 如何在分布式系统中实现令牌桶

为了在分布式系统中实现令牌桶算法，我们需要使用一个**分布式缓存**。

缓存是一个**键值存储**来存储桶信息。我们将使用 Redis 缓存来实现这一点。

在内部，Bucket4j 允许我们插入 Java JCache API 的任何实现。Redis 的 [Redisson](https://redisson.org/) 客户端是我们将使用的实现。

## 项目执行

我们将使用 [Spring Boot](https://spring.io/projects/spring-boot) 框架来构建我们的服务。

我们的服务将包含以下组件:

1.  一个简单的 REST API。
2.  使用 Redisson 客户端连接到服务的 Redis 缓存。
3.  Bucket4J 库包装了 REST API。
4.  我们将 Bucket4J 连接到 JCache 接口，该接口将使用 Redisson 客户端作为后台的实现。

首先，我们将学习对所有请求的 API 进行速率限制。然后，我们将学习为每个用户或每个定价层实现一个更复杂的速率限制机制。

让我们从项目设置开始。

### 安装依赖项

让我们将下面的依赖项添加到我们的 *pom.xml* (或 *build.gradle* )文件中。

```
<dependencies>
    <!-- To build the Rest API -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Redisson Starter = Spring Data Redis starter(excluding other clients) and Redisson client -->
    <dependency>
        <groupId>org.redisson</groupId>
        <artifactId>redisson-spring-boot-starter</artifactId>
        <version>3.17.0</version>
    </dependency>

    <!-- Bucket4J starter = Bucket4J + JCache -->
    <dependency>
        <groupId>com.giffing.bucket4j.spring.boot.starter</groupId>
        <artifactId>bucket4j-spring-boot-starter</artifactId>
        <version>0.5.2</version>
    </dependency>
</dependencies> 
```

### 缓存配置

首先，我们需要启动我们的 Redis 服务器。假设我们在本地机器的端口 6379 上运行一个 Redis 服务器。

我们需要执行两个步骤:

1.  从我们的应用程序创建到此服务器的连接。
2.  设置 JCache 以使用 Redisson 客户机作为实现。

Redisson 的文档提供了在常规 Java 应用程序中实现这一点的简明步骤。我们将实施同样的步骤，但在 Spring Boot。

我们先来看代码。我们需要创建一个配置类来创建所需的 beans。

```
@Configuration
public class RedisConfig  {

    @Bean
    public Config config() {
        Config config = new Config();
        config.useSingleServer().setAddress("redis://localhost:6379");
        return config;
    }

    @Bean
    public CacheManager cacheManager(Config config) {
        CacheManager manager = Caching.getCachingProvider().getCacheManager();
        cacheManager.createCache("cache", RedissonConfiguration.fromConfig(config));
        return cacheManager;
    }

    @Bean
    ProxyManager<String> proxyManager(CacheManager cacheManager) {
        return new JCacheProxyManager<>(cacheManager.getCache("cache"));
    }
} 
```

这是做什么的？

1.  创建一个配置对象，我们可以用它来创建连接。
2.  使用配置对象创建缓存管理器。这将在内部创建一个到 Redis 实例的连接，并在其上创建一个名为“cache”的散列。
3.  创建将用于访问缓存的代理管理器。无论我们的应用程序试图使用 JCache API 缓存什么，它都将被缓存在 Redis 实例中名为“cache”的散列内。

### 构建 API

让我们创建一个简单的 REST API。

```
@RestController
public class RateLimitController {
    @GetMapping("/user/{id}")
    public String getInfo(@PathVariable("id") String id) {
        return "Hello " + id;
    }
} 
```

如果我点击 URL 为`http://localhost:8080/user/1`的 API，我将得到响应`Hello 1`。

### Bucket4J 配置

为了实现速率限制，我们需要配置 Bucket4J。幸运的是，由于有了 starter 库，我们不需要编写任何样板代码。

它还**自动检测我们在上一步中创建的代理管理器 bean** ,并使用它来缓存存储桶。

我们需要做的是围绕我们创建的 API 配置这个库。同样，有多种方法可以做到这一点。

我们可以使用在 starter 库中定义的基于属性的配置。
对于简单的情况，比如对所有用户或所有访客用户进行速率限制，这是最方便的方式。

然而，如果我们想要实现更复杂的东西，比如对每个用户的速率限制，最好为它编写自定义代码。

我们将对每个用户实施速率限制。假设我们在数据库中存储了每个用户的速率限制，我们可以使用用户 id 查询它。

让我们一步一步地为它编写代码。

#### 创建一个桶

在我们开始之前，让我们看看如何创建一个 bucket。

```
Refill refill = Refill.intervally(10, Duration.ofMinutes(1));
Bandwidth limit = Bandwidth.classic(10, refill);
Bucket bucket = Bucket4j.builder()
        .addLimit(limit)
        .build(); 
```

*   **重新装满**–多长时间后铲斗将被重新装满。
*   **带宽**–桶有多少带宽。基本上，每个补充期的请求。
*   **Bucket**–使用这两个参数配置的对象。此外，它维护一个令牌计数器来跟踪桶中有多少令牌可用。

以此为基础，让我们做一些改变，使之适合我们的用例。

#### 使用 ProxyManager 创建和缓存存储桶

我们创建代理管理器是为了在 Redis 上存储存储桶。一旦创建了一个 bucket，就需要将其缓存在 Redis 上，不需要再次创建。

为了实现这一点，我们将把`Bucket4j.builder()`替换为`proxyManager.builder()`。ProxyManager 将负责缓存这些桶，并且不再创建它们。

ProxyManager 的构建器接受两个参数——一个**键**,将根据该键缓存存储桶，另一个**配置对象**,将使用该对象创建存储桶。

让我们看看如何实现它:

```
@Service
public class RateLimiter {
    //autowiring dependencies

    public Bucket resolveBucket(String key) {
        Supplier<BucketConfiguration> configSupplier = getConfigSupplierForUser(key);

        // Does not always create a new bucket, but instead returns the existing one if it exists.
        return buckets.builder().build(key, configSupplier);
    }

    private Supplier<BucketConfiguration> getConfigSupplierForUser(String key) {
        User user = userRepository.findById(userId);
        Refill refill = Refill.intervally(user.getLimit(), Duration.ofMinutes(1));
        Bandwidth limit = Bandwidth.classic(user.getLimit(), refill);
        return () -> (BucketConfiguration.builder()
                .addLimit(limit)
                .build());
    }
} 
```

我们已经创建了一个方法，为提供的键返回一个桶。在下一步中，我们将看到如何使用它。

#### 如何消费代币和设置速率限制

当一个请求进来时，我们将尝试从相关的桶中消费一个令牌。
我们将使用 bucket 的`tryConsume()`方法来做到这一点。

```
@GetMapping("/user/{id}")
public String getInfo(@PathVariable("id") String id) {
    // gets the bucket for the user
    Bucket bucket = rateLimiter.resolveBucket(id);

    // tries to consume a token from the bucket
    if (bucket.tryConsume(1)) {
        return "Hello " + id;
    } else {
        return "Rate limit exceeded";
    }
} 
```

如果令牌被成功消费，则`tryConsume()`方法返回`true`，如果令牌未被消费，则返回`false`。

## 如何测试我们的服务

我们可以使用任何自动化测试技术对此进行测试。例如，我们可以使用 [JUnit](https://junit.org/) 。让我们编写一个测试用例，多次调用`getInfo()`方法，并验证响应是否正确。

假设我们有一个 id 为`1`的用户，每分钟的请求限制为`10`。让我们假设我们也有一个 id 为`2`的用户，每分钟的请求限制为`20`。

我们将为两个用户命中 11 个请求，并验证 id 为`1`的用户的请求失败，但 id 为`2`的用户的请求成功。

```
@Test
public void testGetInfo() {

    // calls the method 10 times for user 1
    for (int i = 0; i < 10; i++) {
        rateLimiter.getInfo(1));
        rateLimiter.getInfo(2));
    }

    // verifies that the response is rate limited for user 1
    assertEquals("Rate limit exceeded", rateLimiter.getInfo(1));

    // verifies that the response is successful for user 2
    assertEquals("Hello 2", rateLimiter.getInfo(2));
} 
```

当我们运行测试时，我们将看到测试通过。

## 结论

在本教程中，我们介绍了如何在 Spring Boot 应用程序中使用 Bucket4j 和 Redis 创建速率限制器。我们还研究了如何用 JCache 设置 Redisson 客户机，以及如何用它来缓存存储桶。

最后，我们实现了一个简单的速率限制器，它可以用来限制特定用户的速率请求。

希望你喜欢这个教程。感谢阅读！