
##### 面向对象设计的六大原则？

1 单一职责（Single Responsibility Principle）
这个原则顾名就可以思义，就是一个类应该只负责一个职责

2 开闭原则（Open Close Principle）
提倡一个类一旦开发完成，后续增加新的功能就不应该通过修改这个类来完成，而是通过继承，增加新的类。

3 里氏替换原则（Liskov Substitution Principle）
所有引用基类（父类）的地方必须能透明地使用其子类的对象。

4 依赖倒置原则（Dependence Inversion Principle）
抽象不应该依赖于细节，细节应当依赖于抽象。换言之，要针对接口编程，而不是针对实现编程。

5 接口隔离原则（Interface Segregation Principle）
使用多个专门的接口，而不使用单一的总接口，即客户端不应该依赖那些它不需要的接口。

6 迪米特原则（Law of Demeter 又名Least Knowledge Principle）
一个软件实体应当尽可能少地与其他实体发生相互作用。

##### jdk 性能、有用工具
jmh ：专门用于代码微基准测试的工具套件
JProfiler
JConsole ：它用于对JVM中内存，线程和类等的监控



 #####  方法的重写要满足：三同一大一小,三同：方法名相同，返回值类型，形参相同,一大：访问权限大于等于重写前，一小：抛出异常范围小于等于重写前。
 
 ##### Java8之后，接口跟抽象类的区别？
  * Java8之后，接口中可以有默认方法(default)，静态方法(static)，但是不能写构造方法，而抽象类则可以有构造方法，说明构造方法是参与类的实例化过程的。
  * 抽象类可以有自己的成员变量，并且可以通过非抽象方法进行改变，而接口中的变量只能是 public static final的，不能被外部修改。
  * 接口可以多继承、抽象类只能单继承。
  ##### java 基本数据类型
  整型 long(8) int(4) short(2) byte(1)
  浮点 double(8) float(4)
  字符 char(1)
  boolean(1)
  
 ##### public protected default private 区别
 
 <img width="643" alt="image" src="https://github.com/Scott-YuYan/blog/assets/51253421/dad1dcbf-0d8d-4e5a-9c53-4f209ef31dc9">
 
 
 ##### Java OOP的概念
 
 面向对象编程（Object-Oriented Programming，OOP）是一种程序设计范式，它以对象为中心，通过封装、继承和多态等特性来组织代码。在Java中，OOP是一种核心编程思想，下面是一些关键概念：
 
 类和对象：类是对象的模板，描述了对象的属性和行为；对象是类的实例，具体化了类的属性和行为。
 
 封装：封装是指将对象的状态（属性）和行为（方法）作为一个整体来处理，隐藏内部细节，只暴露必要的接口。
 
 继承：继承允许一个类（子类）基于另一个类（父类）的定义来构建，子类可以继承父类的属性和方法，并且可以添加新的属性和方法。
 
 多态：多态性允许不同类的对象对同一消息作出响应，即不同对象可以对相同的消息做出不同的响应。
 
 
 ##### HTTPS是如何保证传输安全的？
 
 HTTP协议不安全的原因在于存在"中间人攻击"，中间人能看到并且修改 HTTP 通讯中所有的请和响应内容，所以使用 HTTP 是非常的不安全的。
 HTTPS 其实是SSL+HTTP的简称,当然现在SSL基本已经被TLS取代了，不过接下来我们还是统一以SSL作为简称，SSL协议其实不止是应用在HTTP协议上，还在应用在各种应用层协议上，例如：FTP、WebSocket。
 
 其实SSL协议大致就和上一节非对称加密的性质一样，握手的过程中主要也是为了交换秘钥，然后再通讯过程中使用对称加密进行通讯。
 
 ##### 什么是类？
 
 抽象：抽象是将一类对象的共同特征总结出来构造类的过程，包括数据抽象和行为抽象两方面。抽象只关注对象有哪些属性和行为，并不关注这些行为的细节是什么。
 
 继承：继承是从已有类得到继承信息创建新类的过程。提供继承信息的类被称为父类（超类、基类）；得到继承信息的类被称为子类（派生类）。继承让变化中的软件系统有了一定的延续性，同时继承也是封装程序中可变因素的重要手段（如果不能理解请阅读阎宏博士的《Java与模式》或《设计模式精解》中关于桥梁模式的部分）。
 
 封装：通常认为封装是把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。面向对象的本质就是将现实世界描绘成一系列完全自治、封闭的对象。我们在类中编写的方法就是对实现细节的一种封装；我们编写一个类就是对数据和数据操作的封装。可以说，封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口（可以想想普通洗衣机和全自动洗衣机的差别，明显全自动洗衣机封装更好因此操作起来更简单；我们现在使用的智能手机也是封装得足够好的，因为几个按键就搞定了所有的事情）。
 
 多态性：多态性是指允许不同子类型的对象对同一消息作出不同的响应。简单的说就是用同样的对象引用调用同样的方法但是做了不同的事情。多态性分为编译时的多态性和运行时的多态性。方法重载（overload）实现的是编译时的多态性（也称为前绑定），而方法重写（override）实现的是运行时的多态性（也称为后绑定）。


##### 集群高并发情况下如何保证分布式唯一全局ID生成

在复杂分布式系统中，往往需要对大量的数据和消息进行唯一标识

