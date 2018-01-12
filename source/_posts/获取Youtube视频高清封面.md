title: 获取Youtube视频高清封面
tags:
  - Youtube
  - Chrome
date: 2018-01-07 18:11:37
updated: 2018-01-07 18:11:37
---
>大家在Youtube上顺利搬砖之后会遇到一个问题，例如缺少该视频精美的封面,国外的视频制作人都会在封面上制作有趣并且符合视频内容的图片。今天就遇到这个需求，方法也是参考网络众多大牛的，我再重新画个轮子。具体操作如下。

准备工具
1、梯子
2、Google chrome

# Step 1
进入Youtube搜索页
在输入框输入番号，例如：Lady Gaga - Million Reasons
如图1
![](https://ws3.sinaimg.cn/large/006tNc79gy1fn88ijik8uj31kw0kedst.jpg)


# Step 2
第一个便是我想要的封面
右键，点击“显示网页源代码”，进入后
command + f，在搜索框内输入 “JPG”，回车键后自动定位到图片链接上
具体的链接是你搜索主页搜索后显示的顺序为准，
例如我搜索后加载后显示位置在第一条，该图片源码里也是第一条。
如图2
![](https://ws3.sinaimg.cn/large/006tNc79gy1fn88iqo02wj31kw0yh7lq.jpg)


# Step3
点击该链接后页面将显示封面，此时的图片是缩略图，还不符合我们高清的图片。
接着在地址栏现有链接里更改一个英文单词:
```
hqdefault.jpp
changes to
maxresdefault.jpg
```
改完后回车，链接将重新加载，得到高清原图，
如图3
![](https://ws1.sinaimg.cn/large/006tNc79gy1fn88ipek4dj31kw0v9e81.jpg)
最后右键将图片存储到本地，就完成整个获取过程。