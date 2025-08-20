---
up:
  - "[[MyBatis 課程描述]]"
  - "[[动态 SQL]]"
---
- sql 片段，可以记录一段公共 sql 片段，在使用的地方通过 include 标签进行引入

- 声明 sql 片段：`<sql>`标签

```xml
<sql id="empColumns">eid, emp_name, age, sex, email</sql>
```

- 引用 sql 片段：`<include>`标签

```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="Emp">
	select <include refid="empColumns"></include> from t_emp
</select>
```