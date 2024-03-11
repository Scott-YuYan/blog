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
 
 
 ##### SpringAOP示例：
 
 1.添加依赖
 
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-aop</artifactId>
    </dependency>
 
 2.创建切面类
 
    @Aspect
    public class MyAspect {
 
        @Before("execution(* com.example.myapp.service.*.*(..))")
        public void beforeServiceMethodExecution() {
            // 执行前置通知的逻辑
        }
    }
    
    或者
    
    @Aspect
    @Component
    public class LogAspect {
       
        private static Logger logger = LoggerFactory.getLogger(LogAspect.class);
    
        @Pointcut("execution(public * com.example.service.HelloService.*(..))")
        public void log() {
       
       }
    
        @Before("log()")
        public void doBefore(JoinPoint joinPoint) {
            logger.info("method {} start", joinPoint.getSignature().getName());
        }
    
        @AfterReturning(returning = "result", pointcut = "log()")
        public void doAfterReturning(Object result) {
            logger.info("method return value: {}", result);
            logger.info("method end");
        }
    
        @AfterThrowing(throwing = "ex", pointcut = "log()")
        public void doAfterThrowing(Throwable ex) {
            logger.error("method throw exception: {}", ex.getMessage());
        }
    }
    
  3.配置 AOP 启用： 在 Spring Boot 的配置类中，使用@EnableAspectJAutoProxy注解启用 AOP 功能。
  
     import org.springframework.context.annotation.Configuration;
     import org.springframework.context.annotation.EnableAspectJAutoProxy;
  
     @Configuration
     @EnableAspectJAutoProxy
     public class AppConfig {
         // 配置其他 Bean 和应用程序设置
     }
     
     
     
  Aspect：切面，由一系列切点、增强和引入组成的模块对象，可定义优先级，从而影响增强和引入的执行顺序。事务管理（Transaction management）在java企业应用中就是一个很好的切面样例。所以他不是一个被代理的对象。 
   
    
   Join point：接入点，程序执行期的一个点，例如方法执行、类初始化、异常处理。 在Spring AOP中，接入点始终表示方法执行。
   Advice：增强，切面在特定接入点的执行动作，包括 “around,” “before” and "after"等多种类型。包含Spring在内的许多AOP框架，通常会使用拦截器来实现增强，围绕着接入点维护着一个拦截器链。 
   
    
   Pointcut：切点，用来匹配特定接入点的谓词（表达式），增强将会与切点表达式产生关联，并运行在任何切点匹配到的接入点上。通过切点表达式匹配接入点是AOP的核心，Spring默认使用AspectJ的切点表达式。 
   
    
   Introduction：引入，为某个type声明额外的方法和字段。Spring AOP允许你引入任何接口以及它的默认实现到被增强对象上。 
   
    
   Target object：目标对象，被一个或多个切面增强的对象。也叫作被增强对象。既然Spring AOP使用运行时代理（runtime proxies），那么目标对象就总是代理对象。 
   
    
   AOP proxy：AOP代理，为了实现切面功能一个对象会被AOP框架创建出来。在Spring框架中AOP代理的默认方式是：有接口，就使用基于接口的JDK动态代理，否则使用基于类的CGLIB动态代理。但是我们可以通过设置proxy-target-class="true"，完全使用CGLIB动态代理。 
   
    
   Weaving：织入，将一个或多个切面与类或对象链接在一起创建一个被增强对象。织入能发生在编译时 （compile time ）(使用AspectJ编译器)，加载时（load time），或运行时（runtime） 。Spring AOP默认就是运行时织入，可以通过枚举AdviceMode来设置。
 
  ##### Spring AOP支持以下几种通知类型：
        
  前置通知（Before advice）：在目标方法执行前执行的通知。
  后置通知（After returning advice）：在目标方法成功执行后执行的通知，若发生异常则不通知。
  环绕通知（Around advice）：包围目标方法执行的通知，在目标方法执行前后都可以执行自定义操作。
  后置异常通知（After throwing advice）：在目标方法抛出异常后执行的通知。
  后置最终通知（After advice）：在目标方法执行后无论是否发生异常都会执行的通知。
 
  ##### Spring 注解 @After,@Around,@Before 的执行顺序是？
  
  @Around→@Before→@Around→@After执行 ProceedingJoinPoint.proceed() 之后的操作→@AfterRunning(如果有异常→@AfterThrowing)
  
  
  ##### 在Spring MVC中，拦截器需要实现HandlerInterceptor接口，并且需要在配置文件中进行注册和配置。
        
   HandlerInterceptor接口定义了三个方法，分别是：
   
   preHandle：在请求处理之前进行调用，返回一个布尔值。可以通过返回true或false来决定是否继续执行请求。
   
   postHandle：在请求处理之后（Controller方法调用之后）但在视图渲染之前调用，可以对ModelAndView进行操作。
   
   afterCompletion：在整个请求处理完毕后进行调用，即在视图渲染完毕后进行调用，主要用于资源清理工作。
   
   通过实现HandlerInterceptor接口，并重写这三个方法，可以自定义拦截器的行为，并在配置文件中进行注册和配置，从而实现对请求的拦截和处理。