ID生成规则部分硬性要求
 * 全局唯一
 * 趋势递增 在MySQL的InnoDB引擎中使用的是聚集索引，由于多数RDBMS使用Btree的数据结构来存储索引，在主键的选择上面我们应该尽量使用有序的主键保证写入性能
 * 单调递增 保证下一个ID一定大于上一个ID，例如事务版本号、IM增量消息、排序等特殊需求
 * 信息安全 如果ID是连续，恶意用户的爬取工作就非常容易做了，直接按照顺序下载指定URL即可，如果是订单号就危险了，竞争对手可以直接知道我们一天的单量，所以在一些应用场景下，需要ID无规则不规则，让竞争对手不好猜
 * 含时间戳 一样能够快速在开发中了解这个分布式ID什么时候生成的

ID号生成系统的可用性要求
 * 高可用 发布一个获取分布式ID请求，服务器就要保证99.999%的情况下给我创建一个唯一分布式ID
 * 低延迟 发一个获取分布式ID的请求，服务器就要快，极速
 * 高QPS 例如并发一口气10万个创建分布式ID请求同时杀过来，服务器要顶得住且一下子成功创建10万个分布式ID

一般通用解决方法：
* UUID.randomUUID() , UUID的标准型包含32个16进制数字，以连字号分为五段，形式为 8-4-4-4-12的36个字符，性能非常高，本地生成，没有网络消耗。
入数据库性能差，因为UUID是无序的
无序，无法预测他的生成顺序，不能生成递增有序的数字
首先分布式id一般都会作为主键，但是按照mysql官方推荐主键尽量越短越好，UUID每一个都很长，所以不是很推荐。
索引，B+树索引的分裂
既然分布式ID是主键，然后主键是包含索引的，而mysql的索引是通过B+树来实现的，每一次新的UUID数据的插入，为了查询的优化，都会对索引底层的B+树进行修改，因为UUID数据是无序的，所以每一次UUID数据的插入都会对主键的B+树进行很大的修改，这一点很不好，插入完全无序，不但会导致一些中间节点产生分裂，也会白白创造出很多不饱和的节点，这样大大降低了数据库插入的性能。
UUID只能保证全局唯一性，不满足后面的趋势递增，单调递增。

* 数据库自增主键
单机
在分布式里面，数据库的自增ID机制的主要原理是：数据库自增ID和mysql数据库的replace into实现的，这里的replace into跟insert功能 类似，不同点在于：replace into首先尝试插入数据列表中，如果发现表中已经有此行数据（根据主键或唯一索引判断）则先删除，在插入，否则直接插入新数据。
我们每次插入的时候，发现都会把原来的数据给替换，并且ID也会增加,这就满足了
* 递增性
* 单调性
* 唯一性
在分布式情况下，并且并发量不多的情况，可以使用这种方案来解决，获得一个全局的唯一ID。

集群分布式集群
那数据库自增ID机制适合做分布式ID吗？答案是不太适合
系统水平扩展比较困难，比如定义好步长和机器台数之后，如果要添加机器该怎么办，假设现在有一台机器发号是：1,2,3,4,5,（步长是1），这个时候需要扩容机器一台，可以这样做：把第二胎机器的初始值设置得比第一台超过很多，貌似还好，但是假设线上如果有100台机器，这个时候扩容要怎么做，简直是噩梦，所以系统水平扩展方案复杂难以实现。
数据库压力还是很大，每次获取ID都得读写一次数据库，非常影响性能，不符合分布式ID里面的延迟低和高QPS的规则（在高并发下，如果都去数据库里面获取ID，那是非常影响性能的）

* 基于Redis生成全局ID策略
单机版
因为Redis是单线程，天生保证原子性，可以使用原子操作INCR和INCRBY来实现
INCRBY：设置增长步长
集群分布式
注意：在Redis集群情况下，同样和MySQL一样需要设置不同的增长步长，同时key一定要设置有效期，可以使用Redis集群来获取更高的吞吐量。
假设一个集群中有5台Redis，可以初始化每台Redis的值分别是 1,2,3,4,5 ， 然后设置步长都是5
各个Redis生成的ID为：
A：1 6 11 16 21  
B：2 7 12 17 22  
C：3 8 13 18 23  
D：4 9 14 19 24  
E：5 10 15 20 25  
但是存在的问题是，就是Redis集群的维护和保养比较麻烦，配置麻烦。因为要设置单点故障，哨兵值守
但是主要是的问题就是，为了一个ID，却需要引入整个Redis集群，有种杀鸡焉用牛刀的感觉。

最终方案：雪花算法
优点
毫秒数在高维，自增序列在低位，整个ID都是趋势递增的
不依赖数据库等第三方系统，以服务的方式部署，稳定性更高，生成ID的性能也是非常高的
可以根据自身业务特性分配bit位，非常灵活

缺点
依赖机器时钟，如果机器时钟回拨，会导致重复ID生成
在单机上是递增的，但由于涉及到分布式环境，每台机器上的时钟不可能完全同步，有时候会出现不是全局递增的情况，此缺点可以认为无所谓，一般分布式ID只要求趋势递增，并不会严格要求递增，90%的需求只要求趋势递增。
后面有人专门提出了解决的方案

其他方案：
百度开源的分布式唯一ID生成器 UidGenerator-解决了时钟回拨问题，使用互联网时间进行生成ID

Leaf - 美团点评分布式ID生成系统

###### Java 序列化和反序列化为什么要实现 Serializable 接口？

序列化：把对象转换为字节序列的过程称为对象的序列化  反序列化：把字节序列恢复为对象的过程称为对象的反序列化.

