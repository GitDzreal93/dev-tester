# mysql知识

> 什么是DML、DDL、DCL？

**DML**（data manipulation language）：它们是SELECT、UPDATE、INSERT、DELETE，就象它的名字一样，这4条命令是**用来对数据库里的数据进行操作的语言**

**DDL**（data definition language）：主要的命令有CREATE、ALTER、DROP等，**DDL主要是用在定义或改变表（TABLE）的结构，数据类型，表之间的链接和约束等初始化工作上，他们大多在建立表时使用**

**DCL**（Data Control Language）：是**数据库控制功能。是用来设置或更改数据库用户或角色权限的语句，包括（grant,deny,revoke等）语句**。在默认状态下，只有sysadmin,dbcreator,db_owner或db_securityadmin等人员才有权力执行DCL

**TCL**（Transaction Control Language）：**事务控制语言**，包括：set transaction\rollback\savepoin

------

### DDL

##### 创建数据库

```mysql
create database 数据库名;
```

##### 查看数据库列表

```mysql
show databases;
```

##### 使用数据库

```mysql
use 数据库名;
```

##### 删除数据库

```mysql
drop database 数据库名;
```

##### 创建数据表

```mysql
CREATE TABLE IF NOT EXISTS `codes_user`(
    ->    `user_id` INT UNSIGNED AUTO_INCREMENT,
    ->    `user_title` VARCHAR(100) NOT NULL,
    ->    `user_author` VARCHAR(40) NOT NULL,
    ->    `submission_date` DATE,
    ->    PRIMARY KEY ( `user_id` )
    -> )ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

##### 查看数据表设计

```mysql
# 方法一
desc 表名
# 方法二
show create table 表名
```

##### 删除数据表

```mysql
drop table 表名;
```

##### 清空表

```mysql
truncate table 表名;
或
delete from 表名;
```

##### 删除字段

```mysql
ALTER TABLE 表名 DROP 字段名;
```

##### 新增字段

```mysql
ALTER TABLE 表名 ADD 字段名 INT; # INT 是字段类型
```

##### 修改字段类型

```mysql
ALTER TABLE 表名 MODIFY 字段名 CHAR(10); # 修改字段类型  
```

##### 修改字段

```mysql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 BIGINT; # BIGINRT 是新字段的数据类型
```

##### 创建索引

```mysql
# 方法一
alter table 表名 add index 索引名(字段1,字段2...);   # 创建普通索引
alter table 表名 add unique 索引名(字段1,字段2...);   # 创建unique索引
alter table 表名 add primary 索引名(字段1,字段2...);   # 创建主键索引

# 方法二
create index 索引名 on 表名 (字段1，字段2...);   # 创建普通索引
```

##### 查看索引信息

```mysql
SHOW INDEX FROM 表名\G; 
```

##### 删除索引

```mysql
ALTER TABLE 表名 DROP INDEX 索引名
DROP INDEX 索引名 ON 表名
```

### DQL and DML

#### 查

##### 查询结果去重

```mysql
select distinct 字段名 from 表名;
```

##### 限制 limit与offset

```mysql
select * from 表名 limit 5; # 只检索5条数据
select * from 表名 limit 5 offset 10; # 从第5条数据开始，取10条数据
```

##### 排序 order by

```mysql
select * from 表名 order by id; # ASC 默认是升序排列
select * from 表名 order by id desc; # DESC 默认是升序排列
```

##### 数据过滤

```mysql
# or与and
select * from TB_User where （ score=100 or score=200) and age>18; # or 的优先级小于 and，需要加括号

# in 与 not in
select * from TB_User where age in (20, 21)

# between..and
select * from TB_User where age between 18 and 20

# 模糊匹配 关键字 like
# “%” 表示任意多个字符
# 完全匹配张三
select * from TB_User where name like '张三';
# 以张三结尾
select * from TB_User where name like '%张三';
# 以张三开头
select * from TB_User where name like '张三%';
# 结果包含张三
select * from TB_User where name like '%张三%';

# “_” 表示任意单个字符
select * from TB_User where name like '_张三';
select * from TB_User where name like '张三_';

