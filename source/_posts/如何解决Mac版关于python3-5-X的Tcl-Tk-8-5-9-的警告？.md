---
title: 如何解决Mac版关于python3.5.X的Tcl/Tk (8.5.9) 的警告？
date: 2017-12-24 10:47:07
updated: 2017-12-24 10:47:07
tags : 
	- IDLE
	- Python
	-  Tcl/Tk
---



#### 问题： 
>WARNING: The version of Tcl/Tk (8.5.9) in use may be unstable. Visit http://www.python.org/download/mac/tcltk/ for current information. - 

#### 分析：这个警告导致无法在IDLE编辑器上输入中文


#### 解决办法：指示安装相应的Tk版本 如 ： 8.5.18
下载地址：[Download and Install Tcl: ActiveTcl](http://downloads.activestate.com/ActiveTcl/releases/8.5.18.0/ActiveTcl8.5.18.0.298892-macosx10.5-i386-x86_64-threaded.dmg)
在国内下载速度过慢直接百度网盘下载[ActiveTcl8.5.18.0.298892-macosx10.5-i386-x86_64-threaded.dmg](https://pan.baidu.com/s/1geUPMcr)，原文件下载时间 2017-12-24 12:00

#### 结果：警告消失，可以在IDLE里直接输入中文字体。
如图
![](https://ws1.sinaimg.cn/large/006tKfTcgy1fmrqke4mmoj30xk0f2acz.jpg)

