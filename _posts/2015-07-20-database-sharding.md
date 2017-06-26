---
layout: post
title: 数据库分组与分片
description: 本文是对“架构师之路”微信公众号数据库系列文章的整理和总结，主要涉及数据库架构设计中的一些基本概念，常见问题以及对应解决方案。
key: database, sharding
---

### 前言
> “架构师之路”微信公众号的作者是58同城高级架构师以及58到家技术总监**沈剑**  ，旨在撰写接地气的架构文章。本文是对“架构师之路”微信公众号数据库系列文章的整理和总结，主要涉及数据库架构设计中的一些基本概念，常见问题以及对应解决方案。文章为了便于读者理解，以“用户中心”数据库为例，讲解数据库架构设计的常见玩法。

### 用户中心
用户中心是一个常见业务，主要提供用户注册、登录、信息查询与修改的服务，其核心元数据为:  
User (uid, uname, passwd, sex, age, nickname, ...)
- uid 为用户 ID，主键
- uname, passwd, sex, age, nickname 等为用户的属性

数据库设计上，一般来说在业务初期，单库单表就满足需求。

### 单库架构
![单库架构](/images/database-design/pic-1.png)  
最常见的单库架构设计如上:
- user-service: 用户中心服务，对外提供友好的 RPC(Remote Procedure Call) 接口
- user-db: 单库进行数据存储

### 分组架构
![分组架构](/images/database-design/pic-2.png)  
分组架构是最常见的一主多从，主从同步，读写分离数据库架构(分组定义):
- user-service: 依旧是用户中心服务
- user-db-M(master): 主库，提供数据库写服务
- user-db-S(slave): 从库，提供数据库读服务
  
**主和从构成的数据库集群称为“组”**

同一个组里的数据库集群(分组特点):
- 主从之间通过binlog进行数据同步
- 多个实例数据库结构完全相同
- 多个实例存储的数据也完全相同，本质上是将数据进行复制

分组解决的问题:
- 线性提升数据库读性能
- 通过消除读写锁冲突提升数据库写性能
- 通过冗余从库实现数据的“读高可用”
  
大部分互联网业务读多写少，数据库的读往往最先成为性能瓶颈，此时可以使用分组架构，需要注意的是，**分组架构中，数据库的主库依然是写单点。**

### 分片架构
分片的基本思想就要把一个数据库切分成多个部分放到不同的数据库(server)上，从而缓解单一数据库的性能问题<sup>[3]</sup>。
#### 水平切分
![水平切分](/images/database-design/pic-3.png)  
常见的水平切分设计如上:
- user-service: 依旧是用户中心服务
- user-db1: 水平切分成2份中的第一份
- user-db2: 水平切分成2份中的第二份

水平切分，**强烈建议分库**，而不是分表:
- 分表依然公用一个数据库文件，仍然有磁盘IO的竞争
- 分库能够很容易的将数据迁移到不同数据库实例，甚至数据库机器上，扩展性更好

常见的水平切分算法有**“范围法”和“哈希法”**

![水平切分-范围法](/images/database-design/pic-4.png)

**范围法**如上图，以用户中心的业务主键 uid 为划分依据，将数据水平切分到两个数据库实例上去:
- user-db1：存储0到1千万的 uid 数据
- user-db2：存储0到2千万的 uid 数据

![水平切分-哈希法](/images/database-design/pic-5.png)

**哈希法**如上图，也是以用户中心的业务主键 uid 为划分依据，将数据水平切分到两个数据库实例上去：
- user-db1：存储 uid 取模得1的 uid 数据
- user-db2：存储 uid 取模得0的 uid 数据

这两种方法在互联网都有使用，其中哈希法使用较为广泛。

#### 垂直切分
![垂直切分](/images/database-design/pic-6.png)

**垂直切分一般和业务结合比较紧密**，还是以用户中心为例，可以这么进行垂直切分:  
User (uid, uname, passwd, sex, age, ...)  
User_EX (uid, intro, sign, ...)
- 垂直切分开的表，主键都是uid
- 登录名，密码，性别，年龄等属性放在一个垂直表（库）里
- 自我介绍，个人签名等属性放在另一个垂直表（库）里

根据业务对数据进行垂直切分时，一般要考虑属性的**“长度”和“访问频度”**两个因素:
- 长度较短，访问频率较高的放在一起
- 长度较长，访问频度较低的放在一起

这是因为，数据库会以行(row)为单位，将数load到内存(buffer)里，在内存容量有限的情况下，长度短且访问频度高的属性，内存能够load更多的数据，命中率会更高，磁盘IO会减少，数据库的性能会提升。

垂直切分和水平切有相似的地方，也有不同之处，**特点**如下:
- 多个实例之间也不直接产生联系，即没有 binlog 同步
- 多个实例数据库结构，都不一样
- 多个实例存储的数据之间至少有一列交集，一般来说是业务主键，所有实例间数据并集构成全局数据

垂直切分解决的问题: 可以**降低单库的数据量**，还可以**降低磁盘IO从而提升吞吐量**，但它与业务结合比较紧密，并不是所有业务都能够进行垂直切分的

### 分组+分片架构
![分组+分片架构](/images/database-design/pic-7.png)

如果业务读写并发量很高，数据量也很大，通常需要实施分组+分片的数据库架构:
- 通过分片来降低单库的数据量，线性提升数据库的写性能
- 通过分组来线性提升数据库的读性能，保证读库的高可用

### 总结
- 业务初期用**单库**
- 读压力大，读高可用，用**分组**
- 数据量大，写线性扩容，用**水平切分**
- 属性短，访问频度高的属性，**垂直拆分**到一起

### 参考链接
1. [典型数据库架构设计与实践 - 架构师之路](https://mp.weixin.qq.com/s?src=3&timestamp=1498187263&ver=1&signature=xqjBIqXRrTSrhO9bVfPMKw*Gg90a6ZTGaG2SA1uH4jNURygGKr-Vhq4QHCipAaQUkJqr5hPgtOBvAgRmvXEiWR8mY38c5v5MNU*f2YW2ZDOC7B0arFAOIUdfNZvyPaCMixzoYj8lq03XWUvPQfcP8mX9wHmU8kcyhkAB1NelqHk=){:target="_blank"}
2. [一分钟掌握数据库垂直拆分 - 架构师之路](https://mp.weixin.qq.com/s?src=3&timestamp=1498190323&ver=1&signature=xqjBIqXRrTSrhO9bVfPMKw*Gg90a6ZTGaG2SA1uH4jPZnkKDvo8nWswh5Wz7tR7Ida1JGfPmTt6awMg5RUck5N8sJ9X4dC3nzg-ngt3VIIJAyJXfXzXjeFVBkcThLAres6iAyEU*G*9UHP50ROxEf6oBGqUIfgkf3tafo5roT-0=){:target="_blank"}
3. [数据库Sharding的基本思想和切分策略 - CSDN 博客](http://blog.csdn.net/bluishglc/article/details/6161475){:target="_blank"}