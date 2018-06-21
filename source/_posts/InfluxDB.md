---
title: InfluxDB
date: 2018-06-12 14:06:12
tags:
    - 分布式
    - 时序
    - 事件
    - 指标
    - 数据库
    - go
---

# 1 概览
InfluxDB是一个开源分布式时序、事件和指标数据库。使用go语言编写，无需外部依赖。其设计目标是实现分布式和水平伸缩扩展。  
它有三大特性：  

1. Time Series （时间序列）：你可以使用与时间有关的相关函数（如最大，最小，求和等  
2. Metrics（度量）：你可以实时对大量数据进行计算  
3. Eevents（事件）：它支持任意的事件数据  

# 2 特点  

- schemaless(无结构)，可以是任意数量的列  
- scalable（可扩展 ）
- min, max, sum, count, mean, median 一系列函数，方便统计  
- Native HTTP API, 内置http支持，使用http读写  
- Powerful Query Language 类似sql  
- Built-in Explorer 自带管理工具  

# 3 API

InfluxDB 支持两种api方式：

- Http API
- Protobuf API

# 4 查询语言  


InfluxDB 提供了类似sql的查询语言：

``` 
    select * from events where state == 'NY';
    
    select * from log_lines where line =~ /error/i;
    
    select * from events where customer_id == 23 and type == 'click';
    
    select * from response_times where value > 500;
    
    select * from events where email !~ /.*gmail.*/;
    
    select * from nagios_checks where status != 0;
    
    select * from events 
    where (email =~ /.*gmail.* or email =~ /.*yahoo.*/) and state == 'ny';
    
    delete from response_times where time > now() - 1h    
```


非常容易上手, 还支持Group By, Merging Series, Joining Series， 并内置常用统计函数，比如max, min, mean 等

# 5 库  

常用语言的库都有，因为api简单，也很容易自己封装。

InfluxdDB作为很多监控软件的后端，这样监控数据就可以直接存储在InfluxDB。比如：StatsD, CollectD, FluentD。

还有其它的可视化工具支持InfluxDB, 这样就可以基于InfluxDB很方便的搭建监控平台

# 6 InfluxDB 数据可视化工具  

InfluxDB 用于存储基于时间的数据，比如监控数据，因为InfluxDB本身提供了Http API，所以可以使用InfluxDB很方便的搭建了个监控数据存储中心。

对于InfluxDB中的数据展示，官方admin有非常简单的图表, 看起来是这样的:


![](/images/influxdb-2.jpg)

除了自己写程序展示数据还可以选择：

- tasseo https://github.com/obfuscurity/tasseo/  
- grafana https://github.com/torkelo/grafana  

## 6.1 tasseo  

tasseo,为Graphite写的Live dashboard，现在也支持InfluxDB,tasseo 比较简单, 可以配置的选项很少。


![](/images/influxdb-3.png)

## 6.2 Grafana  

Grafana是一个纯粹的html/js应用，访问InfluxDB时不会有跨域访问的限制。只要配置好数据源为InfluxDB之后就可以，剩下的工作就是配置图表。Grafana 功能非常强大。使用ElasticsSearch保存DashBoard的定义文件，也可以Export出JSON文件(Save ->Advanced->Export Schema)，然后上传回它的/app/dashboards目录。 

配置数据源:

```
     datasources: {      
          influx: {
            default: true,
            type: 'influxdb',
            url: 'http://<your_influx_db_server>:8086/db/<db_name>',
            username: 'test',
            password: 'test',
          }
        },        
```


最后可以看到如下图所示界面：

![](/images/influxdb-4.png)

# 7 与SQL数据库比较
## 7.1 总体
InfluxDB被设计用来处理时序数据，而SQL数据库可以处理时序数据但并不是它的主要目的。InfluxDB还有一个区别于SQL数据库的地方就是它可以存储大规模的时序数据，并且快速地对其进行实时分析。

## 7.2 时序就是一切
InfluxDB中时间戳是标识任何数据队列中数据的唯一性的标志，这与SQL数据库中，主键唯一标识一行数据一样。

## 7.3 术语对比
在InfluxDB中你不用事先定义一张表，你可以随时增加一个表的字段。InfluxDB中表被称作measurement，索引被称作tag，没有索引的字段称作field，行被称作points。InfluxDB不支持join操作。InfluxDB中的时间戳必须是Unix时间戳（GMT）或者格式化为RFC3339的有效时间字符串。

## 7.4 InfluxSQL vs SQL

InfluxSQL 是一种类似于SQL的语言，除了不支持Join以外，其他大部分都支持，包括支持正则表达式、支持GROUP BY、支持MAX、支持COUNT等函数。

# 8 参考
1. http://www.ttlsa.com/monitor-safe/monitor/distributed-time-series-database-influxdb/
2. https://docs.influxdata.com/influxdb/v1.5/concepts/crosswalk/