什么时候需要用到序列化和反序列化呢?当我们只在本地JVM里运行下Java实例, 这个时候是不需要什么序列化和反序列化的, 
但当我们需要将内存中的对象持久化到磁盘, 数据库中时, 当我们需要与浏览器进行交互时, 当我们需要实现RPC时, 这个时候就需要序列化和反序列化了.
前两个需要用到序列化和反序列化的场景, 是不是让我们有一个很大的疑问? 我们在与浏览器交互时, 还有将内存中的对象持久化到数据库中时, 
好像都没有去进行序列化和反序列化, 因为我们都没有实现Serializable接口, 但一直正常运行.

下面先给出结论:只要我们对内存中的对象进行持久化或网络传输, 这个时候都需要序列化和反序列化.

理由:服务器与浏览器交互时真的没有用到Serializable接口吗? JSON格式实际上就是将一个对象转化为字符串, 所以服务器与浏览器交互时的数据格式其实是字符串
实现Serializable接口就算了, 为什么还要显示指定serialVersionUID的值?

如果不显示指定serialVersionUID, JVM在序列化时会根据属性自动生成一个serialVersionUID, 然后与属性一起序列化,
 再进行持久化或网络传输. 在反序列化时, JVM会再根据属性自动生成一个新版serialVersionUID, 然后将这个新版serialVersionUID与序列化时生成的旧版serialVersionUID进行比较, 
 如果相同则反序列化成功, 否则报错.
 
如果显示指定了serialVersionUID, JVM在序列化和反序列化时仍然都会生成一个serialVersionUID, 但值为我们显示指定的值, 这样在反序列化时新旧版本的serialVersionUID就一致了.

在实际开发中, 不显示指定serialVersionUID的情况会导致什么问题? 
如果我们的类写完后不再修改, 那当然不会有问题, 但这在实际开发中是不可能的, 我们的类会不断迭代, 一旦类被修改了, 那旧对象反序列化就会报错. 所以在实际开发中, 我们都会显示指定一个serialVersionUID, 值是多少无所谓, 只要不变就行.

##### 线程池的创建方式
          
 程池的创建方式总共包含以下 7 种（其中 6 种是通过 Executors 创建的，1 种是通过ThreadPoolExecutor 创建的）：
 
 Executors.newFixedThreadPool：创建一个固定大小的线程池，可控制并发的线程数，超出的线程会在队列中等待；
 
 public static void fixedThreadPool() {
     // 创建 2 个数据级的线程池
     ExecutorService threadPool = Executors.newFixedThreadPool(2);
 
     // 创建任务
     Runnable runnable = new Runnable() {
         @Override
         public void run() {
             System.out.println("任务被执行,线程:" + Thread.currentThread().getName());
         }
     };
 
     // 线程池执行任务(一次添加 4 个任务)
     // 执行任务的方法有两种:submit 和 execute
     threadPool.submit(runnable);  // 执行方式 1:submit 有返回值Future,可通过Future.get获取返回值
     threadPool.execute(runnable); // 执行方式 2:execute
     threadPool.execute(runnable);
     threadPool.execute(runnable);
 }
 
 
 Executors.newCachedThreadPool：创建一个可缓存的线程池，若线程数超过处理所需，缓存一段时间后会回收，若线程数不够，则新建线程；
 Executors.newSingleThreadExecutor：创建单个线程数的线程池，它可以保证先进先出的执行顺序；
 Executors.newScheduledThreadPool：创建一个可以执行延迟任务的线程池；
 Executors.newSingleThreadScheduledExecutor：创建一个单线程的可以执行延迟任务的线程池；
 Executors.newWorkStealingPool：创建一个抢占式执行的线程池（任务执行顺序不确定）【JDK 1.8 添加】。
 ThreadPoolExecutor：最原始的创建线程池的方式，它包含了 7 个参数可供设置，推荐使用。
 

##### 操作系统中的进程有哪些状态？

新建（New）：当一个进程被创建但尚未开始执行时，处于新建状态。在这个阶段，操作系统正在为进程分配必要的资源。

就绪（Ready）：进程已经准备好运行，等待系统分配处理器资源来执行。在多道程序设计中，可能有多个就绪状态的进程等待执行。

运行（Running）：进程正在 CPU 上执行指令，处于运行状态。在单核处理器中，同时只能有一个进程处于运行状态；在多核处理器中，可能有多个进程同时处于运行状态。

阻塞（Blocked）：进程由于某种原因（例如等待 I/O 操作完成、等待某个事件发生）而暂时无法继续执行，进入阻塞状态。一旦满足了条件，进程就会从阻塞状态转为就绪状态。

终止（Terminated）：进程执行完毕或者出现错误被终止后，处于终止状态。在终止状态下的进程会被系统回收资源，并从进程表中移除。


##### Java线程有哪些状态？
在 JVM 运行中，线程一共有 NEW、RUNNABLE、BLOCKED、WAITING、TIMED_WAITING、TERMINATED 六种状态，这些状态对应 Thread.State 枚举类中的状态。

NEW,TERMINATED
这两个状态比较好理解，当创建一个线程后，还没有调用start()方法时，线程处在 NEW 状态，线程完成执行，退出后变为TERMINATED终止状态。

RUNNABLE
运行 Thread 的 start 方法后，线程进入 RUNNABLE 可运行状态

BLOCKED
在运行态中的线程进入 synchronized 同步块或者同步方法时，如果获取锁失败，则会进入到 BLOCKED 状态。当获取到锁后，会从 BLOCKED 状态恢复到就绪状态。

WAITING,TIMED_WAITING
WAITING 状态：

