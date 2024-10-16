---
title: 利用私有gitlab项目自动部署网站
categories:
- [WEB,Hexo]
tags:
- [Hexo]
- [Gitlab]
date: 2023-11-21 20:00:00
updated: 2023-11-21 20:00:00
---



## 配置CI/CD,每次提交项目后自动生成网站文件并发布

> 前篇https://www.szpzhy.com/2023/11/21/hexo_install_users/
>
> vim .gitlab-ci.yml
```bash
image: node:20.9.0

cache:
  paths:
    - node_modules/

before_script:
    - cd ${CI_PROJECT_DIR}
    - npm install
pages:
  tags:
    - newren250
  script:
    - cd ${CI_PROJECT_DIR}
    - hexo clean
    - hexo generate
    - sudo cp -rf ${CI_PROJECT_DIR}/public/* /data/www/szpzhycom/
    - sudo chown -R www-data:www-data /data/www/
  artifacts:
    paths:
      - public
  only:
    - master

```



## 提交项目

> 在通过私有Gitlab提交项目自动部署并发布网站步骤中已经配置好了git

```bash
git add .
git commit -m "update project"
git push --set-upstream gitww master
```



## 查看CI/CD构建过程

> 进入私有的gitlab网站hexoblog项目页面,可以看到文件已经更新
>
> 在构建--流水线里面可以查看是否构建成功.
>
> 构建成功后即可输入网站网址查看新的页面啦!

### 后续网站更新

> 以后就可以在工作站的电脑上，git clone项目
>
> 在source/_post/文件夹下面新建Markdown文档并提交
>
> 就可以通过私有gitlab的CI/CD自动构建并发布到网站了。
>
> Git部分命令[Git使用命令 - 王大爷爱学习 (szpzhy.com)](https://szpzhy.com/2023/11/22/git_users/)
