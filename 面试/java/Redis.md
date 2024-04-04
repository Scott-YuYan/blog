##### 什么是Redis?
Redis 是一个开源（BSD许可），内存存储的数据结构服务器，可用作数据库，高速缓存和消息队列代理。
它支持字符串、哈希表、列表、集合、有序集合，位图，HyperLogLogs 等数据类型。内置复制、Lua 脚本、LRU 收回、事务，以及不同级别磁盘持久化功能，
同时通过 Redis Sentinel 提供高可用，通过 Redis Cluster 提供自动分区。根据月度排行网站 DB-Engines的数据，Redis 是最流行的键值对存储数据库。

##### Redis 有什么优点和缺点？ 

Redis 优点
读写性能优异， Redis能读的速度是110k次/s，写的速度是81k次/s。
支持数据持久化，支持AOF和RDB两种持久化方式。
支持事务，Redis的所有操作都是原子性的，同时Redis还支持对几个操作合并后的原子性执行。
数据结构丰富，除了支持string类型的value外还支持hash、set、zset、list等数据结构。
支持主从复制，主机会自动将数据同步到从机，可以进行读写分离。

Redis 缺点
数据库容量受到物理内存的限制，不能用作海量数据的高性能读写，因此Redis 适合的场景主要局限在较小数据量的高性能操作和运算上。
Redis 不具备自动容错和恢复功能，主机从机的宕机都会导致前端部分读写请求失败，需要等待机器重启或者手动切换前端的IP才能恢复。
主机宕机，宕机前有部分数据未能及时同步到从机，切换IP后还会引入数据不一致的问题，降低了系统的可用性。
Redis 较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。为避免这一问题，运维人员在系统上线时必须确保有足够的空间，这对资源造成了很大的浪费。


##### Redis提供了以下基本数据类型：

字符串（String）：存储字符串、整数或浮点数。
示例：

SET name "John"
GET name

最大容量：512M

哈希（Hash）：类似于关联数组，存储字段和与之相关联的值。
示例：

HSET user:id1 name "John"
HSET user:id1 age 25
HGET user:id1 name
HGET user:id1 age
列表（List）：有序的字符串元素集合，允许在列表的两端进行插入、删除和修改操作。
示例：

LPUSH list "apple"
LPUSH list "banana"
RPUSH list "orange"
LRANGE list 0 -1
集合（Set）：无序的字符串元素集合，不允许重复的元素。
示例：

SADD set "apple"
SADD set "banana"
SADD set "orange"
SMEMBERS set
有序集合（Sorted Set）：类似于集合，每个元素都有一个分数（score）与之关联，通过分数进行排序。
示例：

ZADD leaderboard 1000 "Player1"
ZADD leaderboard 800 "Player2"
ZADD leaderboard 1200 "Player3"
ZRANGE leaderboard 0 -1 WITHSCORES


##### Redis穿透和雪崩是Redis应用中常见的两种问题，它们都可能导致系统性能下降或崩溃。

Redis缓存击穿:是指在使用缓存的系统中，针对某个热点数据在缓存失效的瞬间，大量的并发请求同时访问该热点数据，导致这些请求直接穿透缓存到达数据库，给数据库造成巨大压力，甚至引起宕机。
解决方式：
设置热点数据永不过期：对于热点数据，可以设置永不过期，或者采用懒加载的方式，在缓存失效时异步更新缓存。
互斥锁（Mutex）：在缓存失效时，只允许一个线程去查询数据库，其他线程等待该线程将数据加载到缓存后再从缓存中获取数据。
布隆过滤器（Bloom Filter）：用于快速判断请求的数据是否存在，如果不存在则直接拦截请求，避免访问数据库。
失效标记（Cache-Aside with Invalidations）：在缓存失效时，先将缓存标记为失效状态，然后异步更新缓存，直到更新完成前所有请求都返回旧值。


Redis穿透：指的是恶意请求或者非法请求，访问不存在的key，导致请求直接穿透到DB层。由于DB层没有相应的缓存，每个请求都会直接查询DB，从而导致DB压力过大。为了避免Redis穿透，可以采取如下措施：
解决方式：
对查询结果为空的key也进行缓存，但设置一个较短的过期时间，防止同一请求频繁访问不存在的key。
过滤掉非法请求。
在应用层设置布隆过滤器（Bloom Filter），判断请求是否合法。

