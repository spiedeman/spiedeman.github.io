---
title: 在Ubuntu中使用Citrix Workspace App办公
tags: 远程办公
category: 工作
abbrlink: c80075c3
date: 2020-10-24 11:04:17
---

# 远程办公
在家里办公需要下载`Citrix Reciver`远程连接公司服务器。Google之后发现在Linux平台上，
`Citrix Workspace APP`已经取代前者并提供兼容性支持，因此将选择它进行安装。

# 安装使用
## 下载安装包
在最新版本所在
[页面](https://www.citrix.com/downloads/workspace-app/linux/workspace-app-for-linux-latest.html)
根据Linux发行版找到相应的安装包并下载。

## 遇到的问题
安装完成后，启动程序并填入服务器地址进行连接，发现缺少SSL证书导致连接失败。
网上搜索之后找到解决办法，如下

```bash
# 将证书软链接至相应目录
sudo ln -s /usr/share/ca-certificates/mozilla/GlobalSign_Root_CA.crt /opt/Citrix/ICAClient/keystore/cacerts/
# 更新证书记录
sudo c_rehash /opt/Citrix/ICAClient/keystore/cacerts
```

由于本人所用笔记本屏幕分辨率较高`2560x1600`，打开远程桌面后，字体很小，眼睛累。公司用的服务器系统是`window server 2008`，
调整字体大小为`150%`之后，基本解决问题。不过这只是治标，治本得等Linux客户端程序提供功能改进了。

> 使用体验暂时OK，仍然不用切换到Windows办公，就这样！
