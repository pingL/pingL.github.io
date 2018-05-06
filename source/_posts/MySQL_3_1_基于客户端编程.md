---
title: MysqlCookbook_使用Mysql客户端1/2
date: 2018-02-22 22:40:22
updated: 2018-02-22 22:40:22
tags:
			mysql
---
# 引言
MySQL数据库使用以mysqld服务器为中心的客户端——服务器架构。服务器是事实操作数据库的程序。客户端程序并不直接操作服务器。相反，它们使用SQL语句来向服务器传达你的意图。客户端程序被安装在你想访问MySQL的本地机器上，但服务器可以安装在任何地方，只要客户端能连接上。MySQL生来就是网络数据库系统，所以客户端能够和运行本机或任何其他机器上的服务器进行通信。可能因为多个不同目的而写出一些客户端，但每个客户端和服务器的交互都是先连接服务器，发送SQL语句到服务器来执行数据库操作，然后从服务器接收语句执行结果。

这其中的一个客户端就包含在MySQL发布包中mysql程序。当交互使用时，mysql提示你输入一条语句，将其发送到MySQL服务器执行，然后显示结果。此项功能使mysql很有用，而且在你进行MySQL编程活动时也是一个颇有价值的工具，它可以你用script访问时帮你很方便地快速查看表结构等等。

本章描述了mysql 的功能以便你能更高效地使用它。
	• 启动、停止mysql
	• 指定连接参数，使用可选择的文件
	• 设定你的PATH变量以便命令解释器能够找到mysql（以及其他MySQL程序）
	• 交互执行SQL语句，使用批量文件
	• 取消和编辑语句
	• 控制mysql输出格式

本章前两节描述如何用mysql配置这些。作为示范目的，示例假设按照如下方式使用MySQL：
	• MySQL服务器运行在本机
	• MySQL用户名和密码分别为causer 和cbpass
	• 数据库名为cookbook

# 1.1 建立MySQL用户账户
使用GRANT语句建立MySQL用户账户，然后账户的名字和密码与服务器建立连接，使用mysql客户端前需要启动相应服务。
% mysql -h localhost -p -u root
连接到MySQL服务器需要一个用户名和密码，你还可以指定服务器正运行的主机名称。如果你没有指定主机名，mysql会假定服务器运行在本机上。
mysql的参数包括
-h localhost  表示连接到运行于本机的MySQL服务器；
-p 用于告知mysql要提示输入密码，
-u root 表示作为MySQL的root用户连接

mysql> GRANT  ALL ON cookbook.* TO 'cbuser'@'localhost' IDENTIFIED BY 'cbpass';

% mysql -h localhost -p -u cbuser

# 1.2 创建数据库和样表
解决方案
使用 create database语句创建数据库，create table 语句创建你想用的每个表，用INSERT语句向表中添加数据行。

创建数据库：
mysql> create database cookbook;

创建表：
首先将cookbook选定默认的数据库：
mysql> USE cookbook;

# 1.3 启动和停止mysql
---
mysqld start
mysql.server start    # 1. 启动
mysql.server stop     # 2. 停止
mysql.server restart  # 3. 重启
---
中止mysql回话： quit

# 1.4 使用可选项文件来指定连接参数
通过设置选项文件my.cnf,使用默认用户登录，
配置教程
http://blog.neten.de/posts/2014/01/27/install-mysql-using-homebrew/
模板下载链接
http://blog.neten.de/attachment/code/mysql/my.cnf.txt

# 1.5 保护选项文件以阻止其他用户读取

# 1.6 混合使用命令行和选项文件参数

# 1.7 找不到mysql时该怎么做

# 1.8 发起SQL语句
分号是最普遍的终结符，但你也可以使用\g （‘go’）

# 1.9 取消一部分输入的语句
一开始输入一条语句，但最终决定不执行它，使用你的行删除符或者\c来取消语句
mysql> select *
    -> from limbs
    -> prig\c
mysql>

# 1.10 重复和编辑SQL语句
如果输入的语句包含一个错误，使用mysql内建的输入行编辑能力。

# 1.11 自动完成数据库和表名
使用mysql的名称自动完成工具，输入部分的数据库，然后按下Tab键

# 1.12 让mysql从文件中读取文件
问题：你需要mysql从文件中读取语句以避免手工输入它们
解决方案：重定向mysql的输入，或者用source命令

# 1.13 让mysql从其他程序读取语句
问题：以另一个程序的输出作为mysql的输入
解决方案： 使用 pipe（管道）
展示mysql如何从文件中读取SQL语句：
% mysql cookbook < limbs.sql

# 1.14 一行输入所有SQL
直接在mysql命令行中指定要执行的语句，mysql能从其参数列表中读取语句，
例如：查出limbs表中有多少行，运行一下命令：
$ mysql -e "select count(*) from limbs" cookbook

要用此方式执行多条语句，以分号分隔即可：
$ mysql -e "select count(*) from limbs; select now()" cookbook

# 1.15 使用拷贝粘贴作为mysql输入源
问题：你想利用你的图形用户界面（GUI）使mysql更易于使用。
解决方案：使用拷贝粘贴为mysql提供要执行的语句，这样就能利用你GUI能力为mysql展现的终端接口提供补充。
讨论：在Window环境下，拷贝粘贴非常有用，它能帮你一次运行多个程序并在它们之间传输信息。如果你在一个视窗打开包含语句的文档，你可以从那里拷贝语句然后将它们粘贴进你正运行mysql的视窗中。这和你自己手工输入语句并无二致，只是更快了，对于你频繁发出的语句，保持他们在一个独立的窗口中是一个很好的方法，这样就能确保他们总是触手可及。
