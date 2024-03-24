##### Elasticsearch概述

1.1 Elasticsearch是什么
Elasticsearch(ES)是一个基于Apache的开源索引库Lucene而构建的 开源、分布式、具有RESTful接口的全文搜索引擎, 还是一个 分布式文档数据库.

ES可以轻松扩展数以百计的服务器(水平扩展), 用于存储和处理数据. 它可以在很短的时间内存储、搜索和分析海量数据, 通常被作为复杂搜索场景下的核心引擎.

由于Lucene提供的API操作起来非常繁琐, 需要编写大量的代码, Elasticsearch对Lucene进行了封装与优化, 并提供了REST风格的操作接口, 开箱即用, 很大程度上方便了开发人员的使用.

关于全文检索与Lucene方面的简介, 请参照博主的博客: Lucene 01 - 初步认识全文检索和Lucene

1.2 Elasticsearch的优点
(1) 横向可扩展性: 作为大型分布式集群, 很容易就能扩展新的服务器到ES集群中; 也可运行在单机上作为轻量级搜索引擎使用.
(2) 更丰富的功能: 与传统关系型数据库相比, ES提供了全文检索、同义词处理、相关度排名、复杂数据分析、海量数据的近实时处理等功能.
(3) 分片机制提供更好地分布性: 同一个索引被分为多个分片(Shard), 利用分而治之的思想提升处理效率.
(4) 高可用: 提供副本(Replica)机制, 一个分片可以设置多个副本, 即使在某些服务器宕机后, 集群仍能正常工作.
(5) 开箱即用: 提供简单易用的API, 服务的搭建、部署和使用都很容易操作.

##### Elasticsearch的使用场景

(1) 维基百科(类似百度百科): 全文检索, 高亮, 搜索推荐;
(2) The Guardian(新闻网站): 用户行为日志(点击, 浏览, 收藏, 评论) + 社交网络数据(对某某新闻的相关看法), 数据分析(将公众对文章的反馈提交至文章作者);
(3) Stack Overflow(IT技术论坛): 全文检索, 搜索相关问题和答案;
(4) GitHub(开源代码管理), 搜索管理其托管的上千亿行代码;
(5) 日志数据分析: ELK技术栈(Elasticsearch + Logstash + Kibana)对日志数据进行采集&分析;
(6) 商品价格监控网站: 用户设定某商品的价格阈值, 当价格低于该阈值时, 向用户推送降价消息;
(7) BI系统(Business Intelligence, 商业智能): 分析某区域最近3年的用户消费额的趋势、用户群体的组成结构等;
(8) 其他应用: 电商、招聘、门户等网站的内部搜索服务, IT系统(OA, CRM, ERP等)的内部搜索服务、数据分析(ES的又一热门使用场景).

##### ES的基本数据类型
* 1.1.1 文本类型 - text
在Elasticsearch 5.4 版本开始, text取代了需要分词的string.

—— 当一个字段需要用于全文搜索(会被分词), 比如产品名称、产品描述信息, 就应该使用text类型.

text的内容会被分词, 可以设置是否需要存储: "index": "true|false".
text类型的字段不能用于排序, 也很少用于聚合.

使用示例:

```
PUT website
{
	"mappings": {
        "blog": {
            "properties": {
        		"summary": {"type": "text", "index": "true"}
            }
        }
    }
} 
```

.在现实场景中，keyword经常用于描述姓名、产品类型、用户ID、URL和状态码等。keyword类型数据一般用于比较字符串是否相等，不对数据进行部分匹配，因此一般查询这种类型的数据时使用term查询。
 
* 1.1.2 关键字类型 - keyword
在Elasticsearch 5.4 版本开始, keyword取代了不需要分词的string.

—— 当一个字段需要按照精确值进行过滤、排序、聚合等操作时, 就应该使用keyword类型.

keyword的内容不会被分词, 可以设置是否需要存储: "index": "true|false".

使用示例:

```
PUT website
{
	"mappings": {
        "blog": {
            "properties": {
        		"tags": {"type": "keyword", "index": "true"}
            }
        }
    }
} 
```

1.2 数字类型 - 8种
数字类型有如下分类:

类型	说明
byte	有符号的8位整数, 范围: [-128 ~ 127]
short	有符号的16位整数, 范围: [-32768 ~ 32767]
integer	有符号的32位整数, 范围: [−2^31 ~ 2^31-1]
long	有符号的64位整数, 范围: [−2^63 ~ 2^63-1]
float	32位单精度浮点数
double	64位双精度浮点数
half_float	16位半精度IEEE 754浮点类型
scaled_float	缩放类型的的浮点数, 比如price字段只需精确到分, 57.34缩放因子为100, 存储结果为5734
使用注意事项:

