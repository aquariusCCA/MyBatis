---
up:
  - "[[MyBatis 課程描述]]"
  - "[[动态 SQL]]"
---
- trim 用于去掉或添加标签中的内容

- 常用属性
    - prefix：在 trim 标签中的内容的前面添加某些内容
    - suffix：在 trim 标签中的内容的后面添加某些内容
    - prefixOverrides：在 trim 标签中的内容的前面去掉某些内容
    - suffixOverrides：在 trim 标签中的内容的后面去掉某些内容

- 若 trim 中的标签都不满足条件，则 trim 标签没有任何效果，也就是只剩下`select * from t_emp`

```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="Emp">
	select * from t_emp
	<trim prefix="where" suffixOverrides="and|or">
		<if test="empName != null and empName !=''">
			emp_name = #{empName} and
		</if>
		<if test="age != null and age !=''">
			age = #{age} and
		</if>
		<if test="sex != null and sex !=''">
			sex = #{sex} or
		</if>
		<if test="email != null and email !=''">
			email = #{email}
		</if>
	</trim>
</select>
```

```java
//测试类
@Test
public void testGetEmpByCondition() {
	SqlSession sqlSession = SqlSessionUtils.getSqlSession();
	DynamicSQLMapper mapper = sqlSession.getMapper(DynamicSQLMapper.class);
	List<Emp> list = mapper.getEmpByCondition(new Emp(null, "kevin", null, null, null, null, null));
	System.out.println(list);
}
```
