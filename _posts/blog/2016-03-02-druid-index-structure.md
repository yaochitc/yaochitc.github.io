---
layout: post
title: druid.io索引格式及建索引过程分析
description: 这部分是druid.io的核心内容
category: blog
---

# 概述
在[前一篇][1]中，已经对druid.io的索引结构大致做了介绍，这一篇会对其内部格式做详细介绍。这部分的分析会结合代码展开，配合ide阅读效果更佳。  

# 索引格式
关于druid.io的索引结构，首先可以回顾一下如下这幅图：
![druid.io segment](http://druid.io/docs/img/druid-column-types.png)

接下来分别介绍3部分的具体索引格式：
## Timestamp column


## Dimension column

## Metric column

# 索引创建

[Yaotc]:    http://yaotec.info  "Yaotc"
[1]: http://yaotec.info/druid-index