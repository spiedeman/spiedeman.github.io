---
title: 树莓派新手指南
mathjax: false
toc: true
category:
  - 树莓派
tags:
  - Raspberrypi
abbrlink: 2712929324
date: 2018-11-18 19:12:14
---

# 刻录系统
选好合适的系统镜像之后，用`dd`命令刻录系统到 `sd` 卡中。

```bash
dd bs=16M if=/path/to/img of=/dev/sdc
```

Mac系统下类似，主要注意输出文件是`/dev/sdc`而不是`sd`卡上的分区。

# 开启SSH
最新版树莓派系统默认不开启`ssh`服务。没有显示器的情况下，这是不能忍的。好在有一个简单的办法可以解决，系统刻录完成后在`boot`分区下新建名为`ssh`的空白文件。

```bash
# 挂载并进入boot分区
touch ssh
```

# 启动树莓派
用网线连接树莓派和Mac，打开Mac终端，通过`ssh`连接树莓派。

```bash
# 搜索raspberrypi的ip地址
arp -a

# 将公钥加入远程主机的 authorized_keys 文件中，实现无密码登陆
ssh-copy-id pi@raspberrypi
```

# 更新源
将文件`/etc/apt/sources.list`中的`raspbian.raspberrypi.org`替换为`mirrors.ustc.edu.cn/raspbian`。


树莓派的archive.raspberrypi.org软件源，也即`/etc/apt/sources.list.d/raspi.list`。

是由树莓派基金会提供的软件源，包括ui相关程序（如Raspbian的桌面环境PIXEL
DE）及部分由树莓派基金会为树莓派编写的软件，通常与archive.raspbian.org一起使用。

将`archive.raspberrypi.org/`替换为`mirrors.ustc.edu.cn/archive.raspberrypi.org/`。

# 初始化配置
输入命令 `sudo raspi-config` 进入配置模式。常用的有扩展系统分区，修改密码，启动设置等。

# 修改swap空间大小

```bash
# 修改文件
sudo vim /etc/dphys-swapfile
# 把 CONF_SWAPSIZE=100 改成需要的大小，单位是m
# 重启dphys-swapfile服务
sudo service dphys-swapfile restart
```
查看当前swap空间

```bash
free -m
```

# VNC 远程桌面
树莓派自带`VNC`服务器，通过配置实现远程图形化界面登陆。

[树莓派官方VNC教程](https://www.raspberrypi.org/documentation/remote-access/vnc/)

# debian 安装后必做的几件事
1、安装`shadowsocks`。注意`pip`安装时不要`--user`选项。配置服务，开机自启动。
```bash
# 安装最新版 shadowsocks
pip install https://github.com/shadowsocks/shadowsocks/archive/master.zip
```

pip更新之后可能会遇到问题`can not import name main`
修改 /usr/bin/pip 文件。

```python
# 修改前
from pip import main
if __name__ == '__main__':
    sys.exit(main())

# 修改后
from pip import __main__
if __name__ == '__main__':
    sys.exit(__main__.main())
```
-----------
2、安装`pyenv`
```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```
详细内容参见文章 {% post_link pyenv-guide pyenv基础教程 %}

-----------
3、安装字体，VNC连接时视觉上不能遭罪。

