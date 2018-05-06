---
title: MySQL_4_从表中查询
date: 2018-05-06 09:22:32
updated: 2018-05-06 09:22:32
tags:
---
# 3.0 引言

select允许你对行查询的几个方面进行控制：
所要查询的表
所要查询的行和列
重命名查询结果列
对查询结果进行排序

# 3.1 制定查询列/从指定列中查询

讨论 最简单的方法是使用 select * from tb_name 获取一个 数据库里的每一列信息，其中 “ * ” 说明对数据库中的所有列进行查询，但是查询结果不能安装用户指定的任意顺序显示，要达到这一目的，可以使用一下的语句指定查询列。
显示地查询列的另一个好处在于，在查询结果中可以只显示用户所关注的列，而不是显示每一个列的信息，例如：

# 3.2 指定查询行
使用where语句设定查询条件，使得查询结果只包含符合查询条件的行，例如指定查询生活在某个城市的客户信息，或者状态为“finished”的任务。
where srchost = 'venus';
where srchost like 's%';
where srchost = 'barb' and dstuser = 'tricia';


# 3.3 格式化显示查询结果
问题：自定义查询结果（列名或者对应值）
使用AS name 子句来指定列别名

# 3.4 使用别名来简化程序
# 3.5 合并多列来构建复合值
查询结果需要组合表中多个不同的列值来获得
使用concat（）函数，
mysql> select DATE_FORMAT(t,'%M %e,%Y') AS data_sent,
    -> concat(srcuser, '@', srchost) AS sender,
    -> concat(datuser, '@', dathost) AS recipient,
    -> size from mail;
# 3.6 where表达式中的别名
mysql> select t,srcuser, dstuser, size/1024 AS kilobytes from mail where size/1024 > 500
    -> \g

# 3.7 调试比较表达式
mysql> select * from mail where srcuser < 'c' and size > 5000;
+---------------------+---------+---------+---------+---------+-------+
| t                   | srcuser | srchost | dstuser | dsthost | size  |
+---------------------+---------+---------+---------+---------+-------+
| 2014-05-11 10:15:08 | barb    | saturn  | tricia  | mars    | 58274 |
| 2014-05-14 14:42:21 | barb    | venus   | barb    | venus   | 98151 |
+---------------------+---------+---------+---------+---------+-------+
2 rows in set (0.00 sec)

mysql> select srcuser, srcuser < 'c' ,size , size > 5000 from mail;
+---------+---------------+---------+-------------+
| srcuser | srcuser < 'c' | size    | size > 5000 |
+---------+---------------+---------+-------------+
| barb    |             1 |   58274 |           1 |
| tricia  |             0 |  194925 |           1 |
| phil    |             0 |    1048 |           0 |
| barb    |             1 |     271 |           0 |
| gene    |             0 |    2291 |           0 |
| phil    |             0 |    5781 |           1 |
| barb    |             1 |   98151 |           1 |
| tricia  |             0 | 2394482 |           1 |
| gene    |             0 |    3824 |           0 |
| phil    |             0 |     978 |           0 |
| gene    |             0 |  998532 |           1 |
| gene    |             0 |    3856 |           0 |
| gene    |             0 |     613 |           0 |
| phil    |             0 |   10294 |           1 |
| phil    |             0 |     873 |           0 |
| gene    |             0 |   23992 |           1 |
+---------+---------------+---------+-------------+
16 rows in set (0.00 sec)

# 3.8 使查询结果唯一化
mysql> select srcuser from mail;
+---------+
| srcuser |
+---------+
| barb    |
| tricia  |
| phil    |
| barb    |
| gene    |
| phil    |
| barb    |
| tricia  |
| gene    |
| phil    |
| gene    |
| gene    |
| gene    |
| phil    |
| phil    |
| gene    |
+---------+
16 rows in set (0.00 sec)