当线程调用 Object 类的 wait() 方法时，线程会进入 WAITING 状态。在这种状态下，线程会一直等待，直到其他线程调用 notify() 或 notifyAll() 方法来唤醒它。
除了显式地调用 wait() 方法外，线程还可能由于其他原因（如 LockSupport.park()）而进入 WAITING 状态。
TIMED_WAITING 状态：

当线程调用带有超时参数的方法，如 Thread.sleep(long millis)、Object.wait(long timeout)、Thread.join(long millis) 等时，线程会进入 TIMED_WAITING 状态。
在 TIMED_WAITING 状态下，线程会等待指定的时间，如果在超时时间内条件仍未满足，线程会自动恢复到就绪状态。
线程也可能由于其他原因（如 LockSupport.parkNanos()、LockSupport.parkUntil()）而进入 TIMED_WAITING 状态。


##### Thread.run()方法跟Thread.start()方法区别

在Java中，Thread类是用于创建和操作线程的类。Thread类中有两个重要的方法：run()和start()。

start() 方法：
当调用start()方法时，会创建一个新的线程并开始执行线程的run()方法。start()方法会启动线程并调用run()方法，使得线程处于可运行状态。在多线程编程中，应当使用start()方法来启动线程，而不是直接调用run()方法。

run() 方法：
run()方法是线程的主要逻辑执行体，包含了线程的实际执行代码。当调用start()方法后，新线程会开始执行其run()方法中的代码。如果直接调用run()方法，那么该方法就会在当前线程中以普通方法的方式执行，并不会创建新的线程。

总结来说，调用start()方法会创建一个新的线程并开启线程的执行，从而调用线程的run()方法；而直接调用run()方法会在当前线程中顺序执行run()方法的代码，不会创建新的线程。
因此，在多线程编程中，应当始终使用start()方法来启动线程，让系统来调用相应的run()方法。


##### 线程池的submit()与execute()方法区别
Java中的线程池是通过Executor框架实现的，它允许在应用程序中创建多个线程，并将任务分配给这些线程执行。Executor框架提供了两种方法来提交任务到线程池中：submit()和execute()。

submit()方法：
submit()方法将一个任务提交到线程池中，并返回一个Future对象，该对象可以用于检查任务是否执行完成、获取任务执行的结果或取消任务的执行。如果任务执行过程中抛出了异常，那么调用Future.get()方法时会抛出ExecutionException异常，该异常包含了原始的异常信息。

execute()方法：
execute()方法将一个任务提交到线程池中，并让线程池中的某个线程执行该任务。该方法没有返回值，也不能检查任务是否执行完成或获取任务执行的结果。如果任务执行过程中抛出了异常，那么该异常会被线程池捕获并记录到日志中，但不会抛出到调用者。

总的来说，submit()方法比execute()方法更灵活，可以通过返回的Future对象来管理任务的执行状态和结果，支持任务的取消操作，并能够获取任务执行过程中抛出的异常信息。而execute()方法则更为简单和直接，适合于不需要关心任务执行结果和异常信息的场景。

需要注意的是，在使用submit()方法时，线程池会自动在任务执行之前对Callable或Runnable对象进行包装，从而实现异常处理和结果返回的功能。而在使用execute()方法时，需要手动对任务对象进行异常处理和结果返回的封装。

##### 接口幂等实现方案

唯一标识符方案：在每次请求中添加一个唯一标识符（例如UUID），服务端在处理请求之前先检查该标识符是否已经存在，如果已经存在，则认为是重复请求，直接返回之前的结果即可。

Token方案：客户端在请求中携带一个令牌（Token），服务端接收到请求后会校验该令牌的有效性和唯一性，如果令牌已经被使用过，则判定为重复请求，返回之前的结果。

数据库方案：在数据库中使用唯一索引或主键约束来防止插入重复数据。在每次请求时，服务端先尝试插入数据，如果插入失败（由于唯一约束），则认为是重复请求，直接返回之前的结果。

版本号方案：在每个资源上添加一个版本号字段，每次更新资源时，先检查当前版本号是否与请求中携带的版本号匹配，如果匹配，则进行更新操作，否则认为是重复请求，返回之前的结果。

##### Java 对象不使用时，为什么要赋值 null 

在Java中，对象的内存分配是由JVM自动进行的，当一个对象不再被引用时，JVM会自动回收这个对象的内存空间。但是，如果我们在程序中创建了一个对象，却一直保留着对它的引用，那么这个对象就不能被回收，这就会导致内存泄漏和性能问题。因此，在不需要使用对象时，为了及时释放对象所占用的内存空间，我们可以将这个对象的引用赋值为null。

将对象引用赋值为null，可以让垃圾回收器识别出这个对象已经不再被引用，从而将其回收。同时，这样也可以避免在后续的程序执行过程中误用这个对象，以免产生错误。

需要注意的是，将对象引用赋值为null并不会立即释放对象所占用的内存空间，它只是使得该对象变成垃圾对象，等待垃圾回收器回收。另外，在某些情况下，程序可能会出现空指针异常，因此在使用对象前需要进行非空判断。


##### wait/notify为什么必须synchronized包裹？
首先，jvm内部在notify与wait逻辑中都强制判断是否有对等待对象加锁，所以如下代码逻辑会抛异常

