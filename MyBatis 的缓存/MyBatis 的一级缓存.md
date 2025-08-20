---
up:
  - "[[MyBatis 課程描述]]"
---
- 一级缓存是 SqlSession 级别的，通过同一个 SqlSession 查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问

- 使一级缓存失效的四种情况：
    1.  不同的 SqlSession 对应不同的一级缓存
    2.  同一个 SqlSession 但是查询条件不同
    3.  同一个 SqlSession 两次查询期间执行了任何一次增删改操作
    4.  同一个 SqlSession 两次查询期间手动清空了缓存