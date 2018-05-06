---
title: MySQL_3_2_使用MySQL客户端
date: 2018-02-25 16:37:22
updated: 2018-02-25 16:37:22
tags:
---
###### 1.16 预防查询输出超出屏幕
 
###### 1.17 发送查询输出到文件或程序
 
让mysql输出到其他地方而非屏幕，重定向mysql的输出，或者使用管道
 
% mysql cookbook < inputfile  > outputfile
 
###### 1.18 选择表格或制表符定界的查询输出格式
 
问题：当你想获得制表符定界的输出时，mysql却生成表格式的输出，或者相反，
解决方案 ： 使用合适的命令选项来显示需要的格式
使用-t 或者（--table）选项来生成更具可读性的表格输出：
% mysql -t cookbook < inputfile | lpr
% mysql -t cookbook < inputfile | mail paul
 
反向操作是在交互模式下生成批量输出，达到这点使用-B（或--batch）
 
###### 1.19 指定任意的输出列分隔符
问题： 你想mysql使用除了制表符外的定界符来生成语句输出
解决方案：mysql 自身并未提供设置输出定界符的功能，但你可以对mysql输出进行加工来重新格式化。
 
###### 1.20 生成HTML或XML输出
将查询结果转化成HTML或XML
使用 mysql -H 或 mysql -X
 
例如：
% mysql  -e "select * from limbs where legs=0" cookbook
% mysql -H -e "select * from limbs where legs=0" cookbook
 
###### 1.21 在查询输出中禁止列头部
% mysql -e "select arms from limbs" cookbook | sumarize
要创建仅包含数据值的输出，就要  --skip-column-names 选项禁止列头部行：
% mysql  --skip-column-names -e "select arms from limbs" cookbook | sumarize
你也可通过两次指定“silent” 选项（-s 或 --silent）来达到和--skip-coumn-names 同等的效果：
% mysql -ss -e "select arms from limbs" cookbook | sumarize
 
###### 1.22 使长输出行更具可读性
问题：当你的一个查询生成了很长的输出行，它杂乱无章的出现在你的屏幕上
解决方案：使用垂直的输出格式
mysql> show full columns from limbs;
+-------+-------------+-----------------+------+-----+---------+-------+---------------------------------+---------+
| Field | Type        | Collation       | Null | Key | Default | Extra | Privileges                      | Comment |
+-------+-------------+-----------------+------+-----+---------+-------+---------------------------------+---------+
| thing | varchar(20) | utf8_general_ci | YES  |     | NULL    |       | select,insert,update,references |         |
| legs  | int(11)     | NULL            | YES  |     | NULL    |       | select,insert,update,references |         |
| arms  | int(11)     | NULL            | YES  |     | NULL    |       | select,insert,update,references |         |
+-------+-------------+-----------------+------+-----+---------+-------+---------------------------------+---------+
 
为避免此问题，将每列值显示在单独的一行来生成“垂直的”输出。通过\G 而不是用字符a；或\g 来终止一条语句来实现的。
用垂直格式输出样式：
mysql> show full columns from limbs\G;
*************************** 1. row ***************************
     Field: thing
      Type: varchar(20)
 Collation: utf8_general_ci
      Null: YES
       Key: 
   Default: NULL
     Extra: 
Privileges: select,insert,update,references
   Comment: 
*************************** 2. row ***************************
     Field: legs
      Type: int(11)
 Collation: NULL
      Null: YES
       Key: 
   Default: NULL
     Extra: 
Privileges: select,insert,update,references
   Comment: 
*************************** 3. row ***************************
     Field: arms
      Type: int(11)
 Collation: NULL
      Null: YES
       Key: 
   Default: NULL
     Extra: 
Privileges: select,insert,update,references
   Comment: 
3 rows in set (0.00 sec)
 
ERROR: 
No query specified
 
###### 1.23控制mysql的繁冗级别
问题：你需要mysql产生更多的输出，或者相反
解决方案： 使用-v 或 -s 选项来获取更多或者更少
% echo "select now()" | mysql -v
% echo "select now()" | mysql -vv
% echo "select now()" | mysql -vvv
-v 和--verbose 相对的是 -s 和--silent ，它们也可以多次使用来达到叠加的效果
 
###### 1.24记录交互式的mysql回话
问题： 想保留一份在mysql会话中所做事情的记录
解决方案：创建一个tee文件
指定记录会话的文件名：
% mysql  --tee = tmp.out cookbook
如果只想记录会话的某个特定部分就很有用：
mysql> \T tem.out
Logging to file 'tem.out'
mysql> \t
Outfile disabled.
 
tem 文件包含你所输入的语句，以及这些语句的输出，所以它是一个保存完整记录的便捷方法。
 
###### 1.25 以之前执行的语句创建mysql脚本
 
###### 1.26 在SQL 语句中使用用户自定义的变量
 
###### 1.27 为查询输出行计数
 
记录查询结果的行数
对mysql的输出进行处理或者使用用户定义的变量
	•	% mysql --skip-column-names -e "SELECT thing, arms FROM limbs " cookbook;
➜  MysqlFile mysql cookbook  --skip-column-names -e "select thing, arms FROM limbs" | cat -n
     1 human 2
     2 insect 0
     3 squid 10
     4 fish 0
     5 centipede 0
     6 table 0
     7 armchair 2
     8 phonograph 1
     9 tripod 0
    10 Peg Leg Pete 2
    11 space alien NULL
 
	•	SELECT @n := @n + 1 AS rownum, thing, arms, legs FROM limbs;
mysql> SELECT @n := @n + 1 AS rownum, thing, arms, legs FROM limbs;
+--------+--------------+------+------+
| rownum | thing        | arms | legs |
+--------+--------------+------+------+
|      1 | human        |    2 |    2 |
|      2 | insect       |    0 |    6 |
|      3 | squid        |   10 |    0 |
|      4 | fish         |    0 |    0 |
|      5 | centipede    |    0 |  100 |
|      6 | table        |    0 |    4 |
|      7 | armchair     |    2 |    4 |
|      8 | phonograph   |    1 |    0 |
|      9 | tripod       |    0 |    3 |
|     10 | Peg Leg Pete |    2 |    1 |
|     11 | space alien  | NULL | NULL |
+--------+--------------+------+------+
11 rows in set (0.01 sec)
 
 
###### 1.28 将mysql用作计算器
将mysql用作计算器。MySQL并不要求每个SELECT语句都关联某个表，所以你可以选择任意表达式的结果
 
###### 1.29 在Shell脚本中使用mysql
你想在一个shell脚本中调用mysql而不是交互式地使用它
-#!/bin/sh
-# mysql_uptime2.sh  报道服务器运行时间
mysql -e STATUS | grep "^Uptime"
 
结果如下：
 MysqlFile ./mysql_uptime2.sh 
Uptime: 27 min 50 sec
