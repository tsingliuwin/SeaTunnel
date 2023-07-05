#### 介绍Config
在SeaTunnel，Config文件非常重要，用户可以最大化地定制他们的数据同步方案。所以，接下来，我们将介绍如何配置Config文件。

Config文件最重要的格式是`hocon`，更多介绍可以参考[HOCON-GUIDE](https://github.com/lightbend/config/blob/main/HOCON.md)。同时，SeaTunnel还支持`json`格式，但是config文件命名需要以`.json`结尾。

#### hocon格式
```sh
env {
  job.mode = "BATCH"
}

source {
  FakeSource {
    result_table_name = "fake"
    row.num = 100
    schema = {
      fields {
        name = "string"
        age = "int"
        card = "int"
      }
    }
  }
}

transform {
  Filter {
    source_table_name = "fake"
    result_table_name = "fake1"
    fields = [name, card]
  }
}

sink {
  Clickhouse {
    host = "clickhouse:8123"
    database = "default"
    table = "seatunnel_console"
    fields = ["name", "card"]
    username = "default"
    password = ""
    source_table_name = "fake1"
  }
}
```
#### json格式
```sh

{
  "env": {
    "job.mode": "batch"
  },
  "source": [
    {
      "plugin_name": "FakeSource",
      "result_table_name": "fake",
      "row.num": 100,
      "schema": {
        "fields": {
          "name": "string",
          "age": "int",
          "card": "int"
        }
      }
    }
  ],
  "transform": [
    {
      "plugin_name": "Filter",
      "source_table_name": "fake",
      "result_table_name": "fake1",
      "fields": ["name", "card"]
    }
  ],
  "sink": [
    {
      "plugin_name": "Clickhouse",
      "host": "clickhouse:8123",
      "database": "default",
      "table": "seatunnel_console",
      "fields": ["name", "card"],
      "username": "default",
      "password": "",
      "source_table_name": "fake1"
    }
  ]
}
```
#### Env
环境配置

* job.name

任务名

* jars

使用三方`jars`包，比如：`jars="file://local/jar1.jar;file://local/jar2.jar"`

* job.mode

指定任务是批模式还是流模式，`job.mode = "BATCH"`为批模式，`job.mode = "STREAMING"`为流模式

* checkpoint.interval

* parallelism

并发数

* shade.identifier

#### Source
定义SeaTunnel从哪里获取数据。支持同时配置多个源，每个源都有特有的参数用于定义如何获取数据。

此处以`FakeSource`数据源为例：

* FakeSource

虚拟数据源，可以根据用户定义的数据结构随机生成数据，仅用于测试场景。