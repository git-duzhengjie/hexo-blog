---
title: HBase的Region分区
date: 2018-06-12 14:53:40
tags:
    - HBase
    - Region
---

# 1 Region概念
Region是表获取和分布的基本元素，由每个列族的一个Store组成。对象层级如下所示：
Table -> Region -> Store -> MemStore | StoreFile -> Block


1. Store：Store per ColumnFamily for each Region for the table
2. MemStore：MemStore for each Store for each Region for the table
3. StoreFile：StoreFiles for each Store for each Region for the table
4. Block：Blocks within a StoreFile within a Store for each Region for the table

Region的大小是一个棘手的问题，需要考量如下几个因素：  

1. Region是HBase中分布式存储和负载均衡的最小单元。不同Region分布到不同RegionServer上，但并不是存储的最小单元
2. Region由一个或者多个Store组成，每个store保存一个columns family，每个Strore又由一个memStore和0至多个StoreFile 组成。memStore存储在内存中， StoreFile存储在HDFS上
3. HBase通过将region切分在许多机器上实现分布式。也就是说，你如果有16GB的数据，只分了2个region， 你却有20台机器，有18台就浪费了
4. region数目太多就会造成性能下降，现在比以前好多了。但是对于同样大小的数据，700个region比3000个要好
5. region数目太少就会妨碍可扩展性，降低并行能力。有的时候导致压力不够分散。这就是为什么，你向一个10节点的HBase集群导入200MB的数据，大部分的节点是idle的
6. RegionServer中1个region和10个region索引需要的内存量没有太多的差别

当region中的StoreFile大小超过了上面配置的值的时候，该region就会被拆分，具体的拆分策略见下文。

# 2 Region 拆分策略
Region的分割操作是不可见的，因为Master不会参与其中。RegionServer拆分region的步骤是，先将该region下线，然后拆分，将其子region加入到META元信息中，再将他们加入到原本的RegionServer中，最后汇报Master。

# 3 参考
https://www.cnblogs.com/duanxz/p/3154487.html


