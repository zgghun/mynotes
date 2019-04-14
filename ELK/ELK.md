# ELK 日志采集

> NOTE: 以下为Windows环境上的示范

## 1. 安装软件

### 安装 [Elasticsearch](https://www.elastic.co/guide/en/elastic-stack-get-started/6.5/get-started-elastic-stack.html)

1. 下载 [Elasticsearch 6.5.4 Windows zip](https://www.elastic.co/downloads/elasticsearch) 
2. 解压文文件到任意目录：  
如 `D:\devinstall\elasticsearch-6.5.4`  
3. 修改 D:\devinstall\elasticsearch-6.5.4\config\elasticsearch.yml 配置文件，在最后添加 xpack.ml.enabled: false
4. 运行，在解压目录下打开 cmd 窗口  
执行：bin\elasticsearch.bat
5. 检查是否安装成功  
浏览器打开：<http://127.0.0.1:9200>  
正常返回：

    ```json
    {
        "name" : "QtI5dUu",
        "cluster_name" : "elasticsearch",
        "cluster_uuid" : "DMXhqzzjTGqEtDlkaMOzlA",
        "version" : {
            "number" : "6.5.4",
            "build_flavor" : "default",
            "build_type" : "tar",
            "build_hash" : "00d8bc1",
            "build_date" : "2018-06-06T16:48:02.249996Z",
            "build_snapshot" : false,
            "lucene_version" : "7.3.1",
            "minimum_wire_compatibility_version" : "5.6.0",
            "minimum_index_compatibility_version" : "5.0.0"
        },
        "tagline" : "You Know, for Search"
    }
    ```

### 安装 Kibana

1. 下载 [Kibana 6.5.4 Windows zip](https://www.elastic.co/downloads/kibana)
2. 解压到任意目录  
如 `D:\devinstall\kibana-6.5.4-windows-x86_64`
3. 运行，在解压目录下打开 cmd 窗口  
执行：bin\kibana.bat
4. 检查是否启动  
浏览器打开：<http://127.0.0.1:5601>

### 安装 [Logstash](https://www.elastic.co/guide/en/logstash/current/index.html)

> NOTE：java版本要求1.8，不支持1.9及以上

1. 下载 [Logstash installation file](https://www.elastic.co/downloads/logstash)
2. 解压到任意目录  
如 `D:\devinstall\logstash-6.5.4`
3. 添加配置文件。在Logstash目录下新建一个logstash.conf配置文件，输入以下内容：

    ```json
    input {
        beats {
            port => "5044"
        }
    }
    filter {
        grok {
            # 这里利用grok对输入的日志进行解析，请根据各自需求配置，
            match => { "message" => "%{TIMESTAMP_ISO8601:timestamp} %{LOGLEVEL:level} --- \[%{GREEDYDATA:thread}\] %{NOTSPACE:class} : %{GREEDYDATA:loginfo}"}
        }
        # 用日志的时间替换 logstash 时间
        date {
            match => ["timestamp", "yyyy-MM-dd HH:mm:ss.SSS"]
            target => "@timestamp"
        }
    }
    output {
        elasticsearch {
            hosts => ["localhost:9200"]
        }
    }
    ```

    * 可以执行 `bin/logstash -f first-pipeline.conf --config.test_and_exit` 检测配置文件是否正确
    * 对于grok的正则匹配，可以在 Kibana > Dev Tools > Grok Debugger 中测试
    * 多输入源、多输出源配置 参考 [http://blog.51cto.com/tchuairen/1840596](http://blog.51cto.com/tchuairen/1840596)
4. 启动  
运行 `bin/logstash -f first-pipeline.conf --config.reload.automatic`  
`--config.reload.automatic` 参数可自动检测配置文件变化，而无须重启logstash

### 安装 Filebeat

1. 下载 [Filebeat Windows zip](https://www.elastic.co/downloads/beats/filebeat)
2. 解压到 `C:\Program`
3. 重命名 `filebeat-<version>-windows` 为 `Filebeat`
4. 把filebeat注册为系统服务：以管理员身份打开 PowerShell，并执行

    ```bash
    PS > cd 'C:\Program Files\Filebeat'
    PS > PowerShell.exe -ExecutionPolicy UnRestricted -File .\install-service-filebeat.ps1
    ```

5. 打开 Filebeat 目录下的 filebeat.yml，按以下修改.

    ```json   
    filebeat.inputs:
    - type: log
        enabled: true
        paths:
        - d:\log\zcb-api.log
        #- c:\programdata\elasticsearch\logs\*
        # multiline是用于合并多行日志，可以把java异常堆栈信息合并
        multiline.pattern: ^[[:space:]]
        multiline.negate: false
        multiline.match: after
    output.logstash:
        # The Logstash hosts
        hosts: ["localhost:5044"]    
    ```

   参考文档 <https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-log.html>
6. 启动 Filebeat（或者在 任务管理器>服务>filebeat 启动）
    `PS C:\Program Files\Filebeat> Start-Service filebeat`
