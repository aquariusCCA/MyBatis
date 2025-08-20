---
up:
  - "[[MyBatis 課程描述]]"
---
1. 如果查询出的数据只有一条，可以通过
    1. 实体类对象接收
    2. List 集合接收
    3. Map 集合接收，结果`{password=123456, sex=男, id=1, age=23, username=admin}`

2. 如果查询出的数据有多条，一定不能用实体类对象接收，会抛异常 TooManyResultsException，可以通过
    1. 实体类类型的 LIst 集合接收
    2. Map 类型的 LIst 集合接收
    3. 在 mapper 接口的方法上添加 @MapKey 注解