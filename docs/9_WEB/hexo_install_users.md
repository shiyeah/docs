---
title: 通过私有Gitlab提交项目自动部署并发布网站
categories:
- [WEB,Hexo]
tags:
- [Hexo]
date: 2023-11-21 18:00:00
updated: 2023-11-21 18:00:00
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).



# 通过私有Gitlab提交项目自动部署并发布网站

> 已经配置好了[nginx环境](https://szpzhy.com/2023/11/17/ubuntu20.04_gitlab_nginx/)，网站配置详见链接（可忽略第二步中的gitlab网站配置)



## 1. 在Ubuntu20.04中安装Hexo

> 可以通过gitlab-runner自动安装或通过SSH手动安装(已经在要部署的机器上安装好gitlab-runer)
>
> 下面步骤1是在要部署的机器上SSH手动安装
### 1.1 安装Node.js

```bash
curl -SLO https://deb.nodesource.com/nsolid_setup_deb.sh
chmod 500 nsolid_setup_deb.sh
./nsolid_setup_deb.sh 20
sudo apt-get install nodejs -y
```

### 1.2 安装Git

```bash
sudo apt-get install git
```

### 1.3 安装Hexo

```bash
sudo npm install -g hexo-cli
sudo npm install hexo
```



## 2. 初始化Hexo项目

> 前置条件是配置好ssh,参考[Git使用命令 - 王大爷爱学习 (szpzhy.com)](https://szpzhy.com/2023/11/22/git_users/)
>
> 步骤2是在要部署的机器上SSH手动安装

### 2.1 通过git命令自动创建项目

```bash
git config --global user.name "Administrator"
git config --global user.email "wangwei9@me.com"
git config --global --list

git init hexoblog
cd hexoblog
npm install

#新的项目名称,不能是已存在或更名的已有项目!新建的项目是否公开,由网站上项目设置来决定!
git remote add gitww ssh://git@git.szpzhy.com:newren/hexoblog.git
git add .
git commit -m "Create project"
git push --set-upstream gitww master
```

### 2.2 初始化网站并安装主题

> [Hexo Fluid主题 - 王大爷爱学习 (szpzhy.com)](https://szpzhy.com/2023/11/22/hexo_fluid_theme/)
