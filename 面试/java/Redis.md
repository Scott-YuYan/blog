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




