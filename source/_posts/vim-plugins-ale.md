---
title: vim 插件配置
tags:
  - vim
  - 插件
abbrlink: 6e83abdb
date: 2019-09-02 10:18:29
---

## 语法检查与代码格式化——ALE
#### 异步检查语法
- [pycodestyle](https://pycodestyle.readthedocs.io/en/latest/#) ——
  前身为pep8，检查Python语法以及格式

**安装**
```bash
pip install pycodestyle
```

**配置**

配置文件在 `~/.config/pycodestyle`。

#### 代码格式化
- [yapf](https://github.com/google/yapf) ——
  Google出品的Python代码格式化工具
- [isort](https://isort.readthedocs.io/en/latest/#) —— 格式化 `import` 语句

**安装**
```bash
pip install yapf
pip install isort
```

**配置**

配置文件在 `~/.config/yapf/style` 和 `~/.isort.cfg`。
