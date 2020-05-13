---
title: hexo 建站及使用建议
toc: true
categories:
  - Hexo
tags:
  - Hexo
abbrlink: 1795312736
date: 2018-09-17 15:03:18
---
# 建站
## 依赖
- node.js
- git

安装 `node.js`以及`git`
```bash
brew install node
brew install git
```

## 安装 Hexo
```bash
# 全局安装 hexo 命令行工具
npm install -g hexo-cli
```

## 新建博客根目录
```bash
hexo init /path/to/blog
cd /path/to/blog
npm install
# 博客根目录如下：
# |—— _config.yml
# |—— node_modules
# |—— package.json
# |—— scaffolds
# |—— source
# |   |—— _posts
# |—— themes
```
博客配置文件`/path/to/blog/_config.yml`通常搭配主题进行配置，只要不更换主题可以一直使用，顶多需要稍作修改。

## 安装主题
经过一番挑选之后，个人选择使用`pure`主题。
```bash
git clone https://github.com/cofess/hexo-theme-pure.git themes/pure
```
修改主题的配置文件`theme/pure/_config.yml`。这个文件比较重要，配置过后可以一直使用，每次主题版本更新之后最多只需稍作修改即可。

## 安装插件
配合`pure`主题使用的插件有
```bash
npm install gitment --save
npm install hexo-wordcount --save
npm install hexo-generator-json-content --save
npm install hexo-generator-feed --save
npm install hexo-generator-sitemap --save
npm install hexo-neat --save
```
`pure`主题使用`Katex`渲染数学公式，目前最新版本号为`0.10.1`。为了使用`Katex`，需要替换默认的`markdown`渲染引擎为`markdown-it-plus`
```bash
npm un hexo-renderer-marked --save
npm install hexo-renderer-markdown-it-plus --save
```
本地不需要安装`Katex`插件，但是需要修改模版中的一处地方。
```bash
cd themes/pure/layout/_common
vim head.ejs
# 定位到 katex，将版本号0.9.0修改为最新0.10.1
# 保存退出
```

推送到`GitHub`，需要安装插件`hexo-deployer-git`。
```bash
npm install hexo-deployer-git --save
```

以上是利用`Hexo`建立个人静态博客的基本过程，并已将大部分操作保存在了脚本中`hexo.sh`。

# 使用建议
Hexo 使用的 markdown 语法与标准的 markdown 语法有所差别。

## 图片

当需要同时插入多张图片时，就遇到了排版问题。`next`主题有 [`gp`属性](http://ehlxr.me/2016/08/30/使用Hexo基于GitHub-Pages搭建个人博客（三）/#八、图片模式) 可以做到。 `pure`主题没有相似的命令，所以只能直接用`html`标记语言。

**居中**

```markdown
<div align="center">
  <!-- 分作两行的话中间会有缝隙 -->
  <!-- 由于插件 hexo-abbrlink 的使用，路径的获取最好用下面第二种方式 -->
  <img src="path/to/image" [width="200"] [height="300"] [title]>
  <img src="{% asset_path slug %}" [width="200"] [height="300"] [title]>
</div>
```
效果

<div align="center">
  <!-- 分作两行的话中间会有缝隙 -->
  {% img /images/test/千与千寻1.jpeg 300 %}{% img /images/千与千寻2.jpeg 300 %}
</div>

---------
由于图片过大可能会导致无法并排放两张图片，所以需要在插入时调整尺寸。`hexo`中插入命令`asset_img`没有`size`选项，因此按照上例插入图片或使用[标签插件](https://hexo.io/zh-cn/docs/tag-plugins#Image)。

# 参考资料

[Hexo 官方中文文档](https://hexo.io/zh-cn/docs/)
[Hexo 主题文档（中文）](https://github.com/spiedeman/hexo-theme-pure/blob/master/README.cn.md)