```
static class BlockingQ {
        private static final int MAX_SIZE = 10;
        private final List<String> queue = new ArrayList<>(MAX_SIZE);
                private final Object putWait = new Object();
                private final Object getWait = new Object();
        public void put(String s) throws InterruptedException {
            synchronized (queue) { //这里锁住的是queue对象
                while (queue.size() >= MAX_SIZE)
                    putWait.wait(); //这里的wait必须是针对queue对象进行wait
                queue.add(s);
                getWait.notifyAll(); //同理
            }
        }
 
        public String get() throws InterruptedException {
            synchronized (queue) {
                while (queue.isEmpty()) {
                    getWait.wait(); //同理
                }
                String result = queue.remove(0);
                putWait.notifyAll(); //同理
                return result;
            }
        }
    }
```

	修改后的正确代码实现为：
```	
	static class BlockingQ {
        private static final int MAX_SIZE = 10;
        private final List<String> queue = new ArrayList<>(MAX_SIZE);
 
        public void put(String s) throws InterruptedException {
            synchronized (queue) {
                while (queue.size() >= MAX_SIZE)
                    queue.wait();
                queue.add(s);
                queue.notifyAll();
            }
        }
 
        public String get() throws InterruptedException {
            synchronized (queue) {
                while (queue.isEmpty()) {
                    queue.wait();
                }
                String result = queue.remove(0);
                queue.notifyAll();
                return result;
            }
        }
    }
```

##### Java await方法与wait方法区别

在Java中，await()方法和wait()方法都涉及到线程的等待，但是它们存在于不同的类中，并且用途也有所不同。

await()方法：await()方法是Lock接口中Condition对象的方法，通常与ReentrantLock一起使用。它用于实现基于锁的条件等待，允许线程在等待某个条件成立时挂起，并且在条件发生变化时被唤醒。await()方法需要在获取锁后调用，然后当前线程将释放锁并进入等待状态，直到被其他线程通过signal()或signalAll()方法唤醒，或者等待时间超时。

wait()方法：wait()方法是Object类中的方法，用于实现基于对象监视器的线程等待。它需要在synchronized块或方法中调用，使当前线程进入等待状态，直到另一个线程调用相同对象上的notify()或notifyAll()方法来唤醒等待的线程。wait()方法还可以指定等待的超时时间。

总的来说，await()方法通常与显式的锁（例如ReentrantLock）结合使用，而wait()方法通常与隐式的对象监视器（synchronized块或方法）结合使用。它们都是用于线程间的协作和同步，但是在不同的场景和使用方式下。

##### 多线程事务怎么回滚？

在spring中可以使用@Transactional注解去控制事务，使出现异常时会进行回滚，在多线程中，这个注解则不会生效，如果主线程需要先执行一些修改数据库的操作，当子线程在进行处理出现异常时，主线程修改的数据则不会回滚，导致数据错误。

结论：使用sqlSession控制手动提交事务

演示：

https://mp.weixin.qq.com/s?__biz=MzIwNjg4MzY4NA==&mid=2247513889&idx=2&sn=fa321f0ae274813c127bebac59fef067&chksm=9718292aa06fa03c84b0393cdc8b2e727c35f6b8002f73d4e0fb3e96e57486a05401fcb84705&scene=21#wechat_redirect


##### BIO,NIO,AIO的区别？

传统的BIO（同步阻塞的BIO） 
NIO（同步非阻塞的NIO）。它支持面向缓冲的，基于通道的I/O操作方法
AIO（异步非阻塞的AIO）

NIO（New I/O）：

NIO是在Java 1.4 中引入的，它提供了非阻塞的、事件驱动的I/O操作。NIO通过Channel、Buffer和Selector这些新的抽象组件来实现高效的I/O操作。
NIO通过使用较少的线程处理大量的并发连接，因此适合处理大量连接但数据量不大的场景，比如聊天服务器或者实时消息推送等。
NIO是基于多路复用器Selector来实现的，通过Selector可以注册多个Channel，然后通过一个线程就可以监听多个Channel上的事件，从而减少了线程的开销。
NIO提供了非阻塞的I/O操作，当一个通道正在向其目标写入数据时，程序可以继续处理其他事务而不必等待写入完成。
AIO（Asynchronous I/O）：

AIO是在Java 1.7 中引入的，它进一步简化了异步I/O操作。AIO相比于NIO更加强调异步操作，它使用回调机制来实现异步I/O。
AIO相对NIO来说更适合处理大数据量的I/O操作，因为它能够异步地处理I/O事件，并在完成时通过回调的方式通知应用程序。
AIO中的I/O操作完全是异步的，当需要进行I/O操作时，应用程序不需要自己去轮询或者等待I/O操作完成，而是通过回调机制得到通知。
AIO的异步特性使得它更适合处理网络数据传输、文件操作等可能会产生大量等待时间的场景，能够充分利用系统资源。

##### Java CountDownLatch 用法
CountDownLatch 是 Java 并发包中的一个工具类，用于协调多个线程之间的同步，常见的场景是一个线程等待其他线程执行完毕后再进行操作。

CountDownLatch 类有一个计数器，该计数器初始化为一个正整数，表示需要等待的线程数量。每个线程完成工作后都会调用 countDown() 方法，该方法会将计数器减 1。当计数器达到 0 时，等待线程被唤醒，继续执行。

下面是 CountDownLatch 的用法：

初始化 CountDownLatch
在创建 CountDownLatch 实例时，需要指定计数器的初始值，表示需要等待的线程数量。

java
```
CountDownLatch latch = new CountDownLatch(3); // 初始化计数器为 3
```
等待其他线程完成
一个线程可以通过 await() 方法等待其他线程完成工作。该方法会阻塞当前线程直到计数器达到 0。

java
```
latch.await(); // 等待计数器为 0
```
减少计数器的值
在每个线程完成工作后，需要调用 countDown() 方法减少计数器的值。

