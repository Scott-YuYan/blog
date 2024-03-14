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
   
   ##### Spring支持了7种传播机制，分别为：
   
   下面是 Spring 事务的传播机制：
   
   ```
   REQUIRED（默认值）：如果当前方法已经运行在一个事务中，则加入该事务；否则新建一个事务，并在该方法执行期间一直使用该事务。
   SUPPORTS：如果当前方法正在一个事务中运行，则加入该事务；否则以非事务性的方式继续运行。
   MANDATORY：如果当前方法正在一个事务中运行，则加入该事务；否则抛出异常。
   REQUIRES_NEW：无论当前方法是否正在一个事务中运行，都将新开一个事务，并在该方法执行期间只使用该事务。
   NOT_SUPPORTED：以非事务性的方式运行，并挂起任何可能存在的事务。
   NEVER：以非事务性的方式运行，并抛出异常，如果当前方法正在一个事务中运行。
   NESTED：如果当前方法正在一个事务中运行，则在该事务的嵌套事务中执行；否则则与 REQUIRED 行为相同。
   需要注意的是，在使用嵌套事务（NESTED）时，如果外层事务回滚，则内层嵌套事务也会回滚；而内层事务的回滚不会影响外层事务的状态。
   ```
   上面不支持事务的传播机制为：PROPAGATION_SUPPORTS，PROPAGATION_NOT_SUPPORTED，PROPAGATION_NEVER。如果配置了这三种传播方式的话，在发生异常的时候，事务是不会回滚的。
   
   
   嵌套事务：嵌套事务
        
   上面是我想同时回滚UserService与UserService1。但是也会有这种场景只想回滚UserService1中报错的数据库操作，不影响主逻辑UserService中的数据落库。
   
   有两种方式可以实现上述逻辑：
   
   1.直接在UserService1内的整个方法用try/catch包住
   
   2.在UserService1使用Propagation.REQUIRES_NEW传播机制
   ```
       @Transactional(rollbackFor = Exception.class,propagation = Propagation.REQUIRES_NEW)
   ```

##### Spring bean的线程安全问题

在Spring框架中，通常情况下Spring的Bean是单例模式的，也就是说默认情况下Spring容器中的Bean都是共享的。因此，如果一个Bean的方法中存在对共享资源的修改操作，就可能存在线程安全问题。

为了解决这个问题，可以采取以下几种方式：

避免在Bean中定义可变的状态：尽量避免在Bean中定义可变的状态，或者将可变的状态转移到方法的局部变量中。
使用局部变量：尽量使用方法中的局部变量，而不是实例变量，避免多个线程共享状态。
使用ThreadLocal：如果确实需要在Bean中保存状态，可以考虑使用ThreadLocal来保证每个线程都有自己独立的状态副本。
使用同步控制：在必要的时候可以使用synchronized关键字或者Lock来进行同步控制，确保多个线程访问共享资源的安全性。

##### 关于BeanFactory和FactoryBean

BeanFactory是Spring框架中定义的最基本的接口，它是一个大型工厂类，负责管理bean的配置、创建和维护。BeanFactory的主要功能包括：实例化bean、配置bean、装配bean之间的依赖关系、提供bean的生命周期管理等。在使用时，可以通过ApplicationContext接口来获取BeanFactory实例，ApplicationContext是BeanFactory的子接口，提供更多功能和更方便的使用方式。

FactoryBean是一个特殊的bean，实现了FactoryBean接口的bean在Spring容器中会被当作工厂bean处理。FactoryBean接口定义了一个方法getObject()，用于返回一个对象实例，这个对象实例将会被纳入Spring容器的管理范围。FactoryBean的作用是提供一种更加灵活的方式来配置和创建bean，通过FactoryBean可以实现复杂的bean的创建逻辑，例如在获取bean实例前进行一些初始化操作，或者返回不同的实例对象。常见的应用场景包括数据库连接池、缓存管理等。

##### Spring Cloud和Dubbo有什么区别？

生态系统和社区支持：

Spring Cloud是基于Spring生态系统构建的，拥有非常庞大的社区支持和活跃的开发者社区。它集成了Spring框架的各种强大功能，并提供了丰富的解决方案和组件，如服务注册与发现、配置管理、负载均衡等。
Dubbo是由阿里巴巴开源的分布式服务框架，它也有一定的社区支持，但相对于Spring Cloud来说相对较小。
服务注册与发现：

Spring Cloud使用Eureka作为默认的服务注册与发现组件，它提供了高可用性、可水平扩展的服务注册与发现能力。
Dubbo使用Zookeeper或Nacos作为服务注册中心，同样可以实现服务的注册与发现。
通信协议：

