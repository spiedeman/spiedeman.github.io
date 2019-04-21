---
title: matplotlib 技巧
mathjax: false
toc: true
categories:
  - python
  - matplotlib
tags:
  - matplotlib
abbrlink: 1362387800
date: 2018-09-19 16:04:13
---

收集各种技巧、解决问题的办法。不断加深理解、熟悉`matplotlib`的使用。

# 中文

**查看支持的中文字体**

运行如下代码

```python
from matplotlib.font_manager import FontManager
import subprocess

fm = FontManager()
mat_fonts = set(f.name for f in fm.ttflist)

output = subprocess.check_output(
    'fc-list :lang=zh -f "%{family}\n"', shell=True)

zh_fonts = set(f.split(',', 1)[0] for f in output.split('\n'))
available = mat_fonts & zh_fonts

# 可用中文字体
for f in available:
    print f
```

尝试发现`matplotlib`自带了一个可以显示中文的字体`Arial Unicode MS`。于是可以如下设置，不用下载字体，不用修改`matplotlib`配置文件。

```python
from matplotlib import rcParams
rcParams['font.sans-serif'] = ['Arial Unicode MS']
```

搞定中文乱码！


# Figure

## 设置图片大小

`px(pixel), inch, pt(point)`三者之间的关系为

```bash
1 px = constant size on screen
1 inch = dpi px     # dpi is variable
1 inch = 72 pt      # 确定关系
```


**Figure size (`figsize`)** 决定`figure`的尺寸，单位为`inches`。默认大小为`[6.4, 4.8]`。

**Dots per inches (dpi)** 决定`figure`包含多少像素。默认大小为100。设置为`figsize=(w,h)`的`figure`包含

```python
px, py = w * dpi, h * dpi   # pixels
# e.g.
# 6.4 inches * 100 dpi = 640 pixels
```

因此若要获得例如`(1200, 600)`大小的`figure`，单位为`pixel`，可以有各种不同的组合方式

```python
figsize = (15. 7.5), dpi = 80
figsize = (12. 6)  , dpi = 100
figsize = (8, 4)   , dpi = 150
figsize = (6, 3)   , dpi = 200
```

以上组合方式的不同之处在哪里？

差异在图形中线条、文字的粗细不同。`figure`中大多数元素如线条、标记、文本的`size`的单位为`points`。`Matplotlib`设置 **Points per inch (ppi)** 等于72。因此无论 `dpi` 等于多少，一英寸只画72个点。或者说 `dpi` 决定线条粗细，`figsize`决定内容多少。

<div align="center">
  <img src="{%asset_path Figure_1.png %}" width="100"><img src="{%asset_path Figure_2.png %}" width="100">
</div>
<div align="center">
  <img src="{%asset_path Figure_3.png %}" width="200"><img src="{%asset_path Figure_4.png %}" width="200">
</div>
<div align="center">
  <img src="{%asset_path Figure_5.png %}" width="400"><img src="{%asset_path Figure_6.png %}" width="400">
</div>