java
```
latch.countDown(); // 计数器减 1
```
示例代码
下面是一个示例代码，其中使用了三个线程分别执行任务，主线程等待三个线程完成后再输出结果。

java
```
public class CountDownLatchExample {

    public static void main(String[] args) throws InterruptedException {
        CountDownLatch latch = new CountDownLatch(3); // 初始化计数器为 3

        // 创建三个线程分别执行任务
        new WorkerThread(latch, "Worker-1").start();
        new WorkerThread(latch, "Worker-2").start();
        new WorkerThread(latch, "Worker-3").start();

        latch.await(); // 等待计数器为 0

        System.out.println("All workers have finished their tasks.");
    }

    private static class WorkerThread extends Thread {

        private CountDownLatch latch;

        public WorkerThread(CountDownLatch latch, String name) {
            super(name);
            this.latch = latch;
        }

        @Override
        public void run() {
            try {
                // 模拟执行任务
                Thread.sleep((long) (Math.random() * 10000));
                System.out.println(getName() + " has finished the task.");
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                latch.countDown(); // 计数器减 1
            }
        }
    }
}
```



 ##### Java基本数据类型
 
 整形：byte(1) short(2) int(4) long(8) 
 
 字符型：char(1)
 
 浮点型：float(4) double(8)
 
 布尔类型：boolean(1)
 
 ##### Java的继承、多态和封装
 
 封装（Encapsulation）：封装是将数据和方法组合在一个单元中，并对外部隐藏内部实现细节的过程。通过封装，可以限制对数据的直接访问，只能通过定义的公共方法来间接访问和操作数据。这样可以提高代码的安全性和可维护性，同时也隐藏了内部实现细节，使得代码更易于理解和使用。
 
 继承（Inheritance）：继承是基于已存在的类创建新类的机制。通过继承，子类（派生类）可以继承父类（基类）的属性和方法。子类可以重写父类的方法或增加新的方法，从而实现代码的重用和扩展。继承提供了代码层次结构和类之间的关系，使得代码更加灵活和可扩展。
 
 多态（Polymorphism）：多态是指同一类型的对象在不同的情况下表现出不同的行为。多态可以通过继承和接口实现。通过多态，可以通过父类或接口类型引用子类或实现类的对象，并根据实际的对象类型调用相应的方法。这样可以实现代码的通用性和灵活性，使得程序更易于扩展和维护。
 
 ##### synchronized 有多少种锁
       
对象锁（普通锁）：synchronized关键字可以用于修饰实例方法或代码块，以获取对象级别的锁。当一个线程获得了对象的锁后，其他线程必须等待该线程释放锁才能访问该对象的同步代码块或方法。

类锁（静态锁）：synchronized关键字也可以用于修饰静态方法或代码块，以获取类级别的锁。类级别的锁对于整个类的所有实例都起作用，同一时刻只能有一个线程持有该类的锁。

锁对象：除了使用synchronized修饰方法或代码块外，还可以使用自定义的对象作为锁。通过使用synchronized关键字加锁指定的对象，可以实现更细粒度的控制，不同的线程可以独立地获取不同的锁。

ReentrantLock：ReentrantLock是Java提供的可重入锁（Reentrant Lock）实现。相比于synchronized关键字，ReentrantLock提供了更多的灵活性和功能，例如支持公平锁和非公平锁、可中断的获取锁操作、超时获取锁等。

##### 对称加密和非对称加密

对称加密（Symmetric Encryption）使用同一个密钥进行加密和解密。发送方和接收方必须共享相同的密钥。在加密过程中，原始数据通过应用特定算法和密钥进行转换，生成加密后的数据。接收方在收到加密数据后，使用相同的密钥和算法进行解密，还原出原始数据。

优点：

加密和解密速度快，适用于大量数据的加密。
算法简单，实现容易。
缺点：

密钥的安全性需要保证，因为只有一个密钥，一旦泄露，所有的数据都可能被破解。
密钥分发和管理较为困难，特别是在分布式系统中。
非对称加密（Asymmetric Encryption），也称为公钥加密，使用一对密钥：公钥（Public Key）和私钥（Private Key）。发送方使用接收方的公钥对数据进行加密，而接收方使用自己的私钥进行解密。

优点：

安全性高，私钥不需要与他人共享。
密钥交换方便，不需要事先共享密钥。
缺点：

加密和解密速度相比对称加密较慢。
适用于小数据量的加密，不适合大规模数据加密。


##### Java基本类型与引用类型的==与equals方法

基本类型只能通过== 判断值是否相等，

== 对于基本类型来说是值比较，对于引用类型来说是比较的是引用；
而equals 默认情况下是引用比较，只是很多类重写了equals 方法，比如String、Integer 等把它变成了值比较，所以一般情况下equals 比较的是值是否相等。


##### SimpleDateFormat是线程安全的么？

在Java中，SimpleDateFormat类是线程不安全的。这意味着在多线程环境下，同时对同一个SimpleDateFormat对象进行操作可能会导致不可预测的结果，甚至可能引发异常。

原因在于SimpleDateFormat内部维护了一个Calendar实例来处理日期和时间的格式化和解析。由于Calendar本身也是可变的，而SimpleDateFormat没有进行充分的同步措施，
因此可能导致线程安全问题。

为了在多线程环境下安全地使用SimpleDateFormat，可以采取以下两种方法之一：

在每个线程中使用单独的SimpleDateFormat实例，保证线程间互相独立，不共享对象。
使用线程安全的日期时间处理类，比如java.time.format.DateTimeFormatter，它是线程安全的，推荐在新的代码中使用。



