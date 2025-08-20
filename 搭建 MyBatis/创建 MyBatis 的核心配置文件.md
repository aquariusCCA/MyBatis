---
up:
  - "[[MyBatis 課程描述]]"
---
> 习惯上命名为`mybatis-config.xml`，这个文件名仅仅只是建议，并非强制要求。将来整合 Spring 之后，这个配置文件可以省略，所以大家操作时可以直接复制、粘贴。

核心配置文件主要用于配置连接数据库的环境以及 MyBatis 的全局配置信息

核心配置文件存放的位置是 `src/main/resources` 目录下

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver  
jdbc.url=jdbc:mysql://localhost:3306/mybatis  
jdbc.username=root  
jdbc.password=Lins860210SStar
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!-- 引入properties文件 -->
    <properties resource="jdbc.properties" />

    <!-- 设置连接数据库的环境 -->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <!-- 设置连接数据库的驱动 -->
                <property name="driver" value="${jdbc.driver}" />
                <!-- 设置连接数据库的连接地址 -->
                <property name="url" value="${jdbc.url}" />
                <!-- 设置连接数据库的用户名 -->
                <property name="username" value="${jdbc.username}" />
                <!-- 设置连接数据库的密码 -->
                <property name="password" value="${jdbc.password}" />
            </dataSource>
        </environment>
    </environments>

    <!--引入映射文件-->
    <mappers>
        <mapper resource="mappers/UserMapper.xml"/>
    </mappers>
</configuration>
```