mysql> select distinct srcuser from mail;
+---------+
| srcuser |
+---------+
| barb    |
| tricia  |
| phil    |
| gene    |
+---------+
4 rows in set (0.01 sec)

# 3.9 如何处理 null值

如何将列值与NULL值比较呢
选择使用合适的比较操作符： is null  、is not null或者 <=>

没有样本表暂时跳过

# 3.10 在用户程序中使用NULL 作为比较参数
···从另一个角度来避免这个问题，在定义数据库时，如果不是特别需要，就不允许列值为NULL。
3.11 结果集序列
在查询语句中使用一个ORDER BY 子句对查询结果进行排序，可以在order by子句中指定数据库的一列或者多列作为排序依据，
mysql> select * from mail where size > 5000 order by size ;
+---------------------+---------+---------+---------+---------+---------+
| t                   | srcuser | srchost | dstuser | dsthost | size    |
+---------------------+---------+---------+---------+---------+---------+
| 2014-05-14 11:52:17 | phil    | mars    | tricia  | saturn  |    5781 |
| 2014-05-16 23:04:19 | phil    | venus   | barb    | venus   |   10294 |
| 2014-05-19 22:21:51 | gene    | saturn  | gene    | venus   |   23992 |
| 2014-05-11 10:15:08 | barb    | saturn  | tricia  | mars    |   58274 |
| 2014-05-14 14:42:21 | barb    | venus   | barb    | venus   |   98151 |
| 2014-05-12 12:48:13 | tricia  | mars    | gene    | venus   |  194925 |
| 2014-05-15 10:25:52 | gene    | mars    | tricia  | saturn  |  998532 |
| 2014-05-14 17:03:01 | tricia  | saturn  | phil    | venus   | 2394482 |
+---------------------+---------+---------+---------+---------+---------+
8 rows in set (0.00 sec)

在order by 子句的最后，即定义的排序依据的列名之后加上关键字DESC使查询结果按逆序排序。

# 3.12 使用视图来简化查询
mysql> select 
    -> date_format(t,'%M %e %Y') as date_sent,
    -> concat(srcuser,'@',srchost) as sender,
    -> concat(dstuser,'@',dsthost) as recipient,
    -> size from mail;
+-------------+---------------+---------------+---------+
| date_sent   | sender        | recipient     | size    |
+-------------+---------------+---------------+---------+
| May 11 2014 | barb@saturn   | tricia@mars   |   58274 |
| May 12 2014 | tricia@mars   | gene@venus    |  194925 |
| May 12 2014 | phil@mars     | phil@saturn   |    1048 |
| May 12 2014 | barb@saturn   | tricia@venus  |     271 |
| May 14 2014 | gene@venus    | barb@mars     |    2291 |
| May 14 2014 | phil@mars     | tricia@saturn |    5781 |
| May 14 2014 | barb@venus    | barb@venus    |   98151 |
| May 14 2014 | tricia@saturn | phil@venus    | 2394482 |
| May 15 2014 | gene@mars     | gene@saturn   |    3824 |
| May 15 2014 | phil@venus    | phil@venus    |     978 |
| May 15 2014 | gene@mars     | tricia@saturn |  998532 |
| May 15 2014 | gene@saturn   | gene@mars     |    3856 |
| May 16 2014 | gene@venus    | barb@mars     |     613 |
| May 16 2014 | phil@venus    | barb@venus    |   10294 |
| May 19 2014 | phil@mars     | tricia@saturn |     873 |
| May 19 2014 | gene@saturn   | gene@venus    |   23992 |
+-------------+---------------+---------------+---------+
16 rows in set (0.00 sec)

 当需要频繁使用类似上例的查询时，就需要每次都在查询语句中重复写入类似复杂的运算表达式，在这样的情况下，就可以使用视图来简化查询语句，视图是一个虚拟的数据库，它并不包含任何实际的数据，实际上，一个视图可以看作一个特定的select语句，下面的视图mail_view 与定义它的select语句是等价的：
