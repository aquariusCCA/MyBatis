---
up:
  - "[[MyBatis 課程描述]]"
---
**打包方式：jar**
```xml
<packaging>jar</packaging>
```

**引入依赖**

```xml
<dependencies>
	<!-- MyBatis核心 -->
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis</artifactId>
		<version>3.5.7</version>
	</dependency>

	<!-- juint测试 -->
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.12</version>
		<scope>test</scope>
	</dependency>

	<!-- MySQL驱动 -->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>8.0.30</version>
	</dependency>

	<!-- Lombok -->
	<dependency>
		<groupId>org.projectlombok</groupId>
		<artifactId>lombok</artifactId>
		<version>1.18.34</version>
	</dependency>
</dependencies>
```
