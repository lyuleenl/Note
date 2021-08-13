# 查询字段是否存在

```JSON
GET:http://localhost:9200/testrun/_search
{
    "query": {
        "bool": {
            "must_not": {
                "exists": {
                    "field": "callStack"
                }
            }
        }
    }
}
```

# 查询数量

```json
GET:http://localhost:9200/testrun/_count
```

# 创建索引

```JSON
PUT：http://localhost:9200/testrun
```

# 删除索引

```json
DELETE:http://localhost:9200/testrun
```

# 条件删除

```json
POST:http://10.164.83.120:9200/testrun/_delete_by_query
{
    "query": {
        "bool": {
            "filter": {
                "regexp": {
                    "bugId": {
                        "value": ".{8,}"
                    }
                }
            }
        }
    }
}
```

# 字符串排序

```json
GET /lib/_search
{
  "query": {
    "match_all": {}
  }
  , "sort": [
    {
      "content.keyword": {
        "order": "desc"
      }
    }
  ]
}
```

# 字符串范围查询

```json
GET  /fptest_2019-04-03/rrr/_search
{
  "query":{
     "range": {
      "birthday.keyword": {
        "gte": "1994-01-01",
        "lte": "1994-12-31",
        "format": "yyyy-MM-dd"
      }
    }
    }
}
```

注：range查询使用的参数

- gte：大于等于
- gt：大于
- lte：小于等于
- lt：小于