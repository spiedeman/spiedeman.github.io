---
title: Pyenv基础教程
mathjax: false
toc: true
abbrlink: 1952119038
date: 2018-09-30 16:52:46
category:
  - Python
tags:
  - Pyenv
---

[pyenv](https://github.com/pyenv/pyenv#table-of-contents) 是一个`python`版本管理软件。受到`ruby`的同类软件 [rbenv](https://github.com/rbenv/rbenv)和 [ruby-build](https://github.com/rbenv/ruby-build)的启发。

# 安装 pyenv

## MacOS

使用`homebrew`安装

```bash
brew update
brew install pyenv
```

## Linux
```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

# 配置

在`.bashrc`中添加下列内容：

```bash
export PYENV_ROOT="$home/.pyenv"
export PATH="$PYENV_ROOT/bin:$path"
# 上面两行不需要，如果采用homebrew安装方式
eval "$(pyenv init -)"
```

# 依赖环境

`pyenv`可以用于安装多个`python`版本，并对其进行管理。为了避免出现可能的错误，安装之前需要准备好依赖环境。

- macos
  ```bash
  brew install openssl readline sqlite xz zlib
  ```
  如果运行 Mojave 或更高版本，还需额外安装
  ```bash
  sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
  ```
- Debian/Ubuntu
  ```bash
  apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev
  ```
  在服务器上`import pandas as pd`可能会出现`lzma module could not found,
  your python was incomplete`这样的错误。解决办法是安装库`liblzma-dev`。
  ```bash
  apt install liblzma-dev
  ```

# 安装python

`pyenv install`命令用于安装指定`python`版本。

为了添加`framework`支持，安装代码应如下

```bash
# MacOS
env python_configure_opts="--enable-framework" pyenv install 3.6.6
# Linux
env python_configure_opts="--enable-shared" pyenv install 3.6.6
```
若提示错误`no module named pyexpat`，则用如下方式安装
```bash
SDKROOT=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.14.sdk MACOSX_DEPLOYMENT_TARGET=10.14 pyenv install 2.7.16
```


# 参考命令

## 查看已安装python版本

```bash
pyenv versions

  system
* 2.7.15 (set by /users/xuwutao/.python-version)
  3.6.6
```

## 版本切换
`pyenv`总共设置了三个命令用于`python`的版本控制，按优先级由高到低以此为

1. `pyenv shell`
2. `pyenv local`
3. `pyenv global`

### pyenv shell
效果等价于设定环境变量`pyenv_version`。

```bash
pyenv shell 3.6.6

# unset
pyenv shell --unset
```

### pyenv local
在当前目录下新建文件`.python-version`，记录`python`版本号，一行一个。如果当前目录下没有该文件，则一路往上寻找直到系统根目录。
```bash
pyenv local 2.7.15
# 等价于
touch .python-version && echo '2.7.15' > !$
```

### pyenv global
和`pyenv global`对应的文件为`$(pyenv root)/version`。

```bash
pyenv global 2.7.10
# 等价于
touch ~/.pyenv/version && echo '2.7.10' > !$
```

有一个特殊的版本名`system`是指`pyenv`之外的`python`（`macos`自带，`homebrew`安装版本等）。


三个命令都可以后接多个版本号。