Redis雪崩：指的是缓存中的大量key同时失效，导致所有的请求都访问DB。由于缓存失效是随机的，无法预测，因此每个请求都需要直接查询DB，从而导致DB压力过大。为了避免Redis雪崩，可以采取如下措施：
解决方式：
为不同的key设置不同的过期时间，避免所有key同时失效。
使用Redis集群，将缓存数据分散到不同节点上，降低节点失效的风险。
在应用层设置限流、降级等措施，减少请求量，降低DB压力。

##### Redis ZSET的底层是什么？

数据量较少的情况下，Redis7.0 使用zipList(压缩列表)，7.0之后使用listpack(紧凑列表) 数据量较多的时候，使用skiplist-跳跃表。

##### 为什么使用跳跃表

有序性： 跳跃表是一种有序数据结构，能够按照元素的分值（score）进行排序存储。这使得在有序集合中可以快速实现按照分值范围进行查找、插入和删除操作，而无需对整个数据集合进行排序。

高效性： 跳跃表的插入、删除和查找操作的时间复杂度为 O(log(N))，其中 N 是元素数量。相比于传统的有序数组，在数据量较大时，跳跃表的性能更加稳定，不会因为数据量增加而导致性能下降。

平衡性： 跳跃表通过随机化的方式维护多层索引，使得在查找、插入和删除操作时能够保持平衡，避免出现极端情况下的性能退化。

可扩展性： 跳跃表的结构相对简单，易于实现和扩展。同时，跳跃表支持动态调整索引层数，可以根据数据规模和访问模式进行灵活调整，提高了适应性和可扩展性。

##### Redis布隆过滤器实战

首先对布隆过滤器进行初始化：
初始化bitMap
```
@GetMapping(value = "/init")
public AjaxResult init(){
  List<Long> uselIds = EntityUtils.toList(tbUserService.list(),TbUser::getUserId);
  RedisBitMapUtils.init(USER BITMAP KEY, userIds);
  return AjaxResult.success();
}
```

场景1：无布隆过滤器 无缓存 直接查询数据库
```
@0verride
public TbUser selectUser1(Long userId){
   return this.getById(userId);
}
```

场景2： 有布隆过滤器 无缓存 主键查询数据库 对于数据库中不存在的主键不会查询数据库
```
@0verride
public TbUser selectUser2(Long userId){
  return RedisBitMapUtils.ifPresent(USER_BITMAP_KEY,userId, this::getById);
}
```

场景3：有布隆过滤器 有缓存 主键查询数据
```
@0verride
public TbUser selectUser3(Long userId){
  return RedisBitMapUtils.ifPresent(USER_BITMAP_KEy, userId,e -> this.getById(e, 60000));// 将第一次查询的结果进行缓存，时间为60s
}
```

删除及更新操作：
```
public AjaxResult modify(){
  Long id = 1L;
  // 添加
  RedisBitMapUtils.setBit(USER_ BITMAP KEY,id);
  // 移除
  RedisBitMapUtils.removeBit(USER BITMAP KEY, id);
  return AjaxResult.success();
}
```

##### 为什么 Redis 单线程模型也能效率这么高？
C语言实现，效率高
纯内存操作
基于非阻塞的IO复用模型机制
单线程的话就能避免多线程的频繁上下文切换问题
丰富的数据结构（全称采用hash结构，读取速度非常快，对数据存储进行了一些优化，比如压缩表，跳表等）

##### Redis 有几种持久化方式？ 
1、【全量】RDB 持久化，是指在指定的时间间隔内将内存中的数据集快照写入磁盘。实际操作过程是，fork 一个子进程，先将数据集写入临时文件，写入成功后，再替换之前的文件，用二进制压缩存储。

2、【增量】AOF持久化，以日志的形式记录服务器所处理的每一个写、删除操作，查询操作不会记录，以文本的方式记录，可以打开文件看到详细的操作记录。


##### Redis哨兵和集群的区别是什么？ 

Redis 的哨兵作用是管理多个 Redis 服务器，提供了监控、提醒以及自动的故障转移的功能。哨兵可以保证当主服务器挂了后，可以从从服务器选择一台当主服务器，把别的从服务器转移到读新的主机。
Redis 哨兵的主要功能有：

集群监控：对 Redis 集群的主从进程进行监控，判断是否正常工作。
消息通知：如果存在 Redis 实例有故障，那么哨兵可以发送报警消息通知管理员。
故障转移：如果主机（master）节点挂了，那么可以自动转移到从（slave）节点上。
配置中心：当存在故障时，对故障进行转移后，配置中心会通知客户端新的主机（master）地址。

