##### Nacos启动报No datasource set

nacos默认用户名密码：nacos / nacos

db.url的问题:

db.url.0=jdbc:mysql://localhost:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&serverTimezone=UTC

或者：

通过grep 发现config-fatal.log 中有
Connection Java - MySQL : Public Key Retrieval is not allowed （重启了很多次有时没有这个报错输出）

最核心的原因是mysql 8.0.13开始， 使用sslMode属性代替了原来的useSSL属性， 所以吧useSSL改成sslMode=DISABLED 或者添加allowPublicKeyRetrieval=true

##### nacos默认以集群方式启动
 需要加 -m standalone 以单机方式执行
 

