---
title: hexo 使用建议
toc: true
categories:
  - hexo
tags:
  - hexo
abbrlink: 1795312736
date: 2018-09-17 15:03:18
---

Hexo 使用的 markdown 语法与标准的 markdown 语法有所差别。

# 图片

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
