---
up:
  - "[[MyBatis 課程描述]]"
---

- resultMap：设置自定义映射
    - 属性：
        - id：表示自定义映射的唯一标识，不能重复
        - type：查询的数据要映射的实体类的类型
    - 子标签：
        - id：设置主键的映射关系
        - result：设置普通字段的映射关系
        - 子标签属性：
            - property：设置映射关系中实体类中的属性名
            - column：设置映射关系中表中的字段名

- 若字段名和实体类中的属性名不一致，则可以通过 resultMap 设置自定义映射，即使字段名和属性名一致的属性也要映射，也就是全部属性都要列出来

```xml
<resultMap id="empResultMap" type="Emp">  
    <id property="eid" column="eid"></id>  
    <result property="empName" column="emp_name" />  
    <result property="age" column="age" />  
    <result property="sex" column="sex" />  
    <result property="email" column="email" />  
</resultMap>  
  
<!-- List<Emp> getAllEmp(); -->  
<select id="getAllEmp" resultMap="empResultMap">  
    select * from t_emp  
</select>
```

若字段名和实体类中的属性名不一致，但是字段名符合数据库的规则（使用 `_` ），实体类中的属性名符合 Java 的规则（使用驼峰）。此时也可通过以下两种方式处理字段名和实体类中的属性的映射关系

可以通过为字段起别名的方式，保证和实体类中的属性名保持一致

```xml
<!--List<Emp> getAllEmp();-->  
<select id="getAllEmp" resultType="Emp">  
    select eid, emp_name empName, age, sex, email from t_emp  
</select>
```

可以在 MyBatis 的核心配置文件中的`setting`标签中，设置一个全局配置信息 mapUnderscoreToCamelCase，可以在查询表中数据时，自动将 `_` 类型的字段名转换为驼峰，例如：字段名 user_name，设置了 mapUnderscoreToCamelCase，此时字段名就会转换为 userName。[核心配置文件详解](#核心配置文件详解)

```xml
<settings>
	<setting name="mapUnderscoreToCamelCase" value="true"/>
</settings>
```