
 #####  方法的重写要满足：三同一大一小,三同：方法名相同，返回值类型，形参相同,一大：访问权限大于等于重写前，一小：抛出异常范围小于等于重写前。
 
 ##### Java8之后，接口跟抽象类的区别？
  * Java8之后，接口中可以有默认方法(default)，静态方法(static)，但是不能写构造方法，而抽象类则可以有构造方法，说明构造方法是参与类的实例化过程的。
  * 抽象类可以有自己的成员变量，并且可以通过非抽象方法进行改变，而接口中的变量只能是 public static final的，不能被外部修改。
  * 接口可以多继承、抽象类只能单继承。
  
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
 
 ##### 线程池ThreadPoolExecutor核心方法execute()原理源码分析
 
 1. 构造方法
 首先解释一下各个参数：
 
 ①corePoolSize：核心线程数

  使用Runtime.getRuntime().availableProcessors()可以获取当前可用的处理器数量
 
 ②maximumPoolSize：最大线程数(最大线程数只限制了下限，没有限制上限，但是上限也有限制并不能达到Integer.MAX_VALUE，下文分析)，通过该下限限制可以发现最大线程最少有1个
 
 ③keepAliveTime:当前线程数>核心线程数时，允许超过核心线程数的这些线程等待任务的最大时间
 
 ④unit：③的时间单位
 
   TimeUnit.DAYS：天
   TimeUnit.HOURS：小时
   TimeUnit.MINUTES：分
   TimeUnit.SECONDS：秒
   TimeUnit.MILLISECONDS：毫秒
   TimeUnit.MICROSECONDS：微妙
   
 ⑤workQueue：用来存放任务的队列
 
   一个阻塞队列，用来存储线程池等待执行的任务，均为线程安全，它包含以下 7 种类型：
   
   ArrayBlockingQueue：一个由数组结构组成的有界阻塞队列。
   LinkedBlockingQueue：一个由链表结构组成的有界阻塞队列。
   SynchronousQueue：一个不存储元素的阻塞队列，即直接提交给线程不保持它们。
   PriorityBlockingQueue：一个支持优先级排序的无界阻塞队列。
   DelayQueue：一个使用优先级队列实现的无界阻塞队列，只有在延迟期满时才能从中提取元素。
   LinkedTransferQueue：一个由链表结构组成的无界阻塞队列。与SynchronousQueue类似，还含有非阻塞方法。
   LinkedBlockingDeque：一个由链表结构组成的双向阻塞队列。
   较常用的是 LinkedBlockingQueue 和 Synchronous，线程池的排队策略与 BlockingQueue 有关。
 
 ⑥threadFactory：用来创建线程池中线程的工厂类(默认使用的默认提供的共厂类，通过源码可以了解到该工厂创建的线程是非守护线程并且优先级为5)
 
 ⑦handler：拒绝服务的对象，提供拒绝服务功能
   
   触发拒绝策略来处理新提交的任务。以下是几种常见的拒绝策略：
   
   AbortPolicy（默认）：抛出RejectedExecutionException异常，表示无法处理新任务。
   
   CallerRunsPolicy：将任务退回给调用者，如果线程池的执行器已关闭，则直接丢弃任务。
   
   DiscardOldestPolicy：丢弃工作队列中最旧的任务，然后尝试提交新任务。
   
   DiscardPolicy：默默地丢弃无法处理的任务，不提供任何反馈。
   
   这些拒绝策略可以通过ThreadPoolExecutor类的构造方法或setRejectedExecutionHandler()方法进行设置。在选择拒绝策略时，需要根据具体的业务场景和需求来决定如何处理无法接受的任务。

 
 2.execute()方法分析前提知识
 ①首先要知道线程池中有线程池的自身状态，以及线程池中运行线程的个数，程池很巧妙的将这两个信息保存在一个32bit的int类型变量中，并且这个变量 是线程安全的AtomicInteger对象，其中使用高3位来保存线程池的自身状态，低29位保存线程池中工作线程的个数。
 从源码可以看出，该方法主要分为三步分别如下：
 ```
         if (command == null)
             throw new NullPointerException();
         int c = ctl.get();
         if (workerCountOf(c) < corePoolSize) {
             if (addWorker(command, true))
                 return;
             c = ctl.get();
         }
         if (isRunning(c) && workQueue.offer(command)) {
             int recheck = ctl.get();
             if (! isRunning(recheck) && remove(command))
                 reject(command);
             else if (workerCountOf(recheck) == 0)
                 addWorker(null, false);
         }
         else if (!addWorker(command, false))
             reject(command);
```
 
 
 首先获取记录线程池状态以及工作线程数的int变量值c
 
 第一步：根据c计算出工作线程数，如果线程数<核心线程数，创建一个核心线程并立刻处理该次提交的任务，然后该方法结束。
 
 第二步：如果第一步条件不成立(工作线程>=核心线程||创建核心线程因为某种原因失败)，判断线程池是否是运行状态，如果是运行状态然后把该次提交的任务放在任务队列中_延时处理_，然后再重新获取这个c再检查一次线程池的状态是否是运行状态，如果不是运行状态就把刚才添加的任务从任务队列中移除，把这个移除的任务交给构造方法传入的拒绝服务对象，走拒绝服务逻辑。如果是运行状态然后检查线程池是否还有工作线程数，如果没有则创建一个线程保证，至少有一个线程消费该任务队列。
 
 第三步：如果前两步不成立(线程池不是运行状态||任务队列添加满了)，然后添加一个非核心线程立刻处理该次提交的任务(早于大多数还在队列中等待的任务)，如果添加失败，则会把该次任务交给拒绝服务对象，走拒绝服务逻辑。
 
 总结：如果核心线程<用户输入的核心线程数则添加核心线程，并用该线程处理该任务，否则把该任务添加到任务队列中等待线程处理，如果核心线程队列满了，则添加非核心线程，并处理该次提交的任务，如果都不满足，则拒绝服务该任务。

