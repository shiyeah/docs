---
title: ubuntu20.04-架设Gitlab服务器-外部nginx
categories:
- [Linux,Gitlab]
- [Linux,LNMP]
tags:
- [ubuntu]
- [Gitlab]
- [LNMP]
date: 2023-11-17 18:00:00
updated: 2023-11-17 18:00:00
---

# Ubuntu20.04 架设Gitlab服务器-外部nginx



- [ubuntu20.04-架设Gitlab服务器篇为基础](https://szpzhy.com/2023/11/14/ubuntu20.04_gitlab/)



## 安装nginx

```bash
sudo apt -y install nginx
```



## 修改配置文件

```bash
sudo vim /etc/gitlab/gitlab.rb
 web_server['external_users'] = ['www-data']
 nginx['enable'] = false

#设置自动备份路径和保留时间
 gitlab_rails['backup_path'] = "/data/bak/"
 gitlab_rails['backup_keep_time'] = 604800

#注释掉之前设置的
# nginx['ssl_certificate'] = "/data/nginx/ssl/git.szpzhy.com.pem"
# nginx['ssl_certificate_key'] = "/data/nginx/ssl/git.szpzhy.com.key"
# nginx['custom_nginx_config'] = "include /data/nginx/vhosts/*.conf;"
```



## 重启Gitlab服务

```bash
sudo gitlab-ctl stop
sudo gitlab-ctl reconfigure
sudo gitlab-ctl restart
```



## 配置Nginx

###  1. 修改Nginx主配置文件

```bash
sudo mkdir -p /data/nginx/vhosts/
sudo vim /etc/nginx/nginx.conf

user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 1024;
        # multi_accept on;
}

http {

        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
        server_tokens off;

        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;
        ssl_session_tickets off;
        ssl_session_timeout  5m;
        ssl_session_cache shared:SSL:10m;
        ssl_ciphers  HIGH:!aNULL:!MD5;


        ##
        # Logging Settings
        ##
        log_format main '$remote_addr - "$http_user_agent" $remote_user [$time_local] "$request" $status $body_bytes_sent "$http_referer" "$http_x_forwarded_for"';

        access_log /var/log/nginx/access.log main;
        error_log /var/log/nginx/error.log;

        ##
        # Gzip Settings
        ##

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /data/nginx/vhosts/*.conf;
}

```



### 2. 修改nginx_gitlab配置文件

```bash
sudo rm -rf /data/nginx/vhosts/default
sudo vim /data/nginx/vhosts/gitszpzhy.conf
upstream gitlab-workhorse {
  server unix:/var/opt/gitlab/gitlab-workhorse/sockets/socket fail_timeout=0;
}

map $http_upgrade $connection_upgrade_gitlab_ssl {
    default upgrade;
    ''      close;
}

log_format gitlab_ssl_access '$remote_addr - "$http_user_agent" $remote_user [$time_local] "$request_method $gitlab_ssl_filtered_request_uri $server_protocol" $status $body_bytes_sent "$gitlab_ssl_filtered_http_referer"';

map $request_uri $gitlab_ssl_temp_request_uri_1 {
  default $request_uri;
  ~(?i)^(?<start>.*)(?<temp>[\?&]private[\-_]token)=[^&]*(?<rest>.*)$ "$start$temp=[FILTERED]$rest";
}
map $gitlab_ssl_temp_request_uri_1 $gitlab_ssl_temp_request_uri_2 {
  default $gitlab_ssl_temp_request_uri_1;
  ~(?i)^(?<start>.*)(?<temp>[\?&]authenticity[\-_]token)=[^&]*(?<rest>.*)$ "$start$temp=[FILTERED]$rest";
}
map $gitlab_ssl_temp_request_uri_2 $gitlab_ssl_filtered_request_uri {
  default $gitlab_ssl_temp_request_uri_2;
  ~(?i)^(?<start>.*)(?<temp>[\?&]feed[\-_]token)=[^&]*(?<rest>.*)$ "$start$temp=[FILTERED]$rest";
}
map $http_referer $gitlab_ssl_filtered_http_referer {
  default $http_referer;
  ~^(?<temp>.*)\? $temp;
}
server {
  listen 80;
  server_name git.szpzhy.com;
  return 301 https://$http_host$request_uri;
}

server {
  listen 443 ssl http2;

  server_name git.szpzhy.com;
  ssl_certificate /data/nginx/ssl/git.szpzhy.com.pem;
  ssl_certificate_key /data/nginx/ssl/git.szpzhy.com.key;

  server_name sz.sinnen.top;
  ssl_certificate /etc/nginx/ssl/sz.sinnen.top.pem;
  ssl_certificate_key /etc/nginx/ssl/sz.sinnen.top.key;

  server_tokens off;

  real_ip_header X-Real-IP;
  real_ip_recursive off;

  access_log  /var/log/nginx/gitlab_access.log gitlab_ssl_access;
  error_log   /var/log/nginx/gitlab_error.log;


  location / {
    client_max_body_size 0;
    gzip off;

    proxy_read_timeout      300;
    proxy_connect_timeout   300;
    proxy_redirect          off;

    proxy_http_version 1.1;

    proxy_set_header    Host                $http_host;
    proxy_set_header    X-Real-IP           $remote_addr;
    proxy_set_header    X-Forwarded-Ssl     on;
    proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
    proxy_set_header    X-Forwarded-Proto   $scheme;
    proxy_set_header    Upgrade             $http_upgrade;
    proxy_set_header    Connection          $connection_upgrade_gitlab_ssl;

    proxy_pass http://gitlab-workhorse;
    
  }

  error_page 404 /404.html;
  error_page 422 /422.html;
  error_page 500 /500.html;
  error_page 502 /502.html;
  error_page 503 /503.html;
  location ~ ^/(404|422|500|502|503)\.html$ {
    root /opt/gitlab/embedded/service/gitlab-rails/public;
    internal;
  }
}
```



### 3. 测试配置文件并启动nginx服务

```bash
sudo nginx -t

sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl status nginx
```



### 4. 配置主网站

```bash
sudo mkdir -p /data/www/szpzhycom
sudo vim /data/nginx/vhosts/szpzhycom.conf
server
{ 
        listen 443 ssl http2;
        server_name szpzhy.com www.szpzhy.com;
        ssl_certificate /data/nginx/ssl/szpzhy.com.pem;
        ssl_certificate_key /data/nginx/ssl/szpzhy.com.key;

        charset utf-8;

        location / {
                root /data/www/szpzhycom;
                index  index.html index.htm index.php;
                try_files $uri $uri/ /index.php?$args ;

        location ~ \.php$ {
                fastcgi_pass   unix:/run/php/php-fpm.sock;
                fastcgi_index  index.php;
                fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
                fastcgi_intercept_errors on;
                fastcgi_buffers 16 16k;
                fastcgi_buffer_size 32k;
                include        fastcgi_params;
        }
        }
}

sudo chown -R www-data:www-data /data/www/
```



### 5. 重新加载配置

```bash
sudo nginx -t

sudo nginx -s reload
```

