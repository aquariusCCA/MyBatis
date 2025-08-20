---
up:
  - "[[MyBatis 課程描述]]"
  - "[[MyBatis 的各种查询功能]]"
---
# 方法一

> 将表中的数据以 map 集合的方式查询，一条数据对应一个 map；若有多条数据，就会产生多个map 集合，此时可以将这些 map 放在一个 list 集合中获取

```java
/**
 * 查询所有用户信息为map集合
 * @return
 */
@MapKey("eid")  
List<Map<String, User>> getAllUserToMap();
```

```xml
<!-- List<Map<String, User>> getAllUserToMap(); -->  
<select id="getAllUserToMap" resultType="map">  
    select * from t_user  
</select>
```

```java
@Test  
public void testGetAllUserToMap() {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    SelectMapper mapper = sqlSession.getMapper(SelectMapper.class);  
    System.out.println(mapper.getAllUserToMap());  
}
/*
結果：
[
{eid=1, password=123456, sex=1, emp_name=admin, age=23, email=12345@qq.com}, {eid=2, password=123456, sex=1, emp_name=Tom, age=12, email=123@321.com}
]
*/
```

---

# 方法二

> 将表中的数据以 map 集合的方式查询，一条数据对应一个 map；若有多条数据，就会产生多个map 集合，并且最终要以一个 map 的方式返回数据，此时需要通过 @MapKey 注解设置 map 集合的键，值是每条数据所对应的 map 集合

```java
/**
 * 查询所有用户信息为map集合
 * @return
 */
@MapKey("eid")
Map<String, User> getAllUserToMap();
```

```xml
<!--Map<String, User> getAllUserToMap();-->
<select id="getAllUserToMap" resultType="map">
	select * from t_user
</select>
```

```java
@Test  
public void testGetAllUserToMap() {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    SelectMapper mapper = sqlSession.getMapper(SelectMapper.class);  
    System.out.println(mapper.getAllUserToMap());  
}
/*
結果：
{
1={eid=1, password=123456, sex=1, emp_name=admin, age=23, email=12345@qq.com}, 
2={eid=2, password=123456, sex=1, emp_name=Tom, age=12, email=123@321.com}
}
*/
```

---