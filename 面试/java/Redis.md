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

Redis穿透：指的是恶意请求或者非法请求，访问不存在的key，导致请求直接穿透到DB层。由于DB层没有相应的缓存，每个请求都会直接查询DB，从而导致DB压力过大。为了避免Redis穿透，可以采取如下措施：

对查询结果为空的key也进行缓存，但设置一个较短的过期时间，防止同一请求频繁访问不存在的key。
过滤掉非法请求。
在应用层设置布隆过滤器（Bloom Filter），判断请求是否合法。
Redis雪崩：指的是缓存中的大量key同时失效，导致所有的请求都访问DB。由于缓存失效是随机的，无法预测，因此每个请求都需要直接查询DB，从而导致DB压力过大。为了避免Redis雪崩，可以采取如下措施：

为不同的key设置不同的过期时间，避免所有key同时失效。
使用Redis集群，将缓存数据分散到不同节点上，降低节点失效的风险。
在应用层设置限流、降级等措施，减少请求量，降低DB压力。
