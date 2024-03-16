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