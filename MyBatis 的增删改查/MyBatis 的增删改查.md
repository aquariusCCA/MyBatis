---
up:
  - "[[MyBatis 課程描述]]"
---
# User 實體類

```java
/**  
 * 對應表：t_user  
 * 注意：sex = 1(男), 2(女)  
 */
@Data  
@NoArgsConstructor  
@AllArgsConstructor  
public class User implements Serializable {  
    private Integer eid;      // PK，自增  
    private String  empName;  // 對應 emp_name    private Integer age;  
    private Integer sex;      // 1=男, 2=女  
    private String  email;  
    private Integer did;  
}
```

---

# UserMaper 接口

```java
public interface UserMapper {  
    /**  
     * MyBatis面向接口编程的两个一致：  
     * 1.映射文件的namespace要和mapper接口的全类名保持一致  
     * 2.映射文件中SQL语句的id要和mapper接口中的方法名一致  
     *  
     * 表--实体类--mapper接口--映射文件  
     */  
  
    /**     * 添加用户信息  
     *  
     * @return  
     */  
    int insertUser();  
  
    /**  
     * 修改用户信息  
     */  
    void updateUser();  
  
    /**  
     * 删除用户信息  
     */  
    void deleteUser();  
  
    /**  
     * 根据 id 查询用户信息  
     *  
     * @return  
     */  
    User getUserById();  
  
    /**  
     * 查询所有的用户信息  
     *  
     * @return  
     */  
    List<User> getAllUser();  
}
```

---

# 映射文件

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

    <!-- void deleteUser(); -->
    <delete id="deleteUser">
        DELETE FROM t_user WHERE eid = 1
    </delete>

    <!-- void updateUser();-->
    <update id="updateUser">
        update t_user set emp_name = 'test' where eid = 2
    </update>

    <!-- User getUserById(); -->
    <select id="getUserById" resultType="com.atguigu.mybatis.pojo.User">
        select * from t_user where eid = 2
    </select>

    <!-- List<User> getAllUser(); -->
    <select id="getAllUser" resultType="com.atguigu.mybatis.pojo.User">
        select * from t_user
    </select>
</mapper>
```

> [!NOTE] 注意：
> 
> 1. 查询的标签 select 必须设置属性 resultType 或 resultMap，用于设置实体类和数据库表的映射关系
> 	- resultType：自动映射，用于属性名和表中字段名一致的情况
> 	- resultMap：自定义映射，用于一对多或多对一或字段名和属性名不一致的情况
> 	  
> 2. 当查询的数据为多条时，不能使用实体类作为返回值，只能使用集合，否则会抛出异常 TooManyResultsException；但是若查询的数据只有一条，可以使用实体类或集合作为返回值

---