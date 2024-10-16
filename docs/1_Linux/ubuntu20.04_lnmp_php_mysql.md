---
title: ubuntu20.04-架设WEB服务器-php-mysql
categories:
- [Linux,LNMP]
tags:
- [ubuntu]
- [LNMP]
date: 2023-11-18 18:00:00
updated: 2023-11-18 18:00:00
---

# Ubuntu20.04 架设WEB服务器-php-mysql



- [ubuntu20.04-架设Gitlab服务器-外部nginx](https://szpzhy.com/2023/11/17/ubuntu20.04_gitlab_nginx/)



## 安装php

```bash
sudo apt install php7.4-fpm php7.4-amqp php7.4-apcu php7.4-bcmath php7.4-bz2 php7.4-cgi php7.4-cli php7.4-common php7.4-curl php7.4-dba php7.4-ds php7.4-enchant php7.4-gd php7.4-gearman php7.4-gmagick php7.4-gmp php7.4-gnupg php7.4-http php7.4-igbinary php7.4-imap php7.4-interbase php7.4-intl php7.4-ldap php7.4-mailparse php7.4-mbstring php7.4-memcache php7.4-memcached php7.4-mongodb php7.4-msgpack php7.4-mysql php7.4-oauth php7.4-odbc php7.4-opcache php7.4-pcov php7.4-pgsql php7.4-phpdbg php7.4-ps php7.4-pspell php7.4-psr php7.4-raphf php7.4-readline php7.4-redis php7.4-rrd php7.4-snmp php7.4-soap php7.4-solr php7.4-sqlite3 php7.4-ssh2 php7.4-sybase php7.4-tidy php7.4-uuid php7.4-xdebug php7.4-xml php7.4-xmlrpc php7.4-xsl php7.4-yaml php7.4-zip php7.4-zmq
```



## 修改配置文件

```bash
sudo cp /usr/lib/php/7.4/php.ini-production /usr/lib/php/7.4/php.ini
sudo vim /usr/lib/php/7.4/php.ini
cgi.fix_pathinfo=0
upload_max_filesize = 20M
date.timezone = Asia/Shanghai
```



## 重启php服务

```bash
sudo systemctl enable php7.4-fpm
sudo systemctl start php7.4-fpm
sudo systemctl status php7.4-fpm
```



## 测试php

```bash
sudo vim /data/www/szpzhycom/i.php
<?php
phpinfo();
?>

```



## 安装mariadb

```bash
sudo apt install -y mariadb-server
```



## 修改配置文件并更改data目录



### 1. 创建data目录

```bash
sudo mkdir -p /data/mariadb
```



### 2. 查找配置文件并修改

```bash
sudo grep -R --color /var/lib/mysql /etc/mysql/*
sudo vim /etc/mysql/mariadb.conf.d/50-server.cnf
datadir                 = /data/mariadb

sudo vim /etc/apparmor.d/tunables/alias
alias /var/lib/mysql/ -> /data/mariadb/,
```



### 3. 同步data到新目录并设置权限

```bash
sudo rsync -avph /var/lib/mysql/ /data/mariadb/
sudo chown -R mysql:mysql /data/mariadb/
sudo systemctl restart apparmor
```



## 重启mysql服务

```bash
sudo systemctl enable mariadb
sudo systemctl start mariadb
sudo systemctl status mariadb
```



## 更改root密码

```bash
sudo mysql -uroot
use mysql;
select host,user,plugin from user;
alter user 'root'@'localhost' identified by'password';
select host,user,plugin from user;
flush privileges;
exit
```