尽可能选择范围小的数据类型, 字段的长度越短, 索引和搜索的效率越高;
优先考虑使用带缩放因子的浮点类型.  

使用示例:

```
PUT shop
{
    "mappings": {
        "book": {
            "properties": {
                "name": {"type": "text"},
                "quantity": {"type": "integer"},  // integer类型
                "price": {
                    "type": "scaled_float",       // scaled_float类型
                    "scaling_factor": 100
                }
            }
        }
    }
}
```

1.3 日期类型 - date
JSON没有日期数据类型, 所以在ES中, 日期可以是:

包含格式化日期的字符串, "2018-10-01", 或"2018/10/01 12:10:30".
代表时间毫秒数的长整型数字.
代表时间秒数的整数.
如果时区未指定, 日期将被转换为UTC格式, 但存储的却是长整型的毫秒值.
可以自定义日期格式, 若未指定, 则使用默认格式: strict_date_optional_time||epoch_millis

(1) 使用日期格式示例:

// 添加映射
PUT website
{
    "mappings": {
        "blog": {
            "properties": {
                "pub_date": {"type": "date"}   // 日期类型
            }
        }
    }
}

// 添加数据
PUT website/blog/11
{ "pub_date": "2018-10-10" }

PUT website/blog/12
{ "pub_date": "2018-10-10T12:00:00Z" }	// Solr中默认使用的日期格式

PUT website/blog/13
{ "pub_date": "1589584930103" }			// 时间的毫秒值


(2) 多种日期格式:

多个格式使用双竖线||分隔, 每个格式都会被依次尝试, 直到找到匹配的.
第一个格式用于将时间毫秒值转换为对应格式的字符串.

使用示例:

// 添加映射
PUT website
{
    "mappings": {
        "blog": {
            "properties": {
                "date": {
                    "type": "date",  // 可以接受如下类型的格式
                    "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
                }
            }
        }
    }
}

1.4 布尔类型 - boolean
可以接受表示真、假的字符串或数字:

真值: true, "true", "on", "yes", "1"...
假值: false, "false", "off", "no", "0", ""(空字符串), 0.0, 0


1.5 二进制型 - binary
二进制类型是Base64编码字符串的二进制值, 不以默认的方式存储, 且不能被搜索. 有2个设置项:

(1) doc_values: 该字段是否需要存储到磁盘上, 方便以后用来排序、聚合或脚本查询. 接受true和false(默认);
(2) store: 该字段的值是否要和_source分开存储、检索, 意思是除了_source中, 是否要单独再存储一份. 接受true或false(默认).

使用示例:

// 添加映射
PUT website
{
    "mappings": {
        "blog": {
            "properties": {
                "blob": {"type": "binary"}   // 二进制
            }
        }
    }
}
// 添加数据
PUT website/blog/1
{
    "title": "Some binary blog",
    "blob": "hED903KSrA084fRiD5JLgY=="
}
注意: Base64编码的二进制值不能嵌入换行符\n, 逗号(0x2c)等符号.
  
1.6 范围类型 - range
range类型支持以下几种:

类型	范围
integer_range	−2^31 ~ 2^31−1
long_range	−2^63 ~ 2^63−1
float_range	32位单精度浮点型
double_range	64位双精度浮点型
date_range	64位整数, 毫秒计时
ip_range	IP值的范围, 支持IPV4和IPV6, 或者这两种同时存在
(1) 添加映射:

PUT company
{
    "mappings": {
        "department": {
            "properties": {
                "expected_number": {  // 预期员工数
                    "type": "integer_range"
                },
                "time_frame": {       // 发展时间线
                    "type": "date_range", 
                    "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
                },
                "ip_whitelist": {     // ip白名单
                    "type": "ip_range"
                }
            }
        }
    }
}
(2) 添加数据:

PUT company/department/1
{
    "expected_number" : {
        "gte" : 10,
        "lte" : 20
    },
    "time_frame" : { 
        "gte" : "2018-10-01 12:00:00", 
        "lte" : "2018-11-01"
    }, 
    "ip_whitelist": "192.168.0.0/16"
}
(3) 查询数据:

GET company/department/_search
{
    "query": {
        "term": {
            "expected_number": {
                "value": 12
            }
        }
    }
}
GET company/department/_search
{
    "query": {
        "range": {
            "time_frame": {
                "gte": "208-08-01",
                "lte": "2018-12-01",
                "relation": "within" 
            }
        }
    }
}
查询结果：

