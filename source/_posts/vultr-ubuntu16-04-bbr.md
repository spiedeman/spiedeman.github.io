---
title: Ubuntu 16.04 LTS 开启BBR
mathjax: false
toc: true
category:
  - Vultr
tags:
  - BBR
abbrlink: 3381164653
date: 2019-01-06 00:56:29
---
Vultr 上安装 ubuntu 16.04 并开启BBR。

# 更新内核
`BBR`只支持4.9+的Linux内核版本，因此首先检查并更新内核。
```bash
# 查看当前内核版本
uname -r

# 安装最新版内核
sudo apt install linux-image-generic-hwe-16.04
```

# 重启
```bash
sudo reboot
```

# 开启BBR
添加如下两行至文件`/etc/sysctl.conf`
```bash
net.ipv4.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
```

启动bbr
```bash
sysctl -p
```

验证是否成功
```bash
lsmod | grep bbr

tcp_bbr         20480 1
```
