---
title: GitBook 安装配置 & 导出 PDF
category: 电子书
tags:
  - GitBook
abbrlink: 3741144490
date: 2019-04-18 19:46:20
---

## 安装 GitBook
```bash
npm install -g gitbook-cli
gitbook -V # 查看版本
```

## 使用 calibre 插件生成PDF
```bash
brew cask install calibre

cd path/to/book

# 生成 pdf
gitbook pdf . mypdf.pdf
```
