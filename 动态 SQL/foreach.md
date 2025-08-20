---
up:
  - "[[MyBatis 課程描述]]"
  - "[[动态 SQL]]"
---
-   属性：
    -   collection：设置要循环的数组或集合
    -   item：表示集合或数组中的每一个数据
    -   separator：设置循环体之间的分隔符，分隔符前后默认有一个空格，如`,`
    -   open：设置 foreach 标签中的内容的开始符
    -   close：设置 foreach 标签中的内容的结束符

---

# 批量删除

```java
/**  
 * 通过数组实现批量删除  
 */  
int deleteMoreByArray(@Param("eids") Integer[] eids);
```

方式一

```xml
<!-- int deleteMoreByArray(Integer[] eids); -->
<delete id="deleteMoreByArray">
	delete from t_emp where
	<foreach collection="eids" item="eid" separator="or">
		eid = #{eid}
	</foreach>
</delete>
```

方式二

```xml
<!--int deleteMoreByArray(Integer[] eids);-->
<delete id="deleteMoreByArray">
	delete from t_emp where eid in
	<foreach collection="eids" item="eid" separator="," open="(" close=")">
		#{eid}
	</foreach>
</delete>
```

```java
@Test  
public void deleteMoreByArray() {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    DynamicSQLMapper mapper = sqlSession.getMapper(DynamicSQLMapper.class);  
    int result = mapper.deleteMoreByArray(new Integer[]{4, 5, 6});  
    System.out.println(result);  
}
```

---

# 批量添加

```java
/**
 * 通过list集合实现批量添加
 */
int insertMoreByList(@Param("emps") List<Emp> emps);
```

```xml
<!-- int insertMoreByList(List<Emp> emps); -->  
<insert id="insertMoreByList">  
    insert into t_emp values  
    <foreach collection="emps" item="emp" separator=",">  
        (null, #{emp.empName}, #{emp.age}, #{emp.sex}, #{emp.email}, null)  
    </foreach>  
</insert>
```

```java
@Test
public void insertMoreByList() {
	SqlSession sqlSession = SqlSessionUtils.getSqlSession();
	DynamicSQLMapper mapper = sqlSession.getMapper(DynamicSQLMapper.class);
	Emp emp1 = new Emp(null, "tom", 30, 1, null, null, null);
	Emp emp2 = new Emp(null, "david", 27, 1, null, null, null);
	Emp emp3 = new Emp(null, "elsa", 38, 1, null, null, null);
	List<Emp> emps = Arrays.asList(emp1, emp2, emp3);
	int result = mapper.insertMoreByList(emps);
	System.out.println(result);
}
```