创建线程池举例：
```
/**  线程池配置
 * @version V1.0
 */
public class ExecutorConfig {
    private static int maxPoolSize = Runtime.getRuntime().availableProcessors();
    private volatile static ExecutorService executorService;
    public static ExecutorService getThreadPool() {
        if (executorService == null){
            synchronized (ExecutorConfig.class){
                if (executorService == null){
                    executorService =  newThreadPool();
                }
            }
        }
        return executorService;
    }

    private static  ExecutorService newThreadPool(){
        int queueSize = 500;
        int corePool = Math.min(5, maxPoolSize);
        return new ThreadPoolExecutor(corePool, maxPoolSize, 10000L, TimeUnit.MILLISECONDS,
            new LinkedBlockingQueue<>(queueSize),new ThreadPoolExecutor.AbortPolicy());
    }
    private ExecutorConfig(){}
}
```

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
百度开源的分布式唯一ID生成器 UidGenerator

Leaf - 美团点评分布式ID生成系统

###### Java 序列化和反序列化为什么要实现 Serializable 接口？

序列化：把对象转换为字节序列的过程称为对象的序列化  反序列化：把字节序列恢复为对象的过程称为对象的反序列化.

什么时候需要用到序列化和反序列化呢?当我们只在本地JVM里运行下Java实例, 这个时候是不需要什么序列化和反序列化的, 
但当我们需要将内存中的对象持久化到磁盘, 数据库中时, 当我们需要与浏览器进行交互时, 当我们需要实现RPC时, 这个时候就需要序列化和反序列化了.
前两个需要用到序列化和反序列化的场景, 是不是让我们有一个很大的疑问? 我们在与浏览器交互时, 还有将内存中的对象持久化到数据库中时, 
好像都没有去进行序列化和反序列化, 因为我们都没有实现Serializable接口, 但一直正常运行.

