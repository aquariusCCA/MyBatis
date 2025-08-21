---
up:
  - "[[MyBatis 課程描述]]"
---
# pom.xml引入依赖

```xml
<dependency>  
    <groupId>com.github.pagehelper</groupId>  
    <artifactId>pagehelper</artifactId>  
    <version>5.1.11</version>  
</dependency>
```

---

# mybatis-config.xml 配置分页插件

在 MyBatis 的配置文件中添加 PageHelper 的插件：

```xml
<plugins>
	<plugin interceptor="com.github.pagehelper.PageInterceptor">
		<property name="helperDialect" value="mysql"/>
	</plugin>
</plugins>
```

其中，`com.github.pagehelper.PageInterceptor` 是 PageHelper 插件的名称，`dialect` 属性用于指定数据库类型（支持多种数据库）

---

# 页插件使用

在查询方法中使用分页：

```java
public class MyBatisTest {

	private SqlSession sqlSession;

	@BeforeEach  //每次走测试方法之前 先走的初始化方法
	public void before() throws IOException {
		InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
		SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
		sqlSession = sqlSessionFactory.openSession(true);//开启自动提交
	}

	
	//使用分页插件
	@Test
	public void test_01(){
		EmployeeMapper mapper = sqlSession.getMapper(EmployeeMapper.class);

		//调用之前,先设置分页数据
		PageHelper.startPage(1, 2);

		List<Employee> list =   mapper.selectAll();

		//将查询数据封装到一个PageInfo的分页实体类
		PageInfo<Employee> pageInfo = new PageInfo<>(list);

		//pageInfo获取分页的数据
		System.out.println("pageInfo = " + pageInfo);
		long total = pageInfo.getTotal(); // 获取总记录数
		System.out.println("total = " + total);
		int pages = pageInfo.getPages();  // 获取总页数
		System.out.println("pages = " + pages);
		int pageNum = pageInfo.getPageNum(); // 获取当前页码
		System.out.println("pageNum = " + pageNum);
		int pageSize = pageInfo.getPageSize(); // 获取每页显示记录数
		System.out.println("pageSize = " + pageSize);
		List<Employee> employees = pageInfo.getList(); //获取查询页的数据集合
		System.out.println("employees = " + employees);
		employees.forEach(System.out::println);
	}

	@AfterEach //每次走测试方法之后调用的方法!
	public void clean(){
		sqlSession.close();
	}
}
```