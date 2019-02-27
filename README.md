# springboot
## spring boot特性
* 创建可以独立运行的Spring应用
* 直接嵌入 Tomcat 或 Jetty 服务器，不需要部署 WAR 文件
* 提供推荐的基础 POM 文件来简化 Apache Maven 配置
* 尽可能的根据项目依赖来自动配置 Spring 框架
* 提供可以直接在生产环境中使用的功能，如性能指标、应用信息和应用健康检查
* 没有代码生成，也没有 XML 配置文件
## spring boot基础POM文件
* spring-boot-starter 核心POM，包含自动配置支持、日志库和对YAML配置文件的支持
* spring-boot-starter-amqp 通过spring-rabbit支持AMQP
* spring-boot-starter-aop 包含spring-aop和AspectJ来支持面向切向编程（AOP）
* spring-boot-starter-batch 支持Spring Batch，包含HSQLDB
* spring-boot-starter-data-jpa 包含 spring-data-jpa、spring-orm 和 Hibernate 来支持 JPA
* spring-boot-starter-data-mongodb 包含 spring-data-mongodb 来支持 MongoDB
* spring-boot-starter-data-rest 通过 spring-data-rest-webmvc 支持以 REST 方式暴露 Spring Data 仓库
* spring-boot-starter-jdbc 支持使用 JDBC 访问数据库
* spring-boot-starter-security 包含 spring-security
* spring-boot-starter-test 包含常用的测试所需的依赖，如 JUnit、Hamcrest、Mockito 和 spring-test 等
* spring-boot-starter-velocity 支持使用 Velocity 作为模板引擎
* spring-boot-starter-web 支持 Web 应用开发，包含 Tomcat 和 spring-mvc
* spring-boot-starter-websocket 支持使用 Tomcat 开发 WebSocket 应用
* spring-boot-starter-ws 支持 Spring Web Services
* spring-boot-starter-actuator 添加适用于生产环境的功能，如性能指标和监测等功能
* spring-boot-starter-remote-shell 添加远程 SSH 支持
* spring-boot-starter-jetty 使用 Jetty 而不是默认的 Tomcat 作为应用服务器
* spring-boot-starter-log4j 添加 Log4j 的支持
* spring-boot-starter-logging 使用 Spring Boot 默认的日志框架 Logback
* spring-boot-starter-tomcat 使用 Spring Boot 默认的 Tomcat 作为应用服务器
## spring boot自动配置
Spring Boot 对于开发人员最大的好处在于可以对 Spring 应用进行自动配置。Spring Boot 会根据应用中声明的第三方依赖来自动配置 Spring 框架，而不需要进行显式的声明。比如当声明了对 HSQLDB 的依赖时，Spring Boot 会自动配置成使用 HSQLDB 进行数据库操作。
Spring Boot 推荐采用基于 Java 注解的配置方式，而不是传统的 XML。只需要在主配置 Java 类上添加`@EnableAutoConfiguration`注解就可以启用自动配置。Spring Boot 的自动配置功能是没有侵入性的，只是作为一种基本的默认实现。开发人员可以通过定义其他 bean 来替代自动配置所提供的功能。比如当应用中定义了自己的数据源 bean 时，自动配置所提供的 HSQLDB 就不会生效。这给予了开发人员很大的灵活性。既可以快速的创建一个可以立即运行的原型应用，又可以不断的修改和调整以适应应用开发在不同阶段的需要。可能在应用最开始的时候，嵌入式的内存数据库（如 HSQLDB）就足够了，在后期则需要换成 MySQL 等数据库。Spring Boot 使得这样的切换变得很简单
## spring boot外部配置
在应用中管理配置并不是一个容易的任务，尤其是在应用需要部署到多个环境中时。通常会需要为每个环境提供一个对应的属性文件，用来配置各自的数据库连接信息、服务器信息和第三方服务账号等。通常的应用部署会包含开发、测试和生产等若干个环境。不同的环境之间的配置存在覆盖关系。测试环境中的配置会覆盖开发环境，而生产环境中的配置会覆盖测试环境。Spring 框架本身提供了多种的方式来管理配置属性文件。Spring 3.1 之前可以使用 PropertyPlaceholderConfigurer。Spring 3.1 引入了新的环境（Environment）和概要信息（Profile）API，是一种更加灵活的处理不同环境和配置文件的方式。不过 Spring 这些配置管理方式的问题在于选择太多，让开发人员无所适从。Spring Boot 提供了一种统一的方式来管理应用的配置，允许开发人员使用属性文件、YAML 文件、环境变量和命令行参数来定义优先级不同的配置值。
Spring Boot 所提供的配置优先级顺序比较复杂。按照优先级从高到低的顺序，具体的列表如下所示。
* 命令行参数。
* 通过 System.getProperties() 获取的 Java 系统参数。
* 操作系统环境变量。
* 从java:comp/env 得到的 JNDI 属性。
* 通过 RandomValuePropertySource 生成的“random.*”属性。
* 应用 Jar 文件之外的属性文件。
* 应用 Jar 文件内部的属性文件。
在应用配置 Java 类（包含`@Configuration`注解的 Java 类）中通过`@PropertySource`注解声明的属性文件。
通过`SpringApplication.setDefaultProperties`声明的默认属性。
Spring Boot 的这个配置优先级看似复杂，其实是很合理的。比如命令行参数的优先级被设置为最高。这样的好处是可以在测试或生产环境中快速地修改配置参数值，而不需要重新打包和部署应用。
SpringApplication 类默认会把以“--”开头的命令行参数转化成应用中可以使用的配置参数，如 “--name=Alex” 会设置配置参数 “name” 的值为 “Alex”。如果不需要这个功能，可以通过 “SpringApplication.setAddCommandLineProperties(false)” 禁用解析命令行参数。
## spring boot属性文件
属性文件是最常见的管理配置属性的方式。Spring Boot 提供的 SpringApplication 类会搜索并加载 application.properties 文件来获取配置属性值。SpringApplication 类会在下面位置搜索该文件
* 当前目录的“/config”子目录
* 当前目录
* classpath 中的“/config”包
* classpath
上面的顺序也表示了该位置上包含的属性文件的优先级。优先级按照从高到低的顺序排列。可以通过`spring.config.name`配置属性来指定不同的属性文件名称。也可以通过`spring.config.location`来添加额外的属性文件的搜索路径。如果应用中包含多个 profile，可以为每个 profile 定义各自的属性文件，按照`application-{profile}`来命名，对于配置属性，可以通过`@Value`来配置属性
## YAML
相对于属性文件来说，YAML 是一个更好的配置文件格式。YAML 在 Ruby on Rails 中得到了很好的应用。SpringApplication 类也提供了对 YAML 配置文件的支持，只需要添加对 SnakeYAML 的依赖即可。
```javascript
spring:
  profiles:dev
db:
  url:jdbc:mysql://localhost
```
 除了使用`@Value`注解绑定配置属性值之外，还可以使用更加灵活的方式
```javascript
@Component
@ConfigurationProperties(prefix="db")
public class DBSettings {
 private String url;
 private String username;
 private String password;
}
```
通过`@ConfigurationProperties(prefix="db")`注解，配置属性中以“db”为前缀的属性值会被自动绑定到 Java 类中同名的域上，如 url 域的值会对应属性“db.url”的值。只需要在应用的配置类中添加`@EnableConfigurationProperties`注解就可以启用该自动绑定功能
