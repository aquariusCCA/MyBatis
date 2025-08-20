---
up:
  - "[[MyBatis 課程描述]]"
  - "[[动态 SQL]]"
---
- `choose、when、otherwise`相当于`if...else if..else`

- when 至少要有一个，otherwise 至多只有一个

```java
/**
 * 测试choose、when、otherwise
 */
List<Emp> getEmpByChoose(Emp emp);
```

```xml
<select id="getEmpByChoose" resultType="Emp">
	select * from t_emp
	<where>
		<choose>
			<when test="empName != null and empName != ''">
				emp_name = #{empName}
			</when>
			<when test="age != null and age != ''">
				age = #{age}
			</when>
			<when test="sex != null and sex != ''">
				sex = #{sex}
			</when>
			<when test="email != null and email != ''">
				email = #{email}
			</when>
			<otherwise>
				did = 1
			</otherwise>
		</choose>
	</where>
</select>
```

```java
@Test  
public void testGetEmpByChoose() {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    DynamicSQLMapper mapper = sqlSession.getMapper(DynamicSQLMapper.class);  
    List<Emp> list = mapper.getEmpByChoose(new Emp(null, "kevin", 27, 1, null, null, null));  
    System.out.println(list);  
}
```

> 相当于`if a else if b else if c else d`，只会执行其中一个