##### Java中有哪几种方式来创建线程任务

1. 通过继承 Thread 类，并重写它的 run 方法

class MyThread extends Thread {
    public void run() {
        // 线程任务代码
    }
}

MyThread thread = new MyThread();
thread.start();

2. 实现 Runnable 接口

class MyRunnable implements Runnable {
    public void run() {
        // 线程任务代码
    }
}

MyRunnable runnable = new MyRunnable();
Thread thread = new Thread(runnable);
thread.start();

3.实现 Callable 接口，并结合 Future 实现

Callable<Integer> task = new Callable<Integer>() {
    public Integer call() throws Exception {
        // 线程任务代码
        return 42;
    }
};

ExecutorService executor = Executors.newSingleThreadExecutor();
Future<Integer> future = executor.submit(task);
Integer result = future.get(); // 获取任务结果
executor.shutdown();

4.使用Executor框架

ExecutorService executor = Executors.newFixedThreadPool(5); // 创建一个固定大小的线程池
executor.execute(new Runnable() {
    public void run() {
        // 线程任务代码
    }
});
executor.shutdown(); // 关闭线程池


##### 为什么不建议使用Executors创建线程池
      
1. 固定的线程池大小：Executors 提供的工厂方法创建的线程池通常具有固定的线程池大小，例如 newFixedThreadPool() 方法创建的线程池固定大小为指定的线程数。这意味着如果存在大量的任务提交，可能导致线程池中的线程过多，从而消耗过多的系统资源。

2. 无界队列：使用 Executors 创建的线程池通常使用无界队列来保存等待执行的任务。当任务提交速度远远快于任务执行速度时，无界队列可能会导致大量的任务积压，最终导致内存溢出或系统性能下降。

3. 缺乏对线程池参数的精细控制：Executors 提供的工厂方法通常缺乏对线程池各个参数的精细控制，例如核心线程数、最大线程数、线程存活时间等。在某些场景下，需要根据实际业务需求来精细调整线程池的参数。

##### 线程与进程的区别

1.定义：

进程是操作系统分配资源的基本单位，每个进程都有独立的内存空间和系统资源。一个进程可以包含多个线程。
线程是进程中的执行单元，是 CPU 调度的基本单位。一个进程至少包含一个线程。

2.资源占用：

进程间相互独立，每个进程拥有独立的内存空间，需要独立分配系统资源。进程间通信比较复杂，通常需要通过进程间通信机制来实现。
线程共享所属进程的内存空间和资源，同一进程中的线程之间可以直接进行通信，共享数据更方便。

3.切换开销：

由于进程拥有独立的内存空间，进程切换时需要保存和恢复整个进程的上下文，切换开销较大。
线程切换时只需保存和恢复线程的上下文，切换开销较小，线程切换更为高效。

4.并发性：

多进程之间可以并发执行，提高了系统的并发能力，但进程间通信的开销较大。
多线程共享同一进程的资源，可以更方便地实现并发操作，且线程间通信更为简单。

5.稳定性：

由于进程间相互独立，一个进程的崩溃不会影响其他进程的稳定性。
线程间共享同一进程的资源，一个线程的异常或崩溃可能会影响其他线程的稳定性。

##### JDK动态代理与CGLib动态代理

JDK动态代理，是Java提供的动态代理技术，可以在运行时创建接口的代理实例-Spring AOP默认采用这种方式，并且由于JDK动态代理的底层，如果没有接口，Spring用的就是Cglib代理。
CGLib动态代理，采用底层的字节码技术，在运行时创建子类代理的实例 - 目标对象不存在接口时，采用这种方式，SpringBoot使用这种代理方式。
正是由于CGLib使用这种运行时继承被代理类实现动态代理的原理，所以CGlib不能代理fianal类。

##### finally语句块什么情况下不会执行

1. 程序没有进入到try语句块
2. 在try或者catch语句块中执行System.exit(0),导致JVM退出

##### volatile关键字
volatile是Java提供的一种轻量级的同步机制。与synchronized修饰方法、代码块不同，volatile只用来修饰变量。
并且与synchronized、ReentrantLock等重量级锁不同的是，volatile更轻量级，因为它不会引起线程上下文的切换和调度。

说volatile作用之前，先说一下并发编程的三大特性：原子性、可见性和有序性。

原子性
即一个或者多个操作作为一个整体，要么全部执行，要么都不执行，并且操作在执行过程中不会被线程调度机制打断；而且这种操作一旦开始，就一直运行到结束，中间不会有任何上下文切换。

可见性
可见性是指当多个线程访问同一个变量时，一个线程修改了这个变量的值，其他线程能够立即看得到修改的值。

有序性
为了提高程序的执行效率，编译器会对编译后的指令进行重排序，即代码的编写顺序不一定就是代码的执行顺序。

并发编程中只有同时满足这三大特性，才能保证程序正确的执行。而volatile的只保证了可见性和有序性，不保证原子性。
volatile的作用只有两个：

保证内存的可见性
Volatile实现禁止指令重排优化（保证有序性）

##### 那么volatile是怎么解决可见性问题呢？
volatile主要通过汇编lock前缀指令，它会锁定当前内存区域的缓存（缓存行），并且立即将当前缓存行数据写入主内存（耗时非常短），
回写主内存的时候会通过MESI协议使其他线程缓存了该变量的地址失效，从而导致其他线程需要重新去主内存中重新读取数据到其工作线程中。

