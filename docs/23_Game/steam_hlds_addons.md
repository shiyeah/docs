---
title: CS1.6服务器插件安装
categories:
- [Linux,GameServer]
tags:
- [ubuntu]
- [HLDS]
- [AMXX]
- [CS1.6]
date: 2023-11-13 18:00:00
updated: 2023-11-13 18:00:00
---



# CS1.6服务器插件安装

> [原版HLDS CS1.6服务器安装](https://szpzhy.com/2023/11/13/ubuntu20.04_steam/) 安装的版本为8684（最新版9890发布了，暂未测试）

> 将原版的steamcmd\steamapps\common\Half-Life文件夹复制出来
>
> 在工作站上新建Hlds_pack文件夹



## 1. 插件下载

### 主要插件

- [AMX Mod X](https://www.amxmodx.org/downloads-new.php?branch=master&all=1) (   使用的版本[1.10.0.5467])
- [ReHLDS](https://github.com/dreamstalker/rehlds/) (   使用的版本[3.13.0.788])
- [ReGameDLL](https://github.com/s1lentq/ReGameDLL_CS) (   使用的版本[5.22.0.593])
- [Reunion](https://cs.rin.ru/forum/viewtopic.php?f=29&t=69235) (   使用的版本[0.1.0.137])
- [Metamod-r](https://github.com/theAsmodai/metamod-r) (   使用的版本[1.3.0.131])
- [VoiceTranscoder](https://github.com/WPMGPRoSToTeMa/VoiceTranscoder) (   使用的版本[2017 RC5])
- [Reapi](https://github.com/s1lentq/reapi) (   使用的版本[5.22.0.254])

### 自定义插件

可在此文件内开启或关闭 **cstrike/addons/metamod/plugins.ini**.

- [ReAuthChecker](https://dev-cs.ru/resources/63/) (  使用的版本[0.1.6])
- [ReChecker](https://dev-cs.ru/resources/72/) (  使用的版本[2.7])
- [ReSemiclip](https://dev-cs.ru/resources/71/) (  使用的版本[2.4.3])
- [WHBlocker](https://dev-cs.ru/resources/76/) (  使用的版本[1.5.697])

> 下载地址:
```html
https://qn.newren.cn/hlds_files/yapb-4.3.734-windows.zip
https://qn.newren.cn/hlds_files/whblocker_1_5_697.zip
https://qn.newren.cn/hlds_files/VoiceTranscoder_2017RC5.zip
https://qn.newren.cn/hlds_files/steamcmd.zip
https://qn.newren.cn/hlds_files/reunion_0.1.0.92d.zip
https://qn.newren.cn/hlds_files/resemiclip-2.4.3.zip
https://qn.newren.cn/hlds_files/rehlds-bin-3.13.0.788.zip
https://qn.newren.cn/hlds_files/regamedll-bin-5.22.0.593.zip
https://qn.newren.cn/hlds_files/rechecker_2_7.zip
https://qn.newren.cn/hlds_files/reauthcheck_0.1.6.rar
https://qn.newren.cn/hlds_files/reapi-bin-5.22.0.254.zip
https://qn.newren.cn/hlds_files/reaimdetector_0.2.2.rar
https://qn.newren.cn/hlds_files/metamod-bin-1.3.0.131.zip
https://qn.newren.cn/hlds_files/hackdetector_0.15.328.lite.zip
https://qn.newren.cn/hlds_files/amxmodx-1.10.0-git5467-cstrike-windows.zip
https://qn.newren.cn/hlds_files/amxmodx-1.10.0-git5467-cstrike-linux.tar.gz
https://qn.newren.cn/hlds_files/amxmodx-1.10.0-git5467-base-windows.zip
https://qn.newren.cn/hlds_files/amxmodx-1.10.0-git5467-base-linux.tar.gz
```



## 2. 解压插件并复制主要插件

> 根据平台新建windows或linux文件夹,这里以windows演示

### 2.1 解压插件

> 解压为压缩包名,不要直接解压到当前文件夹,解压完如图

<img src="https://qn.newren.cn/markdown/image-20231125040249440.png" align="left" alt="image-20231125040249440" style="zoom: 67%;" />

> 在windows文件夹内新建cstrike文件夹

### 2.2 REHLDS

> 复制rehlds-bin-3.13.0.788\bin\win32内所有文件到windows文件夹内

### 2.3 Metamod

> 复制metamod-bin-1.3.0.131\addons文件夹到windows\cstrike文件夹内

### 2.4 REGAMEEDLL

> 复制regamedll-bin-5.22.0.593\bin\win32\cstrike内所有文件到windows\cstrike文件夹内

### 2.5 REUNION

> 在windows\cstrike\addons里面新建reunion文件夹
> 复制reunion_0.1.0.92d\bin\Windows\reunion_mm.dll到windows\cstrike\addons\reunion文件夹内
> 复制reunion_0.1.0.92d\reunion.cfg文件到windows\cstrike文件夹内

### 2.6 VoiceTranscode

> 复制VoiceTranscoder_2017RC5\addons\VoiceTranscoder文件夹到windows\cstrike\addons文件夹内

### 2.7 Amxmodx

> 复制amxmodx-1.10.0-git5467-base-windows\addons\amxmodx文件夹到windows\cstrike\addons文件夹内
> 复制amxmodx-1.10.0-git5467-cstrike-windows\addons\amxmodx文件夹到windows\cstrike\addons文件夹，提示已存在，选择全部替换

### 2.8 REAPI

> 复制reapi-bin-5.22.0.254\addons\amxmodx文件夹到windows\cstrike\addons文件夹内



## 3. 复制其他插件

### 3.1 Reauthcheck

> 复制reauthcheck_0.1.6\windows\addons\reauthcheck文件夹到windows\cstrike\addons文件夹内

### 3.2 Rechecker

> 复制rechecker_2_7\bin\addons\rechecker文件夹到windows\cstrike\addons文件夹内

### 3.3 Resemiclip

> 复制resemiclip-2.4.3\addons\resemiclip文件夹到windows\cstrike\addons文件夹内

### 3.4 Whblocke

> 在windows\cstrike\addons里面新建whblocker文件夹
> 复制whblocker_1_5_697\whblocker_1_5_697\bin\windows内所有文件到windows\cstrike\addons\whblocker文件夹内

### 3.5 Yapb Bot

> 复制yapb-4.3.734-windows\addons\yapb文件夹到windows\cstrike\addons文件夹内



## 4. 编辑配置文件加载插件

> 从Half-Life\cstrike\里面复制liblist.gam文件到windows\cstrike文件夹内
> 用记事本(推荐使用文本编辑工具,因为所有文本文件都要保存为UTF-8编码)打开并编辑（二选一）

### 4.1 加载metamod插件 

```bash
#Windows
gamedll "dlls\mp.dll"
gamedll "addons\metamod\metamod.dll"
#Linux
gamedll_linux "dlls/cs.so"
gamedll_linux "addons/metamod/metamod_i386.so"
```

### 4.2 通过metamod加载其他插件

> 在windows\cstrike\addons\metamod文件夹内新建plugins.ini文本文档,内容如下（二选一）

```bash
#windows
win32 addons\amxmodx\dlls\amxmodx_mm.dll
win32 addons\reunion\reunion_mm.dll
win32 addons\reauthcheck\reauthcheck_mm.dll
win32 addons\rechecker\rechecker_mm.dll
win32 addons\VoiceTranscoder\VoiceTranscoder.dll
win32 addons\whblocker\whblocker_mm.dll
win32 addons\yapb\bin\yapb.dll

#Linux
linux addons/amxmodx/dlls/amxmodx_mm_i386.so
linux addons/reunion/reunion_mm_i386.so
linux addons/reauthcheck/reauthcheck_mm_i386.so
linux addons/rechecker/rechecker_mm_i386.so
linux addons/VoiceTranscoder/VoiceTranscoder.so
linux addons/whblocker/whblocker_mm_i386.so
linux addons/yapb/bin/yapb.so
```

### 4.3 其他配置文件(不确定作用项尽量不要去修改)
> 在windows\cstrike文件夹内新建文本文档banned.cfg和listip.cfg
> windows\cstrike\banned.cfg 记录封禁的steamid
> windows\cstrike\listip.cfg 记录封禁的ip
> windows\cstrike\game.cfg
> windows\cstrike\addons\reauthcheck\reauthcheck.cfg
> windows\cstrike\addons\VoiceTranscoder\VoiceTranscoder.cfg
> windows\cstrike\addons\whblocker\config.ini
> windows\cstrike\addons\amxmodx\configs

### 4.4 windows\cstrike\server.cfg
> 服务器配置信息
```bash
hostname " #新人电竞娱乐服"

rcon_password password123
allow_spectators 0
pausable 0

mp_timelimit 60
mp_roundtime 2
mp_freezetime 5
mp_c4timer 35
mp_winlimit 13
mp_fadetoblack 0
mp_forcechasecam 0
mp_forcecamera 0
mp_friendlyfire 0
mp_flashlight 1
mp_limitteams 0
mp_autokick 1
mp_autokick_timeout 120
mp_buytime "0.25"
mp_startmoney "800"
mp_maxmoney "16000"
mp_autoteambalance "1"
mp_logecho 0
mp_logdetail 3
mp_logfile 1
mp_logmessages 1

sv_maxspeed 320
sv_logbans 1
sv_cheats 0
sys_ticrate 1000

log on

// load ban files
exec listip.cfg
exec banned.cfg
```

