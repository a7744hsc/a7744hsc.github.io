1. spring解决了什么问题？
支持web，事务支持，
快速开发和分布式。！！
消息处理（与JMS，RabbitMQ，Kafka集成）。
数据持久层（JPA）
Container？？？？？


spring核心：IoC，AOP
Ioc：由系统来控制依赖之间的关系
Aop：将通用功能与业务逻辑分开（日志，安全等）


Spring问题： 配置复杂，过时技术被引用，同一功能有多种实现方式（配置or注解）；与第三方工具兼容性，不具备热部署功能

SpringBoot优点：约定大于配置，需哟的配置项少。提供内置容器（jetty 或Tomcat）

Hibernate，实现了JPA，自动将数据库表加工成对象。

@Controller springMVC中标记类用来处理web请求。
@Controller @Service @Configuration等可以标记Bean，用于给@Autowired标记的对象进行注入。
@Bean加到方法上代表方法返回值是一个Spring容器管理的Bean

@PostConstruct和@PreDestroy可以在Bean的声明周期中执行

@Autowired通过类型绑定Bean， @Qualifier可以通过名字绑定bean

@PathVAriable可以作用在方法上，又来捕获http地址上的参数
@InitBinder可以用来扩展参数绑定器
参数，类变量等都可以校验，@NotNull@Size等，可以通过组来管理校验规则
使用@validagted可以触发校验
@Configuration可以通过实现接口来配置应用。
@Transactional 标记类或者方法为事务

@OnetoMany用mappedBy字段来确定映射关系

CrudRepository基础repository，PagingAndSortingRepository 带翻页功能，JPARepository JPA专用，功能更丰富

@ControllerAdvice  统一处理所有RequestMap方法抛出的异常。

@ResponseBody 和@ResponseStatus能帮助构建http请求返回的对象
Repository能根据方法名来构建SQL语句，也可以通过@Quary来手动指定SQL语句

读取配置可以使用@Value绑定到变量或者使用Environment对象（使用Autowired注入到需要的Bean中），
也可以使用@ConfigurationProperties将配置映射到类上。配置文件可以使用标准的.properties文件，也可以使用yml变种

使用@Configuration +@Bean是配置spring的常用方法。
@ConditionalOnBean，@ConditionalOnProperty可以按条件创建Bean

@Profile注解可以将bean设定为只对指定配置生效
Swagger：Restful描述文档生成。



tip：
spring包含许多spring-boot-starter-xxx的包来快速提供服务。