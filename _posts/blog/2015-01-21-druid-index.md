---
layout: post
title: druid索引介绍
description: 介绍druid.io索引组成及基本用途
category: blog
---

关于druid.io索引的基本介绍，可以先参考一下[官网文档][1]。我们知道，druid.io中的索引主要由索引段（segment）组成，segment的结构如下所示：
![druid.io segment](http://druid.io/docs/img/druid-column-types.png)

在深入研究索引格式之前，首先有必要来了解一下各部分的基本用途（我们研究一个新的系统时，往往也需要这样，先整体了解，不要刚开始就淹没在细节中）。为了便于说明，这里假设我们有这样一张表`playinfo`，需要统计客户端软件的用户播放行为：

* `uid`
* `version`
* `os`
* `songid`
* `timestamp`

假设我们需要做这样的查询：

`select count(songid) from playinfo where uid = 'yaotc' and timestamp between 2016-01-01T00:00 and 2016-01-02T00:00`

按照关系数据库的做法，往往是根据where中指定的字段索引（`uid`），找到对应的数据行，然后根据其他字段（`timestamp`）筛选出满足所有条件的行，然后对字段应用聚合函数（`count(songid)`）。

当数据量并不大的时候，一切看上去都非常美好。但是，当数据总量很大的时候，就会带来两个问题：
<ul>
    <li>b树随着结构越来越大，查询和写入性能都会明显下降</li>
    <li>传统的rdb往往是行式存储，所有的字段都要基于行查询，所以即便我们只关心其中少数的字段，也避免不了由于行读取带来的大量io</li>
</ul>

对于第一个问题，通常的解决办法就是分库分表（sharding），只是我们需要考虑到由此带来的后遗症，即表会失去join的能力（当然很多时候我们不见得需要join，但是不能join还用rdb搞毛线╮(╯_╰)╭）。如果我们顺着这个思路往下走，由于这类业务往往是在某一个时间粒度内做查询，那么基于时间做sharding，这样每个shard上的索引都不至于过大，然后将基于shard的查询服务并行化（elasticsearch,hbase等灵魂附体），这样第一个问题也就解决了。

对于第二个问题，很显然，如果我们能够基于列去做查询和读取，当我们需要的列并不多时，由于io规模的降低，性能也会有不小的提升。

ok，说了这么多，我们考虑一下，假如我们实现了这样的一个系统，那么基于它做上面的sql查询，大概就会是这样几步：
1. 基于时间区间（`granularity`）找到相应的shard
2. 基于时间（`timestamp`）在时间列找到对应的行号
3. 基于筛选列（`uid`）找到对应的行号，与其他筛选列及时间列得出的行号求交
4. 根据最终的行号在关心的列（`songid`）中取出对应的值，应用聚合函数（`count`）

说了这么多，对于druid.io的索引结构其实也就不难理解了：

* `segment` 基于时间的shard
* `timestamp column` 根据时间筛选行号
* `timestamp column` 根据列筛选行号 
* `metric column` 获取需要应用聚合函数的列值，聚合

值得注意的是，这里遗漏了一种情况，即通过行号查找普通列列值的情况（比如在`count(songid) group by version`中的`version`列）。

到此为止，druid.io的索引结构及其用途就大致介绍清楚（了吧）。限于篇幅，各个列的详细格式会在后面的博客中详细分析，本篇就到这里了，拜了个拜╮(╯▽╰)╭。

[Yaotc]:    http://yaotec.info  "Yaotc"
[1]:   http://druid.io/docs/latest/design/segments.html "druid.io segments"