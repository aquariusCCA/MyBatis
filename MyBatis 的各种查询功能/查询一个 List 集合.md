---
up:
  - "[[MyBatis 課程描述]]"
  - "[[MyBatis 的各种查询功能]]"
---
```java
/**  
 * 查询所有用户  
 *  
 * @return  
 */  
List<User> getAllUser();
```

```xml
<!-- List<User> getAllUser(); -->  
<select id="getAllUser" resultType="User">  
    select * from t_user  
</select>
```

```java
@Test  
public void testGetAllUser() {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    SelectMapper mapper = sqlSession.getMapper(SelectMapper.class);  
    System.out.println(mapper.getAllUser());  
}
```