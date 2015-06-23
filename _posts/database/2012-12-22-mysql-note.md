---
layout: post
title: "高性能MySQL读书笔记整理"
description: ""
category: database
subhead: ''
tags: [mysql, database]

---


### 1.如何计算mysql缓冲命中率

**命中率=Qcache_hits/(Qcache_hits+Com_select)**

	show global status like '%com_select%';
	show global status like '%qcache_hits%';
一般缓存命中率大于30%就不错。

### 2.列作为索引的选择率

	count(distinct col)/count(*) >= 0.31
如果列的选择率接近0.31，基本可以。

### 3.查看回话变量，系统变量
show status 和 show global status

	select * from INFORMATION_SCHEMA.GLOBAL_STATUS 或 select * from 	INFORMATION_SCHEMA.SESSION_STATUS
	
### 4.InnDB处理死锁的方式

回滚拥有最少排他行级锁的事务(一种对最易回滚事务的大致估计)
### 5.三种方式修改数据库的引擎

**a.直接alter table**

	ALTER TABLE youTable ENGINE=InnoDB; 
这种方式最简单，但是对于大数据的表会消耗很长时间，因为MySQL要执行旧表到新表的逐行复制。而且alter table操作不管哪种引擎，MySQL都会锁整个表。

**b.利用dump和source**

首先dump需要的表，然后修改dump文件，去掉DROP TABLE修改CREATE TABLE代码，执行source。
这种方式不能在线修改引擎，需要让数据库下线；或者在线修改后进行同步。

**c.利用CREATE和SELECT**

	CREATE TABLE myTableCopy LIKE myTable; 
	ALTER TABLE myTableCopy ENGINE=InnoDB; 
	INSERT INTO myTableCopy SELECT * FROM myTable WHERE id BETWEEN x AND y;  
这种方式时候表的数据量比较大的情况，可以分批根据范围倒入，不会锁myTable。

### 6.选择数据类型
**更小通常更好，简单就好(ip地址用整数保存)**

![image](http://blog.lichengwu.cn/images/database/ip_type.png)

	3232235521=192*256^3+168*256^2+0*256^1+1*256^0

**尽量避免null**

![image](http://blog.lichengwu.cn/images/database/avoid_null.png)

mysql定义整数的宽度对于大多数应用程序使没有意义的，如int(1)和int(20)是一样的，它不会限制值的范围，只规定了mysql的交互工具(命令行，jdbc)用来显示处理字符的个数。

`当保存char值的时候，mysql会去掉任何末尾的空格。`

![image](http://blog.lichengwu.cn/images/database/char.png)

**mysql对text和blob类型的排序**

![image](http://blog.lichengwu.cn/images/database/text_blog.png)

如果再explain中的extra列显示“Using Temporary”，就说明使用了隐式临时表。

**datetime与timestampe的存储区别**

![image](http://blog.lichengwu.cn/images/database/datetime.png)

![image](http://blog.lichengwu.cn/images/database/timestamp.png)

**字符串类型：**

![image](http://blog.lichengwu.cn/images/database/string_type.png)

### 7.on duplicate key update可以在插入重复的时候更新数据。

### 8.在explain的type列，访问类型包括Full Table Scan, Index Scan, Range Scan, Unique Index Lookup, Constant. 访问它们的速度依次递增。

### 9.mysql使用where子句：

### 10.减少结果返回的行数与访问行数的差距

### 11.重构查询方式

复杂查询分解为多个简单查询

缩短查询(每次查询一部分，多次查询完成)

分解联接

### 12. 查询优化过程

对联接中的表重新排序

将外链接转换成内联接

代数等价法则

优化max，min，count

计算和减少常量表达式

覆盖索引

子查询优化

早期终结

相等传递

比较in()里的数据

表和索引统计

### 13.优化连接

![image](http://blog.lichengwu.cn/images/database/join.png)

### 14.InnoDB事务限制查询缓存

![image](http://blog.lichengwu.cn/images/database/innodb.png)

### 15.分析和调整缓冲的流程

![image](http://blog.lichengwu.cn/images/database/buffer_cache.png)

