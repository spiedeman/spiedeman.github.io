---
title: 在多台电脑上同步更新Hexo博客
tags:
  - Hexo
abbrlink: 77781092
date: 2019-04-20 16:04:50
---

# 背景
目前更新博客文章，只在自己的笔记本操作。一旦想要在办公室的台式机上写文章并发布到
Github Pages 上就变得很麻烦，一时不知从何下手。


# 需求
博客的源文件能在多个设备上同步更新，并且在任意设备上都能自如写文章并发布。

# 方案
利用 `GitHub` 的分支功能，将 `Hexo`建站的必备源文件以及博客文章的源文件备份到 `source` 分支上。而 `hexo`生成的静态博客文件仍然放在默认的 `master` 分支上。这样在任何设备上只需将`source` 分支克隆到本地，即可开始写新博客。之后通过 `hexo`命令即可将新生成的网站内容推送到 `master` 分支上。两个分支上的内容互不干扰，都能维持最新状态。

## 操作

### 创建新的分支
新分支的命名没有要求，本文采用`source`。

创建分支的方法有两种，
- 在浏览器中登陆[GitHub](https://github.com)进行操作。
- 在本地通过命令行操作。

由于浏览器操作比较简单，就不详细讨论了。重点记录一下终端操作过程。

1、克隆仓库 `username.github.io` 到本地
```bash
git clone https://github.com/username/username.github.io
```

2、进入该文件夹并创建新的分支
```bash
cd username.github.io

# 创建新的分支 source
git checkout -b source
```

3、在 `source` 分支内删除原有的所有文件，然后将 `Hexo`
建站所需的必备文件以及所有的博客文章拷贝进来

```bash
# 删除所有的原有内容，.git 文件夹保留
rm -rf *

# 拷贝必要的文件到当前目录
cp -r /path/to/hexo/source .
cp -r /path/to/hexo/scaffolds .
cp -r /path/to/hexo/themes .
cp /path/to/hexo/_config.yml .
cp /path/to/hexo/package.json .
```

需要注意的是 `themes` 中可能存在 `.git` 文件夹，需要把它们全部删除。

4、提交 source 分支
由于 `Hexo` 命令生成网站内容时会新建两个文件夹 `public` 和`.deploy_git`。`public`中包含的正是所有网站的源文件，这些内容并不需要保存在 `source`分支，因此提交时要忽略。`.deploy_git` 是 `Hexo`帮我们把网站内容推送到 `master`分支上所建的文件夹，故也可以忽略。另外不需要推送的一个文件夹，便是`node_modules`。这个文件夹里是 `Hexo`用到的所有插件，在本地安装一下就有了，不需要保存在 `GitHub` 上。

因此，提交 `source` 分支前需要

```bash
echo 'node_modules' >> .gitignore
echo 'public' >> .gitignore
echo '.deploy_git' >> .gitignore
```

现在可以提交了

```bash
git add .
git commit -m "update blog files"
git push origin source
```

### 在新设备上的准备工作
要在新设备上愉快地用 `Hexo` 写博客之前，需要做一些准备工作。

1、安装 git，并将新设备上的 ssh key 添加到 GitHub 账户上。

2、安装 Hexo

3、把 souce 分支克隆到本地

```bash
git clone -b source git@github.com:username/username.github.io.git

# 若已将 source 分支设为默认分支，则可以简化命令
git clone https://github.com/username/username.github.io
```

4、安装所有依赖，生成 `node_modules` 目录
```bash
npm install
```

5、创建新文章，生成博客的静态文件（即 `public` 目录）
```bash
hexo new "title"

# 文章写完之后
hexo g
```

6、将博客内容推送到 GitHub 上
```bash
# 博客的静态文件在 master 分支，其他的源文件在 source 分支
# _config.yml 中 deploy 的 branch 值必须为 master
hexo d
```

7、最后把修改推送到 source 分支
```bash
git add .
git commit -m "commit message"
git push origin source
```

**注意**：在新设备上的所有操作都在 `source` 分支下完成。

### 日常操作
由于上一次更新博客有可能不是在手头的电脑上操作的。因此 `GitHub` 上的 `source` 分支可能比本地的更新，因此首先要更新本地的 `source` 分支。

```bash
git pull origin source
```

接下来，按照正常流程写文章并用`hexo`部署到`GitHub`上。

最后，把对源文件的修改推送到 `source` 分支上。


---
参考资料：
[theqwang's blog](https://theqwang.github.io/2017/03/17/在多台电脑间使用hexo/#more)
