---
title: Jupyter-With-Virtualenv
mathjax: false
toc: true
category:
  - Python
tags:
  - Jupyter
  - Virtualenv
abbrlink: 3264731549
date: 2018-11-26 13:36:56
---

# 添加内核
终端切换`python`版本很容易，但在`jupyter`中应该如何切换环境？

假如有一个虚拟环境`science`，将其导入`jupyter`的过程如下：

1、进入虚拟环境`science`
```bash
pyenv active science
```

2、安装`ipykernel`
```bash
pip install ipykernel
```

3、为`jupyter`安装新内核
```bash
ipython kernel install --user --name science --display-name "python3 (science)"
```

4、查看可用内核
```bash
jupyter-kernelspec list

Available kernerls:
  science    /Users/xuwutao/Library/Jupyter/kernels/science
  python3 /Users/xuwutao/.pyenv/versions/3.7.1/Python.framework/Versions/3.7/share/jupyter/kernels/python3
```

<img src="{%asset_path jupyter-with-virtualenv.png %}" width="400" height="400">

# 修改内核
内核在`jupyter`中以`json`文件方式存在。每新建一个内核，`jupyter`会在系统的特定位置新建文件夹，存储相应的`json`文件。这些位置分布如下：

|          | Unix                                                                          | Windows                        |
| :---:    | :---:                                                                         | :----:                         |
| `System` | `/usr/share/jupyter/kernels`<br>`/usr/local/share/jupyter/kernels`            | `%PROGRAMDATA\jupyter\kernels` |
| `Env`    | `{sys.prefix}/share/jupyter/kernels`                                          |                                |
| `User`   | `~/Library/Jupyter/kernels` (Mac)<br>`~/.local/share/jupyter/kernels` (Linux) | `%APPDATA%\jupyter\kernels`
