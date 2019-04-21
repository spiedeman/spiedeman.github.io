---
title: 在集群（CentOS 6.6）上安装YouCompleteMe
mathjax: false
toc: true
category:
  - Vim
tags:
  - YouCompleteMe
abbrlink: 3242500049
date: 2018-12-09 00:25:58
---
# 准备工作
## 获取动态库
安装`YouCompleteMe`所需的其实只有一个动态库文件`libclang.so.$version`。由于`YCM`一直在升级，对`clang`的版本要求也越来越高。
无奈官网最新的几个版本都没有给出`CentOS`的预编译包，只能下载官网`unknown-linux-gnu`版本或自行编译`clang`得到库文件。

实际尝试过后发现官网`unknown-linux-gnu`版本虽然可以编译通过，但是无法使用。因此只能选择自行编译`clang`，具体编译安装过程可以{% post_link clang-from-source-on-centos6 参考 %}。

## 配置gcc及g++版本
由于集群默认`gcc`版本太低，故需切换高版本`gcc`并设置`CC`和`CXX`
```bash
export CC=gcc
export CXX=g++
# 不做上述设置则需要添加 CMAKE 选项
# -DCMAKE_C_COMPILER=gcc
# —DCMAKE_CXX_COMPILER=g++
```

## 设置python版本
`YouCompleteMe`目前同时支持`python2`及`python3`，因此开启`vim`对`python2/3`的支持均可以。
```bash
# 切换shell中python版本，使其与vim支持的一致
pyenv shell 2.7.15
# or
pyenv shell 3.6.6
```

**问题**：原本想编译`vim`使其同时支持`python2`和`python3`，但不知哪里出了问题，虽然显示同时支持，可实际上一个都不支持。


# 安装 YCM

## 方法一
使用自带安装脚本`install.py`。
```bash
# YCM 通过sha256来判断是否需要从官网下载需要的库文件
# 为了避免自动下载，使其使用自行编译并打包的动态库
# 需要修改如下文件
cd $HOME/.vim/vim_plugin/YouCompleteMe/third_party/ycmd/cpp/ycm/
vim CMakeLists.txt

# 修改第74行，将该sha256值替换为自己的压缩文件的sha256值，保存并推出。

# 将打包的库文件放到如下目录中
mv /path/to/my/libclang-$version-*.tar.bz2 ../../clang_archive/
```

## 方法二
使用我自己写的脚本进行`Full Installation`。
```bash
# 自定义路径
pathbuild=$HOME/ycm_build
pathdest=$HOME/.vim/vim_plugin/YouCompleteMe/third_party/ycmd/cpp

# 新建目录 build
[ -d $pathbuild ] && rm -rf $pathbuild
mkdir $pathbuild && cd $pathbuild

# 开始编译
cmake -DCMAKE_C_COMPILER=gcc \
    -DCMAKE_CXX_COMPILER=g++ \
    -DUSE_PYTHON2='OFF' \
    -DPYTHON_INCLUDE_DIR=$HOME/.pyenv/versions/3.6.6/include/python3.6m \
    -DPYTHON_LIBRARY=$HOME/.pyenv/versions/3.6.6/lib/libpython3.6m.so \
    -G "Unix Makefiles" -DEXTERNAL_LIBCLANG_PATH=/path/to/libclang.so .  $pathdest

cmake --build . --target ycm_core --config Release

# 删除目录 build
rm -rf $pathbuild
```
