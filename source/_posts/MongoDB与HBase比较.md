---
title: MongoDB与HBase比较
date: 2018-06-12 13:29:07
tags:
   - MongoDB
   - HBase
---

# 一、概览
MongoDB与HBase都是技术领先的非关系性数据库。MongoDB使用C++编写，HBase使用Java编写，HBase会受Java GC暂停的影响。MongoDB将数据存储为BSON（JSON的二进制形式表示）的文档。HBase专为 具有随机读取和写入访问模式Key-Value工作负载设计。

# 二、关键概念
许多关系数据库概念与MongoDB和HBase有相似之处。该表概述了每个系统中的一些常见概念。  

|RDBMS|MongoDB|Hbase|
|---|---|---|  
| 表| collection|表 |
| 行| Document| 列族|
| 没有类似的| 分片|分区 |
| GROUP BY| Aggregation Pipeline| MapReduce|
| 多记录的ACID事务|多记录的ACID事务 | 没有|

# 特性
## 开发者关注特性  

| MongoDB|HBase|
|---|---|
| __数据模型__  |Document |宽列 |
| __支持的数据类型__ |多种|数据转化为二进制|
|  __查询模型__ | 多功能的查询|Key-Value |
|  __二级索引__ |支持 |需要开发人员自己解决 |
|  __聚合__ |支持 |需要将数据移动到特定的数据分析框架中 |
|  __文本搜索__ | 支持|需要将数据转移到的特定的数据分析框架中 |
|  __数据保证__ | 支持| 没有|
|  __构建响应、事件驱动应用__ | 支持| 没有|
| __驱动支持__  |11种支持的驱动和30+社区支持 |支持Java和Thrift |
| __事务保证__  | 支持简单的事务| 单一行原子操作|

## 管理者关注特性  

|  |MongoDB|HBase|  
|--|--|--|  
| __创建产品级集群需要的最少节点__ |3个：主节点和次节点 |10个：主和次级HMaster,RegionServers、hdfs和zookeeper |  
|  __推荐每个节点存储的最大数据量__ |没有限制 |4TB |  
| __主节点失效恢复时间__ |2秒，数据可在次级节点读取 |60秒，数据可在次级节点读取 |  
|  __性能维护__ |C++编写不会出现Java GC暂停 |Java 编写，GC会出现暂停的问题|  
| __数据分区__ | 支持hash、range、zone|仅支持hash |  
| __备份和恢复__|||  
|  __Spark和Hadoop适应__ |支持 |支持 |

# 参考
1. https://www.mongodb.com/compare/mongodb-hbase
