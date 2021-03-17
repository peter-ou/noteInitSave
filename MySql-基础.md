# MySql 基础

- [MySql 基础](#mysql-基础)
  - [1，查询语句--sql执行顺序 （重点）](#1查询语句--sql执行顺序-重点)
  - [mysql中的联合查询（内联、左联、外联、右联、全联）](#mysql中的联合查询内联左联外联右联全联)
  - [窗口函数](#窗口函数)
  - [插入语句--如果数据已经存在，请忽略(不支持使用replace操作)](#插入语句--如果数据已经存在请忽略不支持使用replace操作)
  - [MySQL中给字段创建索引的四种方式](#mysql中给字段创建索引的四种方式)
  - [创建视图](#创建视图)

## 1，查询语句--sql执行顺序 （重点）

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

[内联inner join 、左联left outer join 、右联right outer join 、全联full outer join 的好处及用法](!https://www.cnblogs.com/withscorpion/p/9454490.html)

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

[开窗函数使用参考链接](!https://blog.csdn.net/weixin_39010770/article/details/87862407)

## 插入语句--如果数据已经存在，请忽略(不支持使用replace操作)

```sql
# mysql中常用的三种插入数据的语句:
# insert into表示插入数据，数据库会检查主键，如果出现重复会报错；
# replace into表示插入替换数据，需求表中有PrimaryKey，或者unique索引，如果数据库已经存在数据，则用新数据替换。如果没有数据,效果则和insert into一样；
# insert ignore表示，如果中已经存在相同的记录，则忽略当前新数据；
insert ignore into actor values("3","ED","CHASE","2006-02-15 12:34:33");
```

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
