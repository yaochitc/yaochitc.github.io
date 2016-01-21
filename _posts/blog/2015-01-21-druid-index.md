---
layout: post
title: druid索引介绍
description: 介绍druid.io索引组成及基本用途
category: blog
---

关于druid.io索引的基本介绍，可以先参考一下[官网文档][1]。我们知道，druid.io中的索引主要由以下几个部分组成：

<ul>
    <li>dimension column</li>
    <li>metric column</li>
    <li>timestamp column</li>
</ul>

在深入研究索引格式之前，首先来了解一下各部分的基本用途（我们研究一个新的系统时，往往也需要这样，先整体了解，不要刚开始就淹没在细节中）。


[Yaotc]:    http://yaotec.info  "Yaotc"
[1]:   http://druid.io/docs/latest/design/segments.html "druid.io segments"