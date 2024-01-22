---
title:  Loki日志系统部署
date: 2024-01-22
categories:
- Loki
tags: 
- Loki
- 日志
---

# Loki日志系统部署

## 介绍

Loki 是一个由Grafana Labs 开发的开源日志聚合系统，旨在为云原生架构提供高效的日志处理解决方案。

Loki 通过使用类似 Prometheus 的标签索引机制来存储和查询日志数据，这使得它能够快速地进行分布式查询和聚合，而不需要将所有数据都从存储中加载到内存中。Loki还使用了压缩和切割日志数据的方法来减少存储空间的占用，从而更好地适应云原生环境下的高速增长的日志数据量。

Loki的架构由以下几个主要组件组成：

Promtail: 负责采集应用程序和系统的日志数据，并将其发送到 Loki 的集群中。

Loki:负责存储日志数据，提供 HTTP API 的日志查询，以及数据过滤和筛选。

Grafana: 负责 UI 展示日志数据。

![](https://idoc.longfor.com/editor-server/doc/wv8hvfx7fKLtretf8xlxFxgVPX32/resources/QWrGGiegzDOUDtTBfVgPRg_xhLXE7I3EBTOqYeCIRV0.png?token=W.uENCf76j57K93JLeVg7Vy_oIx-cu8UjgQs1YiJw5MRaJ-20)

## Loki对比ELK

### 优点：

*   存储资源占用小：Loki 可以通过压缩和切割日志数据的方法来控制磁盘占用。  



*   内存资源占用小：Loki 使用标签索引机制存储和查询日志数据，不需要将所有数据都从存储中加载到内存中。



*   部署简单：Loki的架构简单，仅部署三个组件即可使用，是一个轻量级的日志聚合系统。



*   资源可复用：在日志的可视化上可以使用 Grafana，可以和Prometheus 监控共用，节省系统资源。

### 缺点：

*   系统较新，不如ELK社区活跃。



*   ELK有强大的可视化功能，Loki的可视化功能较为简单。



*   ELK 可以连用各种技术进行日志的大数据处理，但是 loki 不行。



*   ELK是分布式系统，可以通过增加节点来提高扩展性，Loki是单节点系统。

结合中交项目的特点，故选择Loki日志系统。

# Loki部署示例

## 下载组件

Loki下载地址：<https://github.com/grafana/loki/releases>

Promtail下载地址：<https://github.com/grafana/loki/releases>

Grafana下载地址：<https://grafana.com/grafana/download?edition=oss>

## 安装loki

解压loki-linux-amd64.zip到安装目录

打开安装目录，创建配置文件loki-config.yml，配置文件参考

```
auth\_enabled: false

server:

  http\_listen\_port: 3100

common:

  ring:

   instance\_addr: 127.0.0.1

   kvstore:

     store: inmemory

  replication\_factor: 1

  path\_prefix: /data/loki

schema\_config:

  configs:

  - from: 2020-05-15

   store: boltdb-shipper

   object\_store: filesystem

   schema: v11

   index:

     prefix: index\_

     period: 24h

limits\_config:

  reject\_old\_samples: true

  reject\_old\_samples\_max\_age: 168h

chunk\_store\_config:

  # 最大可查询历史日期 90天

  max\_look\_back\_period: 2160h

\# 表的保留期90天  

table\_manager:

  retention\_deletes\_enabled: true

  retention\_period: 2160h
```

启动Loki

```
nohup ./loki-linux-amd64 -config.file=/data/loki/loki-config.yml &
```

## 安装Promtail

解压promtail-linux-amd64.zip到安装目录

打开安装目录，创建配置文件config-promtail.yml，配置文件参考，clients-url修改为Loki的安装地址

```
server:

  http\_listen\_port: 9080

  grpc\_listen\_port: 0

positions:

  filename: /tmp/positions.yaml # This location needs to be writeable by Promtail.

clients:

  - url: <http://10.12.100.83:3100/loki/api/v1/push>

scrape\_configs:

 - job\_name: system

  pipeline\_stages:

  static\_configs:

  - targets:

      - localhost

    labels:

      env: dev

      job: applogs  # A \`job\` label is fairly standard in prometheus and useful for linking metrics and logs.

      host: 10.12.100.85 # A \`host\` label will help identify logs from this machine vs others

      \_\_path\_\_: /data/log/\*.log  # The path matching uses a third party library: <https://github.com/bmatcuk/doublestar>
```

启动promtail

```
nohup ./promtail-linux-amd64 -config.file=/data/promtail/config-promtail.yml &
```

## 安装Grafana

```
yum install -y grafana-10.2.0-1.x86\_64.rpm
```

## Grafana 添加数据源

左侧导航栏选择Connections-Data sources并新建

![](https://idoc.longfor.com/editor-server/doc/wv8hvfx7fKLtretf8xlxFxgVPX32/resources/aJlbWL95yL-j1ukaKctmy7-jO6rWYYQ4QLCPmythvDI.png?token=W.uENCf76j57K93JLeVg7Vy_oIx-cu8UjgQs1YiJw5MRaJ-20)

选择Loki并创建

![](https://idoc.longfor.com/editor-server/doc/wv8hvfx7fKLtretf8xlxFxgVPX32/resources/UK-5UnNSy2TB6kPPAbCVOdhL813h4bhilIYFDZ1cfs8.png?token=W.uENCf76j57K93JLeVg7Vy_oIx-cu8UjgQs1YiJw5MRaJ-20)

Connection-url填入Loki地址及参数

![](https://idoc.longfor.com/editor-server/doc/wv8hvfx7fKLtretf8xlxFxgVPX32/resources/WViU0bM4d-5WUTHH1IrRpUSpDpFVTzcr2FGFjKudPwQ.png?token=W.uENCf76j57K93JLeVg7Vy_oIx-cu8UjgQs1YiJw5MRaJ-20)

点击Save & Test，提示Data source successfully connected即代表成功。

![](https://idoc.longfor.com/editor-server/doc/wv8hvfx7fKLtretf8xlxFxgVPX32/resources/xJVNyKEsYHPg4ozUJa9Tc0VtIeomko-is2JcMb2iNvM.png?token=W.uENCf76j57K93JLeVg7Vy_oIx-cu8UjgQs1YiJw5MRaJ-20)

# 使用Loki查看日志

点击左侧Explore

![](https://idoc.longfor.com/editor-server/doc/wv8hvfx7fKLtretf8xlxFxgVPX32/resources/fyMXXC2tebqtWHJG74JxnmOvoy7v50iucJbjg8Ng6lY.png?token=W.uENCf76j57K93JLeVg7Vy_oIx-cu8UjgQs1YiJw5MRaJ-20)

最上面下拉选择刚才创建好的Loki数据源，再选择自定义的label标签，点击Run Query即可看到日志

![](https://idoc.longfor.com/editor-server/doc/wv8hvfx7fKLtretf8xlxFxgVPX32/resources/0uQzLDPqVv6WzizB_hvmmmzzTTd4Le91mNFUa5zbDZE.png?token=W.uENCf76j57K93JLeVg7Vy_oIx-cu8UjgQs1YiJw5MRaJ-20)

查看上下文方法，点击某行日志的的Show context按钮即可

![](https://idoc.longfor.com/editor-server/doc/wv8hvfx7fKLtretf8xlxFxgVPX32/resources/TmulczEolSOguvyrMun66wMLL1Ji_cQebLefA9P9g2k.png?token=W.uENCf76j57K93JLeVg7Vy_oIx-cu8UjgQs1YiJw5MRaJ-20)

# 参考文档

1.  [Loki官方部署文档](https://grafana.com/docs/loki/latest/get-started/)



1.  [Loki配置文件说明](https://grafana.com/docs/loki/latest/configure/)



1.  [Promtail配置文件说明](https://grafana.com/docs/loki/latest/send-data/promtail/configuration/)



1.  [日志收集系统loki部署](https://blog.51cto.com/u_15315026/3206956)

