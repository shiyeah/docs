---
title: ubuntu20.04-Mininal安装后系统配置
categories:
- [Linux,ubuntu]
tags:
- [ubuntu]
- [LNMP]
- [Gitlab]
- [HLDS]
date: 2023-11-12 18:00:00
updated: 2023-11-12 18:00:00
---

# Ubuntu20.04 Mininal安装后系统配置



## 安装过程中需要设置的

- 手动配置网络,配置固定IP
- 修改apt源:mirrors.ustc.edu.cn
- 设置用户及密码
- 分区(个人习惯是不选中LVM),如果有其他硬盘也可在这里直接挂载,如果没数据或数据不重要,也可以重新分区并挂载



## 修改SSH配置

- 用安装设置的用户名和密码连接服务器
- 先更新源
```bash
sudo apt update
sudo apt -y upgrade
```

```bash
sudo vim /etc/ssh/sshd_config
	
LoginGraceTime 2m
PermitRootLogin prohibit-password
MaxAuthTries 6
TCPKeepAlive yes
ClientAliveInterval 60
ClientAliveCountMax 3
UseDNS no
	
sudo systemctl restart sshd
```



## 关闭ufw防火墙并卸载

  ```bash
  sudo ufw disable
  sudo apt-get -y remove ufw
  ```



## 修改时区,并设置时间自动同步

### 修改时区

```bash
sudo timedatectl set-timezone Asia/Shanghai
date
```



### 设置时间自动同步

```bash
sudo apt install chrony -y
sudo vim /etc/chrony/chrony.conf

pool ntp.aliyun.com
pool time1.cloud.tencent.com
pool time.ustc.edu.cn

sudo systemctl enable chrony
sudo systemctl start chrony
sudo systemctl status chrony

chronyc sources -v
```



## 安装常用软件

```bash
sudo apt -y install net-tools zip unzip dstat nload screen htop tmux software-properties-common tree
```
物理机可选
```bash
sudo apt -y install rt-tests lm-sensors
```
其他库
```bash
sudo apt -y install build-essential gzip libsdl2-2.0-0 libsdl2-dev lib32stdc++6 asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3 unzip zlib1g-dev libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf wget curl swig rsync cmake libicu-dev
```



## 开机自动运行rc.local

```bash
sudo vim /lib/systemd/system/rc-local.service
[Install]
WantedBy=multi-user.target
Alias=rc-local.service

sudo touch /etc/rc.local
sudo chmod 755 /etc/rc.local
sudo vim /etc/rc.local

#!/bin/bash

sudo systemctl daemon-reload
sudo systemctl start rc-local.service
sudo systemctl status rc-local.service
```



## 开机时在network那里卡好久

```bash
sudo vim /etc/systemd/system/network-online.target.wants/systemd-networkd-wait-online.service

[Service]
Type=oneshot
ExecStart=/lib/systemd/systemd-networkd-wait-online
RemainAfterExit=yes
TimeoutStartSec=2sec
```



## 物理机更改CPU运行模式为性能模式