Spring Cloud支持多种通信协议，如HTTP、REST等，可以基于HTTP协议进行服务之间的通信。
Dubbo使用RPC（远程过程调用）作为通信协议，通过高性能的序列化技术实现跨进程的方法调用。
开发模型：

Spring Cloud使用Spring Boot作为基础，通过注解驱动的方式简化了微服务的开发和部署。
Dubbo更加侧重于服务治理和高性能，提供了更多底层的细粒度配置和扩展点。
需要注意的是，Spring Cloud和Dubbo并不是互斥的选择，它们可以结合使用。在某些场景下，可以利用它们各自的优势来构建更强大和灵活的分布式系统。


##### 服务雪崩 服务降级 服务熔断 服务限流
      
服务雪崩（Service Avalanche）：
当一个服务出现故障或不可用时，其他依赖该服务的服务也会受到影响，导致整个系统出现大规模的故障。这种现象被称为服务雪崩。

为了避免服务雪崩，可以采取以下措施：
引入服务注册与发现机制，及时发现并移除不可用的服务。
通过设置合理的超时时间和重试策略来处理服务调用的超时或错误。
采用负载均衡策略，将请求分散到多个实例上，减少单点故障的影响。

服务降级（Service Degradation）：
当系统负载过高或某个服务出现故障时，为了保证核心功能的正常运行，可以主动暂时关闭一些非核心或耗时较长的功能，以减轻系统的压力。这就是服务降级。

通常可以使用以下方法进行服务降级：
返回默认值或静态数据，而不是真实的结果。
返回缓存的数据。
返回预设的错误码或异常。

服务熔断（Circuit Breaking）：
服务熔断是一种防止故障蔓延的机制。当某个服务的错误率或响应时间超过一定阈值时，可以将该服务的请求熔断，不再向该服务发送请求，而是直接返回错误响应。这种方式可以避免连锁式的故障扩散，并且能够快速恢复。一般来说，服务熔断的实现需要结合监控和自动恢复机制。

服务限流（Rate Limiting）：
服务限流是一种控制系统访问频率的机制，用于保护系统免受过多请求的影响。通过限制每个服务的最大并发数或请求频率，可以防止系统被过多的请求压垮。常见的限流算法有漏桶算法和令牌桶算法

##### Spring 事务（注解 @Transactional）失效的12种场景及代码示例

* 1.事务管理器未被正确配置
如果没有正确配置事务管理器，则 @Transactional 注解将不起作用。

  Java 配置事务管理器有几种方式以及对应的代码示例：
  
  1.1 基于 XML 配置
  在基于 XML 配置的方式中，需要在 Spring 配置文件中定义一个 DataSource，然后配置一个 PlatformTransactionManager 实例并将其绑定到 DataSource 上。
  
  下面是一个示例代码：
  
  ```
  <!-- 配置数据源 -->
  <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
      <property name="driverClassName" value="${jdbc.driver}" />
      <property name="url" value="${jdbc.url}" />
      <property name="username" value="${jdbc.username}" />
      <property name="password" value="${jdbc.password}" />
  </bean>
  
  <!-- 配置事务管理器 -->
  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
      <property name="dataSource" ref="dataSource" />
  </bean>
  ```

1.2 基于注解配置
  在基于注解配置的方式中，需要在配置类中定义一个 DataSource，然后使用 @EnableTransactionManagement 注解开启事务管理，并通过 @Bean 注解配置一个 PlatformTransactionManager 实例并将其绑定到 DataSource 上。
  
  下面是一个示例代码：
  
  ```
@Configuration
@EnableTransactionManagement
public class AppConfig {

    @Value("${jdbc.driver}")
    private String driver;

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;

    // 配置数据源
    @Bean
    public DataSource dataSource() {
        BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName(driver);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }

    // 配置事务管理器
    @Bean
    public PlatformTransactionManager transactionManager() {
        return new DataSourceTransactionManager(dataSource());
    }
}

```
需要注意的是，在使用基于注解配置的方式时，配置类中需要开启事务管理，否则事务将无法生效。可以通过在配置类上添加 @EnableTransactionManagement 注解来开启事务管理。


2. 方法必须是 public 的
Spring 只能拦截公共方法上的注解，因此必须确保使用 @Transactional 注解的方法是 public 的。

3. 方法必须从另一个 bean 中调用才能工作
在同一个 bean 中调用带有 @Transactional 注解的方法将不会触发事务，因为 Spring 无法通过 AOP 代理来拦截该调用。

例如：

