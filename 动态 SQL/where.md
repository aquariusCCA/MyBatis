---
up:
  - "[[MyBatis 課程描述]]"
  - "[[动态 SQL]]"
---
- where 和 if 一般结合使用：
    - 若 where 标签中的 if 条件都不满足，则 where 标签没有任何功能，即不会添加 where 关键字
    - 若 where 标签中的 if 条件满足，则 where 标签会自动添加 where 关键字，并将条件最前方多余的 and/or 去掉

```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="Emp">
	select * from t_emp
	<where>
		<if test="empName != null and empName !=''">
			emp_name = #{empName}
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
	</where>
</select>
```

注意：where 标签不能去掉条件后多余的 and/or

```xml
<!--这种用法是错误的，只能去掉条件前面的and/or，条件后面的不行-->
<if test="empName != null and empName !=''">
emp_name = #{empName} and
</if>
<if test="age != null and age !=''">
	age = #{age}
</if>
```