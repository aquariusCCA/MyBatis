---
up:
  - "[[MyBatis 課程描述]]"
  - "[[动态 SQL]]"
---
- if 标签可通过 test 属性（即传递过来的数据）的表达式进行判断，若表达式的结果为 true，则标签中的内容会执行；反之标签中的内容不会执行

- 在 where 后面添加一个恒成立条件`1=1`
    - 这个恒成立条件并不会影响查询的结果
    - 这个`1=1`可以用来拼接`and`语句，例如：当 empName 为 null 时
        -   如果不加上恒成立条件，则 SQL 语句为`select * from t_emp where and age = ? and sex = ? and email = ?`，此时`where`会与`and`连用，SQL 语句会报错
        -   如果加上一个恒成立条件，则 SQL 语句为`select * from t_emp where 1= 1 and age = ? and sex = ? and email = ?`，此时不报错

```java
/**  
 * 多条件查询  
 */  
List<Emp> getEmpByCondition(Emp emp);
```

```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="Emp">
	select * from t_emp where 1=1
	<if test="empName != null and empName !=''">
		and emp_name = #{empName}
	</if>
	<if test="age != null and age !=''">
		and age = #{age}
	</if>
	<if test="sex != null and sex !=''">
		and sex = #{sex}
	</if>
	<if test="email != null and email !=''">
		and email = #{email}
	</if>
</select>
```

```java
@Test  
public void testGetEmpByCondition() {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    DynamicSQLMapper mapper = sqlSession.getMapper(DynamicSQLMapper.class);  
    List<Emp> list = mapper.getEmpByCondition(new Emp(null, null, 27, null, null, null, null));  
    System.out.println(list);  
}
```