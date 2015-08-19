---
layout: post
title: mysql metadata lock 原因分析（草稿）
categories:
- 数据库
tags:
- MYSQL 
- 事务
- DDL
---

#### 事由


目前由于业务需求，需要对数据库进行ddl操作更改业务库的表结构，timeline如下：
1. 执行alter语句
2. 等了很久也没有成功，以为是表的数据量太大导致的。此时依赖于此表的业务系统不能正常访问。
3. 另一个窗口show processlist，发现了alter语句被锁，提示如下：
> Waiting for table metadata lock
4. 发现有很多sleep的process将alter语句锁住，kill掉alter语句，系统恢复正常

#### 原因分析
目前mysql为了保持数据的一致性使用了metadata locking,详情参考mysql官方文档：
>[metadata locking](https://dev.mysql.com/doc/refman/5.7/en/metadata-locking.html)

具体原因是由于事务操作在尚未commit之前，如果有ddl对表结构做变更就会导致上面所说的错误，由于业务系统会有一些事务操作，而且部分事务保持的是长连接的方式，那么就会导致上面的alter语句执行被事务操作锁住。

#### 方案
1.执行alter语句，直接等待解锁。但是会带来一个问题，业务系统在此时是无法使用的
2.使用权限高的账户去执行alter语句

