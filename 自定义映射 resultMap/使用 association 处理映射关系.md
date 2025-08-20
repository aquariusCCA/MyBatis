---
up:
  - "[[MyBatis 課程描述]]"
  - "[[多对一映射处理]]"
---
- association：处理多对一的映射关系
- property：需要处理多对的映射关系的属性名
- javaType：该属性的类型

```xml
<!-- 处理多对一映射关系方式二：association -->
<resultMap id="empAndDeptResultMapTwo" type="Emp">
	<id property="eid" column="eid" />
	<result property="empName" column="emp_name" />
	<result property="age" column="age" />
	<result property="sex" column="sex" />
	<result property="email" column="email" />
	<!--
		assocation:处理多对一的映射关系
		property:需要处理多对一的映射关系的属性名
		javaType:该属性的类型
	 -->
	<association property="dept" javaType="Dept">
		<id property="did" column="did" />
		<result property="deptName" column="dept_name" />
	</association>
</resultMap>

<!-- Emp getEmpAndDept(@Param("eid") Integer eid); -->
<select id="getEmpAndDept" resultMap="empAndDeptResultMapTwo">
	select * from t_emp
	left join t_dept on t_emp.did = t_dept.did
	where t_emp.eid = #{eid}
</select>
```

```java
@Test  
public void testGetEmpAndDept() {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);  
    Emp emp = mapper.getEmpAndDept(1);  
    System.out.println(emp);  
}
```