{
  "took": 26,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 1,
    "max_score": 1.0,
    "hits": [
      {
        "_index": "company",
        "_type": "department",
        "_id": "1",
        "_score": 1.0,
        "_source": {
          "expected_number": {
            "gte": 10,
            "lte": 20
          },
          "time_frame": {
            "gte": "2018-10-01 12:00:00",
            "lte": "2018-11-01"
          },
          "ip_whitelist" : "192.168.0.0/16"
        }
      }
    ]
  }
}
  

##### ES - 检索和过滤的区别 (query v.s filter) 
1.1 准备测试数据
```
PUT website/_doc/1
{
    "title": "小白学ES01",
    "desc": "the first blog about es",
    "level": 1, 
    "post_date": "2018-10-10",
    "post_address": {
        "country": "China",
        "province": "GuangDong",
        "city": "GuangZhou"
    }
}

PUT website/_doc/2
{
    "title": "小白学ES02",
    "desc": "the second blog about es",
    "level": 3,
    "post_date": "2018-11-11",
    "post_address": {
        "country": "China",
        "province": "ZheJiang",
        "city": "HangZhou"
    }
}
```

1.2 搜索测试 
 
 不使用filter
```
GET website/_doc/_search
{
    "query": {
        "bool": {
            "must": [
                { "match": { "post_date": "2018-11-11" } }, 
                { "range": { "level": { "gte": 2 } } }
            ]
        }
    }
}
// 结果信息: 
"hits": {
    "total": 1,
    "max_score": 2.0,
    "hits": [
        {
            "_index": "website2",
        	"_type": "blog",
        	"_id": "2",
        	"_score": 2.0,			// 评分为2.0
        	"_source": {
          		"title": "小白学ES02",
          		"desc": "the second blog about es",
          		"level": 3,
          		"post_date": "2018-11-11",
          		"post_address": {
            		"country": "China",
            		"province": "ZheJiang",
            		"city": "HangZhou"
          		}
        	}
      	}
	]
}
```
使用filter:
```
GET website/_doc/_search
{
    "query": {
        "bool": {
            "must": { 
                "match": { "post_date": "2018-11-11" }
            }, 
            "filter": {
                "range": { "level": { "gte": 2 } }
            }
        }
    }
}
// 结果信息: 
"hits": {
    "total": 1,
    "max_score": 1.0,
    "hits": [
        {
        	"_index": "website2",
        	"_type": "blog",
        	"_id": "2",
        	"_score": 1.0,		// 评分为1.0
        	"_source": {
          		"title": "小白学ES02",
          		"desc": "the second blog about es",
          		"level": 3,
          		"post_date": "2018-11-11",
          		"post_address": {
            		"country": "China",
            		"province": "ZheJiang",
            		"city": "HangZhou"
          		}
        	}
      	}
    ]
}
```
2 复杂数据类型
2.1 数组类型 - array
ES中没有专门的数组类型, 直接使用[]定义即可;

数组中所有的值必须是同一种数据类型, 不支持混合数据类型的数组:

① 字符串数组: ["one", "two"];
② 整数数组: [1, 2];
③ 由数组组成的数组: [1, [2, 3]], 等价于[1, 2, 3];
④ 对象数组: [{"name": "Tom", "age": 20}, {"name": "Jerry", "age": 18}].

注意:

动态添加数据时, 数组中第一个值的类型决定整个数组的类型;
不支持混合数组类型, 比如[1, "abc"];
数组可以包含null值, 空数组[]会被当做missing field —— 没有值的字段.
2.2 对象类型 - object
JSON文档是分层的: 文档可以包含内部对象, 内部对象也可以包含内部对象.

(1) 添加示例:

PUT employee/developer/1
{
    "name": "ma_shoufeng",
    "address": {
        "region": "China",
        "location": {"province": "GuangDong", "city": "GuangZhou"}
    }
}
(2) 存储方式:

{
    "name":                       "ma_shoufeng",
    "address.region":             "China",
    "address.location.province":  "GuangDong", 
    "address.location.city":      "GuangZhou"
}
(3) 文档的映射结构类似为:

PUT employee
{
    "mappings": {
        "developer": {
            "properties": {
                "name": { "type": "text", "index": "true" }, 
                "address": {
                    "properties": {
                        "region": { "type": "keyword", "index": "true" },
                        "location": {
                            "properties": {
                                "province": { "type": "keyword", "index": "true" },
                                "city": { "type": "keyword", "index": "true" }
                            }
                        }
                    }
                }
            }
        }
    }
}
2.3 嵌套类型 - nested
嵌套类型是对象数据类型的一个特例, 可以让array类型的对象被独立索引和搜索.