```bash
sudo apt -y install cpufrequtils sysfsutils
查看CPU运行模式
cpufreq-info
查看CPU当前工作主频
cat /sys/devices/system/cpu/*/cpufreq/scaling_cur_freq

sudo cpufreq-set -c 0 -g performance
sudo cpufreq-set -c 1 -g performance
sudo cpufreq-set -c 2 -g performance
sudo cpufreq-set -c 3 -g performance
sudo cpufreq-set -c 4 -g performance
sudo cpufreq-set -c 5 -g performance
sudo cpufreq-set -c 6 -g performance
sudo cpufreq-set -c 7 -g performance

sudo cpufreq-set -c 8 -g performance
sudo cpufreq-set -c 9 -g performance
sudo cpufreq-set -c 10 -g performance
sudo cpufreq-set -c 11 -g performance
sudo cpufreq-set -c 12 -g performance
sudo cpufreq-set -c 13 -g performance
sudo cpufreq-set -c 14 -g performance
sudo cpufreq-set -c 15 -g performance

sudo vim /etc/sysfs.conf

devices/system/cpu/cpu0/cpufreq/scaling_governor = performance
devices/system/cpu/cpu1/cpufreq/scaling_governor = performance
devices/system/cpu/cpu2/cpufreq/scaling_governor = performance
devices/system/cpu/cpu3/cpufreq/scaling_governor = performance
devices/system/cpu/cpu4/cpufreq/scaling_governor = performance
devices/system/cpu/cpu5/cpufreq/scaling_governor = performance
devices/system/cpu/cpu6/cpufreq/scaling_governor = performance
devices/system/cpu/cpu7/cpufreq/scaling_governor = performance

devices/system/cpu/cpu8/cpufreq/scaling_governor = performance
devices/system/cpu/cpu9/cpufreq/scaling_governor = performance
devices/system/cpu/cpu10/cpufreq/scaling_governor = performance
devices/system/cpu/cpu11/cpufreq/scaling_governor = performance
devices/system/cpu/cpu12/cpufreq/scaling_governor = performance
devices/system/cpu/cpu13/cpufreq/scaling_governor = performance
devices/system/cpu/cpu14/cpufreq/scaling_governor = performance
devices/system/cpu/cpu15/cpufreq/scaling_governor = performance

sudo systemctl restart sysfsutils
sudo systemctl status sysfsutils
```



## iptables防火墙

### 安装防火墙并创建黑名单

```bash
sudo apt -y install iptables-persistent ipset
sudo modprobe ipt_recent ip_list_tot=1000000
sudo cat /sys/module/xt_recent/parameters/ip_list_tot
创建IP黑名单
sudo ipset create banip hash:net maxelem 1000000 timeout 0
sudo ipset create blackip hash:ip maxelem 1000000 timeout 600
保存IP黑名单
sudo ipset save banip -f /etc/iptables/banip
sudo ipset save blackip -f /etc/iptables/blackip
```


### 编辑防火墙规则

```bash
sudo vim /etc/iptables/rules.v4

*filter
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT ACCEPT [0:0]

-A INPUT -i lo -j ACCEPT
-A INPUT -s 172.16.0.0/24 -j ACCEPT

-A INPUT -p udp -m multiport --dports 26900:27014 -j ACCEPT
-A INPUT -p udp -m udp --dport 27015 -j ACCEPT
-A INPUT -p udp -m udp --dport 27016 -j ACCEPT
-A INPUT -p udp -m udp --dport 27017 -j ACCEPT
-A INPUT -p udp -m udp --dport 27018 -j ACCEPT
-A INPUT -p udp -m udp --dport 27019 -j ACCEPT
-A INPUT -p udp -m udp --dport 27020 -j ACCEPT

-A INPUT -p tcp -m multiport --dports 22,80,443 -j ACCEPT
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p udp -m udp --sport 53 -j ACCEPT
-A INPUT -m conntrack --ctstate INVALID -j DROP
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT

*nat
:INPUT ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:POSTROUTING ACCEPT [0:0]
:PREROUTING ACCEPT [0:0]

COMMIT

*raw
:OUTPUT ACCEPT [0:0]
:PREROUTING ACCEPT [0:0]

-A PREROUTING -s 172.16.0.0/24 -j ACCEPT

-A PREROUTING -p udp -m string --hex-string "|74496e666f457800484c5357|" --algo kmp -j MARK --set-xmark 0x1/0xffffffff
-A PREROUTING -p udp -m mark --mark 0x1 -j SET --add-set blackip src
-A PREROUTING -p udp -m set --match-set blackip src -j DROP
-A PREROUTING -m set --match-set banip src -j DROP

COMMIT
```



### 导入防火墙规则

```bash
sudo iptables-restore < /etc/iptables/rules.v4
```



### 查看防火墙规则

```bash
sudo iptables -t filter -nL --line-number
sudo iptables -t nat -nL --line-number
sudo iptables -t raw -nL --line-number
```



### 添加到开机启动

```bash
sudo vim /etc/rc.local

#!/bin/bash
sleep 3
modprobe ipt_recent ip_list_tot=1000000

sleep 3
ipset restore -f /etc/iptables/banip
sleep 3
ipset restore -f /etc/iptables/blackip
sleep 3
iptables-restore < /etc/iptables/rules.v4

sleep 5
systemctl restart sysfsutils
```
