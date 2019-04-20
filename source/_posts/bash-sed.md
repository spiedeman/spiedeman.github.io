---
title: sed 学习笔记
mathjax: false
abbrlink: 2825119572
date: 2018-09-18 15:09:07
toc: true
categories:
  - bash
tags:
  - bash
  - sed
---

# sed 简介

`sed` 是强大的文本编辑工具。所有的操作都在**模式空间**进行，且每次只会将一行文本复制到其中等待 `sed` 命令对其操作。因此，从第二条命令开始，操作的对象都是上一条命令作用后的文本。

## 命令格式
```bash
sed [option] 'COMMANDS' file
```

常用的`option`有

```bash
-e      表明后接 sed 命令。命令多于一条时不能省略
-i      直接对文件进行操作。
-f      后接包含一列 sed 命令的脚本文件。
```

命令`COMMANDS` 通常格式为

```bash
'address[,address]cmd[options]'     # 三部分组成：地址、命令、具体操作
```

- 地址：右行号、正则表达式给出，用于指定需要操作的行。有些命令只能操作单行，有些可以操作连续多行。地址对所有的命令都是可选项，如果不指定，则作用在所有的行上。
- 命令：有追加（`a`）、改变（`c`）、插入（`i`）、替换（`s`）等最常用。
- 选项：格式由前面的命令决定。

### 替换（s）

`options`的格式为`/pattern/replacement/flags`。

- `pattern`与`replacement`都支持正则表达式。
- `flags`选项有
  - `n`：1~512的整数，表示只替换第`n`个匹配项。
  - `g`：替换全部匹配项
  - 默认替换第一个匹配项


#### 技巧

`pattern`和`replacement`中包含`Shell`变量`$var`。如果命令用单引号括起来，那么美元符号`$`分别表示行尾和符号本身。只有用双引号，`$var`才会被替换为变量值。

```bash
#!/bin/bash
var=LINUX
sed 's/linux/$var/' sonfile
...$var...      # linux 被替换为 $var

var=LINUX
sed "s/linux/$var"  somefile
...LINUX...     # linux 被替换为 LINUX
```

替换命令（s）格式中的斜杠`/`作为定界符，可以换成任何其它符号（好像有一个例外），并且必须出现三次。通常当匹配项或替换项中出现斜杠`/`时会变更定界符，如`#`。

```bash
's#pattern#replacement#'
```

#### 例子

- 删除空白行

`sed -i '/^$/d' input.xxx   # -i，修改源文件`

- 输出某一行到其他文件

`sed -n '3p' input.xxx >> output.xxx`
