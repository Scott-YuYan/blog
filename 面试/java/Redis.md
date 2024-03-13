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


##### Redis 哨兵和集群的区别是什么？ 

Redis 的哨兵作用是管理多个 Redis 服务器，提供了监控、提醒以及自动的故障转移的功能。哨兵可以保证当主服务器挂了后，可以从从服务器选择一台当主服务器，把别的从服务器转移到读新的主机。Redis 哨兵的主要功能有：

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