Redis 的集群的功能是为了解决单机 Redis 容量有限的问题，将数据按一定的规则分配到多台机器，对内存的每秒访问不受限于单台服务器，可受益于分布式集群高扩展性

##### 哪些限流方案？

方法一、使用google的guava，令牌桶算法实现：平滑突发限流 ( SmoothBursty) 、平滑预热限流 ( SmoothWarmingUp) 实现

方法二、请求一次redis增加1，key可以是IP+时间或者一个标识+时间，没有就创建，需要设置过期时间

方法三、分布式限流，分布式限流最关键的是要将限流服务做成原子化，而解决方案可以使用redis+lua或者nginx+lua技术进行实现。

方法四、可以使用池化技术来限制总资源数：连接池、线程池。比如分配给每个应用的数据库连接是 100，那么本应用最多可以使用 100 个资源，超出了可以 等待 或者 抛异常。

方法五、限流总并发/连接/请求数，如果你使用过 Tomcat，其 Connector 其中一种配置有如下几个参数:

maxThreads: Tomcat 能启动用来处理请求的 最大线程数，如果请求处理量一直远远大于最大线程数，可能会僵死。
maxConnections: 瞬时最大连接数，超出的会 排队等待。
acceptCount: 如果 Tomcat 的线程都忙于响应，新来的连接会进入 队列排队，如果 超出排队大小，则 拒绝连接。

方法六、限流某个接口的总并发/请求数，使用 Java 中的 AtomicLong

方法七、 限流某个接口的时间窗请求数使用 Guava 的 Cache

##### redis内存满的淘汰策略

只有在 Redis 的运行内存达到了某个阀值，才会触发内存淘汰机制，这个阀值就是我们设置的最大运行内存。

此值在 Redis 的配置文件 redis.conf 中可以找到，配置项为 maxmemory。

查询最大运行内存
使用命令 config get maxmemory 来查看设置的最大运行内存。

结果为 0 是 64 位操作系统默认的值，当 maxmemory 为 0 时，表示没有内存大小限制。

32 位操作系统，默认的最大内存值是 3 GB。

内存淘汰策略
使用 config get maxmemory-policy 命令，来查看当前 Redis 的内存淘汰策略。
Redis 使用的是 noeviction 类型的内存淘汰机制，它表示当运行内存超过最大设置内存时，不淘汰任何数据，但新增操作会报错。

Redis 4.0 版本以后一共提供了 8 种数据淘汰策略，从淘汰数据的候选集范围来看，我们有两种候选范围：一种是所有数据都是候选集，一种是设置了过期时间的数据是候选集。无论是面向哪种候选数据集进行淘汰数据选择，我们都有三种策略，分别是随机选择，根据 LRU 算法选择，以及根据 LFU 算法选择

设置过期时间：
volatile-ttl 在筛选时，会针对设置了过期时间的键值对，根据过期时间的先后进行删除，越早过期的越先被删除。
volatile-random 就像它的名称一样，在设置了过期时间的键值对中，进行随机删除。
volatile-lru 会使用 LRU 算法筛选设置了过期时间的键值对。
volatile-lfu 会使用 LFU 算法选择设置了过期时间的键值对

所有数据进行淘汰：
allkeys-random 策略，从所有键值对中随机选择并删除数据；
allkeys-lru 策略，使用 LRU 算法在所有数据中进行筛选。
allkeys-lfu 策略，使用 LFU 算法在所有数据中进行筛选。

不淘汰数据：
noeviction:不淘汰任何数据，当内存不足时，新增操作会报错，Redis 默认内存淘汰策略；

LRU：最近最少使用(最长时间)淘汰算法（Least Recently Used）。 LRU是淘汰最长时间没有被使用的页面。

LFU：最不经常使用(最少次)淘汰算法（Least Frequently Used）。 LFU是淘汰一段时间内，使用次数最少的页面。

##### Redis过期删除策略

1、设置Redis键过期时间
　　Redis提供了四个命令来设置过期时间（生存时间）。

　　①、EXPIRE <key> <ttl> ：表示将键 key 的生存时间设置为 ttl 秒。

　　②、PEXPIRE <key> <ttl> ：表示将键 key 的生存时间设置为 ttl 毫秒。

　　③、EXPIREAT <key> <timestamp> ：表示将键 key 的生存时间设置为 timestamp 所指定的秒数时间戳。

　　④、PEXPIREAT <key> <timestamp> ：表示将键 key 的生存时间设置为 timestamp 所指定的毫秒数时间戳。