下面先给出结论:只要我们对内存中的对象进行持久化或网络传输, 这个时候都需要序列化和反序列化.
理由:服务器与浏览器交互时真的没有用到Serializable接口吗? JSON格式实际上就是将一个对象转化为字符串, 所以服务器与浏览器交互时的数据格式其实是字符串
实现Serializable接口就算了, 为什么还要显示指定serialVersionUID的值?如果不显示指定serialVersionUID, JVM在序列化时会根据属性自动生成一个serialVersionUID, 然后与属性一起序列化, 再进行持久化或网络传输. 在反序列化时, JVM会再根据属性自动生成一个新版serialVersionUID, 然后将这个新版serialVersionUID与序列化时生成的旧版serialVersionUID进行比较, 如果相同则反序列化成功, 否则报错.
如果显示指定了serialVersionUID, JVM在序列化和反序列化时仍然都会生成一个serialVersionUID, 但值为我们显示指定的值, 这样在反序列化时新旧版本的serialVersionUID就一致了.
在实际开发中, 不显示指定serialVersionUID的情况会导致什么问题? 如果我们的类写完后不再修改, 那当然不会有问题, 但这在实际开发中是不可能的, 我们的类会不断迭代, 一旦类被修改了, 那旧对象反序列化就会报错. 所以在实际开发中, 我们都会显示指定一个serialVersionUID, 值是多少无所谓, 只要不变就行.

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

##### 线程池有哪些状态

RUNNING（运行）：线程池处于 RUNNING 状态时，可以接受新任务，并处理阻塞队列中的任务。当且仅当线程池处于 RUNNING 状态时，才能接受新任务。

SHUTDOWN（关闭）：线程池进入 SHUTDOWN 状态后，不再接受新任务，但会继续执行阻塞队列中的任务。可以调用线程池的 shutdown() 方法将线程池转换为 SHUTDOWN 状态。

STOP（停止）：线程池进入 STOP 状态后，不再接受新任务，不再执行阻塞队列中的任务，并尝试中断正在执行的任务。可以调用线程池的 shutdownNow() 方法将线程池转换为 STOP 状态。

TERMINATED（终止）：线程池进入 TERMINATED 状态后，所有的任务都已经完成，线程池已经关闭。可以通过线程池的 isTerminated() 方法来判断线程池是否已经终止。


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
== 对于基本类型来说是值比较，对于引用类型来说是比较的是引用；
而equals 默认情况下是引用比较，只是很多类重写了equals 方法，比如String、Integer 等把它变成了值比较，所以一般情况下equals 比较的是值是否相等。


##### SimpleDateFormat是线程安全的么？

在Java中，SimpleDateFormat类是线程不安全的。这意味着在多线程环境下，同时对同一个SimpleDateFormat对象进行操作可能会导致不可预测的结果，甚至可能引发异常。

原因在于SimpleDateFormat内部维护了一个Calendar实例来处理日期和时间的格式化和解析。由于Calendar本身也是可变的，而SimpleDateFormat没有进行充分的同步措施，因此可能导致线程安全问题。

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

JDK动态代理，是Java提供的动态代理技术，可以在运行时创建接口的代理实例-Spring AOP默认采用这种方式。CGLib动态代理，采用底层的字节码技术，在运行时创建子类代理的实例 - 目标对象不存在接口时，采用这种方式。

##### 使用＠Autowired注解时，如果一个类可以有多种类型，如何解决
      
1.使用@Qualifier注解：可以和@Autowired一起使用，用于指定具体要注入的bean名称。在@Autowired注解中使用@Qualifier注解，指定要注入的bean的名称，这样就可以明确指定要注入的bean实例。
```
@Autowired
@Qualifier("beanName")
private BeanClass bean;
```

2.使用@Primary注解：可以在多个bean实例中使用@Primary注解，用于指定首选的bean。当存在多个同类型的bean实例时，被标记为@Primary的bean将被优先选择进行注入。
```
@Primary
@Bean
public BeanClass primaryBean() {
   // bean的定义
}
```

3.使用List或Map集合注入：可以将所有同类型的bean实例注入到一个List或Map集合中。Spring会自动将所有符合类型的bean注入到集合中，然后可以通过遍历集合来使用不同的bean实例。
```
@Autowired
private List<BeanClass> beanList;

@Autowired
private Map<String, BeanClass> beanMap;
```

##### 






