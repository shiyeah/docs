---
title: Git使用命令
categories:
- [Linux,Tools]
- [Tools,Git]
tags:
- [Git]
author: Root
date: 2023-11-22 18:00:00
updated: 2023-11-24 12:00:00
---

## GIT使用命令



### Git 全局设置

```bash
git config --global user.name "Administrator"
git config --global user.email "wangwei9@me.com"
git config --global --list

#Git提示“warning: LF will be replaced by CRLF”的解决办法
git config --global core.autocrlf true
```


### 前提

私有项目,提前配置好GIT服务端SSH公钥和本地的私钥.



### 创建一个新仓库

```bash
git clone ssh://git@git.szpzhy.com:newren/hexoblog.git
cd blog
git switch --create master
touch README.md
git add README.md
git commit -m "add README"
git push --set-upstream origin master
```


### 推送现有文件夹

```bash
cd existing_folder
git init
git remote add origin ssh://git@git.szpzhy.com:newren/hexoblog.git
git add .
git commit -m "Initial commit"
git push --set-upstream origin master
```


### 推送现有的 Git 仓库

```bash
cd existing_repo
git remote rename origin old-origin
git remote add origin ssh://git@git.szpzhy.com:newren/hexoblog.git
git push --set-upstream origin --all
git push --set-upstream origin --tags
```



### 配置忽略文件

```bash
vim .gitignore
.DS_Store
Thumbs.db
db.json
package-lock.json
*.log
node_modules/
public/
.deploy*/
_multiconfig.yml
```



### 其他

错误1

```bash
To git.szpzhy.com:root/blog.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'ssh://git@git.szpzhy.com:newren/hexoblog.git'
```
错误1解决方法
```bash
git pull --rebase origin master
git push -u origin master
```



错误2

```bash
git push -u origin master
error: src refspec master does not match any
error: failed to push some refs to 'ssh://gitee.com:awp46/hexoblog.git'
```

错误2原因:

> Gitee项目里的部署公钥只能pull和clone,不能push

错误2解决方法:

> 需要在Gitee个人设置里面添加公钥,而不是在项目里面添加部署公钥


SSH非默认22端口
>原项目地址: 
>`git@git.szpzhy.com:root/hexoblog.git`


>修改后地址
>`ssh://git@git.szpzhy.com:7250/root/hexoblog.git`



## 同一个本地文件夹推送到多个不同的Git仓库

```bash
cd existing_folder
git init
#添加远端仓库1
git remote add gitww ssh://git@git.szpzhy.com:newren/hexoblog.git
#添加远端仓库2
git remote add gitee git@gitee.com:awp46/hexoblog.git

git add .
git commit -m "Initial commit"

#查看更改内容
git show

#查看
git remote -v
#gitww   ssh://git@git.szpzhy.com:newren/hexoblog.git (fetch)
#gitww   ssh://git@git.szpzhy.com:newren/hexoblog.git (push)
#origin  git@gitee.com:awp46/hexoblog.git (fetch)
#origin  git@gitee.com:awp46/hexoblog.git (push)
#如果已有其他仓库,对其进行改名操作
#git remote rename origin gitee

#查看分支
git branch -a
#* main	只有main分支

#创建并切换分支master
git checkout -b master

#切换分支master
#git checkout master

#再次查看分支
#git branch -a
#  main
#* master	已经切换到master分支

#分别推送不同的仓库
git push --set-upstream gitww master
git push --set-upstream gitee master
#报错误1
#git push --set-upstream -u -f gitee master
#如果继续报错误1
#进入网站项目设置,取消保护分支
#git push --set-upstream -u -f gitee master
```



## 不需要在Gitlab网站上新建项目,通过本地Git命令直接新建

```bash
mkdir -p basepack
cd basepack
git init
#新的项目名称,不能是已存在或更名的已有项目!新建的项目是否公开,由网站上项目设置来决定!
git remote add gitww ssh://git@git.szpzhy.com:newren/basepack.git
git add .
git commit -m "Create project"
git push --set-upstream gitww master
```



> 其他项目地址备忘录(私有项目)

```bash
ssh://git@git.szpzhy.com:newren/hexoblog.git
ssh://git@git.szpzhy.com:newren/hlds_pack.git

ssh://git@gitee.com:awp46/hexoblog.git
ssh://git@gitee.com:awp46/hlds_pack.git
```

