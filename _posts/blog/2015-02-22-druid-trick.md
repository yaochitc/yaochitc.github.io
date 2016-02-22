---
layout: post
title: druid.io小技巧
description: druid.io开发运维小技巧，持续更新
category: blog
---

##进程查询
我们都知道，druid.io中，所有的节点都是通过同一个入口启动的，形如这样：  
`io.druid.cli.Main server historical`
那么，当我们通过jps查看进程信息时，就会出现这样的状况：  
`[hadoop@apm-druid-1 ~]$ jps`  
`34718 Main`  
`35873 Main`  
`21546 Main`  
`52038 Main`  
`54736 Main`  
`45349 Main`  
`34345 Jps`  
所有的节点都会显示同样的进程名╮(╯_╰)╭。
但是其实有一个非常简单的办法，就是通过ps和awk来获取真实的进程信息，如下所示：  
`ps -ef | grep druid | grep -v grep | awk '{print $2, $(NF -0)}'`
可以看到，此时显示的进程信息已经是我们想要的东西了╮(￣▽￣)╭：
`[hadoop@apm-druid-1 ~]$ ps -ef | grep druid | grep -v grep | awk '{print $2, $(NF -0)}'`
`21546 coordinator`
`34718 overlord`
`35873 historical`
`45349 realtime`
`52038 broker`
`54736 middleManager`

[Yaotc]:    http://yaotec.info  "Yaotc"