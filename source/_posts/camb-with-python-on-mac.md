---
title: Mac下安装Python版CAMB
mathjax: false
toc: true
category:
  - python
tags:
  - python
  - camb
abbrlink: 3538923028
date: 2018-11-26 17:48:03
---
# 安装过程遇到的坑
`camb`的最新版已经支持`Intel Fortran`编译器进行编译。然而编译`python`版本`camb`的过程中会遇到不少问题。

1、编译选项冲突
查看`Makefile`发现，`MacOS`平台下`camb`共享库`camblib.so`的编译选项中`SFFLAGS`保持默认会出现编译错误。

```bash
# -fminshared 与 -fpic 选项冲突
# 修改为
SFFLAGS = -dynamiclib
```

2、动态库加载错误
修改`Makefile`之后，编译没有问题，但是在`Python`中`import`时会出现库未加载的错误。
```python
OSError:
dlopen(/Users/xuwutao/.pyenv/versions/3.7.1/envs/science/lib/python3.7/site-packages/camb/camblib.so, 6): Library not loaded: @rpath/libiomp5.dylib
  Referenced from : /Users/xuwutao/.pyenv/versions/3.7.1/envs/science/lib/python3.7/site-packages/camb/camblib.so
  Reason: image not found
```

解决办法：
将`libiomp5.dylib`所在的目录加入`@rpath`中。

```bash
install_name_tool -add_rpath /Users/xuwutao/Program/intel/2018/compilers_and_libraries_2018.1.126/mac/compiler/lib /Users/xuwutao/.pyenv/versions/3.7.1/envs/science/lib/python3.7/site-packages/camb/camblib.so
```

# 疑问
原以为设置好`DYLD_LIBRARY_PATH`就没有问题，没想到还需要设置`@rpath`。估计和`Mac`下动态库的加载机制有关，详细情况以后有时间再研究吧。
