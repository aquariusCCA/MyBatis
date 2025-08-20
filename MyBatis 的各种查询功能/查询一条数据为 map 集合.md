---
up:
  - "[[MyBatis 課程描述]]"
  - "[[MyBatis 的各种查询功能]]"
---

```java
/**
 * 根据id查询用户信息为map集合
 */
Map<String, User> getUserByIdToMap(@Param("id") Integer id);
```

```xml
<!-- Map<String, User> getUserByIdToMap(@Param("id") Integer id); -->
<select id="getUserByIdToMap" resultType="map">
	select * from t_user where eid = #{id}
</select>
```

```java
@Test
public void testGetUserByIdToMap() {
	SqlSession sqlSession = SqlSessionUtils.getSqlSession();
	SelectMapper mapper = sqlSession.getMapper(SelectMapper.class);
	System.out.println(mapper.getUserByIdToMap(1));
}
// 結果：
// {eid=1, password=123456, sex=1, emp_name=admin, age=23, email=12345@qq.com}
```
