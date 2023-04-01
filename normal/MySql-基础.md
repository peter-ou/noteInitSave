# MySql 基础

- [MySql 基础](#mysql-基础)
  - [一，查询语句--sql执行顺序 （重点）](#一查询语句--sql执行顺序-重点)
  - [mysql中的联合查询（内联、左联、外联、右联、全联）](#mysql中的联合查询内联左联外联右联全联)
  - [窗口函数](#窗口函数)
  - [插入语句--如果数据已经存在，请忽略(不支持使用replace操作)](#插入语句--如果数据已经存在请忽略不支持使用replace操作)
  - [mysql 的批量插入SQL](#mysql-的批量插入sql)
  - [MySQL中给字段创建索引的四种方式](#mysql中给字段创建索引的四种方式)
  - [创建视图](#创建视图)
  - [字段拼接合并](#字段拼接合并)
  - [mysql中的强制索引](#mysql中的强制索引)
  - [mysql如何删除重复记录，保留一条记录。](#mysql如何删除重复记录保留一条记录)
  - [mysql 中的触发器](#mysql-中的触发器)

## 一，查询语句--sql执行顺序 （重点）

```sql
from 
join 
on 
where 
group by xxx [with rollup 是在group分组字段的基础上再进行统计数据]
avg,sum(IF(条件,列值,0)).... (聚合函数可以和分组一起使用)
having （分组后的过滤条件）
select 
distinct （查询后再去重）
union 完全是对select的结果进行合并（默认去掉重复的记录,想保留重复记录使用union all）
Window 窗口函数(RANK(), DENSE_RANK(), ROW_NUMBER(), NTILE()...)
order by xxx  
limit [offset 偏移行数,rows 取多少条数据] | [rows OFFSET offset ]
```

<div align='center'><img src=./images/mysql-基础/mysql-基础_2021-03-17-10-54-23.png width='100%'/></div><br/>

顺序说明：
>从这个顺序中我们不难发现，所有的 查询语句都是从from开始执行的，在执行过程中，每个步骤都会为下一个步骤生成一个虚拟表，这个虚拟表将作为下一个执行步骤的输入。

>第一步：首先对from子句中的前两个表执行一个笛卡尔乘积，此时生成虚拟表 vt1（选择相对小的表做基础表）。

>第二步：接下来便是应用on筛选器，on 中的逻辑表达式将应用到 vt1 中的各个行，筛选出满足on逻辑表达式的行，生成虚拟表 vt2 。

>第三步：如果是outer join 那么这一步就将添加外部行，left outer jion 就把左表在第二步中过滤的添加进来，如果是right outer join 那么就将右表在第二步中过滤掉的行添加进来，这样生成虚拟表 vt3 。

>第四步：如果 from 子句中的表数目多余两个表，那么就将vt3和第三个表连接从而计算笛卡尔乘积，生成虚拟表，该过程就是一个重复1-3的步骤，最终得到一个新的虚拟表 vt3。 

>第五步：应用where筛选器，对上一步生产的虚拟表引用where筛选器，生成虚拟表vt4，在这有个比较重要的细节不得不说一下，对于包含outer join子句的查询，就有一个让人感到困惑的问题，到底在on筛选器还是用where筛选器指定逻辑表达式呢？on和where的最大区别在于，如果在on应用逻辑表达式那么在第三步outer join中还可以把移除的行再次添加回来，而where的移除的最终的。举个简单的例子，有一个学生表（班级,姓名）和一个成绩表(姓名,成绩)，我现在需要返回一个x班级的全体同学的成绩，但是这个班级有几个学生缺考，也就是说在成绩表中没有记录。为了得到我们预期的结果我们就需要在on子句指定学生和成绩表的关系（学生.姓名=成绩.姓名）那么我们是否发现在执行第二步的时候，对于没有参加考试的学生记录就不会出现在vt2中，因为他们被on的逻辑表达式过滤掉了,但是我们用left outer join就可以把左表（学生）中没有参加考试的学生找回来，因为我们想返回的是x班级的所有学生，如果在on中应用学生.班级='x'的话，left outer join会把x班级的所有学生记录找回（感谢网友康钦谋__康钦苗的指正），所以只能在where筛选器中应用学生.班级='x' 因为它的过滤是最终的。 

>第六步：group by 子句将中的唯一的值组合成为一组，得到虚拟表vt5。如果应用了group by，那么后面的所有步骤都只能得到的vt5的列或者是聚合函数（count、sum、avg等）。原因在于最终的结果集中只为每个组包含一行。这一点请牢记。 

>第七步：应用cube或者rollup选项，为vt5生成超组，生成vt6. 

>第八步：应用having筛选器，生成vt7。having筛选器是第一个也是为唯一一个应用到已分组数据的筛选器。 

>第九步：处理select子句。将vt7中的在select中出现的列筛选出来。生成vt8. 

>第十步：应用distinct子句，vt8中移除相同的行，生成vt9。事实上如果应用了group by子句那么distinct是多余的，原因同样在于，分组的时候是将列中唯一的值分成一组，同时只为每一组返回一行记录，那么所以的记录都将是不相同的。 

>第十一步：应用order by子句。按照order_by_condition排序vt9，此时返回的一个游标，而不是虚拟表。sql是基于集合的理论的，集合不会预先对他的行排序，它只是成员的逻辑集合，成员的顺序是无关紧要的。对表进行排序的查询可以返回一个对象，这个对象包含特定的物理顺序的逻辑组织。这个对象就叫游标。正因为返回值是游标，那么使用order by 子句查询不能应用于表表达式。排序是很需要成本的，除非你必须要排序，否则最好不要指定order by，最后，在这一步中是第一个也是唯一一个可以使用select列表中别名的步骤。

## mysql中的联合查询（内联、左联、外联、右联、全联）

[内联inner join 、左联left outer join 、右联right outer join 、全联full outer join 的好处及用法](https://www.cnblogs.com/withscorpion/p/9454490.html)

## 窗口函数

+ 窗口函数简介

**MySQL从8.0开始支持开窗函数**，这个功能在大多商业数据库中早已支持，也叫分析函数。
开窗函数与分组聚合比较像，分组聚合是通过制定字段将数据分成多份，每一份执行聚合函数，每份数据返回一条结果。
开窗函数也是通过指定字段将数据分成多份，也就是多个窗口，对每个窗口的每一行执行函数，每个窗口返回等行数的结果。
窗口函数分为静态窗口和滑动窗口，静态窗口的大小是固定的，滑动窗口的大小可以根据设置进行变化，在当前窗口下生成子窗口。
概念比较难理解，看完代码在返回来看效果更好。

+ 语法简介

语法：**函数名([参数]) over(partition by [分组字段] order by [排序字段] asc/desc rows/range between 起始位置 and 结束位置)**

+ 函数解读：函数分为两个部分。

第一部分是函数名称，开窗函数的数量较少，只有11个窗口函数+聚合函数（所有聚合函数都可以用作开窗函数），根据函数性质，有的要写参数，有的不需要写参数；

第二部分是over语句，over()是必须要写的，里面有三个参数，都是非必须参数，根据需求选写：

1.第一个参数是 partition by +分组字段，将数据根据此字段分成多份，如果不加partition by参数，那会把整个数据当做一个窗口。

2.第二个参数是 order by +排序字段，每个窗口的数据要不要进行排序。

3.第三个参数 rows/range between 起始位置 and 结束位置，这个参数仅针对滑动窗口函数有用，是在当前窗口下分出更小的子窗口。其中起始位置和结束位置可写：current row 边界是当前行，unbounded preceding 边界是分区中的第一行，unbounded following 边界是分区中的最后一行，expr preceding 边界是当前行减去expr的值，expr following 边界是当前行加上expr的值。rows是基于行数，range是基于值的大小，到讲解到滑动窗口函数时再详细介绍。

+ 开窗函数有哪些（此处不包括先执行的聚合函数类型的开窗函数）

<div align='center'><img src=./images/mysql-基础/mysql-基础_2021-03-16-10-05-08.png width='100%'/></div><br/>

[开窗函数使用参考链接](https://blog.csdn.net/weixin_39010770/article/details/87862407)

## 插入语句--如果数据已经存在，请忽略(不支持使用replace操作)

```sql
# mysql中常用的三种插入数据的语句:
# insert into表示插入数据，数据库会检查主键，如果出现重复会报错；
# replace into表示插入替换数据，需求表中有PrimaryKey，或者unique索引，如果数据库已经存在数据，则用新数据替换。如果没有数据,效果则和insert into一样；
# insert ignore表示，如果中已经存在相同的记录，则忽略当前新数据；
insert ignore into actor values("3","ED","CHASE","2006-02-15 12:34:33");
```

## mysql 的批量插入SQL

题目一：

请你对于表actor批量插入如下数据(不能有2条insert语句哦!)。

```sql
//题目已经先执行了如下语句
drop table if exists actor;
CREATE TABLE actor (
   actor_id  smallint(5)  NOT NULL PRIMARY KEY,
   first_name  varchar(45) NOT NULL,
   last_name  varchar(45) NOT NULL,
   last_update  DATETIME NOT NULL)
```

答案sql

```sql
INSERT INTO actor(actor_id,
                  first_name,
                  last_name,
                  last_update)
VALUES(1,'PENELOPE','GUINESS','2006-02-15 12:34:33'),
      (2,'NICK','WAHLBERG','2006-02-15 12:34:33');
```

题目二：

题目描述
对于如下表actor，其对应的数据为:

| actor_id | first_name | last_name | last_update         |
|----------|------------|-----------|---------------------|
| 1        | PENELOPE   | GUINESS   | 2006-02-15 12:34:33 |
| 2        | NICK       | WAHLBERG  | 2006-02-15 12:34:33 |

请你创建一个actor_name表，并且将actor表中的所有first_name以及last_name导入该表。

actor_name表结构如下：
| 列表         | 类型          | 是否为NULL  | 含义 |
|------------|-------------|----------|----|
| first_name | varchar(45) | not null | 名字 |
| last_name  | varchar(45) | not null | 姓氏 |

答案sql

```sql
create table if not exists actor_name as
select first_name,last_name from actor;
```

或者

```sql
create table if not exists actor_name (
    first_name varchar(45) not null,
    last_name varchar(45) not null
);
insert into actor_name select first_name,last_name from actor;
或者
insert into actor_name
(first_name,last_name)
(select first_name,last_name from actor);

```

>1.用create table 语句建立actor_name 表

>2.用inset into actor select插入子查询的结果集(不需要用values()，()这种形式。这种形式是手工插入单条数据或多条数据时用圆括号分割。插入结果集是不需要）

>3.【重点】如何把一个表中的记录插入另一个表：insert语句中values的部分替换成子查询，并且把values这个词去掉。


## MySQL中给字段创建索引的四种方式

```sql
1，ALTER语句添加索引

添加主键
// 该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL。
ALTER TABLE tbl_name ADD PRIMARY KEY (col_list);

添加唯一索引
// 这条语句创建索引的值必须是唯一的。
ALTER TABLE tbl_name ADD UNIQUE index_name (col_list);

添加普通索引
// 添加普通索引，索引值可出现多次。
ALTER TABLE tbl_name ADD INDEX index_name (col_list);

添加全文索引
// 该语句指定了索引为 FULLTEXT ，用于全文索引。
ALTER TABLE tbl_name ADD FULLTEXT index_name (col_list);

2，CREATE语句创建索引

//创建唯一索引
CREATE UNIQUE INDEX uniq_idx_name ON tbl_name (col_list);
//创建普通索引
CREATE INDEX idx_name ON tbl_name (col_list);

3, 删除索引
PS: 附赠删除索引的语法：
DROP INDEX index_name ON tbl_name;
// 或者
ALTER TABLE tbl_name DROP INDEX index_name；
ALTER TABLE tbl_name DROP PRIMARY KEY;
```

## 创建视图

1，语法：

```sql
CREATE [TEMP | TEMPORARY] VIEW view_name AS
SELECT column1, column2..... FROM table_name
WHERE [condition];
```

例如：
针对actor表创建视图actor_name_view，只包含first_name以及last_name两列，并对这两列重新命名，first_name为first_name_v，last_name修改为last_name_v：

```java
CREATE TABLE  actor  (
   actor_id  smallint(5)  NOT NULL PRIMARY KEY,
   first_name  varchar(45) NOT NULL,
   last_name  varchar(45) NOT NULL,
   last_update datetime NOT NULL);
```

答案参考：

```sql
CREATE VIEW actor_name_view as
SELECT first_name as first_name_v,last_name as last_name_v
FROM actor;
```

## 字段拼接合并

将employees表的所有员工的last_name和first_name拼接起来作为Name，中间以一个空格区分
(注：sqllite,字符串拼接为 || 符号，不支持concat函数，**mysql支持concat()函数**)

```sql
CREATE TABLE `employees` ( `emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));
```

答案sql

```sql
SELECT CONCAT(last_name, ' ', first_name)Name FROM employees;
```

## mysql中的强制索引

题目描述
针对salaries表emp_no字段创建索引idx_emp_no，查询emp_no为10005, 使用强制索引。
```sql
CREATE TABLE `salaries` (
`emp_no` int(11) NOT NULL,
`salary` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`from_date`));

create index idx_emp_no on salaries(emp_no);
```

```sql
create index idx_emp_no on salaries(emp_no);
select * from salaries FORCE INDEX (idx_emp_no) where emp_no = 10005;
```

>解题思路：先创建索引，再创建强制索引查询，（题目这里默认已经创建索引）。索引名一定要加括号，否则错误。

>强制索引：FORCE INDEX(<索引名>);

>SELECT * FROM <表名>  **FORCE** INDEX (<索引名>)

## mysql如何删除重复记录，保留一条记录。

**题目描述**

删除emp_no重复的记录，只保留最小的id对应的记录。

```sql
CREATE TABLE IF NOT EXISTS titles_test (
id int(11) not null primary key,
emp_no int(11) NOT NULL,
title varchar(50) NOT NULL,
from_date date NOT NULL,
to_date date DEFAULT NULL);

insert into titles_test values 
('1', '10001', 'Senior Engineer', '1986-06-26', '9999-01-01'),
('2', '10002', 'Staff', '1996-08-03', '9999-01-01'),
('3', '10003', 'Senior Engineer', '1995-12-03', '9999-01-01'),
('4', '10004', 'Senior Engineer', '1995-12-03', '9999-01-01'),
('5', '10001', 'Senior Engineer', '1986-06-26', '9999-01-01'),
('6', '10002', 'Staff', '1996-08-03', '9999-01-01'),
('7', '10003', 'Senior Engineer', '1995-12-03', '9999-01-01');

```

删除后titles_test表为

| id | emp_no | title           | from_date | to_date  |
|----|--------|-----------------|-----------|----------|
| 1  | 10001  | Senior Engineer | 1986-6-26 | 9999-1-1 |
| 2  | 10002  | Staff           | 1996-8-3  | 9999-1-1 |
| 3  | 10003  | Senior Engineer | 1995-12-3 | 9999-1-1 |
| 4  | 10004  | Senior Engineer | 1995-12-3 | 9999-1-1 |


**答案**

#删除指定数据的语句为 delete from 表名 where限制条件
#思路：不是emp_no分组中的最小id时，删除。因为emp_no不重复时，id唯一，在分组中既是最小值也是最大值。
语言运行环境：Sql(mysql 8.0)

```sql
// 这是一个报错的sql。
// 报错原因：不能先select出同一表中的某些值，然后在同一语句中更改这个相同的表。

delete from titles_test
where id not in  
 (select min(id) 
 from titles_test
 group by emp_no);
```

>结果报错：You can't specify target table 'titles_test' for update in FROM clause"
意思是说，不能先select出同一表中的某些值，然后在同一语句中更改这个表。

解决方法：把用到titles_test这个表的查询作为中间表，用from子查询再查一遍。where中括起来是不管用的。

```sql
// 能过成功 是因为 t1 和 titles_test 已经不是同一个表了，所以可以修改不同的表。
delete from titles_test      
  where id not in(
    select min_id
    from
      (select min(id) as min_id
      from titles_test
      group by emp_no)t1);
```

## mysql 中的触发器

**题目描述**

构造一个触发器audit_log，在向employees_test表中插入一条数据的时候，触发插入相关的数据到audit中。

```sql
CREATE TABLE employees_test(
ID INT PRIMARY KEY NOT NULL,
NAME TEXT NOT NULL,
AGE INT NOT NULL,
ADDRESS CHAR(50),
SALARY REAL
);

CREATE TABLE audit(
EMP_no INT NOT NULL,
NAME TEXT NOT NULL
);
```

**解题答案**

构造触发器语句答案

```sql
CREATE TRIGGER audit_log 
AFTER INSERT ON employees_test
FOR EACH ROW
BEGIN
    INSERT INTO audit VALUES(new.id,new.name);
END
```

在MySQL中，创建触发器语法如下：

```sql
CREATE TRIGGER trigger_name
trigger_time trigger_event ON tbl_name
FOR EACH ROW
trigger_stmt

其中：

trigger_name：标识触发器名称，用户自行指定；
trigger_time：标识触发时机，取值为 BEFORE 或 AFTER；
trigger_event：标识触发事件，取值为 INSERT、UPDATE 或 DELETE；
tbl_name：标识建立触发器的表名，即在哪张表上建立触发器；
trigger_stmt：触发器程序体，可以是一句SQL语句，或者用 BEGIN 和 END 包含的多条语句，每条语句结束要分号结尾。
```
