---
up:
  - "[[MyBatis 課程描述]]"
  - "[[MyBatis 获取参数值的两种方式]]"
---
> 若 mapper 接口中的方法需要的参数为多个时，此时可以手动创建 map 集合，将这些数据放在 map 中只需要通过 `${}` 和 `#{}` 访问 map 集合的键就可以获取相对应的值，注意 `${}` 需要手动加单引号

```java
/**
 * 验证登陆（参数为map）
 *
 * @param map
 * @return
 */
User checkLoginByMap(Map<String, Object> map);
```

```xml
<!--User checkLoginByMap(Map<String,Object> map);-->
<select id="checkLoginByMap" resultType="User">
	select * from t_user where emp_name = #{emp_name} and password = #{password}
</select>
```

```java
@Test  
public void testCheckLoginByMap() {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    ParameterMapper mapper = sqlSession.getMapper(ParameterMapper.class);  
    Map<String, Object> map = new HashMap<>();  
    map.put("emp_name", "admin");  
    map.put("password", "123456");  
    User user = mapper.checkLoginByMap(map);  
    System.out.println(user);  
}
```