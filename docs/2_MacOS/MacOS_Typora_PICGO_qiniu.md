---
title: MacOS13下Typora搭配PICGO APP使用七牛做图床设置
categories:
- [MacOS,Tools]
- [Tools,MarkDown]
tags:
- [MacOS]
- [MarkDown]
date: 2023-2-12 18:00:00
updated: 2023-2-12 18:00:00
---
# MacOS下Typora搭配PICGO APP使用七牛做图床设置



## 1.1 安装Typora、PicGo、注册七牛云

https://typora.io/

https://molunerfinn.com/PicGo/

https://www.qiniu.com/products/kodo

Typora和PicGo App安装教程省略

先看一下PicGo图床设置中需要的参数

![alt](http://qn.newren.cn/markdown/image-20230212134243179.png)

Akey和Skey在右上角

![image-20230212141151171](https://qn.newren.cn/markdown/image-20230212141151171.png)

![image-20230212141249535](https://qn.newren.cn/markdown/image-20230212141249535.png)

七牛云(对象存储 Kodo)注册过程

> https://developer.qiniu.com/kodo/4263/operational-guidelines-for-the-new-bucket

bucket为你创建的存储空间名称

网址注意不要选CDN加速的域名

七牛云存储区域(注意你的存储空间是在哪个区域)

> https://developer.qiniu.com/kodo/1671/region-endpoint-fq

存储路径为你创建的文件夹名字,后面记得加"/"

## 1.2 Typora中的设置

![image-20230212141756834](https://qn.newren.cn/markdown/image-20230212141756834.png)

## 1.3 设置完成

现在在Tytora中粘贴图片后就会自动上传并显示了,快去试一下吧!