2.3.1 对象数组是如何存储的
① 添加数据:

PUT game_of_thrones/role/1
{
    "group": "stark",
	"performer": [
        {"first": "John", "last": "Snow"},
        {"first": "Sansa", "last": "Stark"}
    ]
}
② 内部存储结构:

{
    "group": 	         "stark",
    "performer.first": [ "john", "sansa" ],
    "performer.last":  [ "snow", "stark" ]
}
③ 存储分析:

可以看出, user.first和user.last会被平铺为多值字段, 这样一来, John和Snow之间的关联性就丢失了.

在查询时, 可能出现John Stark的结果.

2.3.2 用nested类型解决object类型的不足
如果需要对以最对象进行索引, 且保留数组中每个对象的独立性, 就应该使用嵌套数据类型.

—— 嵌套对象实质是将每个对象分离出来, 作为隐藏文档进行索引.

① 创建映射:

PUT game_of_thrones
{
    "mappings": {
        "role": {
            "properties": {
                "performer": {"type": "nested" }
            }
        }
    }
}
② 添加数据:

PUT game_of_thrones/role/1
{
    "group" : "stark",
    "performer" : [
        {"first": "John", "last": "Snow"},
        {"first": "Sansa", "last": "Stark"}
    ]
}
③ 检索数据:

GET game_of_thrones/_search
{
    "query": {
        "nested": {
            "path": "performer",
            "query": {
                "bool": {
                    "must": [
                        { "match": { "performer.first": "John" }},
                        { "match": { "performer.last":  "Snow" }} 
                    ]
                }
            }, 
            "inner_hits": {
                "highlight": {
                    "fields": {"performer.first": {}}
                }
            }
        }
    }
}
3 地理数据类型
3.1 地理点类型 - geo point
地理点类型用于存储地理位置的经纬度对, 可用于:

查找一定范围内的地理点;
通过地理位置或相对某个中心点的距离聚合文档;
将距离整合到文档的相关性评分中;
通过距离对文档进行排序.
(1) 添加映射:

PUT employee
{
    "mappings": {
        "developer": {
            "properties": {
                "location": {"type": "geo_point"}
            }
        }
    }
}
(2) 存储地理位置:

// 方式一: 纬度 + 经度键值对
PUT employee/developer/1
{
    "text": "小蛮腰-键值对地理点参数", 
    "location": {
        "lat": 23.11, "lon": 113.33		// 纬度: latitude, 经度: longitude
    }
}

// 方式二: "纬度, 经度"的字符串参数
PUT employee/developer/2
{
  "text": "小蛮腰-字符串地理点参数",
  "location": "23.11, 113.33" 			// 纬度, 经度
}

// 方式三: ["经度, 纬度"] 数组地理点参数
PUT employee/developer/3
{
  "text": "小蛮腰-数组参数",
  "location": [ 113.33, 23.11 ] 		// 经度, 纬度
}
(3) 查询示例:

GET employee/_search
{
    "query": { 
        "geo_bounding_box": { 
            "location": {
                "top_left": { "lat": 24, "lon": 113 },		// 地理盒子模型的上-左边
                "bottom_right": { "lat": 22, "lon": 114 }	// 地理盒子模型的下-右边
            }
        }
    }
}
3.2 地理形状类型 - geo_shape
是多边形的复杂形状. 使用较少, 这里省略.

可以参考这篇文章: Elasticsearch地理位置总结


4 专门数据类型
4.1 IP类型
IP类型的字段用于存储IPv4或IPv6的地址, 本质上是一个长整型字段.

(1) 添加映射:

PUT employee
{
    "mappings": {
        "customer": {
            "properties": {
                "ip_addr": { "type": "ip" }
            }
        }
    }
}
(2) 添加数据:

PUT employee/customer/1
{ "ip_addr": "192.168.1.1" }
(3) 查询数据:

GET employee/customer/_search
{
    "query": {
        "term": { "ip_addr": "192.168.0.0/16" }
    }
}
4.2 计数数据类型 - token_count
token_count类型用于统计字符串中的单词数量.

本质上是一个整数型字段, 接受并分析字符串值, 然后索引字符串中单词的个数.

(1) 添加映射:

PUT employee
{
    "mappings": {
        "customer": {
            "properties": {
                "name": { 
                    "type": "text",
                    "fields": {
                        "length": {
                            "type": "token_count", 
                            "analyzer": "standard"
                        }
                    }
                }
            }
        }
    }
}
(2) 添加数据:

PUT employee/customer/1
{ "name": "John Snow" }
PUT employee/customer/2
{ "name": "Tyrion Lannister" }
(3) 查询数据:

