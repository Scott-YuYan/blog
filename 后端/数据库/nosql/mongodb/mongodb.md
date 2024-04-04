docker方式安装Mongodb:

拉取最新版本镜像：
```
$ docker pull mongo:latest
```
运行mongodb容器：
```
docker run -d -p 27017:27017 --name mongo mongo
```
上面这种方式允许的mongodb默认是无序认证即可登陆的，可采用 --auth参数增加认证
```
docker run -itd --name mongo -p 27017:27017 mongo --auth
```
接着使用以下命令添加用户和设置密码，并且尝试连接。
```
docker exec -it mongo mongo admin
```
注意：
如果这时候报：
```
OCI runtime exec failed: exec failed: container_linux.go:380: starting container process caused: exec: "mongo": executable file not found in $PATH: unknown
```
原因是：
mongo5.0以上的版本使用mongo来执行mongodb命令已经不支持了，你需要改用mongosh来替代mongo！
改成：
```
docker exec -it mongo mongosh admin
```
接下来：
```
# 创建一个名为 admin，密码为 123456 的用户。
>  db.createUser({ user:'admin',pwd:'123456',roles:[ { role:'userAdminAnyDatabase', db: 'admin'},"readWriteAnyDatabase"]});
# 尝试使用上面创建的用户信息进行连接。
> db.auth('admin', '123456')
```
这样再使用可视化程序(例如MongoDB Compass)连接时，则会提示需要登陆账号密码。


##### MongoDB中的基本概念

1. 文档(Document):文档是MongoDB中最基本的数据单元

##### MongoDB系列之适用场景和不适用场景
1、MongoDB优点
可以简单列举MongoDB一些明显的主要的优点

速度：MongoDB比一般的关系型数据库快很多，作为面向文档的NoSQL数据库，MongoDB可以通过索引使访问文档变得很容易而且快速
分片：MongoDB另外一个优势是允许用户存储大量的数据，其通过分片的方式将数据分发到多个服务器上。
灵活性：因为MongoDB是非结构化的数据库系统，而且多种数据类型，所以不需要像关系型数据那样，进行特别的表结构设计，存储数据更加灵活
分布式：MongoDB数据库默认支持分布式，内带分布式的解决方案

2、MongoDB局限性
不支持连接：与支持连接的理性数据库不同，MongoDB 不支持。尽管可以通过手动编码来添加连接功能，但执行起来可能会很慢并影响性能。
数据大小有限制：MongoDB允许的文档最大值为16MB
不能无限嵌套：MongoDB的数据格式是BSON的，但是其不支持无限的嵌套，用户不能超过100级的文档嵌套
高内存：MongoDB会存储每个值对的键名。它还受到数据冗余的影响，因为它缺乏连接的功能。这会导致高内存使用率。
不支持业务复杂查询：MySQL这些类型数据库都可以进行表连接等等复杂业务查询，MongoDB是文档型的数据库，所以不支持联表(Collection)查询

在MongoDB官网也会列举了MongoDB的适用场景：

1）网站实时数据:MongoDB 非常适合实时的插入，更新与查询，并具备网站实时数据存储所需的复制及
高度伸缩性。
2）数据缓存:由于性能很高，MongoDB 也适合作为信息基础设施的缓存层。在系统重启之后，由 MongoDB
搭建的持久化缓存层可以避免下层的数据源过载。
3）大尺寸、低价值数据存储:使用传统的关系型数据库存储一些数据时可能会比较昂贵，在此之前，很
多时候程序员往往会选择传统的文件进行存储。
4）高伸缩性场景:MongoDB 非常适合由数十或数百台服务器组成的数据库。MongoDB 的路线图中已经包
含对 MapReduce 引擎的内置支持。
5）对象或 JSON 数据存储:MongoDB 的 BSON 数据格式非常适合文档化格式的存储及查询。

4、不适用场景
1）高度事务性系统:例如银行或会计这些金融系统。传统的关系型数据库目前还是更适用于需要大量原子性复杂事务的应用程序。
2）传统的商业智能应用:针对特定问题的 BI 数据库会对产生高度优化的查询方式。对于此类应用，关系型可能是更合适的选择。

##### MongoDB基本数据类型
Object  ID ：Documents 自动生成的 _id，插入数据时候会生成 _id，唯一字段

String： 字符串，必须是utf-8

Boolean：布尔值，true 或者false 

Integer：整数 (Int32 Int64 你们就知道有个Int就行了,一般我们用Int32)

Double：浮点数 (没有float类型,所有小数都是Double)

Arrays：数组或者列表list，多个值存储到一个键 

Object：这个数据类型就是字典

Null：空数据类型 , 一个特殊的概念,None Null

Timestamp：时间戳

Date：存储当前日期或时间unix时间格式 
