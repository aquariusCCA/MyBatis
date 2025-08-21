---
up:
  - "[[MyBatis 課程描述]]"
  - "[[MyBatis 的逆向工程]]"
---
> 「QBC」是 **Query By Criteria** 的縮寫——指用「條件物件」來組裝查詢條件的風格。放到 MyBatis 逆向工程（MBG）語境下，就是它會為每個表生出一個 `XXXExample` 類與其內部的 `Criteria` 類，讓你用程式碼鏈式組出 WHERE 子句，而不是手寫 SQL 片段。

---
# 查询

- `selectByExample`：按条件查询，需要传入一个 example 对象或者 null；如果传入一个 null，则表示没有条件，也就是查询所有数据

- `example.createCriteria().xxx`：创建条件对象，通过 andXXX 方法为 SQL 添加查询添加，每个条件之间是 and 关系

- `example.or().xxx`：将之前添加的条件通过 or 拼接其他条件
    ![](example的方法.png)


```java
@Test public void testMBG() throws IOException {
	InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
	SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
	SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
	SqlSession sqlSession = sqlSessionFactory.openSession(true);
	EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
	EmpExample example = new EmpExample();
	//名字为张三，且年龄大于等于20
	example.createCriteria().andEmpNameEqualTo("张三").andAgeGreaterThanOrEqualTo(20);
	//或者did不为空
	example.or().andDidIsNotNull();
	List<Emp> emps = mapper.selectByExample(example);
	emps.forEach(System.out::println);
}
```

---

# 增改

`updateByPrimaryKey`：通过主键进行数据修改，如果某一个值为 null，也会将对应的字段改为 null

```java
mapper.updateByPrimaryKey(new Emp(1,"admin",22,null,"456@qq.com",3));
```

`updateByPrimaryKeySelective()`：通过主键进行选择性数据修改，如果某个值为 null，则不修改这个字段

```java
mapper.updateByPrimaryKeySelective(new Emp(2,"admin2",22,null,"456@qq.com",3));
```