GET employee/customer/_search
{
    "query": {
        "term": { "name.length": 2 }
    }
}

##### Elasticsearch如何定制分词器 (自定义分词策略) 
分析器的组成
① 字符过滤器(character filter): 比如去除HTML标签、把&替换为and等.

② 分词器(tokenizer): 按照某种规律, 如根据空格、逗号等, 将文本块进行分解.

③ 标记过滤器(token filter): 所有被分词器分解的词都将经过token filters的处理, 它可以修改词(如小写化处理)、去掉词(根据某一规则去掉无意义的词, 如"a", "the", "的"等), 增加词(如同义词"jump"、"leap"等).

(1) ES中的默认分词器: standard tokenizer, 是标准分词器, 它以单词为边界进行分词. 具有如下功能:

① standard token filter: 去掉无意义的标签, 如<>, &, - 等.
② lowercase token filter: 将所有字母转换为小写字母.
③ stop token filer(默认被禁用): 移除停用词, 比如"a"、"the"等.

见博客：https://www.cnblogs.com/shoufeng/p/10562746.html


##### Elasticsearch的分页查询及其深分页问题 (deep paging) 

1 分页查询方法
在GET请求中拼接from和size参数
// 查询10条数据, 默认从第0条数据开始
GET book_shop/_search?size=10
// 从第0条数据开始(包括第0条), 查询10条数据
GET book_shop/_search?from=0&size=10
// 从第5条数据开始(包括第5条), 查询10条数据
GET book_shop/_search?from=5&size=10

2 分页查询的deep paging问题
deep paging, 就是深层分页搜索:

分页搜索的深度越深, 协调节点(负责分发查询、汇总结果的ES节点)上要存储的数据就越多, 协调节点对这些数据整体排序后, 再取对应页的数据.

这个过程既耗费网络资源, 也耗费内存和CPU资源.

应该尽可能避免deep paging操作. —— 方法类似于Solr的游标

##### 正排索引 和 倒排索引 区别
一、什么是正排索引（forward index）？

简言之，由key查询实体的过程，使用正排索引。

例如，用户表：

t_user(uid, name, passwd, age, sex)

由uid查询整行的过程，就是正排索引查询。

画外音：时间复杂度可以认为是O(1)。

二、什么是倒排索引（inverted index）？

与正排索引相反，由item查询key的过程，使用倒排索引。

对于网页搜索，倒排索引可以理解为：

Map<item, list<url>>

能够由查询词快速找到包含这个查询词的网页的数据结构。

画外音：时间复杂度也是O(1)。

举个例子，假设有3个网页：

url1 -> “我爱北京”

url2 -> “我爱到家”

url3 -> “到家美好”

这是一个正排索引：

Map<url, page_content>。

分词之后：

url1 -> {我，爱，北京}

url2 -> {我，爱，到家}

url3 -> {到家，美好}

这是一个分词后的正排索引：

Map<url, list<item>>。

分词后倒排索引：

我 -> {url1, url2}

爱 -> {url1, url2}

北京 -> {url1}

到家 -> {url2, url3}

美好 -> {url3}

由检索词item快速找到包含这个查询词的网页Map<item, list<url>>就是倒排索引。

画外音：明白了吧，词到url的过程，是倒排索引。

正排索引和倒排索引是spider和build_index系统提前建立好的数据结构，为什么要使用这两种数据结构，是因为它能够快速的实现“用户网页检索”需求。

画外音，业务需求决定架构实现，查询起来都很快。

#####  filter与query的区别
filter和query一起使用时, 会先执行filter.

2.1 相关度处理上的不同
filter —— 只根据搜索条件过滤出符合的文档, 将这些文档的评分固定为1, 忽略TF/IDF信息, 不计算相关度分数;
query —— 先查询符合搜索条件的文档, 然后计算每个文档对于搜索条件的相关度分数, 再根据评分倒序排序.

建议:

如果对搜索结果有排序的要求, 要将最匹配的文档排在最前面, 就用query;
如果只是根据一定的条件筛选出部分数据, 不关注结果的排序, 就用filter.
2.2 性能上的对比
filter 性能更好, 无排序 —— 不计算相关度分数, 不用根据相关度分数进行排序, 同时ES内部还会缓存(cache)比较常用的filter的数据 (使用bitset <0或1> 来记录包含与否).

query 性能较差, 有排序 —— 要计算相关度分数, 要根据相关度分数进行排序, 并且没有cache功能.

2.3 对比结论
业务关心的、需要根据匹配的相关度进行排序的搜索条件 放在 query 中;

业务不关心、不需要根据匹配的相关度进行排序的搜索条件 放在 filter 中.