---
title: bash-tips
mathjax: false
abbrlink: 3452120969
date: 2017-06-09 14:54:21
toc: true
categories:
  - bash
tags:
  - bash
---

持续更新 `bash` 小技巧

- 检测程序是否已安装

```bash
#!/bin/bash
check_software(){
    local software=("vim" "git" "tmux" "npm")   # 待测程序名
    for soft in ${software[@]}
    do
        type $soft 2>&1 > /dev/null     # 已安装，则返回零
        if [ $? -ne 0 ]; then
            echo "ERROR: **$soft** is not installed!"
            exit 1
        fi
        echo "Checking $soft...ok!"
    done
}

check_software
```

{% codeblock %}
运行结果
bash check_software.sh
Checking vim...ok!
Checking git...ok!
Checking tmux...ok!
Checking npm...ok!
{% endcodeblock %}

