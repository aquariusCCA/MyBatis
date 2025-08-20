---
up:
  - "[[MyBatis 課程描述]]"
  - "[[MyBatis 获取参数值的两种方式]]"
---
- 可以通过 @Param 注解标识 mapper 接口中的方法参数，此时，会将这些参数放在 map 集合中
    1.  以 @Param 注解的 value 属性值为键，以参数为值；
    2.  以 param1,param2...为键，以参数为值；

-   只需要通过 `${}` 和 `#{}` 访问 map 集合的键就可以获取相对应的值，注意 `${}` 需要手动加单引号

```java
/**
 * 验证登陆（@Param）
 *
 * @param username
 * @param password
 * @return
 */
User checkLoginByParam(
	@Param("username") String username, 
	@Param("password") String password
);
```

```xml
<!--
	User checkLoginByParam(
		@Param("username") String username, 
		@Param("password") String password
	);
-->  
<select id="checkLoginByParam" resultType="User">  
    select * from t_user where emp_name = #{username} and password = #{password} 
</select>
```

```java
@Test
public void checkLoginByParam() {
	SqlSession sqlSession = SqlSessionUtils.getSqlSession();
	ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);
	mapper.checkLoginByParam("admin","123456");
}
```