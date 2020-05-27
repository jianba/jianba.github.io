---
layout:     post
title:      "Mysql"
subtitle:   ""
date:       2020-05-27 23:00:00
author:     "jianba"
header-img: "img/post-bg-android.jpg"
tags:
    - Mysq
---
# Mysql 基础

## 基础语法 

### 启动开启mysql(win平台)

- 启动mysql 
```
cd C:\mysql-5.0.45-win32\bin

mysqld --console
```

- 关闭Mysql
```
mysqladmin -uroot shutdown
```

- 启动Mysql 服务
```
C:\Program Files\MySQL\MySQL Server 5.0\bin>net start mysql
```

- 关闭Mysql 服务
```
C:\Program Files\MySQL\MySQL Server 5.0\bin>net stop mysql5
```

### Mysql 关键字
- DDL: create、drop、alter （字段）
- DML: insert、delete、udpate、select（查询）
- DCL: grant、revoke（安全）

### DDL 语句
- 连接数据库
```
 mysql -uroot -p
```
在以上命令行中，mysql代表客户端命令，-u后面跟连接的数据库用户，-p表示需要输入密码。

- 创建数据库 (CREATE DATABASE dbname)
 ```
  create database test1;

  如果数据库已存在报错
  mysql> create database test1;
  ERROR 1007 (HY000): Can't create database 'test1'; database exists
  成功提示：Query OK, 1 row affected (0.00 sec)
  Tip：所有的DDL和DML（不包括SELECT）操作执行成功后都显示“Query OK。
 ```
 - 选择数据库（USE dbname）
 ```
 mysql> use test1
 ```
 - 查看所有数据表
 ```
  show tables
 ```
 - 删除数据库（drop database dbname）
 ```
  drop database test1
 ```
 - 创建表
 ```
 CREATE TABLE  tablename (column_name_1 column_type_1 constraints，column_name_2  column_type_2  constraints，......column_name_n  column_type_n constraints)

example:

 create table emp(ename varchar(10),hiredate date,sal decimal(10,2),deptno int(2))

 设置主键？

 设置外键？

 设置自增？
 ```
 - 查看表（DESC tablename）
 ```
 desc emp
 ```
 - 查看创表语句
 ```
show create table emp \G
 ```
 - 删除表
 ```
 DROP TABLE tablename
 ```
- 修改表
```
ALTER TABLE tablename MODIFY [COLUMN] column_definition [FIRST | AFTER col_name]

Example:
更改字段
 alter table emp modify ename varchar(20)

插入字段
 alter table emp add column age int(3)

删除表字段 ( ALTER TABLE tablenameDROP [COLUMN]col_name )
alter table emp drop column age

字段改名 ( LTER TABLE tablename CHANGE [COLUMN] old_col_name column_definition[FIRST|AFTER col_name] )
 alter table emp change age age1 int(4)

Tip:
 change和modify都可以修改表的定义，不同的是change后面需要写两次列名，不方便。但是change的优点是可以修改列名称，modify则不能。

修改字段位置
 alter table emp add birth date after ename
 alter table emp modify age int(3) first(修改字段age，将它放在最前面)

改表名( ALTER TABLE  tablename RENAME [TO] new_tablename )
 alter table emp rename emp1
```
### DML语句

- 插入语句
```
INSERT INTO  tablename (field1,field2,......fieldn) VALUES(value1,value2,......valuesn)

 insert into emp (ename,hiredate,sal,deptno) values('zzx1','2000-01-01','2000',1)

不用指定字段名称，但是values后面的顺序应该和字段的排列顺序一致
  insert intoemp  values('lisa','2003-02-01','3000',2)

部分插入（没有提及的插入为空或者默认自增去自增）
insert into emp(ename,sal) values('dony',1000)

一次插入多条
 INSERT INTO  tablename  (field1,field2,......fieldn)
 VALUES(record1_value1,record1_value2,......record1_valuesn),
     (record2_value1,record2_value2,......record2_valuesn),
     ......
     (recordn_value1,recordn_value2,......recordn_valuesn)

 insert into dept values(5,'dept5'),(6,'dept6')
```
- 更新数据
```
UPDATE tablename SET field1=value1，field2.=value2，......fieldn=valuen [WHERE CONDITION]

 update emp set sal=4000 where ename='lisa'

同时更新多个表
 UPDATE t1,t2...tn set t1.field1=expr1,tn.fieldn=exprn  [WHERE CONDITION]

 update emp a,dept b 
 set
 a.sal=a.sal*b.deptno,b.deptname=a.ename
 where
 a.deptno=b.deptno;
```
- 删除数据
```
DELETE FROM tablename [WHERE CONDITION]

 delete from emp where ename='don'

一次删除多个表中的数据
DELETE t1,t2...tn FROM t1,t2...tn [WHERE CONDITION]

 delete a,b 
 from emp a,dept b 
 where a.deptno=b.deptno and a.deptno=3
```
- 查询数据
```
SELECT * FROM tablename [WHERE CONDITION]

 select ename,hiredate,sal,deptno from emp

查询不重复字段
 select distinct deptno from emp

条件查询
 select *from emp where deptno=1
 select * from emp where deptno=1 and sal<300

排序
 SELECT  *  FROM  tablename  [WHERE  CONDITION] 
 [
  ORDER  BY  field1  [DESC|ASC]，field2 
  [DESC|ASC]，......fieldn [DESC|ASC]
 ]
 
 select * from emp order by sal 升序
 select * from emp order by deptno,sal desc sal字段降序

 SELECT ......[LIMIT offset_start,row_count]

前三条
 select * from emp order by sal limit 3

第二条记录开始显示三条记录
 select * from emp order by sal limit 1,3

```

