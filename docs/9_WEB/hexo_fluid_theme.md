---
title: Hexo Fluid主题
categories:
- [WEB,Hexo]
tags:
- [Hexo]
date: 2023-11-21 19:00:00
updated: 2023-11-21 19:00:00
---



# 安装Fluid主题

> 前篇https://www.szpzhy.com/2023/11/21/hexo_install_users/

> 主题网址https://hexo.fluid-dev.com



## 安装主题

> 以下操作均在博客根目录下面进行(cd hexoblog/)

```bash
npm install --save hexo-theme-fluid
```

> 创建`_config.fluid.yml`

> 复制主题的`[_config.yml](https://github.com/fluid-dev/hexo-theme-fluid/blob/master/_config.yml)`内容进去.



## 设置网站

> 修改`_config.yml`

```bash
language: zh-CN
timezone: Asia/Shanghai
theme: fluid

url: https://www.szpzhy.com
```

> 创建[关于页面]

```bash
hexo new page about
vim /source/about/index.md
---
title: 关于我们
layout: about
---
```



## 主题更新

```bash
npm update --save hexo-theme-fluid
```



# 初始化网站

```
hexo generate
```



## 浏览网站

> 复制public目录里面的内容到网站根目录

```bash
sudo cp -rf public/* /data/www/szpzhycom/
```

> 打开https://www.szpzhy.com即可浏览,可以看到系统已经自动生成了HelloWorld页面
>
> [Hello World]: https://szpzhy.com/2023/11/21/hello-world/