```
    @Override
    @Transactional(rollbackFor = Exception.class)
    @SneakyThrows
    public void creatUser(){
        this.creatUser1();
    }

    @Override
    @Transactional(rollbackFor = Exception.class,propagation = Propagation.REQUIRES_NEW)
    public void creatUser1(){

    }
```
并且createUser1方法上设置了事务传播策略为：REQUIRES_NEW，但是因为是内部直接调用，createUser1不能不代理处理，无法进行事务管理。
解决方法：

方式1：新建一个Service，将方法迁移过去，有点麻瓜。

方式2：在当前类注入自己，调用createUser1时通过注入的userService调用
```
    @Autowired
    UserService userService;

    @Override
    @Transactional(rollbackFor = Exception.class)
    @SneakyThrows
    public void creatUser(){
        userService.creatUser1();
    }
```
方式3：通过AopContext.currentProxy()获取代理对象，与方法2类似
```
    (UserService)(AopContext.currentProxy()).creatUser1()
```

4. 异常被吞噬
如果在 @Transactional 注解标记的方法中抛出了异常，并且异常被捕获并处理了，那么事务可能不会被正确回滚。应该避免在带有 @Transactional 注解的方法中吞噬异常。

示例代码：

```
@Transactional
public void doSomething() {
    try {
        // Some code that throws an exception
    } catch (Exception e) {
        // Exception is caught and not rethrown, so transaction will not roll back
    }
}
```


5. 事务只对受检查的异常起作用
默认情况下，@Transactional 注解只对受检查的异常回滚事务，而不对未受检查的异常（例如 IOException）回滚事务。因此，应该在方法上声明 throws Exception，以便将所有异常都视为受检查的异常。

示例代码：
```
@Transactional(rollbackFor = Exception.class)
public void doSomething() throws Exception {
    // ...
}
```

6. 被final、static关键字修饰的类或方法
CGLIB是通过生成目标类子类的方式生成代理类的，被final、static修饰后，无法继承父类与父类的方法。

示例代码：
```
@Transactional
public final void doSomething() {
    // ...
}
```

7、
将注解标注在接口方法上

@Transactional是支持标注在方法与类上的。一旦标注在接口上，对应接口实现类的代理方式如果是CGLIB，将通过生成子类的方式生成目标类的代理，将无法解析到@Transactional，从而事务失效。

  
8、如果一个事务嵌套在另一个事务中，则内部事务可能会被忽略
当嵌套使用事务时，必须注意事务传播属性和隔离级别。

示例代码：

```
@Transactional(propagation = Propagation.REQUIRED)
public void doSomething() {
    // This will start a new transaction
    doSomethingElse();
}

@Transactional(propagation = Propagation.REQUIRES_NEW)
public void doSomethingElse() {
    // This is a new, independent transaction
}
```

9、不同的数据源之间的事务
如果在应用程序中使用了多个数据源，则需要使用特殊的策略来管理跨数据源的事务。例如，可以使用 JTA 管理器来跨多个数据源进行事务管理。

示例代码：

```
@Bean
public JtaTransactionManager transactionManager() {
    return new JtaTransactionManager();
}
```

10、多线程场景

事务信息是跟线程绑定的。

因此在多线程环境下，事务的信息都是独立的，将会导致Spring在接管事务上出现差异。

11、数据库本身不支持事务
   
   比如Mysql的Myisam存储引擎是不支持事务的，只有innodb存储引擎才支持。
   
   这个问题出现的概率极其小，因为Mysql5之后默认情况下是使用innodb存储引擎了。
   
   但如果配置错误或者是历史项目，发现事务怎么配都不生效的时候，记得看看存储引擎本身是否支持事务。
  
 

##### Spring MVC 系列之拦截器 Interceptor

拦截器在 Spring MVC 中使用接口 HandlerInterceptor 表示，这个接口包含了三个方法：preHandle、postHandle、afterCompletion，
三个方法具体的执行流程如下。

preHandle：处理器执行之前执行，如果返回 false 将跳过处理器、拦截器 postHandle 方法、视图渲染等，直接执行拦截器 afterCompletion 方法。
postHandle：处理器执行后，视图渲染前执行，如果处理器抛出异常，将跳过该方法直接执行拦截器 afterCompletion 方法。
afterCompletion：视图渲染后执行，不管处理器是否抛出异常，该方法都将执行。

那么拦截器的顺序是如何指定的呢？

对于 xml 配置来说，Spring 将记录 bean 声明的顺序，先声明的拦截器将排在前面。
对于注解配置来说，由于通过反射读取方法无法保证顺序，因此需要在方法上添加@Order注解指定 bean 的声明顺序。
对应API配置来说，拦截器的顺序并非和添加顺序完全保持一致，为了控制先后顺序，需要自定义的拦截器实现Ordered接口。



