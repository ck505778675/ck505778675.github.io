---
title: 使用elasticsearch-dump备份和还原elasticsearch
date: 2019-08-06 23:20:10
author: 测试呢称
top: true
categories: 后端
tags:
  - elasticsearch
  - elasticsearch-dump
  - docker
---

# 本文主要介绍使用docker版的elasticsearch-dump迁移elasticsearch数据

## 准备Docker镜像
``` bash
$ docker pull taskrabbit/elasticsearch-dump
```

## 使用文件备份和还原数据
### 保存文件备份
``` bash
$ docker run --net=host --rm -ti -v /tmp:/tmp taskrabbit/elasticsearch-dump \
  --input=http://localhost:9200/my_index \
  --output=/tmp/my_index_mapping.json \
  --type=mapping

$ docker run --net=host --rm -ti -v /tmp:/tmp taskrabbit/elasticsearch-dump \
  --input=http://localhost:9200/my_index \
  --output=/tmp/my_index_data.json \
  --type=data
```
### 从文件备份中还原数据
``` bash
$ docker run --net=host --rm -ti -v /tmp:/tmp taskrabbit/elasticsearch-dump \
  --input=/tmp/my_index_mapping.json \
  --output=http://localhost:9200/my_index \
  --type=mapping

$ docker run --net=host --rm -ti -v /tmp:/tmp taskrabbit/elasticsearch-dump \
  --input=/tmp/my_index_data.json \
  --output=http://localhost:9200/my_index \
  --type=data
```
### 将数据直接迁移到另外一个实例
``` bash
$ docker run  --rm -ti  taskrabbit/elasticsearch-dump \
  --input=http://production.es.com:9200/my_index \
  --output=http://newnodes.es.com:9200/my_index \
  --type=mapping
$ docker run --rm -ti elasticdump \
  --input=http://production.es.com:9200/my_index \
  --output=http://newnodes.es.com:9200/my_index \
  --type=data
```
参数说明：
--net=host  将容器的网络配置设置与宿主机一样，可以通过localhost:9200来访问本机的elasticsearch 
-v /tmp:/tmp 设置备份文件的挂载目录


参考文章：
[使用Docker备份和还原ElasticSearch数据库](https://khanhicetea.com/post/backup-and-restore-elasticsearch-using-docker/)