要注意volatile关键字是无法替代synchronized关键字的，因为volatile关键字无法保证操作的原子性。通常来说，使用volatile必须具备以下2个条件：

对变量的写操作不依赖于当前值
该变量没有包含在具有其他变量的不变式中

##### 正向代理和反向代理

反向代理例如Nginx（服务器端）,
正向代理：加速器(客户端)

##### 线程池如何知道一个线程的任务已经执行完成
1. 在线程池内部，当我们把一个任务丢给线程池去执行，线程池会调度工作线程来执行这个任务的 run 方法，run 方法正常结束，也就意味着任务完成了。
所以线程池中的工作线程是通过同步调用任务的 run()方法并且等待 run 方法返回后，再去统计任务的完成数量。

2. 如果想在线程池外部去获得线程池内部任务的执行状态，有几种方法可以实现。
  • 线程池提供了一个 isTerminated()方法，可以判断线程池的运行状态，我们可以循环判断 isTerminated()方法的返回结果来了解线程池的运行状态，
   一旦线程池的运行状态是 Terminated，意味着线程池中的所有任务都已经执行完了。想要通过这个方法获取状态的前提是，
   程序中主动调用了线程池的 shutdown()方法。在实际业务中，一般不会主动去关闭线程池，因此这个方法在实用性和灵活性方面都不是很好。
   
  • 在线程池中，有一个 submit()方法，它提供了一个 Future 的返回值，我们通过 Future.get()方法来获得任务的执行结果，
  当线程池中的任务没执行完之前，future.get()方法会一直阻塞，直到任务执行结束。因此，只要 future.get()方法正常返回，
  也就意味着传入到线程池中的任务已经执行完成了！
  
  • 可以引入一个 CountDownLatch 计数器，它可以通过初始化指定一个计数器进行倒计时，
  其中有两个方法分别是 await()阻塞线程，以及 countDown()进行倒计时，一旦倒计时归零，
  所以被阻塞在 await()方法的线程都会被释放。基于这样的原理，我们可以定义一个 CountDownLatch 对象并且计数器为 1，
  接着在线程池代码块后面调用 await()方法阻塞主线程，然后，当传入到线程池中的任务执行完成后，
  调用 countDown()方法表示任务执行结束。最后，计数器归零 0，唤醒阻塞在 await()方法的线程。

代码实现：
```
private static void countDownLatchTest() throws Exception { 
    //计数器，判断线程是否执行结束      
    CountDownLatch taskLatch = new CountDownLatch(30);
    for (int i = 0; i < 30; i++) {          
        int index = i;          
        pool.execute(() -> { 
            sleepMtehod(index);
            taskLatch.countDown(); 
            System.out.println("当前计数器数量：" + taskLatch.getCount());     
        });     
    }      
    //当前线程阻塞，等待计数器置为0   
    taskLatch.await();      
    System.out.println("全部执行完毕"); 
} 
```


##### top和jStack分析CPU飙升问题

CPU飙升可能出现在以下几种场景中：
死循环：当程序中存在无限循环时，CPU会一直执行循环代码，导致CPU占用率飙升。
大量计算：当程序需要进行大量的计算操作时，会导致CPU负载增加，从而使CPU占用率飙升。
死锁：当多个线程相互等待对方释放资源时，会导致死锁现象，此时CPU会一直处于忙碌状态，导致CPU占用率飙升。
高并发请求：当系统面临大量并发请求时，CPU需要同时处理多个请求，导致CPU占用率飙升。
病毒或恶意软件：当计算机感染病毒或运行恶意软件时，这些恶意程序可能会占用大量CPU资源，导致CPU占用率飙升

1.top
在服务器上，我们可以通过top命令查看各个进程的cpu使用情况，它默认是按cpu使用率由高到低排序的

2. top -Hp pid
通过top -Hp 21340可以查看该进程下，各个线程的cpu使用情况

3. 将线程pid转换为16进制
转换命令：$printf '%x\n'  线程pid 

4. jstack 进度pid | grep -A 200 16进制线程ID

4. jstack -l [PID] >/tmp/log.txt （导出到日志文件中）


Dump 文件分析关注重点

    runnable，线程处于执行中

    deadlock，死锁（重点关注）

    blocked，线程被阻塞 （重点关注）

    Parked，停止

    locked，对象加锁

    waiting，线程正在等待

    waiting to lock 等待上锁

    Object.wait()，对象等待中

    waiting for monitor entry 等待获取监视器（重点关注）

    Waiting on condition，等待资源（重点关注），最常见的情况是线程在等待网络的读写

另外，火焰图可以更加直观详细的看到详细信息。

##### sleep和wait有什么区别 
 1.wait 方法必须配合 synchronized 一起使用，不然在运行时就会抛出 IllegalMonitorStateException 的异常,而 sleep 可以单独使用，无需配合 synchronized 一起使用。
 2.wait 方法属于 Object 类的方法，而 sleep 属于 Thread 类的方法
 3.sleep 方法必须要传递一个超时时间的参数，且过了超时时间之后，线程会自动唤醒。而 wait 方法可以不传递任何参数，不传递任何参数时表示永久休眠，
 直到另一个线程调用了 notify 或 notifyAll 之后，休眠的线程才能被唤醒。也就是说 sleep 方法具有主动唤醒功能，而不传递任何参数的 wait 方法只能被动的被唤醒。
 4.wait 方法会主动的释放锁，而 sleep 方法则不会。
 5.调用 sleep 方法线程会进入 TIMED_WAITING 有时限等待状态，而调用无参数的 wait 方法，线程会进入 WAITING 无时限等待状态。
