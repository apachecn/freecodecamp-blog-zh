# 如何在 Spring Boot 应用程序中配置多个 Camel 上下文

> 原文：<https://www.freecodecamp.org/news/configure-multiple-camel-context-in-spring-boot-application-d3a16396266/>

肖申克的夏尔马

![gmbVrFLJunInNfmAqchAyPBNPSBRr7zBft0N](img/c61d6885c54e2066ecec0cacd9140af1.png)

# 如何在 Spring Boot 应用程序中配置多个 Camel 上下文

本文假设您已经熟悉 Apache Camel & Spring Boot，他们的文档将它描述为:

**Apache Camel** 是一个[开源](https://en.wikipedia.org/wiki/Open_source) [框架](https://en.wikipedia.org/wiki/Framework-oriented_design)，用于[面向消息的中间件](https://en.wikipedia.org/wiki/Message-oriented_middleware)，具有基于规则的路由和[中介](https://en.wikipedia.org/wiki/Data_mediation)引擎。它使用一个[应用程序编程接口](https://en.wikipedia.org/wiki/Application_programming_interface)(或声明性 Java [领域特定语言](https://en.wikipedia.org/wiki/Domain-specific_language))来配置路由和中介规则，提供了一个基于 [Java 对象](https://en.wikipedia.org/wiki/Plain_Old_Java_Object)的[企业集成模式](https://en.wikipedia.org/wiki/Enterprise_Integration_Patterns)的实现。

特定于领域的语言意味着 Apache Camel 可以使用常规的 Java 代码在[集成开发环境](https://en.wikipedia.org/wiki/Integrated_development_environment)中支持类型安全的路由规则智能完成，而无需大量的 [XML](https://en.wikipedia.org/wiki/XML) 配置文件。虽然也支持 [Spring 框架](https://en.wikipedia.org/wiki/Spring_Framework)中的 XML 配置。

Spring Boot 是构建所有基于 Spring 的应用程序的起点。Spring Boot 旨在以最少的 Spring 前期配置让您尽快启动并运行。Spring Boot 使得创建可以运行的独立的、生产级的基于 Spring 的应用程序变得很容易。

### 眼前的问题

您正在设计一个平台，让终端用户能够在您的系统中创建多个应用。每个应用程序都可以有自己的骆驼路线，或者可以重复使用预定义的路线。问题是如何保证一个 app 骆驼路线不与其他 app 路线发生冲突。

解决这个问题的一个方法是确保路由总是唯一命名的。但这很难实施，如果最终用户可以定义自己的路线，那就更难了。一个简单的解决方案是为每个应用程序创建单独的 Camel 上下文。更容易管理。有[多篇](http://www.baeldung.com/apache-camel-spring-boot) [文章](https://dzone.com/articles/working-with-object-store-in-mule-part-1)介绍如何用 Spring Boot 应用程序配置 Apache Camel，但是没有一篇介绍如何在运行时配置多个 Camel 上下文。

首先从 Spring Boot 应用程序中排除 *CamelAutoConfiguration* 。我们将根据需要创建 *CamelContext* ，而不是通过 Spring Auto Configuration 创建 *CamelContext* 。

```
@SpringBootApplication@EnableAutoConfiguration(exclude = {CamelAutoConfiguration.class})public class Application {
```

```
 public static void main(String[] args) {    SpringApplication.run(Application.class, args);  }
```

```
}
```

为了处理所有的 Apache Camel 配置并重用 Spring 属性，创建 *CamelConfig* 类。

```
@Configuration@EnableConfigurationProperties(CamelConfigurationProperties.class)@Import(TypeConversionConfiguration.class)public class CamelConfig {
```

```
 @Autowired  private ApplicationContext applicationContext;
```

```
 @Bean  @ConditionalOnMissingBean(RoutesCollector.class)  RoutesCollector routesCollector(ApplicationContext          applicationContext, CamelConfigurationProperties config) {
```

```
 Collection<CamelContextConfiguration> configurations = applicationContext.getBeansOfType(CamelContextConfiguration.class).values();
```

```
 return new RoutesCollector(applicationContext, new ArrayList<CamelContextConfiguration>(configurations), config);  }
```

```
 /**  * Camel post processor - required to support Camel annotations.  */  @Bean  CamelBeanPostProcessor camelBeanPostProcessor(ApplicationContext applicationContext) {    CamelBeanPostProcessor processor = new CamelBeanPostProcessor();    processor.setApplicationContext(applicationContext);    return processor;  }
```

```
}
```

为了创建和管理 *CamelContext，*创建类 *CamelContextHandler* 。

```
@Componentpublic class CamelContextHandler implements BeanFactoryAware {
```

```
 private BeanFactory beanFactory;
```

```
 @Autowired  private ApplicationContext applicationContext;
```

```
 @Autowired  private CamelConfigurationProperties camelConfigurationProperties;
```

```
 /*  * (non-Javadoc)  *  * @see  * org.springframework.beans.factory.BeanFactoryAware  * #setBeanFactory(org.springframework.beans.  * factory.BeanFactory)  */  @Override  public void setBeanFactory(BeanFactory beanFactory) {    this.beanFactory = beanFactory;  }
```

```
 public boolean camelContextExist(int id) {    String name = "camelContext" + id;    return applicationContext.containsBean(name);  }
```

```
 public CamelContext getCamelContext(int id) {    CamelContext camelContext = null;    String name = "camelContext" + id;    if (applicationContext.containsBean(name)) {           camelContext = applicationContext.getBean(name, SpringCamelContext.class);    } else {       camelContext = camelContext(name);    }    return camelContext;  }
```

```
 private CamelContext camelContext(String contextName) {    CamelContext camelContext = new SpringCamelContext(applicationContext);    SpringCamelContext.setNoStart(true);    if (!camelConfigurationProperties.isJmxEnabled()) {      camelContext.disableJMX();    }
```

```
 if (contextName != null) {      ((SpringCamelContext) camelContext).setName(contextName);    }
```

```
 if (camelConfigurationProperties.getLogDebugMaxChars() > 0) {     camelContext.getProperties().put( Exchange.LOG_DEBUG_BODY_MAX_CHARS, "" + camelConfigurationProperties.getLogDebugMaxChars());
```

```
 }
```

```
 camelContext.setStreamCaching( camelConfigurationProperties.isStreamCaching());
```

```
 camelContext.setTracing( camelConfigurationProperties.isTracing());
```

```
 camelContext.setMessageHistory( camelConfigurationProperties.isMessageHistory());
```

```
 camelContext.setHandleFault( camelConfigurationProperties.isHandleFault());
```

```
 camelContext.setAutoStartup( camelConfigurationProperties.isAutoStartup());
```

```
camelContext.setAllowUseOriginalMessage(camelConfigurationProperties.isAllowUseOriginalMessage());
```

```
 if (camelContext.getManagementStrategy().getManagementAgent() != null) {      camelContext.getManagementStrategy().getManagementAgent().setEndpointRuntimeStatisticsEnabled(camelConfigurationProperties.isEndpointRuntimeStatisticsEnabled());
```

```
 camelContext.getManagementStrategy().getManagementAgent().setStatisticsLevel(camelConfigurationProperties.getJmxManagementStatisticsLevel());
```

```
 camelContext.getManagementStrategy().getManagementAgent().setManagementNamePattern(camelConfigurationProperties.getJmxManagementNamePattern());
```

```
 camelContext.getManagementStrategy().getManagementAgent().setCreateConnector(camelConfigurationProperties.isJmxCreateConnector());
```

```
 }
```

```
 ConfigurableBeanFactory configurableBeanFactory = (ConfigurableBeanFactory) beanFactory;    configurableBeanFactory.registerSingleton(contextName, camelContext);
```

```
 try {      camelContext.start();    } catch (Exception e) {      // Log error    }    return camelContext;  }
```

```
}
```

现在，代替自动连线 *CamelContext* ，自动连线 *CamelContextHandler* 。*CamelContextHandler # getCamelContext*将根据其 ID(其中 ID 是不同应用的唯一标识符)返回 *CamelContext* 。如果不存在该 ID 的上下文，那么*camel context handler # getcamel context*将为该 ID 创建 *CamelContext* 并返回。

为了防止不必要的创建 *CamelContext，*我们可以在 *CamelContextHandler* 中定义一个助手函数，在调用 *getCamelContext* 来检查该 ID 的上下文是否存在之前可以调用这个函数。

```
public boolean camelContextExist(int id) {  String name = "camelContext" + id;  return applicationContext.containsBean(name);}
```

您使用相同的方式加载路由，并且您将使用*CamelContextHandler # getCamelContext*来获取上下文。假设您的路线存储在数据库中。并且每条路线都与某个应用 ID 相关联。要加载路线，我们可以定义如下方法:

```
public void loadRoutes(String id) {  String routestr  = fetchFromDB(id);  if (routestr != null && !routestr.isEmpty()) {    try {      RoutesDefinition routes = camelContext.loadRoutesDefinition(IOUtils.toInputStream(routestr, "UTF-8"));
```

```
 getCamelContext(id).addRouteDefinitions(routes.getRoutes());
```

```
 } catch (Exception e) {      // Log error    }  }}
```

为了在服务器启动时加载已经存在于数据库中的路线，我们可以使用 Spring 中的@PostConstruct 注释。

```
@PostConstructpublic void afterPropertiesSet() {  List<String> appIds = getAllEnabledApps();  appIds.forEach(id -> loadRoutes(id));}
```

由于 *CamelContext* 对象不是通过 Spring 创建的，Spring 不会处理这些*camel context*bean 的生命周期。为了在应用程序停止时停止所有上下文，我们可以在 *CamelConfig* 类中定义 *closeContext* 方法来优雅地关闭所有 *CamelContext* 。

```
@PreDestroyvoid closeContext() {  applicationContext.getBeansOfType(CamelContext.class).values() .forEach(camelContext -> {    try {      camelContext.stop();    } catch (Exception e) {      //Log error    }  });}
```

上面的设置可以帮助您在 Spring Boot 应用程序中运行多个 Camel 上下文。如果您有任何问题或建议，请随时写信。干杯。

Shashank Sh 的其他文章。