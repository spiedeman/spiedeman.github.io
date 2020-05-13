---
title: Vim 编译安装及配置
mathjax: false
toc: true
category:
  - Vim
tags:
  - Vim
abbrlink: 3465068350
date: 2018-12-08 14:56:12
---
# 提前准备
为了脚本能顺利运行，需要预先安装好如下几个软件：
- `curl`
- `jq`
- `wget`

# 下载源代码
从`github`下载`vim`，好处是可以自由切换版本。
```bash
# git clone https://github.com/vim/vim.git

# 利用 curl、jq、awk 以及 GitHub 的 API 获取最新版本下载链接
# 利用 wget 下载并保存为 vim-latest.tar.gz
file=vim-latest.tar.gz
api="https://api.github.com/repos/vim/vim/tags"
download_url=$(curl -s $api | jq -r ".[] | .tarball_url" | awk 'NR==1{print}')
latest_version=$(curl -s $api | jq -r ".[] | .name" | awk 'NR==1{print}')

wget -O $file $download_url
```

解压，进入目录
```bash
mkdir vim-latest
tar -zxf vim-latest.tar.gz -C vim-latest --strip-components=1
cd vim || exit

# 查看当前版本
git describe --tags
```

# 编译安装
```bash
# 清除上次configure留下的痕迹
make distclean

# 重新configure
./configure \
--prefix=$HOME/Program/vim/${latest_version##*v} \
--enable-multibyte=yes \
--enable-gui=gtk2 \
--with-x \
--with-features=huge \
--enable-python3interp=yes \
--enable-perlinterp=yes \
--enable-rubyinterp=yes \
--enable-luainterp=yes \
--with-luajit \
--enable-cscope \
--with-compiledby=徐武涛 \

# 安装
make -j 16 && make install
```

# 卸载
`vim`的`Makefile`提供了`uninstall`选项，因此卸载很容易。
```bash
cd vim && make uninstall
```
