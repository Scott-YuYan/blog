用于MySQL数据闪回

操作步骤：

1、首先保证本机上安装了python，以python3为例：
官网 https://www.python.org/downloads/ 下载安装包后，使用python3命令查看安装是否成功，control+z退出。

2、安装binlog2sql(Python版)
```
git clone https://github.com/danfengcao/binlog2sql.git
cd binlog2sql
pip3 install -r requirements.txt
```
如果没有pip，使用：
```
curl https://bootstrap.pypa.io/get-pip.py | python3
```
安装并检查。

MySQL必须 开启binlog，且binlog_format=ROW，且binlog_row_image=FULL，启动时使用命令：
```
docker run -d \
  --name mysql \
  -e MYSQL_ROOT_PASSWORD=my-secret-pw \
  -e MYSQL_SERVER_ID=1 \
  -e MYSQL_LOG_BIN=/var/log/mysql/mysql-bin.log \
  -e MYSQL_MAX_BINLOG_SIZE=1G \
  -e MYSQL_BINLOG_FORMAT=row \
  -e MYSQL_BINLOG_ROW_IMAGE=full \
  -p 3306:3306 \
  -v /Users/mr-yuyan/Documents/Home/custom/:/etc/mysql/conf.d \
  -e MYSQL_ROOT_PASSWORD=password \
  mysql:8.0
```
查看以上配置：
```
show variables like 'log_bin';
show variables like '%binlog_format%';
show variables like '%binlog_row_image%';
```
授权：
```
建议授权
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'root'@'%';
```

3、解析语句
启动后到安装目录下，例/Users/mr-yuyan/binlog2sql/binlog2sql，执行命令：

```
// 解析出标准SQL
python3 binlog2sql.py -h 127.0.0.1 -P 3306 -u root -p password -d ld -t user --start-file='binlog.000002'
// 解析出回滚SQL
python3 binlog2sql.py --flashback -h 127.0.0.1 -P 3306 -u root -p 'password' -d ld -t user --start-file='binlog.000002'
```

如果出现语句报错，则可尝试，升级PyMySQL
```
pip3 install --upgrade PyMySQL
```
如果出现ModuleNotFoundError: No module named 'pymysql.util'，则可尝试：
```
pip3 uninstall pymysql
pip3 install pymysql==0.9.3
```

##### 参考资料
MySQL数据误删flashback：  https://www.cnblogs.com/strongmore/p/17131251.html
binlog2sql: https://github.com/danfengcao/binlog2sql
借用binlog2sql工具轻松解析MySQL的binlog文件，再现Oracle的闪回功能:https://www.modb.pro/db/1703739734627536896
