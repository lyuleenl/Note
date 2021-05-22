# 基本功能及使用范围介绍

## 背景

我们都知道数据要存储在数据库里，当我们需要获取数据时需要在数据库中搜索，比如我们搜索Mac这个字符串，此时会出现一些问题:

1. 搜索数据库需要全局搜索，假如数据有100w条就需要搜索100w次。且每次需要将数据完整加载出来进行匹配。
2. 一条数据长度可能非常长，那我们用传统的方式只能一点一点去匹配，非常耗时。
3. 搜索词必须精准，若我们将Mac错写为Max则可能白白搜索一遍却无结果。

综上，用传统的数据库进行搜索看起来不太靠谱。

## 全文索引和Lucene

全文索引，倒排索引，倒排索引简单来说就是将key与value交换。

Lucene是一个jar包，包含各种建立倒排索引，搜索算法。

参考：[ElasticSearch索引原理介绍](https://www.cnblogs.com/dreamroute/p/8484457.html)

​			[索引原理B与B+树](https://www.jianshu.com/p/6b90a185a795)

​			[Lucene中的FST深入剖析](https://www.shenyanchao.cn/blog/2018/12/04/lucene-fst/)

## ElasticSearch

ElasticSearch简称es，是用来简化Lucene操作的分布式搜索引擎，数据分析引擎，全文检索，数据的实时处理等。

主要是将全文检索、数据分析以及分布式技术，合并在了一起，才形成了独一无二的ES；

lucene（全文检索），商用的数据分析软件（也是有的），分布式数据库（mycat）

# Windows上使用ElasticSearch

1. 下载ElasticSearch和Kibana，https://www.elastic.co/cn/start
2. 解压ElasticSearch，并运行bin\elasticsearch.bat
3. 检查ES是否启动成功：http://localhost:9200/?pretty

```json
{
  "name" : "MD_LEIYU",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "AUObaXlQTJ28gzwf0u6iLA",
  "version" : {
    "number" : "7.12.0",
    "build_flavor" : "default",
    "build_type" : "zip",
    "build_hash" : "78722783c38caa25a70982b5b042074cde5d3b3a",
    "build_date" : "2021-03-18T06:17:15.410153305Z",
    "build_snapshot" : false,
    "lucene_version" : "8.8.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

4. 解压kibana，运行bin\kibana.bat
5. 访问http://localhost:5601/app/home，点击左上角，下拉找到Dev Tools
6. 输入GET _cluster/health获取当前集群关键的信息

```json
{
  "cluster_name" : "elasticsearch",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 9,
  "active_shards" : 9,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 2,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 81.81818181818183
}
```

可以通过 status 的值了解集群的健康状态： green、yellow、red

- green：每个索引的 primary shard 和 replica shard 都是 active 状态的
- yellow：每个索引的 primary shard 都是 active 状态的，但是部分 replica shard 不是 active 状态，处于不可用的状态
- red：不是所有索引的 primary shard 都是 active 状态的，部分索引有数据丢失了

**为什么现在会处于一个 yellow 状态？**

我们现在就一个笔记本电脑，就启动了一个 es 进程，相当于就只有一个 node。

现在 es 中有一个 index，就是 kibana 自己内置建立的 index。由于默认的配置是给每个 index 分配 5个 primary shard 和 5个 replica shard，而且 primary shard 和 replica shard 不能在同一台机器上（为了容错）。

现在 kibana 自己建立的 index 是 1个 primary shard 和 1个 replica shard。

当前就一个 node，所以只有 1个 primary shard 被分配了和启动了，但是一个 replica shard 没有第二台机器去启动。

# ElasticSearch简单操作

## 索引

创建索引`PUT /test_index?pretty`

```json
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "test_index"
}
```

查看索引`GET /_cat/indices?v`

```
yellow open   test_index               gqucjlnWTIS-c9O3Pg96wg   1   1          0            0       208b           208b
```

删除索引`DELETE /test_index?pretty`

```json
{
  "acknowledged" : true
}
```

## 增删改查

插入数据

```json
PUT /testrun/message/1
{
    "id":28237,
    "testMethodId":32333,
    "testRunId":1712,
    "result":2,
    "triaged":"IUSR[06/10]: Crashing when playing SS vod back.",
    "bugId":null,
    "failureMessage":"Assert.Fail failed. Never got VODStart state to be set. Asset title: [RAT_9000901]"
}
```

继续插入

```json
PUT /testrun/message/2
{
    "id":28259,
    "testMethodId":2271,
    "testRunId":1720,
    "result":2,
    "triaged":"IUSR[06/12]: fixed by add zone cache",
    "bugId":null,
    "failureMessage":"Test method Mtp.Tests.ClientApps.GuideDBEpgFull.GridGuideUIProgramInfoPage_2271 threw exception: \r\nSystem.Runtime.Remoting.ServerException: Unable to get Zone Xml data from ListingsUpdater.xml.  Current version is determined to be:Server3_0FP5. Ensure the &lt;ServerVersion&gt; in manifest.xml is set correctly for your test rig."
}
```

```json
PUT /testrun/message/3
{
    "id":28260,
    "testMethodId":2272,
    "testRunId":1720,
    "result":2,
    "triaged": "IUSR[07/13]: unsupport local dvr",
    "bugId":null,
    "failureMessage":"Test method Mtp.Tests.ClientApps.ParentalControls.PconDVRShortPrograms.Apps_18779 threw exception: \r\nMicrosoft.Mediaroom.Test.Client.DVRException: Unable to find any channel recording that show program: Program Title: [Guide Option Movies2 to test Children only Category]\nChannel: [1606]\nCallLetter: [Movies2]\nEpisode Title: []\nStartTime: [7/10/2020 3:30:00 PM]\nEndTime: [7/10/2020 3:40:00 PM]\nRating: [ G]\nCast: []\nId: [2100009273]\nTvSeriesId:[]\n"
}
```

```json
PUT /testrun/message/4
{
    "id":31933,
    "testMethodId":30340,
    "testRunId":1811,
    "result":2,
    "triaged":"IUSR[07/13]: unspport local dvr",
    "bugId":null,
    "failureMessage":"Test method Mtp.Tests.ClientApps.ParentalControls.PconDVRShortPrograms.TC_30340 threw exception: \r\nMicrosoft.Mediaroom.Test.Client.DVRException: Unable to find any channel recording that show program: Program Title: [Live-ele35-36.UHD-0507_000321-077-MasterTS_CC13_AVC_720p_5994]\nChannel: [2507]\nCallLetter: [HEVC-24]\nEpisode Title: []\nStartTime: [7/10/2020 3:30:00 PM]\nEndTime: [7/10/2020 4:00:00 PM]\nRating: []\nCast: []\nId: [2100010687]\nTvSeriesId:[MSTV119768130]\n"
}
```

查询数据

`GET /testrun/message/1`

```json
#! [types removal] Specifying types in document get requests is deprecated, use the /{index}/_doc/{id} endpoint instead.
{
  "_index" : "testrun",
  "_type" : "message",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 4,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "id" : 28237,
    "testMethodId" : 32333,
    "testRunId" : 1712,
    "result" : 2,
    "triaged" : "IUSR[06/10]: Crashing when playing SS vod back.",
    "bugId" : null,
    "failureMessage" : "Assert.Fail failed. Never got VODStart state to be set. Asset title: [RAT_9000901]"
  }
}
```



