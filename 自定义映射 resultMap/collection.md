---
up:
  - "[[MyBatis 課程描述]]"
  - "[[一对多映射处理]]"
---

-   collection：用来处理一对多的映射关系
-   ofType：表示该属性对饮的集合中存储的数据的类型

```java
/**
 * 获取部门以及部门中所有的员工信息
 */
Dept getDeptAndEmp(@Param("did") Integer did);
```

```xml
<resultMap id="deptAndEmpResultMap" type="Dept">
	<id property="did" column="did" />
	<id property="deptName" column="dept_name" />
	<collection property="emps" ofType="Emp">
		<id property="eid" column="eid" />
		<id property="empName" column="emp_name" />
		<id property="age" column="age" />
		<id property="sex" column="sex" />
		<id property="email" column="email" />
	</collection>
</resultMap>

<!-- Dept getDeptAndEmp(@Param("did") Integer did); -->
<select id="getDeptAndEmp" resultMap="deptAndEmpResultMap">
	select * from t_dept
	left join t_emp on t_dept.did = t_emp.did
	where t_dept.did = #{did}
</select>
```

```java
@Test  
public void testGetDeptAndEmp() {  
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();  
    DeptMapper mapper = sqlSession.getMapper(DeptMapper.class);  
    Dept dept = mapper.getDeptAndEmp(1);  
    System.out.println(dept);  
}
```