mysql> create view mail_view as
    -> select
    -> date_format(t, '%M %e %Y') as date_sent,
    -> concat(srcuser, '@', srchost) as sender,
    -> concat(dstuser, '@', dsthost) as recipient,
    -> size from mail;
Query OK, 0 rows affected (0.02 sec)

mysql> select date_sent, sender, size from mail_view
    -> where size > 100000 order by size;
+-------------+---------------+---------+
| date_sent   | sender        | size    |
+-------------+---------------+---------+
| May 12 2014 | tricia@mars   |  194925 |
| May 15 2014 | gene@mars     |  998532 |
| May 14 2014 | tricia@saturn | 2394482 |
+-------------+---------------+---------+
3 rows in set (0.00 sec)


# 3.13 多表查询

需要从多个表中查询数据
使用联合(join)操作,或者使用子查询
没有相应样本表
# 3.14 从查询结果集头或者尾取出部分行
问题：如何才能只显示查询结果的部分行，例如第一行或者最后5行
解决方案： 使用limit 子句，并且经常要跟order by 子句配合使用
LIMIT子句主要用于以下类型的查询：
当需要查询的结果是：第一个或者最后一个，最大或者最小，最新或者最旧，最便宜或者最贵。
当查询结果行数较多，将查询结果划分小块，每次只取出其中一块返回显示给用户，这种技术在WEB中比较常见：将一个跨越多个页面的查询结果划分为多次显示，每次显示一个页面。每次显示查询结果的一部分，可以使查询结果更具有可读性
mysql> select * from profile order by birth limit 1;
+----+--------+------------+-------+----------------+------+
| id | name   | birth      | color | foods          | cats |
+----+--------+------------+-------+----------------+------+
|  7 | Joanna | 1952-08-20 | green | lutefisk,fadge |    0 |
+----+--------+------------+-------+----------------+------+
1 row in set (0.00 sec)

当查询结果无序的，所以从查询结果中取出头N行可能无意义，更常见的结果首先使用order by 子句对查询结果进行排序，然后再使用limit子句从查询结果中取出最大或者最小特征的行，
例如
mysql> select * from profile order by birth limit 1;
+----+--------+------------+-------+----------------+------+
| id | name   | birth      | color | foods          | cats |
+----+--------+------------+-------+----------------+------+
|  7 | Joanna | 1952-08-20 | green | lutefisk,fadge |    0 |
+----+--------+------------+-------+----------------+------+
1 row in set (0.00 sec)

如果要获取查询结果的最后N行，则可以对查询结果进行逆序排序，例如需要查找最晚出生的人，查询语句类似上面，不同的是对查询结果按照birth列值递减排序.
mysql> select * from profile order by birth desc limit 1;
+----+-------+------------+-------+---------------+------+
| id | name  | birth      | color | foods         | cats |
+----+-------+------------+-------+---------------+------+
|  3 | Ralph | 1973-11-02 | red   | eggroll,pizza |    4 |
+----+-------+------------+-------+---------------+------+
1 row in set (0.00 sec)

按照历年内最早或者最迟的出生日，根据birth值的月份和日期进行排序，则可以可以将查询结果先根据生日的月和日进行排序：
mysql> select name , date_format(birth, '%m-%d') as birthday
    -> from profile order by birthday limit 1;
+-------+----------+
| name  | birthday |
+-------+----------+
| Henry | 02-14    |
+-------+----------+
1 row in set (0.00 sec)