- 聚合
```
SELECT [field1,field2,......fieldn]
 fun_name 
 FROM tablename[WHERE where_contition]
 [GROUP BY field1,field2,......fieldn[WITH ROLLUP]]
 [HAVING where_contition]
```
> fun_name表示要做的聚合操作，也就是聚合函数，常用的有sum（求和）、count(*)（记录数）、max（最大值）、min（最小值）。</br>

> GROUP BY关键字表示要进行分类聚合的字段，比如要按照部门分类统计员工数量，部门就应该写在group by后面。</br>

> WITH ROLLUP是可选语法，表明是否对分类聚合后的结果进行再汇总。</br>

> HAVING关键字表示对分类后的结果再进行条件的过滤

>having和where的区别在于having是对聚合后的结果进行条件的过滤，而where是在聚合前就对记录进行过滤，如果逻辑允许，我们尽可能用where先过滤记录，这样因为结果集减小，将对聚合的效率大大提高，最后再根据逻辑看是否用having进行再过滤。

```
 左连接：包含所有的左边表中的记录甚至是右边表中没有和它匹配的记录 
 右连接：包含所有的右边表中的记录甚至是左边表中没有和它匹配的记录 

  select ename,deptname from emp left join dept  on  emp.deptno=dept.deptno
  select ename,deptname from dept right join emp on  dept.deptno=emp.deptno
  （两者等价）
```

> 子查询 （用于子查询的关键字主要包括 in、not in、=、!=、exists、not exists）
```
从 emp表中查询出所有部门在 dept 表中的所有记录： 

 select * from emp where deptno in(select deptno from dept); 
子查询记录数唯一，还可以用=代替 in

可以转化为表连接
 select * from emp where deptno in(select deptno from dept)

```
> 记录联合
将两个表的数据按照一定的查询条件查询出来后，将结果合并 到一起显示出来，这个时候，就需要用 union 和 union all 关键字
```
SELECT * FROM t1 
UNION|UNION ALL
SELECT * FROM t2 …… UNION|UNION ALL SELECT * FROM tn;

UNION 和 UNION ALL 的主要区别是 UNION ALL 是把结果集直接合并在一起，而 UNION 是将 UNION ALL 后的结果进行一次 DISTINCT，去除重复记录后的结果。 

```
### DCL

- 创建用户添加权限
```
创建一个数据库用户 z1，具有对 sakila 数据库中所有表的 SELECT/INSERT 权限
grant select,insert on sakila.* to 'z1'@'localhost' identified by '123'; 

```
- 回收用户权限
```
 mysql -uroot 

 revoke insert on sakila.* from 'z1'@'localhost'

 exit
```
### 使用帮助
- 用“？contents”命令来显示所有可供查询的的分类
- 看 MySQL 中都支持哪些数据类型，可以执行“? data types”命令
- int 类型的具体介绍： ? int 
- 快速查询帮助：? show  ? create table 

## Mysql数据类型

## 常用函数

## 图形化操作
- 数据库备份
- 数据库导出

## 表类型（存储引擎）

## 数据选择

- Text || BlOB</br>
  一般在保存少量字符串的时候，我们会选择 CHAR 或者 VARCHAR；
  而在保存较大文本时， 通常会选择使用 TEXT 或者 BLOB，二者之间的主要差别是 BLOB 能用来保存二进制数据，比 如照片；而 TEXT 只能保存字符数据，比如一篇文章或者日记。
</br>
  TEXT 和 BLOB 中有分别包括 TEXT、MEDIUMTEXT、LONGTEXT 和 BLOB、MEDIUMBLOB、LONGBLOB3 种不同的类型，它们 之间的主要区别是存储文本长度不同和存储字节不同，用户应该根据实际情况选择能够满足需求的最小存储类型。本节主要对 BLOB 和 TEXT 存在的一些常见问题进行介绍。 
</br>
  BLOB 和 TEXT 值会引起一些性能问题，特别是在执行了大量的删除操作时。 删除操作会在数据表中留下很大的“空洞”，以后填入这些“空洞”的记录在插入的性能上 会有影响。为了提高性能，建议定期使用 OPTIMIZE TABLE 功能对这类表进行碎片整理，避 免因为“空洞”导致性能问题。 

## 字符集

## 索引
>&nbsp;&nbsp;在关系数据库中，如果有上万甚至上亿条记录，在查找记录的时候，想要获得非常快的速度，就需要使用索引。
&nbsp;&nbsp;索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。
廖雪峰：https://www.liaoxuefeng.com/wiki/1177760294764384/1218728442198976
```
使用ADD INDEX idx_score (score)就创建了一个名称为idx_score，使用列score的索引
ALTER TABLE students
ADD INDEX idx_score (score);

ALTER TABLE students
ADD INDEX idx_name_score (name, score);

```
- 特性：索引的效率取决于索引列的值是否散列，即该列的值如果越互不相同，那么索引效率越高。反过来，如果记录的列存在大量相同的值，例如gender列，大约一半的记录值是M，另一半是F，因此，对该列创建索引就没有意义。
</br>可以对一张表创建多个索引。索引的优点是提高了查询效率，缺点是在插入、更新和删除记录时，需要同时修改索引，因此，索引越多，插入、更新和删除记录的速度就越慢。
</br>对于主键，关系数据库会自动对其创建主键索引。使用主键索引的效率是最高的，因为主键会保证绝对唯一

Mysql面试：https://juejin.im/post/5baafdccf265da0af93b05e4
https://juejin.im/post/5ba1f32ee51d450e805b43f2



## 视图

## 存储过程

## 触发器

## 事务控制和锁定

## 安全

## 技巧

## 优化

## 锁

## 磁盘io

## 数据库备份容灾
