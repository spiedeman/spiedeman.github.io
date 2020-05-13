---
title: Homebrew 使用方法
mathjax: false
toc: true
category:
  - MacOS
tags:
  - Homebrew
abbrlink: 319510325
date: 2018-12-04 21:55:23
---

# 查看已安装的包
```bash
brew list
```

# 更新 Homebrew
要获取最新的包列表，需先更新 `Homebrew` 自身。
```bash
brew update
```

# 更新包（formula）
查看哪些包有新版本
```bash
brew outdated

brew upgrade                # 更新左右包
brew upgrade $FORMULA       # 更新指定包
```
更新时自动清理旧版本，在`brew upgrade`前设置环境变量`HOMEBREW_UPGRADE_CLEANUP`。

# 清理旧版本
```bash
brew cleanup                # 清理所有包的旧版本
brew cleanup $FORMULA       # 清理指定包的旧版本
brew cleanup -n             # 查看可清理的旧版本包，不会执行清除操作
```
彻底卸载，包括删除旧版本
```bash
brew uninstall formaula_name --force
```

# 锁定包
被锁定的包在更新时会被略过。
```bash
brew pin $FORMULA           # 锁定
brew unpin $FORMULA         # 取消锁定
```

# 切换版本
时间一长，各个包可能都有多个版本并存。
```bash
brew switch $FORMULA $VERSION       # 切换包到指定版本
```

# 查看依赖关系
```bash
brew deps --installed --tree        # 查看已安装的包的依赖关系
```

