---
up:
  - "[[MyBatis 課程描述]]"
---
 > MyBatis 中的 mapper 接口相当于以前的 dao。但是区别在于，mapper 仅仅是接口，我们不需要提供实现类

```java
package com.atguigu.mybatis.mapper;

public interface UserMapper {
	/**
	* 添加用户信息
	*/
	int insertUser();
}
```