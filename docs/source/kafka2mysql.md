#### Kafka 2 Mysql

kafka原始消息格式为：
```json
{"index": 1, "name": "n_1"}
```

###### batch
```yaml
env {
  # 配置SeaTunnel环境
  execution.parallelism = 1
  job.mode = "BATCH"
  checkpoint.interval = 10000
  #execution.checkpoint.interval = 10000
  #execution.checkpoint.data-uri = "hdfs://localhost:9000/checkpoint"
}

source {
  Kafka {
    #result_table_name = "kafka_name"
    schema = {
      fields {
        index = "int"
        name = "string"
      }
    }
    format = json
    #field_delimiter = "#"
    topic = "mytopic1"
    bootstrap.servers = "localhost:9092"
    kafka.config = {
      client.id = client_1
      auto.offset.reset = "latest"
    }
  }
}

sink {
  jdbc {
        url = "jdbc:mysql://localhost:3306/dev"
        driver = "com.mysql.cj.jdbc.Driver"
        user = "root"
        query = "insert into users(`index`,`name`) values(?,?)"
        }
}
```

##### streaming
```yaml
env {
  # You can set flink configuration here
  execution.parallelism = 1
  job.mode = "STREAMING"
  checkpoint.interval = 2000
  #execution.checkpoint.interval = 10000
  #execution.checkpoint.data-uri = "hdfs://localhost:9000/checkpoint"
}

source {
  Kafka {
    #result_table_name = "kafka_name"
    schema = {
      fields {
        index = "int"
        name = "string"
      }
    }
    format = json
    #field_delimiter = "#"
    topic = "mytopic1"
    bootstrap.servers = "localhost:9092"
    kafka.config = {
      client.id = client_1
      auto.offset.reset = "latest"
    }
  }
}

sink {
  jdbc {
        url = "jdbc:mysql://localhost:3306/dev"
        driver = "com.mysql.cj.jdbc.Driver"
        user = "root"
        query = "insert into users(`index`,`name`) values(?,?)"
        }
}
```
kafka消息同步到mysql，batch和steaming模式的基本写法是一致的，特殊差异请参考文档。