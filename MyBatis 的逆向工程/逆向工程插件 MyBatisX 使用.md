---
up:
  - "[[MyBatis 課程描述]]"
  - "[[MyBatis 的逆向工程]]"
---
> MyBatisX 是一个 MyBatis 的代码生成插件，可以通过简单的配置和操作快速生成 MyBatis Mapper、pojo 类和 Mapper.xml 文件。下面是使用 MyBatisX 插件实现逆向工程的步骤：

---
# 安装插件：

在 IntelliJ IDEA 中打开插件市场，搜索 MyBatisX 并安装。

---

# 使用 IntelliJ IDEA 连接数据库

连接数据库

![[image_yKs2Z2_8sQ.png]]

填写信息

![[image_bDboqlZFKD.png]]

展示库表

![[image_mCMuBhwZl2.png]]

逆向工程使用

![[image_DkwlIx_BM9.png]]

![[image_dJvZP76xYm.png]]

![[image_KXYfK5CQd-.png]]

---

# 逆向工程案例使用

正常使用即可，自动生成单表的crud方法！

```java
// 针对表【user】的数据库操作Mapper
public interface UserMapper {

    int deleteByPrimaryKey(Long id);

    int insert(User record);

    int insertSelective(User record);

    User selectByPrimaryKey(Long id);

    int updateByPrimaryKeySelective(User record);

    int updateByPrimaryKey(User record);
}
```

---