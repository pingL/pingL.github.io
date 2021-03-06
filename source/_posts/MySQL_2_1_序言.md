---
title: MySQL_2_1序言
date: 2018-02-19 23:30:06
updated: 2018-02-19 23:30:06
tags:
	mysql
---

1.0 序言
你可以次从本书学的其他知识包括：
	· 如何使用SQL查询、排序和统计行
	· 如何发现两表之间匹配或不匹配的行
	· 如何执行一次事务
	· 如何计算日期或时间的间隔，包括计算年龄
	· 如何识别或移除重复行
	· 如何将图片存入Mysql并在网页中查询出来以供显示
	· 如何合理使用LOAD DATA 读取你的数据文件或者查明文件里的哪些值不正确
	· 如何使用strict模式来阻止错误数据进入你的数据库中
	· 如何将一个表或者一个数据库拷贝到另一个服务器
	· 如何生成序列值以用作唯一的行标识符
	· 如何编写存储过程和函数
	· 如何将视图用在“虚拟表”
	· 如何设置触发器，使其在你插入或更新表行时被激活来执行特定的数据处理操作
	· 如何创建按照计划执行的数据库事件
了解如何使用Mysql的其中一个方面是理解怎么和服务器进行通信，也就是如何使用SQL格式化查询语言。因此本书的一个重点就是使用SQL来阐明回答特定类型问题的查询。学习使用SQL的一个有用工具是包含在Mysql发行包中的mysql客户端。
但仅仅发挥SQL查询能力还不够，从数据库中获取的信息需要进一步处理或者以特定的方式展现。如果有复杂相互关系的查询时，譬如你将一次查询的结果作为其他查询的基础时该怎么办？ 或者你需要生成特定格式需求的报表时该怎么办？这些问题将我们带到本书的另一个重点上——如何通过应用程序接口（API）来编写和Mysql服务器交互的程序。当你了解如何在编程语言的上下文中使用Mysql时，你就获得如以下方式开发Mysql潜力的能力：
	· 记住某次查询的结果，并在以后的某个时刻使用它
	· 充分发挥通用编程语言的功能。这就使你可以基于查询的成功或失败，或者基于返回行的内容作出决定，然后采取行动
	· 随心所欲地格式化并显示查询的结果，如果你正在编写一个命令行脚本，你可以生成普通文本；如果是一个基于Web的脚本，你同样可以生成一个HTML表格，如果它是一个提取信息用于传输到其他系统的应用，你可以生成一个XML形式的数据文件
1.1
