# TOC
- [Elasticsearchの基礎情報を出力する ](#elasticsearchの基礎情報を出力する)
- [インデックスの一覧を表示する](#インデックスの一覧を表示する)
- [特定のドキュメントを指定して表示する](#特定のドキュメントを指定して表示する)
- [ドキュメントを追加する](#ドキュメントを追加する)
  - [GUIで入れる](#guiで入れる)
  - [一つずつ入れる -> PUT](#一つずつ入れる---put)
  - [複数を一気に入れる -> POST, bulk](#複数を一気に入れる---post-bulk)
  - [BeatsやLogstash経由で入れる](#beatsやlogstash経由で入れる)
- [ドキュメント数を出力する](#ドキュメント数を出力する)
- [ドキュメントを削除する -> _delete_by_query](#ドキュメントを削除する---deletebyquery)
- [ドキュメントを検索する](#ドキュメントを検索する)
  - [検索結果の項目や出力ドキュメント数を指定する](#検索結果の項目や出力ドキュメント数を指定する) 
  - [検索条件の付け方](#検索条件の付け方)
    - [textマッチ](#textマッチ)
    - [複数項目に対して検索 -> multi_match](#複数項目に対して検索---multimatch)
    - [type:textのfieldに対してkyewordとして検索 -> match_phrase](#typetextのfieldに対してkyewordとして検索---matchphrase)
    - [特定の項目に対して、検索スコアに重みを加える](#特定の項目に対して検索スコアに重みを加える)
    - [略さない検索条件](#略さない検索条件)
    - [filter](#filter)
    - [should](#should)
- [aggregation](#aggregation)
  - [クエリの作り方](#クエリの作り方)
  - [グループ化して唯一だったドキュメントを探す](#グループ化して唯一だったドキュメントを探す)
  - [sortを使った検索](#sortを使った検索)
- [フィールドが存在するか確認する](#フィールドが存在するか確認する)
- [search template](#search-template)
- [cluster](#cluster)
  - [clusterを指定して検索](#clusterを指定して検索)
  - [複数のclusterを跨いで検索](#複数のclusterを跨いで検索)
- [alias](#alias)
- [async_search](#asyncsearch)
- [script](#script)
- [mapping](#mapping)
  - [mappingの作成](#mappingの作成)
  - [既存indexのmappingを変更したい時](#既存indexのmappingを変更したい時)
    - [reindex (local to local)](#reindexlocal-to-local)
    - [reindex (remote to local)](#reindexremote-to-local)
  - [reindexせずにmappingを変更](#reindexせずにmappingを変更)
- [検索時に新規項目を生成する(runtimeフィールド)](#検索時に新規項目を生成するruntimeフィールド)
  - [scriptで新規fieldを生成して、そのfieldを使って検索をする場合](#scriptで新規fieldを生成してそのfieldを使って検索をする場合)
  - [一時的にmappingを変更して、変更されたmappingのfieldを使って検索する場合](#一時的にmappingを変更して変更されたmappingのfieldを使って検索する場合)
- [analyzer](#analyzer)
  - [analyzerのテスト](#analyzerのテスト)
  - [analyzerを指定してindexを作成する](#analyzerを指定してindexを作成するß)
- [type](#type)
  - [search_as_you_type](#searchasyoutype)
    - [mapping](#mapping)
    - [検索方法](#検索方法)
- [既存index内のdocumentを変更](#既存index内のdocumentを変更)
- [投入するデータを前処理させてindexに投入](#投入するデータを前処理させてindexに投入)
- [Transform, Pipeline, Policy](#transform-pipeline-policy)
  - [Policy](#policy)
  - [Pipeline](#pipeline)
  - [Transform](#transform)
- [document追加時に自動でindexが新規作成されるのを防ぐ](#document追加時に自動でindexが新規作成されるのを防ぐ)
- [Python](#python)

# Elasticsearchの基礎情報を出力する 
```
GET /
```

# インデックスの一覧を表示する
```
GET _cat/indices
```
いい感じに表示
```
GET _cat/indices?v&s=index
```
システムインデックスも表示する。(シャード単位で出力する)
```
GET _cat/shards?v&s=index
```

# 特定のドキュメントを指定して表示する
```
GET blogs/_doc/9Nr1toMBWaFHZzk0hv9m
```

# ドキュメントを追加する
## GUIで入れる
Data Visualizerから入れる。

## 一つずつ入れる -> PUT
以下のクエリを複数回入れると、入れたdocumentが更新されるだけ。　　
(idを指定しているから)
```
PUT new_index/_doc/1
{
    "security_test": "this will fail"
}
```

## 複数を一気に入れる -> POST, bulk
{"create":{}}という行は必要。  
肝心な中身は次の行に書く。  
documentごとに{"create":{}}は必要。
```
POST categories/_bulk
{"create":{}}
{"uid": "blt26ff0a1ade01f60d","title":"User Stories"}
{"create":{}}
{"uid": "bltfaae4466058cc7d6","title": "Releases"}
{"create":{}}
{"uid": "bltc253e0851420b088","title": "Culture"}
```
## BeatsやLogstash経由で入れる
割愛。

# ドキュメント数を出力する
bashで言うところの wc -l
```
GET /my_index/_count
```

# ドキュメントを削除する -> _delete_by_query
```
POST blogs_fixed3/_delete_by_query
{
  "query": {
    "match": {
      "tags.use_case": "uptime monitoring"
    }
  }
}
```
全部削除
```
POST web_traffic/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}
```

# ドキュメントを検索する
## 検索結果の項目や出力ドキュメント数を指定する
size ... 検索結果の数の上限を指定する  
_source ... 特定の項目のみ結果に出力する
```
GET blogs/_search
{
    "size": 50, 
    "query": {
        "match_all": {}
    },
    "_source": ["title"]
}
```
_sourceにfalseを入れて、fieldで項目を指定するやり方もあるらしいけど、違いは不明。  
マニュアル読んでも書いてなさそう。
```
GET blogs/_search
{
    "size": 50, 
    "query": {
        "match_all": {}
    },
    "_source": false,
    "fields": [
      "title"
    ]
}
```

## 検索条件の付け方
### textマッチ
スペースはorの意味になる。  
query->(bool系)->(must系)->match を省略して、query->matchと書ける。
```
GET blogs/_search
{
    "size": 50, 
    "query": {
        "match": {
          "content": "open source"
        }
    },
    "_source": false,
    "fields": [
      "title"
    ]
}
```
### 複数項目に対して検索 -> multi_match
```
GET blogs/_search
{
    "size": 50, 
    "query": {
        "multi_match": {
          "query": "open source",
          "fields": ["title", "content"]
        }
    },
    "_source": false,
    "fields": [
      "title"
    ]
}
```

### type:textのfieldに対してkeywordとして検索 -> match_phrase
```
GET blogs/_search
{
    "query": {
        "match_phrase": {
            "title": "Elastic Certified Engineer"
        }
    }
}
```

### 特定の項目に対して、検索スコアに重みを加える
_scoreが変化する。  
_scoreはprecision(適合率。TP/((TP+FP))) と recall(再現性。TP/(TP+FN)) を合わせた概念な気がする。（真偽不明）
  
- Recall ... どれだけ取りこぼしなく予測することができたか
- Precision ... 正と予測したものがどれだけ正しかったか
[https://qiita.com/K5K/items/5da52e99861483cae876](https://qiita.com/K5K/items/5da52e99861483cae876)

_scoreは検索条件に対してどれだけ関係性が高いかを示す値みたい。高い方があなたに求められている結果ですよ、の意味。  
公式マニュアル的には、relevance score と呼ぶらしい。
[https://www.elastic.co/guide/en/elasticsearch/reference/8.4/query-filter-context.html](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/query-filter-context.html)

^2をつけてスコアを重み付けする。
```
GET blogs/_search
{
  "query": {
    "multi_match": {
      "query": "certified engineer,",
      "fields": ["content","title^2"]
    }
  }
}
```
phrase扱いでmulti-match。  
[https://www.elastic.co/guide/en/elasticsearch/reference/8.4/query-dsl-multi-match-query.html](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/query-dsl-multi-match-query.html)
```
GET blogs/_search
{
  "size": 20,
  "query": {
      "multi_match": {
          "query": "certified engineer",
          "fields": ["content", "title"],
          "type": "phrase"
      }
  },
  "_source": ["content","title"]
}
```
もっと幅広に検索する（荒く検索する）。  
fuzinessを使う。
recallは上がるけど、precisionは下がる。
```
GET blogs/_search
{
    "query": {
        "multi_match": {
            "query": "certified engineer",
            "fields": ["content", "title"],
            "fuzziness": 2
        }
    }
}
```

### 略さない検索条件
```
GET blogs/_search
{
  "query": {
      "bool": {
          "must": [
              {
                  "multi_match": {
                      "query": "meetups",
                      "fields": ["content", "title"]
                  }
              }
          ],
          "filter": [
              {
                  "range": {
                      "publish_date": {
                      "gte": "now/d-12M"
                      }
                  }
              }
          ]
      }
  }
}
```

### filter
filterはスコアに影響を与えない検索。  
スコアに影響を与えない版"must"
[https://www.elastic.co/guide/en/elasticsearch/reference/8.6/query-dsl-bool-query.html](https://www.elastic.co/guide/en/elasticsearch/reference/8.6/query-dsl-bool-query.html)

### should
"should" only impacts the score

# aggregation
## クエリの作り方
試しにダッシュボードでつくって、画面から自動でDSLを作ってもらい、それを編集するやり方もアリ。
```
GET blogs/_search
{
  "size": 0,
  "aggs": {
    "blogs_by_year": {
      "date_histogram": {
        "field": "publish_date",
        "calendar_interval": "year"
      },
      "aggs": {
        "use_cases": {
          "terms": {
            "field": "tags.use_case.keyword",
            "size": 10
          },
          "aggs": {
            "author_by_use_case": {
              "terms": {
                "field": "authors.full_name.keyword",
                "size": 1
              }
            }
          }
        }
      }
    }
  }
} 
```

```
GET web_traffic/_search
{
  "size": 0,
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "@timestamp": {
              "gte": "2021-04-01",
              "lt": "2021-04-01||+1w"
            }
          }
        }
      ]
    }
  },
  "aggs": {
    "top_blogs": {
      "terms": {
        "field": "url.original",
        "size": 3
      }
    }
  }
}
```

## グループ化して唯一だったドキュメントを探す
cardinalityを使う。  
[https://www.elastic.co/guide/en/elasticsearch/reference/8.4/search-aggregations-metrics-cardinality-aggregation.html](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/search-aggregations-metrics-cardinality-aggregation.html)
```
GET blogs/_search
{
  "size": 0,
  "aggs": {
    "NAME": {
      "cardinality": {
        "field": "authors.uid.keyword"
      }
    }
  }
}
```

## sortを使った検索
```
GET blogs/_search
{
  "size": 3,
  "query": {
    "match_phrase": {
      "content": "elastic stack"
    }
  },
  "sort": [
    {
      "authors.last_name": "asc"
    },
    "publish_date",
    "_doc"
  ],
  "search_after": [
    "Anderson",
    1574794800000,
    1991
  ],
  "highlight": {
    "fields": {
      "content" : {}
    },
    "pre_tags": ["<mark>"],
    "post_tags": ["</mark>"]
  }
}
```

# フィールドが存在するか確認する
```
GET blogs_fixed3/_search
{
  "query": {
    "exists": {
      "field": "reindexBatch"
    }
  }
}
```

# search template
テンプレートを作る
```
PUT _scripts/weekly_top_blogs
{
  "script": {
    "lang": "mustache",
    "source": {
      "size": 0,
      "query": {
        "bool": {
          "filter": [
            {
              "range": {
                "@timestamp": {
                  "gte": "{{start_date}}",
                  "lt": "{{start_date}}||+1w"
                }
              }
            }
          ]
        }
      },
      "aggs": {
        "top_blogs": {
          "terms": {
            "field": "url.original",
            "size": "{{top_n}}"
          }
        }
      }
    }
  }
}
```
テンプレートを使った検索
```
GET web_traffic/_search/template
{
  "id": "weekly_top_blogs",
  "params": {
    "start_date": "2021-04-01",
    "top_n": 3
  }
}
```
別のテンプレート
```
PUT _scripts/top_blogs
{
  "script": {
    "lang": "mustache",
    "source": {
      "size": 0,
      "query": {
        "bool": {
          "filter": [
            {
              "range": {
                "@timestamp": {
                  "gte": "{{start_date}}",
                  "lt": "{{end_date}}{{^end_date}}{{start_date}}||+1w{{/end_date}}"
                }
              }
            }
          ]
        }
      },
      "aggs": {
        "top_blogs": {
          "terms": {
            "field": "url.original",
            "size": "{{top_n}}"
          }
        }
      }
    }
  }
}
```

# cluster
## clusterを指定して検索
```
GET cluster2:blogs/_search
```

## 複数のclusterを跨いで検索
```
GET blogs,cluster2:blogs/_search
{
  "query": {
    "match_phrase": {
      "content": "kibana query language"
    }
  }
}
```

# alias
```
POST _aliases
{
  "actions": [
    {
      "add": {
        "index": "blogs_fixed3",
        "alias": "elastic_blogs"
      }
    }
  ]
}
```

# async_search
## 検索結果を待たず、非同期で検索
通常の検索だと、検索結果が返ってくるのに時間がかかりすぎると、タイムアウトになりエラーが返ってくることがある。
```
POST blogs/_async_search
{
  "query": {
    "function_score": {
      "query": {
        "match_all": {}
      },
      "script_score": {
        "script": """
        int m = 30; 
        for (int x = 0; x < m; ++x) 
          for (int y = 0; y < 10000; ++y) 
            Math.log(y);
        """
      }
    }
  }
}
GET _async_search/<your_search_ID_here>
DELETE _async_search/<your_search_ID_here>
```

# script
スクリプト言語はpainless。  
matchの部分はscriptで書く。  
[https://www.elastic.co/guide/en/elasticsearch/reference/master/modules-scripting-painless.html](https://www.elastic.co/guide/en/elasticsearch/reference/master/modules-scripting-painless.html)
```
GET blogs/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "script": {
            "script": """
              return true;
            """
          }
        }
      ]
    }
  }
}
```
```
GET blogs/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "script": {
            "script": """
              return doc['url'].value.length() >= 100;
            """
          }
        }
      ]
    }
  }
}
```
```
GET blogs/_search
{
  "_source": "authors", 
  "query": {
    "bool": {
      "filter": [
        {
          "script": {
            "script": """
              if(doc['authors.last_name.keyword'].size() > 0 && 
                doc['authors.last_name.keyword'].value.startsWith("K"))
                return true;
            """
          }
        }
      ]
    }
  }
}

GET blogs/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "exists": {
            "field": "authors.last_name.keyword"
          }
        }
      ], 
      "filter": [
        {
          "script": {
            "script": """
              return doc['authors.last_name.keyword'].value.startsWith("K");
            """
          }
        }
      ]
    }
  }
}
```
```
GET blogs/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "script": {
            "script": """
              return doc['tags.product.keyword'].size() >= 3;
            """
          }
        }
      ]
    }
  }
}
```

# mapping
## mappingの作成
### TIPS
まずは適当にインデックスを作成して自動のmapppingを作ってもらう。  
それをフォーマット扱いして、必要な部分を自分で修正するのが効率的。  
1. とりあえず自動でmappingを作ってもらう（雛形を作る）
```
POST test_blog/_doc
{
  "@timestamp": "2021-03-10T16:00:00.000Z",
  "abstract": "The Joy of Painting",
  "url": "/blog/happy-little-trees",
  "published": true
}
```
2. mappingを取得する
```
GET test_blog/_mapping
```
3. 仮で作ったindexを削除する（要らないので）
```
DELETE test_blog
```
4. mappingを修正して、修正したmappingをつけてindexを作成する
```
PUT test_blog
{
  "mappings" : {
    "properties" : {
      "@timestamp" : {
        "type" : "date"
      },
      "abstract" : {
        "type" : "text"
      },
      "author" : {
        "type" : "text",
        "fields" : {
          "keyword" : {
            "type" : "keyword",
            "ignore_above" : 256
          }
        }
      },
      "body" : {
        "type" : "text"
      },
      "body_word_count" : {
        "type" : "long"
      },
      "category" : {
        "type" : "text"
      },
      "published" : {
        "type" : "boolean"
      },
      "title" : {
        "type" : "text"
      },
      "url" : {
        "type" : "text",
        "fields" : {
          "keyword" : {
            "type" : "keyword",
            "ignore_above" : 256
          }
        }
      }
    }
  }
}
```

## 既存indexのmappingを変更したい時
### reindex (local to local)
index:blogsの中身のドキュメントを、index:blogs_fixedに投入する
```
POST _reindex
{
  "source": {
    "index": "blogs"
  },
  "dest": {
    "index": "blogs_fixed"
  }
}
```

### reindex (remote to local)
```
POST _reindex
{
  "source": {
    "remote": {
      "host": "https://server4:9204",
      "username": "hogehoge",
      "password": "piyopiyo"
    },
    "index": "blogs"
  },
  "dest": {
    "index": "blogs_fixed3"
  }
}
```

## reindexせずにmappingを変更
### mappingの編集はできないが追加はできる
追加分のmappingをPUTしただけでは、既存のdocumentが追加されたmappingでindexされていないので、下記のコマンドでアップデート分の再indexが必要
1. 追加分のmappingをPUT
```
PUT blogs_fixed3/_mapping
{
  "properties": {
      "content" : {
        "type" : "text",
        "analyzer": "content_analyzer",
        "fields": {
          "eng": {
            "type": "text",
            "analyzer": "english"
          },
          "de": {
            "type": "text",
            "analyzer": "german"
          }
        }
      }
  }
}
```
2. 追加分のmappingを既存documentに反映
```
POST blogs_fixed3/_update_by_query?wait_for_completion=false
```
既存の前ドキュメントに追加したマッピングに対応するフィールドを追加。   
追加する際に初期値を入れる場合は以下。
```
POST blogs_fixed3/_update_by_query
{
  "script": {
    "source": "ctx._source['reindexBatch'] = 1;"
  }
}
```

# 検索時に新規項目を生成する(runtimeフィールド)
## scriptで新規fieldを生成して、そのfieldを使って検索をする場合
```
GET blogs_fixed2/_search
{
  "size": 0,
  "runtime_mappings": {
    "day_of_week": {
      "type": "keyword",
      "script": {
        "source": "emit(doc['publish_date'].value.dayOfWeekEnum.getDisplayName(TextStyle.FULL, Locale.ROOT))"
      }
    }
  },
  "aggs": {
    "top_days": {
      "terms": {
        "field": "day_of_week"
      }
    }
  }
}
```

## 一時的にmappingを変更して、変更されたmappingのfieldを使って検索する場合
```
GET blogs_fixed2/_search
{
  "size": 0,
  "aggs": {
    "top_uids": {
      "terms": {
        "field": "authors.uid"
      }
    }
  }
}

... no result

GET blogs_fixed2/_search
{
  "size": 0,
  "runtime_mappings": {
    "authors.uid": {
      "type": "keyword"
    }
  },
  "aggs": {
    "top_uids": {
      "terms": {
        "field": "authors.uid"
      }
    }
  }
}
```

# analyzer
## analyzerのテスト
テスト的に自分で入れたテキストを指定のアナライザーで同パースされるか確認。
```
GET blogs/_analyze
{
  "analyzer": "standard",
  "text":     "<b>Is</b> this <a href=\"/blogs/\">clean</a> text?"
}
```

## analyzerを指定してindexを作成する
全体のアナライザーの指定に加え、一部のフィールドに個別のアナライザーを追加。
```
PUT blogs_fixed3
{
  "settings": {
    "analysis": {
      "analyzer": {
        "content_analyzer": {
          "tokenizer": "standard",
          "filter": ["lowercase"],
          "char_filter": ["html_strip"]
        }
      }
    }
  }, 
  "mappings" : {
    "_meta" : {
        "created_by" : "Elastic Student"
      },
    "properties" : {
      "@timestamp" : {
        "type" : "date"
      },
      "authors" : {
        "properties" : {
          "company" : {
            "type" : "text",
            "fields" : {
              "keyword" : {
                "type" : "keyword",
                "ignore_above" : 256,
                "index": false
              }
            }
          },
          "first_name" : {
            "type" : "keyword"
          },
          "full_name" : {
            "type" : "text"
          },
          "job_title" : {
            "type" : "text",
            "fields" : {
              "keyword" : {
                "type" : "keyword",
                "ignore_above" : 256,
                "index": false
              }
            }
          },
          "last_name" : {
            "type" : "keyword"
          },
          "uid" : {
            "enabled": false
          }
        }
      },
      "category" : {
        "type" : "keyword"
      },
      "content" : {
        "type" : "text",
        "analyzer": "content_analyzer",
        "fields": {
          "eng": {
            "type": "text",
            "analyzer": "english"
          }
        }
      },
      "locale" : {
        "type" : "keyword"
      },
      "publish_date" : {
        "type" : "date",
        "format" : "iso8601"
      },
      "search_tags": {
        "type": "text"
      },
      "tags" : {
        "properties" : {
          "elastic_stack" : {
            "type" : "keyword",
            "copy_to": "search_tags"
          },
          "industry" : {
            "type" : "keyword",
            "copy_to": "search_tags"
          },
          "level" : {
            "type" : "keyword",
            "copy_to": "search_tags"
          },
          "product" : {
            "type" : "keyword",
            "copy_to": "search_tags"
          },
          "tags" : {
            "type" : "keyword",
            "copy_to": "search_tags"
          },
          "topic" : {
            "type" : "keyword",
            "copy_to": "search_tags"
          },
          "use_case" : {
            "type" : "keyword",
            "copy_to": "search_tags"
          },
          "use_cases" : {
            "type" : "keyword",
            "copy_to": "search_tags"
          }
        }
      },
      "title" : {
        "type" : "text",
        "fields": {
          "search": {
            "type": "search_as_you_type"
          }
        }
      },
      "url" : {
        "type" : "keyword"
      }
    }
  }
}
```
作成したインデックスのアナライザーをテスト
```
GET blogs_fixed3/_analyze
{
  "analyzer": "content_analyzer",
  "text":     "<b>Is</b> this <a href=\"/blogs/\">clean</a> text?"
}
```

# type
## search_as_you_type
あくまでtypeの一つ。textやkeywordと同列の概念。  
search_as_you_typeというタイプでfiledを作ると、自動で._2gramtと._3gramがつくfieldも作られる。
[https://www.elastic.co/guide/en/elasticsearch/reference/master/search-as-you-type.html](https://www.elastic.co/guide/en/elasticsearch/reference/master/search-as-you-type.html)
### mapping
```
...
      "title": {
        "type": "text",
        "fields": {
          "search": {
            "type": "search_as_you_type"
          }
        }
      },
...
```

### 検索方法
```
GET blogs_fixed3/_search
{
  "query": {
    "multi_match": {
      "query": "logs",
      "type": "bool_prefix",
      "fields": [
        "title.search",
        "title.search._2gram",
        "title.search._3gram"
      ]
    }
  }
}
```

# 既存index内のdocumentを変更
reindexするしかない。  
reindexする前に、pipelineを仕込んで、前処理させてからドキュメントを投入。
1. pipelinenの作成（UIのStack Management > Ingest Node Pipelinesからクエリを作成）
2. pipelineとmappingを指定した空のindexを作成
pipeline名は1の時に決める。  
mappingは変更する前の値で作成。
3. reindex後は前のindexを削除
```
PUT web_traffic
{
  "settings": {
    "default_pipeline": "web_traffic_pipeline",
    "number_of_shards": 10, 
    "number_of_replicas": 0            
  },
  "mappings": {
    "properties": {
      "@timestamp": {
        "type": "date"
      },
      "geo": {
        "properties": {
          "location": {
            "type": "geo_point"
          }
        }
      },
      "http": {
        "properties": {
          "request": {
            "properties": {
              "method": {
                "type": "keyword"
              }
            }
          },
          "response": {
            "properties": {
              "status_code": {
                "type": "keyword"
              }
            }
          }
        }
      },
      "runtime_ms": {
        "type": "long"
      },
      "url": {
        "properties": {
          "original": {
            "type": "keyword",
            "fields": {
              "text": {
                "type": "text"
              }
            }
          }
        }
      },
      "user_agent": {
        "properties": {
          "device": {
            "properties": {
              "name": {
                "type": "keyword"
              }
            }
          },
          "name": {
            "type": "keyword"
          },
          "original": {
            "type": "keyword",
            "fields": {
              "text": {
                "type": "text"
              }
            }
          },
          "version": {
            "type": "keyword"
          }
        }
      }
    }
  }
}
```

# 投入するデータを前処理させてindexに投入
「既存インデックス内のドキュメントの中身を変更」する時と同じ。  
pipelineを作成し、pipelineを指定したインデックスを作成。
その後にデータを投入する。

# Transform, Pipeline, Policy
概念の抽象度でいうと、Transform > Pipeline > Policy
## Policy
policyの作成
```
PUT _enrich/policy/webtraffic_policy
{
  "match": {
    "indices": "traffic_stats",
    "match_field": "url.original",
    "enrich_fields": ["@timestamp.value_count","runtime_ms.avg"]
  }
}
```
policyの実行。  
内部的にはpolicyを適用するのではなく、policyからenrichy用のシステムインデックスを作成して、そのシステムインデックスを既存のインデックスに適用する形になる。  
適用するときはpipeline経由。  
pipelineで指定するときも、システムインデックス名ではなくpolicy名で指定するので、実際に設定するときにはシステムインデックはそこまで意識しなくても実装できる。
```
POST _enrich/policy/webtraffic_policy/_execute
```

## Pipeline
policyはなくても使える。  
1. pipelineの準備
```
PUT _ingest/pipeline/webtraffic_transform_pipeline
{
  "processors": [
    {
      "enrich": {
        "policy_name": "webtraffic_policy",
        "field": "url",
        "target_field": "traffic_stats"
      }
    }
  ]
}
```
2. indexへのpipelineの適用
```
POST blogs_fixed3/_update_by_query?pipeline=webtraffic_transform_pipeline&wait_for_completion=false
```

## Transform
クエリの最適化のためのサマリテーブル作成用の仕組み。  
どれくらいの頻度でサマリテーブルを更新するかは決められる。   
作成と実行。Transformは基本GUIで準備する方が良さそう。  
あとライセンスがないと実行できない機能っぽいので、トライアルが切れたらクエリでも使えなくなりそう。  
```
PUT _transform/traffic_stats
{
  "source": {
    "index": [
      "web_traffic"
    ]
  },
  "pivot": {
    "group_by": {
      "url.original": {
        "terms": {
          "field": "url.original"
        }
      }
    },
    "aggregations": {
      "@timestamp.value_count": {
        "value_count": {
          "field": "@timestamp"
        }
      },
      "runtime_ms.avg": {
        "avg": {
          "field": "runtime_ms"
        }
      }
    }
  },
  "frequency": "1m",
  "dest": {
    "index": "traffic_stats"
  },
  "settings": {
    "max_page_search_size": 500
  }
}
```

# document追加時に自動でindexが新規作成されるのを防ぐ
以下のdocumentを追加するとき
```
POST dynamic_test/_doc
{
  "my_field": "Will this document be indexed?"
}
```
自動インデックス生成のオフ
```
PUT _cluster/settings
{
  "persistent": {
    "action.auto_create_index" : false
  }
}
```
```
PUT _cluster/settings
{
  "persistent": {
    "action.auto_create_index" : "dynamic_test_*"
  }
}
```
Response
```
{
  "error" : {
    "root_cause" : [
      {
        "type" : "index_not_found_exception",
        "reason" : "no such index [dynamic_test2] and [action.auto_create_index] is [false]",
        "index_uuid" : "_na_",
        "index" : "dynamic_test2"
      }
    ],
    "type" : "index_not_found_exception",
    "reason" : "no such index [dynamic_test2] and [action.auto_create_index] is [false]",
    "index_uuid" : "_na_",
    "index" : "dynamic_test2"
  },
  "status" : 404
}
```

# Python
```
from elasticsearch import Elasticsearch

  client = Elasticsearch(
      hosts=['server1:9201'],
      http_auth=('training', 'nonprodpwd'),
      scheme="https",
      use_ssl=True,
      verify_certs=False,
      ssl_show_warn=False
  )
```
```
def search():
    query = request.args.get('query')
    # 2. Define search query here...
    body = {
        "query": {
            "multi_match": {
                "query": query,
                "fields": ["content", "title"]
            }
        }
    }
```
```
response = client.search(index='blogs_fixed3', body=body)
```
```
return render_template('results.html', results=response['hits']['hits'])
```
```
python3 -m flask run --host=0.0.0.0
```