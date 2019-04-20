---
title: vim 编译安装及配置
mathjax: false
toc: true
category:
  - Vim
tags:
  - vim
abbrlink: 3465068350
date: 2018-12-08 14:56:12
---
# 下载源代码
从`github`下载`vim`，好处是可以自由切换版本。
```bash
git clone https://github.com/vim/vim.git

# 进入 vim
cd vim

# 查看当前版本
git describe --tags
```

# 编译安装
```bash
# 清除上次configure留下的痕迹
make distclean

# 重新configure
./configure \
--prefix=/path/to/vim \
--with-features=huge \
--enable-pythoninterp=yes \
--enable-python3interp=yes \
--enable-rubyinterp=yes \
--enable-luainterp=yes \
--enable-perlinterp=yes \
--enable-multibyte=yes \
--enable-cscope \

# 安装
make && make install
```

# 卸载
`vim`的`Makefile`提供了`uninstall`选项，因此卸载很容易。
```bash
cd vim && make uninstall
```