　　PS：在Redis内部实现中，前面三个设置过期时间的命令最后都会转换成最后一个PEXPIREAT 命令来完成。

　　另外补充两个知识点：

　　一、移除键的过期时间

　　PERSIST <key> ：表示将key的过期时间移除。

　　二、返回键的剩余生存时间

　　TTL <key> ：以秒的单位返回键 key 的剩余生存时间。

　　PTTL <key> ：以毫秒的单位返回键 key 的剩余生存时间。

2、Redis过期时间的判定
　　在Redis内部，每当我们设置一个键的过期时间时，Redis就会将该键带上过期时间存放到一个过期字典中。
   当我们查询一个键时，Redis便首先检查该键是否存在过期字典中，如果存在，那就获取其过期时间。然后将过期时间和当前系统时间进行比对，比系统时间大，那就没有过期；反之判定该键过期。
   
3、过期删除策略
　　通常删除某个key，我们有如下三种方式进行处理。

①、定时删除
　　在设置某个key 的过期时间同时，我们创建一个定时器，让定时器在该过期时间到来时，立即执行对其进行删除的操作。

　　优点：定时删除对内存是最友好的，能够保存内存的key一旦过期就能立即从内存中删除。

　　缺点：对CPU最不友好，在过期键比较多的时候，删除过期键会占用一部分 CPU 时间，对服务器的响应时间和吞吐量造成影响。

②、惰性删除
　　设置该key 过期时间后，我们不去管它，当需要该key时，我们在检查其是否过期，如果过期，我们就删掉它，反之返回该key。

　　优点：对 CPU友好，我们只会在使用该键时才会进行过期检查，对于很多用不到的key不用浪费时间进行过期检查。

　　缺点：对内存不友好，如果一个键已经过期，但是一直没有使用，那么该键就会一直存在内存中，如果数据库中有很多这种使用不到的过期键，这些键便永远不会被删除，内存永远不会释放。从而造成内存泄漏。

③、定期删除
　　每隔一段时间，我们就对一些key进行检查，删除里面过期的key。

　　优点：可以通过限制删除操作执行的时长和频率来减少删除操作对 CPU 的影响。另外定期删除，也能有效释放过期键占用的内存。

　　缺点：难以确定删除操作执行的时长和频率。

　　　　　如果执行的太频繁，定期删除策略变得和定时删除策略一样，对CPU不友好。

　　　　　如果执行的太少，那又和惰性删除一样了，过期键占用的内存不会及时得到释放。

　　　　　另外最重要的是，在获取某个键时，如果某个键的过期时间已经到了，但是还没执行定期删除，那么就会返回这个键的值，这是业务不能忍受的错误。


##### Redis分布式加锁解锁的正确方式

正确的加解锁方式，要满足以下几个条件：

1、命令必须保证互斥
2、设置的 key必须要有过期时间，防止崩溃时锁无法释放
3、value使用唯一id标志每个客户端，保证只有锁的持有者才可以释放锁

加锁直接使用set命令同时设置唯一id和过期时间；
其中解锁稍微复杂些，加锁之后可以返回唯一id，标志此锁是该客户端锁拥有；释放锁时要先判断拥有者是否是自己，然后删除，这个需要redis的lua脚本保证两个命令的原子性执行。
具体代码：
Jedis实现：
```
@Slf4j
public class RedisDistributedLock {
    private static final String LOCK_SUCCESS = "OK";
    private static final Long RELEASE_SUCCESS = 1L;
    private static final String SET_IF_NOT_EXIST = "NX";
    private static final String SET_WITH_EXPIRE_TIME = "PX";
    // 锁的超时时间
    private static int EXPIRE_TIME = 5 * 1000;
    // 锁等待时间
    private static int WAIT_TIME = 1 * 1000;

    private Jedis jedis;
    private String key;

    public RedisDistributedLock(Jedis jedis, String key) {
        this.jedis = jedis;
        this.key = key;
    }

    // 不断尝试加锁
    public String lock() {
        try {
            // 超过等待时间，加锁失败
            long waitEnd = System.currentTimeMillis() + WAIT_TIME;
            String value = UUID.randomUUID().toString();
            while (System.currentTimeMillis() < waitEnd) {
                String result = jedis.set(key, value, SET_IF_NOT_EXIST, SET_WITH_EXPIRE_TIME, EXPIRE_TIME);
                if (LOCK_SUCCESS.equals(result)) {
                    return value;
                }
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        } catch (Exception ex) {
            log.error("lock error", ex);
        }
        return null;
    }

    public boolean release(String value) {
        if (value == null) {
            return false;
        }
        // 判断key存在并且删除key必须是一个原子操作
        // 且谁拥有锁，谁释放
        String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
        Object result = new Object();
        try {
            result = jedis.eval(script, Collections.singletonList(key),
                    Collections.singletonList(value));
            if (RELEASE_SUCCESS.equals(result)) {
                log.info("release lock success, value:{}", value);
                return true;
            }
        } catch (Exception e) {
            log.error("release lock error", e);
        } finally {
            if (jedis != null) {
                jedis.close();
            }
        }
        log.info("release lock failed, value:{}, result:{}", value, result);
        return false;
    }
}
```

