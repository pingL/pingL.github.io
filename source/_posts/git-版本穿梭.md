---
title: git 版本穿梭
date: 2017-07-01 23:34:10
tags:
- git
---

提交上线了一个版本。后来在本地改动代码出错，找不到原因，决定下载在线上版本，
由于我改动代码未提交，所以可以回到改动前的版本，

首先查看提交版本

+ $ git log
+ $ git log --pretty=oneline //查看时间线
+ $ git reset --hard commit_id //最近的一次提交 commit_id:84077


现在总结一下：

HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令
git reset --hard commit_id。

穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
