---
title: ubuntu20.04-架设CS1.6服务器
categories:
- [Linux,GameServer]
tags:
- [ubuntu]
- [HLDS]
date: 2023-11-13 18:00:00
updated: 2023-11-13 18:00:00
---

# Ubuntu20.04 架设CS1.6服务器



- [以ubuntu20.04-Mininal安装后系统配置篇为基础](https://szpzhy.com/2023/11/12/ubuntu20.04_system_setup/)



## 安装关联软件及Steam

```bash
sudo add-apt-repository multiverse
sudo dpkg --add-architecture i386
sudo apt update
sudo apt -y upgrade
sudo apt install -y steamcmd libsdl2-2.0-0:i386
```



## 下载HLDS服务端

```bash
steamcmd  +login anonymous +app_update 90 validate
```
中间如果出错,可以多执行几次以确保服务端文件完整
```bash
app_update 90 validate
```



## 运行前准备

```bash
mkdir ~/.steam/sdk32/

cp -rp ~/.steam/SteamApps/common/Half-Life/steamclient.so ~/.steam/sdk32/
cd ~/.steam/SteamApps/common/Half-Life/

cp steamclient.so steamservice.so

touch cstrike/listip.cfg
touch cstrike/banned.cfg

```



## 运行服务器

```bash
./hlds_run -console -game cstrike +servercfgfile server.cfg -pingboost 3 -port 27015 +maxplayers 31 +map de_dust2 +ip 192.168.12.250
```