
### 《Maven实战》

#### 5. 坐标和依赖

##### 5.5 依赖范围

|   依赖范围(Scope)     |    对于编译classpath有效   |    对于测试classpath有效  |   对于运行时classpath有效   |  例子    |
|  ----  | ----  | ---- | ---- | ---- |
| compile(默认值)  | Y |   Y   |   Y   |    spring-core   |
| test  | - |   Y   |   -   |    Junit   |
| provided  | Y |    Y  |    -  |    servlet-api   |
| runtime  | - |   Y   |   Y   |    JDBC驱动   |
| system  | Y |    Y  |   -   |    Maven仓库之外的类库文件   |

另外,provided与system不同之处在于，使用system时必须使用systemPath指定依赖文件的路径。import-导入依赖范围，不会对三种classpath产生影响
 
 


##### 5.7 依赖调解

Maven依赖的两个原则：1.最短路径 2.最先引入

##### 5.8 可选依赖
对于B依赖C，A依赖B的情况，那么相当于A依赖了C，这称为传递性依赖。如果出现B依赖D，但是A不需要依赖D的情况，则可在D中新增`<optional>true</optional>`标签，那么A依赖B则不会依赖D。
PS:正常情况下是不应该使用这个属性的。

##### 5.9 最佳实践

###### 5.9.1排除依赖
使用`<exclusions`标签排除依赖。
###### 5.9.2归类依赖
使用`<properties`定义常量，统一依赖的版本。
###### 5.9.3优化依赖
使用`mvn dependency:list` 命令查看所有已解析依赖(项目的直接依赖和传递性依赖，以及解决依赖冲突后的唯一版本依赖)。 <br/>
使用`mvn dependency:tree` 命令查看依赖树。<br/>
使用`mvn dependency:analyze` 命令分析当前项目的依赖，例如：Used undeclared dependencies - 项目中用到的，但是没有显示声明的依赖，Unused declared dependencies - 项目中声明但是未显示使用的依赖，
由于该命令只会分析编译主代码和测试时需要用到的依赖，一些执行测试和运行时需要的依赖会有可能发现不了，所以对于这种依赖不应该简单的删除声明。

#### 6. 仓库
仓库分为：本地仓库、远程仓库，远程仓库又进一步细分为，中央仓库(Maven核心自带)、私服、以及其他公共库(JBoss，Java net)。<br/>

##### 6.1 本地仓库
默认情况下，maven本地仓库的地址是： ${user.home}/.m2/repository 可以通过修改 <localRepository> 修改。<br/>
Maven安装目录下的/conf/setting.xml文件是全局配置的，不建议修改，m2目录下的是区分用户的。

##### 6.2 中央仓库
中央仓库为maven安装时的默认仓库，http://repo1.maven.org


##### 6.3 私服
<em>私服是一种特殊的远程仓库，他是假设在局域网内的仓库服务，私服代理广域网上的远程服务，供局域网内的maven用户使用。当maven下载构建时，先请求私服，私服不存在再从远部的仓库下载。</em>
私服的作用：
* 节省宽带
* 加速Maven构建
* 部署第三方构建，有些构建无法从中央仓库获得，建立私服便可便可将构建部署到内部仓库中。
* 提高稳定性，降低中央仓库的负载。

Maven私服配置：https://www.jianshu.com/p/145959d448a8

##### 6.4 远程仓库配置
对于项目需要的构件存在另一远程仓库的情况，maven支持在项目中的POM文件内配置多个中央仓库，但是需要注意id不能相同。举例：
```
    <repositories>
        <repository>
            <id>jboss</id>
            <name>jboss</name>
            <url>http://repository.jboss.com/maven2/</url>
            <releases>
                <enabled>true</enabled>
                <checksumPolicy>ignore</checksumPolicy>
                <updatePolicy>interval:30</updatePolicy>
            </releases>
            <snapshots>
                <enabled>false</enabled>
                <checksumPolicy>fail</checksumPolicy>
                <updatePolicy>always</updatePolicy>
            </snapshots>
            <layout>legacy</layout>
        </repository>
    </repositories>
```
配置中的`<enabled>`标签表示构件或快照版本是否允许下载，`<updatePolicy>`标签用于配置Maven从远程仓库检查更新的频率，默认值是daily，其他可选值为:never-从不，always-每次，interval:X - 表示每隔X分钟检查一次。

`<layout>`标签有legacy和default两个可选值，用于控制仓库布局，默认值为Maven2或Maven3。

###### 6.4.1 远程仓库的认证
为了阻止非法访问,可以在私服Maven仓库中配置认证信息，在setting.xml文件中：
```
<!-- 访问私服需要的用户名和密码 -->
<servers>
    <server>
        <id>repo-releases</id>
        <username>admin</username>
        <password>admin</password>
    </server>
    <server>
        <id>repo-snapshots</id>
        <username>admin</username>
        <password>admin</password>
    </server>
</servers>
```
然后在项目中POM文件添加：
```
<distributionManagement>
    <repository>
        <id>local-release</id>
        <url>http://localhost:8081/repository/maven-releases/</url>
    </repository>
    <snapshotRepository>
        <id>local-snapshot</id>
        <url>http://localhost:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```
在项目setting.xml文件中添加:
```
<servers>
    <server>
        <id>local-release</id>
        <username>snail</username>
        <password>admin</password>
    </server>
    <server>
        <id>local-snapshot</id>
        <username>snail</username>
        <password>admin</password>
    </server>
</servers>
```
注意id必须对应

###### 6.4.2 部署到远程仓库
POM文件中配置：
```
<distributionManagement>
    <repository>
        <id>local-release</id>
        <url>http://localhost:8081/repository/maven-releases/</url>
    </repository>
    <snapshotRepository>
        <id>local-snapshot</id>
        <url>http://localhost:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```
前者为发布版本、后者为快照版本。配置完毕后执行 `mvn clean deploy` 命令。


##### 6.7 镜像
如果仓库X可以提供仓库Y所存储的所有内容，那么说X是Y的一个镜像。私服配置：
```
<mirrors>
  <mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>*</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
  </mirror>
</mirrors>
```
<mirrorOf>*</mirrorOf>表示匹配所有的远程仓库，均使用镜像仓库。其他几个可选项：<br/> `<mirrorOf>external:*</mirrorOf>`,匹配所有远程仓库,使用localhost的除外。<br/> `<mirrorOf>repo1,repo2</mirrorOf>`，匹配仓库1,仓库2使用逗号分割。<br/> `<mirrorOf>*,! repo1</mirrorOf>`,匹配所有仓库,repo1除外。


#### 7. Maven声明周期和插件
maven打包类型：jar,war,ear,pom.maven-plugin
* compile:编译主代码至输出目录
* test：执行测试用例
* package:创建项目jar包
* Install插件的install命令，将项目的构建输出文件安装到本地仓库，执行命令后，Maven会复制一份打包生成的jar包到本地仓库。
* deploy:将项目输出构件部署到远程仓库

