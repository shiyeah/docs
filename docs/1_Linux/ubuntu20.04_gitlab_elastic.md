---
title: ubuntu20.04-架设Gitlab服务器-elasticsearch
categories:
- [Linux,Gitlab]
tags:
- [ubuntu]
- [Gitlab]
date: 2023-11-16 18:00:00
updated: 2023-11-16 18:00:00
---

# Ubuntu20.04 架设Gitlab服务器-elasticsearch



- [ubuntu20.04-架设Gitlab服务器-Licese篇为基础](https://szpzhy.com/2023/11/16/ubuntu20.04_gitlab_elastic/)



## 下载安装Gitlab-elasticsearch

```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.14-amd64.deb
sudo dpkg -i elasticsearch-7.17.14-amd64.deb
```



## 修改配置文件

```bash
sudo vim /etc/elasticsearch/elasticsearch.yml
xpack.security.enabled: true
```



## 启动服务

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now elasticsearch.service
sudo systemctl status elasticsearch.service
```



## 配置elasticsearch

###  1. 生成elasticsearch密码

```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-setup-passwords auto

Changed password for user apm_system
PASSWORD apm_system = MKYL2e5fxEByku8ECoOl

Changed password for user kibana_system
PASSWORD kibana_system = yqEfjgBDWCsJoW0RQtS0

Changed password for user kibana
PASSWORD kibana = yqEfjgBDWCsJoW0RQtS0

Changed password for user logstash_system
PASSWORD logstash_system = I4M55eHzpaL6rLttC8iC

Changed password for user beats_system
PASSWORD beats_system = QGgLAE7e4ONXO0vMLFZt

Changed password for user remote_monitoring_user
PASSWORD remote_monitoring_user = CDy4AhrqBFO37ZuyxMxV

Changed password for user elastic
PASSWORD elastic = E3gbE4CZYzwSQoFbl3Xx

```



### 2. 登陆Gitlab并设置

- 复制上一步生成的用户elastic和密码
- 登陆Gitlab进入管理界面(网址后面加/admin)
  - 菜单->管理员->设置->通用->高级搜索
  - [x] Elasticsearch 索引
  - [x] 启用 Elasticsearch 的搜索
  - 配置用户名和密码

## 其他

### 重建空索引
```bash
sudo gitlab-rake gitlab:elastic:index
```

### 设置elasticsearch内存占用
```bash
sudo vim /etc/elasticsearch/jvm.options
 -Xms4g
 -Xmx4g
sudo systemctl stop elasticsearch.service
sudo systemctl start elasticsearch.service
```