如果不使用Limit子句，而是人为的忽略除了第一行外的查询结果，用户也一样可以得到需要的信息，使用limit子句的优势在于，数据库服务器只通过网络向用户返回查询结果中的一行，而不会返回其他行，这样避免了网络带宽的浪费，同是提高了用户程序的运行效率。
参考
limit同样应用于delete 和 update 语句，对所要删除或者修改的行进行限制，这种情况，limit子句和where子句组合使用可以有更好的效果，例如：一个数据库表中有5行一样的数据，其中4行是冗余的，此时可以在delect语句中使用一个where子句找到这5行，然后通过语句的最后加上limit 4 删除冗余的4行，这样可以保持数据库信息的唯一性。
# 3.15 在结果集中间选取部分行
Limit 子句可以接收两个参数，使用户可以获取一个查询结果中的任意部分行，这种情况limit两个参数分别指定了从查询结果的第几行开始返回，以及一共返回多少行。
mysql> select * from profile order by birth limit 2,1;
+----+---------+------------+-------+---------------+------+
| id | name    | birth      | color | foods         | cats |
+----+---------+------------+-------+---------------+------+
|  4 | Lothair | 1963-07-04 | blue  | burrito,curry |    5 |
+----+---------+------------+-------+---------------+------+
1 row in set (0.00 sec)

# 3.16 选择合适的LIMIT参数

样本表未特添加成功，暂时跳过

# 3.17 当LIMIT需要"错误"的排列顺序时做什么

问题：limit 和 用于排序的 order by 配合使用，通常在查询中发挥作用，但是有时order by 子句得到的排序结果和你想要的结果相反。

解决方案： 在子查询中使用limit子句取得所需的行，然后在外层查询中对子结果进行排序。

讨论
如果希望得一个查询结果的最后4行记录，只需要将查询结果按照降序排列，然后使用limit 4 子句即可。
如从profile表中查询最晚出生的4个人：
mysql> select name, birth from profile order by birth desc limit 4;
+-------+------------+
| name  | birth      |
+-------+------------+
| Ralph | 1973-11-02 |
| Sybil | 1970-04-13 |
| Nancy | 1969-09-30 |
| Aaron | 1968-09-17 |
+-------+------------+
4 rows in set (0.00 sec)

这样的查询方式，要求首先对查询结果进行降序排列，最后的查询结果也按照降序返回，如果要求结果按照升序排列如何处理？
方法一：使用两个查询语句。首先使用count（） 函数计算表中一共记录了几行数据。
mysql> select count(*) from profile;
+----------+
| count(*) |
+----------+
|        8 |
+----------+
1 row in set (0.00 sec)

然后，使用升序来排列查询结果，并使用带两个参数的LIMIT子句从排序结果中取出所需行。
mysql> select name, birth from profile order by birth limit 4, 4;
+-------+------------+
| name  | birth      |
+-------+------------+
| Aaron | 1968-09-17 |
| Nancy | 1969-09-30 |
| Sybil | 1970-04-13 |
| Ralph | 1973-11-02 |
+-------+------------+
4 rows in set (0.00 sec)

这样的解决方法要求计算表中的总行数，然后还要计算从第几行开始返回结果，用户可能不想这么做，所以有了第二种方法：首先在一个子查询中使用limit子句取出所需的行，然后在外层查询中对该结果的排序，并返回用户。如下：
mysql> select *from                                                                -> (select name ,birth from profile order by birth desc limit 4) AS t
    -> order by birth;
+-------+------------+
| name  | birth      |
+-------+------------+
| Aaron | 1968-09-17 |
| Nancy | 1969-09-30 |
| Sybil | 1970-04-13 |
| Ralph | 1973-11-02 |
+-------+------------+
4 rows in set (0.00 sec)
(from : 后面加了空格再换行，粘贴后变的不规则)
在查询语句中，from 子句后面必须是表名，因此在上例中使用AS t 对子查询结果赋予别名，作为from的一个临时表名。
# 3.18从表达式中计算LIMIT值
如何使用表达式计算LIMIT子句所使用的参数？
实际上，sql语句中，limit子句只能直接以数字作为参数，并不能使用表达式作为limit子句的参数，除非在应用程序中，使用用户程序语句计算出limit参数，然后再将结果插入到sql语句中。
