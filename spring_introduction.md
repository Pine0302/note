### Spring 入门
> Java Config 相关注解
+ @Configration ：当前java类是一个配置类
+ @ImportResource : 用来注入一些xml等配置文件
+ @ComponentScan : 告诉我们可以扫描整个Spring容器下面的哪些package下面的bean
+ @Bean：方法被标注了bean的注解之后，表明它的返回可以作为Spring的bean的配置，存在于整个Spring的Application Context中
+ @ConfigrationProperies: 在 Spring Boot 项目中,我们要求更灵活的配置，更好的模块化整合,为满足以上要求，我们将大量的参数配置在 application.properties 或 application.yml 文件中，通过 @ConfigurationProperties 注解，我们可以方便的获取这些参数值
> Bean的定义相关注解
+ @Component/@Repository/@Service
+ @Controller/@RestController
+ @RequestMapping : 帮助定义类下面的方法是在哪些url下面，做一个映射
> 注入相关注解
+ @AutoWired/@Qualifier/@Resource
+ @Value

>数据库连接池
> + 数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个
> > druid 监控
> + 1.基于Filter－Chain模式的插件体系。 
> + 2.DruidDataSource 高效可管理的数据库连接池。
> + 3.SQLParser
> > HiKariCP
> + 快

> Spring的JDBC(Java Database Connectivity)操作类
> + core，jdbcTemplete 等相关核心接口和类
> + datasource，数据源相关的辅助类
> + object，将基本的JDBC操作封装成对象
> + support，错误码等其他辅助工具

>spring的事务抽象
>> 一致的事务模型
> + JDBC/Hibernate/myBatis
> + DataSource/JTA

+ 如果你想定制一些spring的组件的特性，在spring boot中，打开他的自动配置，看看他的自动配置是怎么做的，有没有给你留一些口子，让你来定义这些bean
