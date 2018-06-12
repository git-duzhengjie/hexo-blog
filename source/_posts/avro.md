---
title: avro
date: 2018-06-12 10:57:27
tags:
    - 数据序列化
    - 大数据
---

# 1 介绍
Avro（[ævrə]）是Hadoop的一个子项目，由Hadoop的创始人Doug Cutting（也是Lucene，Nutch等项目的创始人）牵头开发。Avro是一个数据序列化系统，设计用于支持大批量数据交换的应用。它的主要特点有：支持二进制序列化方式，可以便捷，快速地处理大量数据；动态语言友好，Avro提供的机制使动态语言可以方便地处理Avro数据.
Avro支持两种序列化编码方式：二进制编码和JSON编码.使用二进制编码会高效序列化，并且序列化后得到的结果会比较小；而JSON一般用于调试系统或是基于WEB的应用。对Avro数据序列化/反序列化时都需要对模式以深度优先(Depth-First)，从左到右(Left-to-Right)的遍历顺序来执行。Avro依赖模式(Schema)来实现数据结构定义。可以把模式理解为Java的类，它定义每个实例的结构，可以包含哪些属性。可以根据类来产生任意多个实例对象。对实例序列化操作时必须需要知道它的基本结构，也就需要参考类的信息。这里，根据模式产生的Avro对象类似于类的实例对象。每次序列化/反序列化时都需要知道模式的具体结构。所以，在Avro可用的一些场景下，如文件存储或是网络通信，都需要模式与数据同时存在。Avro数据以模式来读和写(文件或是网络)，并且写入的数据都不需要加入其它标识，这样序列化时速度快且结果内容少。由于程序可以直接根据模式来处理数据，所以Avro更适合于脚本语言的发挥。

# 2 使用

## 2.1 需要的Jar包依赖

avro-1.7.3.jar，avro-tools-1.7.3.jar，jackson-core-asl-1.9.3.jar，jackson-mapper-asl-1.9.3.jar  

## 2.2 定义模式

在avro中，它是用Json格式来定义模式的。模式可以由基础类型（null, boolean, int, long, float, double, bytes, and string）和复合类型(record, enum, array, map, union, and fixed)的数据组成。这里定义了一个简单的模式user.avsc:

```
{
	"namespace": "com.zq.avro",   
	"type": "record",    
	"name": "User",  
	 "fields": [ 
			{
				"name": "name",   
				"type": "string"
			}, 
			{
				"name": "favorite_number", 
			 	"type": ["int", "null"]
			}, 
			{
				"name": "favorite_color",  
				"type": ["string", "null"]
			} 
	] 
}
```

## 2.3 编译模式

Avro可以允许我们根据模式的定义而生成相应的类，一旦我们定义好相关的类，程序中就不需要直接使用模式了。可以用avro-tools jar包根据user.avsc生成User.java，语法如下:

``
java -jar avro-tools-1.7.4.jar compile schema . [注意这里有第三个参数"."]
``

命令执行后会在当前目录根据设定的包结构生成一个User.java类，然后就可以将定义的User对象用avro将它序列化存放到本地文件中，再将其反序列化.

 
