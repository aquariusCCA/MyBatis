---
up:
  - "[[MyBatis 課程描述]]"
---
-   相关概念：ORM（Object Relationship Mapping）对象关系映射。
    -   对象：Java 的实体类对象
    -   关系：关系型数据库
    -   映射：二者之间的对应关系

| Java 概念 | 数据库概念 |
| ------- | ----- |
| 类       | 表     |
| 属性      | 字段/列  |
| 对象      | 记录/行  |

-   映射文件的命名规则
    -   表所对应的实体类的 ==类名 + Mapper.xml==
    -   例如：表 t_user，映射的实体类为 User，所对应的映射文件为 UserMapper.xml
    -   因此一个映射文件对应一个实体类，对应一张表的操作
    -   MyBatis 映射文件用于编写 SQL，访问以及操作表中的数据
    -   MyBatis 映射文件存放的位置是 src/main/resources/mappers 目录下

-   MyBatis 中可以面向接口操作数据，要保证两个一致
	-   mapper 接口的全类名和映射文件的命名空间（namespace）保持一致
	-   mapper 接口中方法的方法名和映射文件中编写 SQL 的标签的 id 属性保持一致

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.atguigu.mybatis.mapper.UserMapper">
    <!-- int insertUser(); -->
    <insert id="insertUser">
        INSERT INTO t_user (emp_name, age, sex, email, did)
        VALUES ('admin', 23, 1, '12345@qq.com', NULL);  -- 1=男, 2=女
    </insert>
</mapper>
```