# 正则匹配 关键字 regexp
# 以张三开头
select * from TB_User where name regexp '^张三';
# 以张三结尾
select * from TB_User where name regexp '张三$';
# “.” 匹配任意一个字符
select * from TB_User where name regexp '.三';
# 匹配以姓是张、赵的学生
SELECT * FROM tb_student WHERE `name` REGEXP '^[张赵]';
# 匹配“张张张三”
SELECT * FROM tb_student WHERE `name` REGEXP '张{3}';
```

##### 拼接字段

```mysql
select Concat(name, '(', age, ')') from user order by age;
```

##### 内置函数

```mysql
select AVG(price) AS avg_price from TB_Order where orderID='100'; # 返回某列的平均值
select Count(*) from TB_User # 返回user表行数 count(*) 不省略null值
select Max(score) from TB_Student # 返回最大值
select Min(score) from TB_Student # 返回最小值
select SUM(score) from TB_Student where name='老王'; # 返回求和
```

##### 分组

```mysql
# group by 以某个字段进行分组 
select name, AVG(score) as avg_score from TB_Student group_by name; # 统计每个学生的平均分

# having 过滤分组，having后面只能跟聚合函数使用，且需要和group by配合使用
select name, AVG(score) as avg_score from TB_Student group_by name having avg_score>90; # 统计每个学生的平均分，并且过滤出平均分大于90的记录
```

##### SELECT 子句顺序

```mysql
select > from > where > group by > having > order by > limit
```

##### 联结查询 JOIN

内联查询

```mysql
# 假设 TB_A 与 TB_B 有公共字段 a
# 写法一
select a, b from TB_A, TB_B where TB_A.a = TB_B.a 
# 写法二
select a, b from TB_A inner join TB_B on TB_A.a = TB_B.a 
```

自联结

```mysql
select student, score from TB_Student where tag = (select tag from TB_StudentTag where tag = '优秀' );   
```

外部连结（左连结和右连结）

```mysql
# 假设 TB_A 与 TB_B 有公共字段 a
select TB_A.a, TB_b.b from TB_A left outer JOIN TB_B ON TB_A.a = TB_B.a;
```

全连结

```mysql
select TB_A.a, TB_B.b from TB_A full join TB_B on TB_A.a=TB_B.a;
```

附上 sql 各种联结查询图解

![undefined](http://ww1.sinaimg.cn/large/00788Gqbgy1g79wtokn9ej30ky0eqdnj.jpg)

##### 组合查询 UNION

```mysql
# 使用场景：
# 1、单表执行多个查询，按单个查询返回数据
# 2、在单个查询中，从不同的表返回类似结构的数据

# 数据去重 union
select A.key1 from A union select B.key1 from B;

# 数据不去重 union all
select A.key1 from A union all select B.key1 from B;
```

#### 增

```mysql
INSERT INTO 表名 VALUES(值1,值2...);  
INSERT INTO 表名(字段1,字段2...) VALUES(值1,值2...);  
INSERT INTO 表名(字段1,字段2...) select 字段1,字段2... from 表名（可以是其他表）;  
```

#### 改

```mysql
update 表名 set 字段名 = 新值 where id = '100';
```

#### 删

```mysql
# 删除 id 为 '100' 的数据
delete from 表名 where id = '100';
```

### DCL

#### 数据库授权

```mysql
grant all privileges  on *.* to root@'%' identified by "root_pwd";
```

#### mysql复制表

```mysql
# mysqldump -u用戶名 -p密码 -d 数据库名 表名 > 脚本名;
# 导出整个数据库结构和数据
mysqldump -h localhost -uroot -p123456 database > dump.sql
# 导出单个数据表结构和数据
mysqldump -h localhost -uroot -p123456  database table > dump.sql
# 导出整个数据库结构（不包含数据）
mysqldump -h localhost -uroot -p123456  -d database > dump.sql 
# 导出单个数据表结构（不包含数据）
mysqldump -h localhost -uroot -p123456  -d database table > dump.sql
```

### 其他

#### 慢查询分析

```mysql
explain select 语句

