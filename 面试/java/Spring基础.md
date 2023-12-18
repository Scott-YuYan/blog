#### Spring事务的七种传播行为
![image](https://user-images.githubusercontent.com/51253421/134360889-9e663ca2-acde-4ca8-b3ad-9f478fd11b14.png)

##### Spring 框架的核心概念

Spring 框架是一个轻量级的、开源的框架，提供了大量的功能来帮助开发人员构建企业级应用程序。Spring 框架的核心概念包括：

控制反转（IoC）：控制反转是 Spring 框架的核心原则之一。它通过将对象的创建和管理交给 Spring 容器来实现，而不是由应用程序代码直接创建对象。这样可以降低组件之间的耦合度，并且更好地支持模块化开发。

依赖注入（DI）：依赖注入是控制反转的一种实现方式，它用于管理对象之间的依赖关系。Spring 框架通过依赖注入来向目标对象提供其所依赖的其他对象或数值。

面向切面编程（AOP）：面向切面编程是一种编程范式，用于将横切关注点（如日志、事务管理等）从应用程序主体逻辑中分离出来。Spring 框架提供了强大的 AOP 功能，能够帮助开发者更好地管理应用程序的横切关注点。
 
##### Spring、SpringBoot用到了哪些设计模式

单例模式（Singleton Pattern）：Spring容器中管理的Bean默认是单例的，即每个Bean只会有一个实例，这样可以节省资源并提高性能。

工厂模式（Factory Pattern）：Spring使用工厂模式创建和管理Bean，通过配置文件或注解来实现，例如通过XML配置文件创建Bean的工厂模式。

代理模式（Proxy Pattern）：Spring AOP（面向切面编程）基于代理模式实现，使用动态代理技术为目标对象生成代理对象，在方法执行前后添加额外的逻辑，如事务管理、日志记录等。

观察者模式（Observer Pattern）：Spring的事件机制基于观察者模式，通过事件监听器（Listener）和发布者（Publisher）来实现事件的发布和订阅，以实现模块之间的解耦。

适配器模式（Adapter Pattern）：Spring MVC框架使用适配器模式来处理请求，将不同类型的请求适配到相应的处理器方法上。

模板方法模式（Template Method Pattern）：Spring的JdbcTemplate使用了模板方法模式，将重复的数据库操作封装在模板方法中，而具体的实现由子类来完成。

建造者模式（Builder Pattern）：Spring的RestTemplate等工具类使用了建造者模式，通过链式调用的方式构建复杂的对象。

责任链模式（Chain of Responsibility Pattern）：Spring Security框架中的过滤器链使用了责任链模式，处理请求的过程中，每个过滤器都有机会对请求进行处理或者传递给下一个过滤器。


 ##### Spring Bean的生命周期？
 
 Spring对bean进行实例化；
 
 Spring将值和bean的引用注入到bean对应的属性中；
 
 如果bean实现了BeanNameAware接口，Spring将bean的ID传递给setBean-Name()方法；
 
 如果bean实现了BeanFactoryAware接口，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入；
 
 如果bean实现了ApplicationContextAware接口，Spring将调用setApplicationContext()方法，将bean所在的应用上下文的引用传入进来；
 
 如果bean实现了BeanPostProcessor接口，Spring将调用它们的post-ProcessBeforeInitialization()方法；
 
 如果bean实现了InitializingBean接口，Spring将调用它们的after-PropertiesSet()方法。类似地，如果bean使用initmethod声明了初始化方法，该方法也会被调用；
 
 如果bean实现了BeanPostProcessor接口，Spring将调用它们的post-ProcessAfterInitialization()方法；
 
 此时，bean已经准备就绪，可以被应用程序使用了，它们将一直驻留在应用上下文中，直到该应用上下文被销毁；
 
 如果bean实现了DisposableBean接口，Spring将调用它的destroy()接口方法。同样，如果bean使用destroy-method声明了销毁方法，该方法也会被调用。
 
  
 ##### Spring Bean的作用域？
 
 Spring框架支持以下五种bean的作用域：
 
 singleton : bean在每个Spring ioc 容器中只有一个实例。
 prototype：一个bean的定义可以有多个实例。
 request：每次http请求都会创建一个bean，该作用域仅在基于web的Spring ApplicationContext情形下有效。
 session：在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。
 global-session：在一个全局的HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。
 
 注意： 缺省的Spring bean 的作用域是Singleton。使用 prototype 作用域需要慎重的思考，因为频繁创建和销毁 bean 会带来很大的性能开销。
 
 
 
 ##### spring 的事务隔离？
 
 spring 有五大隔离级别，默认值为 ISOLATION_DEFAULT（使用数据库的设置），其他四个隔离级别和数据库的隔离级别一致：
 
 ISOLATION_DEFAULT：用底层数据库的设置隔离级别，数据库设置的是什么我就用什么；
 
 ISOLATION_READ_UNCOMMITTED：未提交读，最低隔离级别、事务未提交前，就可被其他事务读取（会出现幻读、脏读、不可重复读）；
 
 ISOLATION_READ_COMMITTED：提交读，一个事务提交后才能被其他事务读取到（会造成幻读、不可重复读），SQL server 的默认级别；
 
 ISOLATION_REPEATABLE_READ：可重复读，保证多次读取同一个数据时，其值都和事务开始时候的内容是一致，禁止读取到别的事务未提交的数据（会造成幻读），MySQL 的默认级别；
 
 ISOLATION_SERIALIZABLE：序列化，代价最高最可靠的隔离级别，该隔离级别能防止脏读、不可重复读、幻读。
 
 脏读（Dirty Read）：脏读指的是一个事务读取了另一个事务还未提交的数据。如果该事务回滚，那么读取到的数据就是无效的，因此称之为“脏读”。
 
 不可重复读（Non-repeatable Read）：不可重复读指的是在同一个事务中，多次读取同一数据，但得到的结果不一致。这是因为在两次读取之间，另一个事务修改了数据。
 
 幻读（Phantom Read）：幻读指的是在同一个事务中，多次执行同一个查询，但得到的结果集不一致。幻读与不可重复读的区别在于，幻读是由于另一个事务插入或删除数据导致的，而不是修改数据。
 
  ##### Spring 注解 @After,@Around,@Before 的执行顺序是？
  
  @Around→@Before→@Around→@After执行 ProceedingJoinPoint.proceed() 之后的操作→@AfterRunning(如果有异常→@AfterThrowing)