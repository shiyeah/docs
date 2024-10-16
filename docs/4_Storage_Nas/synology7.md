---
title: synology7
categories:
- [Nas,Synology]
tags:
- [Synology7]
date: 2023-11-23 18:00:00
updated: 2023-11-23 18:00:00
---

# Synology 7



## 免责声明 - Disclaimer

- 硬盘有价，数据无价，任何对引导的修改都是有风险的，本人不承担数据丢失的责任。
- 本工具仅用作学习交流，严禁用于商业用途。



## 安装条件

1. 引导盘要大于 2GB.
2. 安装盘要大于 32GB.
3. 内存需要大于 4GB.
4. DT的型号（kver 4.4）目前不支持HBA扩展卡.



## 引导下载地址
- 相关链接
	- 说明手册:https://github.com/wjz304/rr/blob/main/guide.md
	- Github项目地址:https://github.com/wjz304/rr
	- 版本下载地址:https://github.com/wjz304/rr/releases/
	- 当前稳定版本:https://github.com/wjz304/rr/releases/tag/23.11.7
	- 下载链接:https://github.com/wjz304/rr/releases/download/23.11.7/rr-23.11.7.img.zip
	- 群晖机型配置:https://kb.synology.com/en-me/DSM/tutorial/What_kind_of_CPU_does_my_NAS_have



## 制作镜像引导

1. 将解压得到的IMG文件,使用Rufus写入U盘

2. 使用UEFI引导,选择型号,根据向导完成引导U盘设置.



## 安装Synology7

- 下载:
  - https://archive.synology.cn/download/Os/DSM
  - https://archive.synology.com/download/Os/DSM
  根据向导完成安装

## 其他

1. 如果是虚拟机安装,需要安装VMTOOLS,下载地址
	- https://github.com/tbc0309/synology-dsm-open-vm-tools
2. 关于洗白
	- 万能的宝购买半洗白序列号
	- 注册美区群晖帐号
	- 在群晖里登陆美区帐号
	- 下载安装解码软件,并显示HEVC版本
	- 不要使用QuickConnect
	- https://wp.gxnas.com/
	-	https://dl.gxnas.com:1443/