# 结果分析
# type（从最优到最差）： const > eq_reg > ref > range > index > all
# key ： 若为null，则没有使用索引
# key_len ：索引长度，越短越好
```

#### mysql优化

1. 索引优化，添加复合索引
2. 分表，大表拆小表
3. 尽量避免 `select *`，杜绝`select * from tb_name`
4. 尽量避免 `select * from tb_name where name like 'xxx'`
5. 避免在大表上的`group by`，`order by`，`offset` 操作
6. WHERE查询条件，尽量按照添加的索引顺序来写 

#### 非关系型数据库和关系型数据库的比较

**关系型数据库**：*MySQL、SQL Server、Oracle、PostgreSQL、SQLite*

优点：

1. 查询能力高，可以操作很复杂的查询
2. 一致性高。在数据同步时，一般采用锁来保证数据的可靠性，在处理数据时，对表进行封锁来保证操作的时候其他操作不能改变查询范围的数据
3. 表具有逻辑性，易于理解

缺点：

1. 不适用高并发读写
2. 不适用海量数据高效读写
3. 层次多，扩展性低
4. 维护一致性开销大
5. 涉及联表查询，复杂，慢

**非关系型数据库**：*Hbase、Redis、MongoDB、Memcached*

优点：

1. 由于数据之间没有关系，所以易扩展，也易于查询  

2. 数据结构灵活，每个数据都可以有不同的结构  

3. 由于降低了一致性的要求，所以查询速度更快

缺点：

1. 数据准确度没有那么高

#### 事务的四大特征

特点：原子性、一致性、隔离性和持久性（ACID）

#### 什么是脏读、不可重复读、幻读？

##### 脏读

脏读就是指当一个事务正在访问数据，并且对数据进行了修改，而这种修改还没有提交到数据库中，这时，另外一个事务也访问这个数据，然后使用了这个数据。（未提交 读）

##### 不可重复读

是指在一个事务内，多次读同一数据。在这个事务还没有结束时，另外一个事务也访问该同一数据。那么，在第一个事务中的两 次读数据之间，由于第二个事务的修改，那么第一个事务两次读到的的数据可能是不一样的。这样就发生了在一个事务内两次读到的数据是不一样的，（不可重复读）

##### 幻读

是指当事务不是独立执行时发生的一种现象，例如第一个事务对一个表中的数据进行了修改，这种修改涉及到表中的全部数据行。 同时，第二个事务也修改这个表中的数据，这种修改是向表中插入一行新数据。那么，以后就会发生操作第一个事务的用户发现表中还有 没有修改的 数据行，就好象 发生了幻觉一样。

#### 如何避免脏读、不可重复读、幻读？

隔离级别设置为：可串行化（Serializable）【锁表】

#### 事务隔离级别有哪些？

从上到下，级别提高

1. 读未提交（read uncommited）【隔离级别最低，并发性能高】（脏读、不可重复读、幻读）
2. 读已提交（read commited）【锁定正在读取的行】（不可重复读、幻读）
3. 可重复读（repeatable read）【锁定所读区的所有行】（幻读）
4. 可串行化（serializable）【锁表】

#### 什么是数据库设计的三大范式？

> 转自：<https://www.cnblogs.com/knowledgesea/p/3667395.html>

##### 三大范式的目的

建立冗余较小、结构合理的数据库。如果有特殊情况，当然要特殊对待，数据库设计最重要的是看需求跟性能，需求>性能>表结构。

##### 第一范式 （1NF）【字段不可再分】

1. 每一列属性都是不可再分的属性值，确保每一列的原子性
2. 两列的属性相近或相似或一样，尽量合并属性一样的列，确保不产生冗余数据。

比如：“地址” 仍可以拆分成 “省”、“市”、“区县”、“详细地址”。

##### 第二范式  （2NF）【每一行数据只做一件事】

1. 每一行的数据只能与其中一列相关，即一行数据只做一件事。只要数据列中出现数据重复，就要把表拆分开来。

比如：商家表中，若维护门店ID、门店名称、门店地址，就不符合第二范式，应该专门拆分出门店表。

##### 第三范式  （3NF） 【每个属性都和主键有直接关系】

1. 数据不能存在传递关系，即没个属性都跟主键有直接关系而不是间接关系。

比如，学生表（学号，姓名，年龄，性别，所在院校，院校地址，院校电话）就不满足第三范式，应该拆分成：（学号，姓名，年龄，性别，所在院校）--（所在院校，院校地址，院校电话）。