Redisson实现：
单机模式
```
Config config = new Config();
config.useSingleServer()
        .setAddress("redis://127.0.0.1:6379")
        .setPassword("123456")
        .setDatabase(0);
RedissonClient redissonClient = Redisson.create(config);
//获取锁对象实例
final String lockKey = "abc";
RLock rLock = redissonClient.getLock(lockKey);

try {
    //尝试5秒内获取锁，如果获取到了，最长60秒自动释放
    boolean res = rLock.tryLock(5L, 60L, TimeUnit.SECONDS);
    if (res) {
        //成功获得锁，在这里处理业务
        System.out.println("获取锁成功");
    }
} catch (Exception e) {
    System.out.println("获取锁失败，失败原因：" + e.getMessage());
} finally {
    //无论如何, 最后都要解锁
    rLock.unlock();
}

//关闭客户端
redissonClient.shutdown();
```
集群模式：
```
Config config1 = new Config();
config1.useSingleServer().setAddress("redis://192.168.3.111:6379").setPassword("a123456").setDatabase(0);
RedissonClient redissonClient1 = Redisson.create(config1);

Config config2 = new Config();
config2.useSingleServer().setAddress("redis://192.168.3.112:6379").setPassword("a123456").setDatabase(0);
RedissonClient redissonClient2 = Redisson.create(config2);

Config config3 = new Config();
config3.useSingleServer().setAddress("redis://192.168.3.113:6379").setPassword("a123456").setDatabase(0);
RedissonClient redissonClient3 = Redisson.create(config3);

//获取多个 RLock 对象
final String lockKey = "abc";
RLock lock1 = redissonClient1.getLock(lockKey);
RLock lock2 = redissonClient2.getLock(lockKey);
RLock lock3 = redissonClient3.getLock(lockKey);

//根据多个 RLock 对象构建 RedissonRedLock （最核心的差别就在这里）
RedissonRedLock redLock = new RedissonRedLock(lock1, lock2, lock3);

try {
    //尝试5秒内获取锁，如果获取到了，最长60秒自动释放
    boolean res = redLock.tryLock(5L, 60L, TimeUnit.SECONDS);
    if (res) {
        //成功获得锁，在这里处理业务
        System.out.println("获取锁成功");

    }
} catch (Exception e) {
    System.out.println("获取锁失败，失败原因：" + e.getMessage());
} finally {
    //无论如何, 最后都要解锁
    redLock.unlock();
}
```
参考：《Redis实战之Redisson使用技巧详解，干活！》 https://www.51cto.com/article/743175.html

如果拿到分布式锁的节点宕机，且这个锁正好处于锁住的状态时，会出现锁死的状态，为了避免这种情况的发生，
锁都会设置一个过期时间。这样也存在一个问题，加入一个线程拿到了锁设置了30s超时，在30s后这个线程还没有执行完毕，锁超时释放了，就会导致问题。

##### Redisson看门狗机制
上面这个问题Redisson给出了自己的答案，就是 watch dog 自动延期机制。
Redisson提供了一个监控锁的看门狗，它的作用是在Redisson实例被关闭前，不断的延长锁的有效期，也就是说，如果一个拿到锁的线程一直没有完成逻辑，
那么看门狗会使用while循环帮助线程不断的延长锁超时时间，锁不会因为超时而被释放。
默认情况下，看门狗的续期时间是30s，也可以通过修改Config.lockWatchdogTimeout来另行指定。
另外Redisson 还提供了可以指定leaseTime参数的加锁方法来指定加锁的时间。超过这个时间后锁便自动解开了，不会延长锁的有效期。

##### Redis哨兵
Redis集群的主从复制模式下，一旦主节点由于故障不能提供服务，需要人工将节点晋升为主节点，同时还要通知应用方更新主节点地址，Redis2.8之后提供Redis Sentinel（哨兵）解决这个问题。
Redis Sentinel能自动完成故障发现和故障转移，并通知应用方，真正实现高可用。

