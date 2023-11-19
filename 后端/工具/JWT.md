JWT官网：https://jwt.io/  <br/>
jwt的两种依赖导入方式：
```
<!--        jwt 依赖-->
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>

<!--另一种token插件 性能更高-->
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>3.10.3</version>
</dependency>
```
SpringBoot集成JWT:https://blog.csdn.net/CSDN2497242041/article/details/115605626 <br/>
Base64解码： https://www.sojson.com/base64.html
JWT生成的String包括三部分：头(加密算法、类型) + 负载（用户自定义值-这部分内容可以解密出来）+ 签名（无法解密）