---
up:
  - "[[MyBatis 課程描述]]"
---
# 添加依赖

```xml
<dependency>
	<groupId>org.mybatis.caches</groupId>
	<artifactId>mybatis-ehcache</artifactId>
	<version>1.3.0</version>
</dependency>
```

---

# # 创建 EHCache 的配置文件 ehcache.xml

> 名字必须叫`ehcache.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<ehcache updateCheck="false" name="myCacheManager">  
    <!-- 暫存路徑 -->  
    <diskStore path="ehcache"/>  
    <!-- 預設策略 -->  
    <defaultCache  
            maxEntriesLocalHeap="1000"  
            eternal="false"  
            timeToIdleSeconds="3600"  
            timeToLiveSeconds="7200"  
            memoryStoreEvictionPolicy="LRU"  
            statistics="true"/>  
</ehcache>
```

---

# 设置二级缓存的类型

> 在 xxxMapper.xml 文件中设置二级缓存类型

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```

---

# EHCache 配置文件说明

| 属性名                          | 是否必须 | 作用                                                                                                                                                |
| ------------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| maxElementsInMemory             | 是       | 在内存中缓存的 element 的最大数目                                                                                                                   |
| maxElementsOnDisk               | 是       | 在磁盘上缓存的 element 的最大数目，若是 0 表示无穷大                                                                                                |
| eternal                         | 是       | 设定缓存的 elements 是否永远不过期。 如果为 true，则缓存的数据始终有效， 如果为 false 那么还要根据 timeToIdleSeconds、timeToLiveSeconds 判断        |
| overflowToDisk                  | 是       | 设定当内存缓存溢出的时候是否将过期的 element 缓存到磁盘上                                                                                           |
| timeToIdleSeconds               | 否       | 当缓存在 EhCache 中的数据前后两次访问的时间超过 timeToIdleSeconds 的属性取值时， 这些数据便会删除，默认值是 0,也就是可闲置时间无穷大                |
| timeToLiveSeconds               | 否       | 缓存 element 的有效生命期，默认是 0.,也就是 element 存活时间无穷大                                                                                  |
| diskSpoolBufferSizeMB           | 否       | DiskStore(磁盘缓存)的缓存区大小。默认是 30MB。每个 Cache 都应该有自己的一个缓冲区                                                                   |
| diskPersistent                  | 否       | 在 VM 重启的时候是否启用磁盘保存 EhCache 中的数据，默认是 false                                                                                     |
| diskExpiryThreadIntervalSeconds | 否       | 磁盘缓存的清理线程运行间隔，默认是 120 秒。每个 120s， 相应的线程会进行一次 EhCache 中数据的清理工作                                                |
| memoryStoreEvictionPolicy       | 否       | 当内存缓存达到最大，有新的 element 加入的时候， 移除缓存中 element 的策略。 默认是 LRU（最近最少使用），可选的有 LFU（最不常使用）和 FIFO（先进先出 |

---

# 測試

```xml
<sql id="empColumns">eid, emp_name, age, sex, email</sql>

<!--List<Emp> getAllEmp();-->
<select id="getAllEmp" resultType="Emp">
	select <include refid="empColumns"></include> from t_emp
</select>
```

```java
@Test
public void testL2CacheWithEhcache() {
	// 第一次查詢：打 DB、把結果放到 L1，commit/close 時才寫入 L2(Ehcache)
	SqlSession s1 = SqlSessionUtils.getSqlSession();
	DynamicSQLMapper m1 = s1.getMapper(DynamicSQLMapper.class);
	List<Emp> r1 = m1.getAllEmp();           // 會看到 Preparing...
	System.out.println("first query size=" + r1.size());
	s1.commit(); // ★沒有 commit/close，不會寫進二級快取


	// 第二次查詢：新的 SqlSession，同樣條件 → 命中 L2，不應再看到 Preparing
	SqlSession s2 = SqlSessionUtils.getSqlSession();
	DynamicSQLMapper m2 = s2.getMapper(DynamicSQLMapper.class);
	List<Emp> r2 = m2.getAllEmp();           
	System.out.println("second query size=" + r2.size());
}
```

---