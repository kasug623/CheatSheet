# jsonファイルをLogstashで取り込みたい

取り込みを試すときはsystemctl でやるよりも、
binで.configを指定して実行した方がやりやすい。
systemctlだといちいちjournalctlで見る必要があり、面倒。

設定ファイルをちゃんと見れてないのか、
設定内容がよくなくて、取り込み対象のファイルを無視しているのかがわかりにくい。詰まったときは大体後者。

・binで実行するとき
```
sudo /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/hogehoge.conf
```

・systemctlで実行する
```
systemctl start logstash.service

# logの確認
journalctl -u logstash.service -xe -n 100
```

## 複数レコードある.json
### 取り込める形
#### .json(改行 \n なし)
```
[{"name":"toda","age":24},{"name":"shimoda","age":28},{"name":"kasuya","age":34}]
```
取り込みやすい形。

#### .conf
```
input {
    file {
        path =>[/data/hogehoge.json],
        sincedb_path => "/dev/null"
        start_position => "beginning"
        codec => "json"
    }
}

filter {

}

output {
    elasticsearch {
        hosts => ["http://X.X.X.X:9200"]
        index => "hogeIndex"
    }
}
```

### 取り込めない形1
#### .json(改行 \n あり)
```
[
    {
        "name":"toda",
        "age":24
    },
    {
        "name":"shimoda",
        "age":28
    },
    {
        "name":"kasuya",
        "age":34
    }
]
```

#### 出力
```
例）"message" : "}",
　　"message" : """ "name":"toda",""",
```

\nがあるため、取り込めない。
一行ずつのレコードができる。
それぞれのレコードのmessageに、一行ごとのこまごまになった値が入る。

取り込もうと思えばできるけど、単純な設定ではできないため、
細かくパースする必要がある。
入れる前にjqコマンドなどで一行の.jsonにした方が楽そう。
```
jq --compact-output
```

### 取り込めない形2
#### .json(改行 \n あり)
```
{
    "name":"toda",
    "age":24
},
{
    "name":"shimoda",
    "age":28
},
{
    "name":"kasuya",
    "age":34
}
```

無理


## レコード一つだけの.json
### 取り込めない形1
#### .json(改行 \n あり)

```
{
    "name":"toda",
    "age":24
}
```
無理

このconfでもダメ
#### .conf
```
input {
    file {
        path =>[/data/hogehoge.json],
        sincedb_path => "/dev/null"
        start_position => "beginning"
        codec => "json"
    }
}

filter {

}

output {
    elasticsearch {
        hosts => ["http://X.X.X.X:9200"]
        index => "hogeIndex"
    }
}
```

#### 出力
例) "message" : """ "name":"tanaka",""",


### 取り込めない形2
#### .json(改行 \n あり)
```
{
    "name":"toda",
    "age":24
}
```
無理

このconfでもダメ
#### .conf
```
input {
    file {
        path =>[/data/hogehoge.json],
        sincedb_path => "/dev/null"
        start_position => "beginning"
    }
}

filter {
    json {
        source => "message"
    }
}

output {
    elasticsearch {
        hosts => ["http://X.X.X.X:9200"]
        index => "hogeIndex"
    }
}
```

#### 出力
例) "message" : """ "name":"tanaka",""",


### 取り込める形1
#### .json
```
{"name":"tanaka","age":"23"}
```
#### 方法1
.conf
```
input {
    file {
        path =>[/data/hogehoge.json],
        sincedb_path => "/dev/null"
        start_position => "beginning"
    }
}

filter {
    json {
        source => "message"
    }
}

output {
    elasticsearch {
        hosts => ["http://X.X.X.X:9200"]
        index => "hogeIndex"
    }
}
```

出力
```
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "test",
        "_type" : "_doc",
        "_id" : "XK9uP3sBW11KQnuEGznB",
        "_score" : 1.0,
        "_source" : {
          "host" : "socuser-virtual-machine",
          "@version" : "1",
          "path" : "/data/d.json",
          "name" : "tanaka",
          "message" : """{"name":"tanaka","age":"23"}""",
          "age" : "23",
          "@timestamp" : "2021-08-13T12:13:05.646Z"
        }
      }
    ]
  }
}
```
この方法の場合は、.confのfilterでmessageフィールドをremoveする必要がある。

### 取り込める形2
#### .json
```
{"name":"tanaka","age":"23"}
```
#### .conf
```
input {
    file {
        path =>[/data/hogehoge.json],
        sincedb_path => "/dev/null"
        start_position => "beginning"
        codec => "json"
    }
}

filter {

}

output {
    elasticsearch {
        hosts => ["http://X.X.X.X:9200"]
        index => "hogeIndex"
    }
}
```

#### 出力
```
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "test",
        "_type" : "_doc",
        "_id" : "Xa9xP3sBW11KQnuE-Tml",
        "_score" : 1.0,
        "_source" : {
          "host" : "socuser-virtual-machine",
          "age" : "23",
          "@timestamp" : "2021-08-13T12:17:19.067Z",
          "@version" : "1",
          "path" : "/data/hogehoge.json",
          "name" : "tanaka"
        }
      }
    ]
  }
}
```
