---
title: ubuntu20.04-架设Gitlab服务器
categories:
- [Linux,Gitlab]
tags:
- [ubuntu]
- [Gitlab]
date: 2023-11-14 18:00:00
updated: 2023-11-14 18:00:00
---

# Ubuntu20.04 架设Gitlab服务器



- [以ubuntu20.04-Mininal安装后系统配置篇为基础](https://szpzhy.com/2023/11/12/ubuntu20.04_system_setup/)



## 安装关联软件

```bash
cd ~
sudo apt-get install -y postfix
```



## 下载安装Gitlab服务端

```bash
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
sudo apt-get install gitlab-ee
```



## 修改配置文件

提前准备好网址的SSL证书并复制到`/data/nginx/vhosts/`

```bash
sudo mkdir -p /data/nginx/vhosts/
sudo vim /etc/gitlab/gitlab.rb
 external_url 'https://git.szpzhy.com'
 gitlab_rails['time_zone'] = 'Asia/Shanghai'
 
 gitlab_rails['smtp_enable'] = true
 gitlab_rails['smtp_address'] = "smtp.exmail.qq.com"
 gitlab_rails['smtp_port'] = 465
 gitlab_rails['smtp_user_name'] = "admin@szpzhy.com"
 gitlab_rails['smtp_password'] = "exmail_password"
 gitlab_rails['smtp_domain'] = "szpzhy.com"
 gitlab_rails['smtp_authentication'] = "login"
 gitlab_rails['smtp_enable_starttls_auto'] = false
 gitlab_rails['smtp_tls'] = true
 gitlab_rails['gitlab_email_enabled'] = true
 gitlab_rails['gitlab_email_from'] = 'admin@szpzhy.com'
 gitlab_rails['gitlab_email_display_name'] = 'Gitlab_git.szpzhy.com'


 git_data_dirs({
   "default" => {
     "path" => "/data/gitdata"
    }
 })

 nginx['ssl_certificate'] = "/data/nginx/ssl/git.szpzhy.com.pem"
 nginx['ssl_certificate_key'] = "/data/nginx/ssl/git.szpzhy.com.key"
 
 
 /etc/nginx/ssl/sz.sinnen.top.pem
 /etc/nginx/ssl/sz.sinnen.top.key
```



## 运行服务器

```bash
sudo gitlab-ctl reconfigure
sudo gitlab-ctl restart
```



## 查看默认密码

```bash
sudo cat /etc/gitlab/initial_root_password
Password: eVelqbzSV2apBwUtq3InP1he6vas49vWyYqjTLupStc=
Password: SeC3jplcDARYzlXdHH0LjZPie3m2Jz7nbiCBL4jfSaY=
```



## 登陆修改密码和邮箱

- 现在可以打开网址,并以上面默认生成的密码登陆
- 修改密码和邮箱



## 使用Gitlab自带的nginx加载其他网站

- 不推荐使用.推荐使用后装的NGINX代替自带的NGINX

```bash
sudo vim /etc/gitlab/gitlab.rb
 nginx['custom_nginx_config'] = "include /data/nginx/vhosts/*.conf;"
```



## 邮箱发送邮件测试

```bash
sudo gitlab-rails console
Notify.test_email('test@yourmail.com', '邮件主题', '邮件内容').deliver_now
```