##### Redis Sentinel实现原理
Redis Sentinel通过三个定时监控任务完成对各个节点发现和监控：
  1.每隔10s，每个sentinel节点会向主节点和从节点发送info命令，获取最新的拓扑结构。
  2.每隔2s，每个sentinel节点会向redis数据节点的_sentinel_hello频道发送该sentinel节点对于主节点的判断以及当前sentinel节点的信息。
  3.每隔1s，每个sentinel节点会向主节点、从节点，其余sentinel节点发送一条ping命令做一次心跳检测，来确认节点是否可达。

##### Redis集群搭建
集群的搭建需要以下三个步骤：
   1.准备节点 2.节点握手 3.分配槽
   
   准备节点：
    Redis集群一般由多个节点组成，节点数量至少为6个才能保证完整的高可用集群，每个节点需要开启cluster-enabled yes，准备节点之后，每个节点并不知道对方的存在。
   节点握手:
    节点握手指的是集群模式下的节点通过Gossip协议彼此通信，感知对方。节点握手由客户端发起命令：cluster meet {ip} {port}
    节点握手之后，集群还不能正常工作，这时候集群处于下线状态，所有的数据读写都被禁止。
   分配槽：
    redis集群把所有的数据映射到16384个槽中，只有当节点分配了槽，才能相应这些槽关联的键命令。通过cluster addslots命令为节点分配槽。

###### Redis为什么要用跳表实现ZSET?

对于这个问题，Redis的作者 @antirez 是怎么说的：

There are a few reasons:

They are not very memory intensive. It's up to you basically. Changing parameters about the probability of a node to have a given number of levels will make then less memory intensive than btrees.

A sorted set is often target of many ZRANGE or ZREVRANGE operations, that is, traversing the skip list as a linked list. With this operation the cache locality of skip lists is at least as good as with other kind of balanced trees.

They are simpler to implement, debug, and so forth. For instance thanks to the skip list simplicity I received a patch (already in Redis master) with augmented skip lists implementing ZRANK in O(log(N)). It required little changes to the code.

简单翻译一下，主要是从内存占用、对范围查找的支持、实现难易程度这三方面总结的原因：

它们不是非常内存密集型的。基本上由你决定。改变关于节点具有给定级别数的概率的参数将使其比 btree 占用更少的内存。
Zset 经常需要执行 ZRANGE 或 ZREVRANGE-逆序 的命令，即作为链表遍历跳表。通过此操作，跳表的缓存局部性至少与其他类型的平衡树一样好。
它们更易于实现、调试等。例如，由于跳表的简单性，我收到了一个补丁（已经在Redis master中），其中扩展了跳表，在 O(log(N) 中实现了 ZRANK。它只需要对代码进行少量修改。

详细补充点：

从内存占用上来比较，跳表比平衡树更灵活一些。平衡树每个节点包含 2 个指针（分别指向左右子树），而跳表每个节点包含的指针数目平均为 1/(1-p)，具体取决于参数 p 的大小。
如果像 Redis里的实现一样，取 p=1/4，那么平均每个节点包含 1.33 个指针，比平衡树更有优势。

在做范围查找的时候，跳表比平衡树操作要简单。在平衡树上，我们找到指定范围的小值之后，还需要以中序遍历的顺序继续寻找其它不超过大值的节点。
如果不对平衡树进行一定的改造，这里的中序遍历并不容易实现。而在跳表上进行范围查找就非常简单，只需要在找到小值之后，对第 1 层链表进行若干步的遍历就可以实现。

从算法实现难度上来比较，跳表比平衡树要简单得多。平衡树的插入和删除操作可能引发子树的调整，逻辑复杂，而跳表的插入和删除只需要修改相邻节点的指针，操作简单又快速。

##### Redisson 介绍
Redisson在基于NIO的Netty框架上，充分的利⽤了Redis键值数据库提供的⼀系列优势，在Java实⽤⼯具包中常⽤接⼝的基础上，
为使⽤者提供了⼀系列具有分布式特性的常⽤⼯具类。使得原本作为协调单机多线程并发程序的⼯具包获得了协调分布式多机多线程并发系统的能⼒，
⼤⼤降低了设计和研发⼤规模分布式系统的难度。同时结合各富特⾊的分布式服务，更进⼀步简化了分布式环境中程序相互之间的协作






