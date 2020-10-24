---
title: 矢量函数分解
mathjax: true
toc: true
category:
  - 数学
tags:
abbrlink: 2388637373
date: 2018-10-08 16:34:27
---

结论：**纵向无旋，横向无源**

----------
任意给定一个矢量函数 $w(\boldsymbol{x})=(w_1(\boldsymbol{x}),w_2(\boldsymbol{x}),w_3(\boldsymbol{x}))$。在傅立叶空间中，$\boldsymbol{k}$模式的系数$w(\boldsymbol{k})=(w_1(\boldsymbol{k}),w_2(\boldsymbol{k}),w_3(\boldsymbol{k}))$
可以分解为动量$k$的平行和垂直方向。

$$
w = w^{\parallel} + w^{\bot}
$$

且有 $w^{\parallel} // \boldsymbol{k}$ 及 $w^{\bot}\cdot{\boldsymbol{k}} = 0$

回到实空间中，发现 $w(\boldsymbol{x})$ 可以分解为纵向和横向两部分，$w^{\parallel}(\boldsymbol{x})$ 和 $w^{\bot}(\boldsymbol{x})$。分别有如下性质：

- 纵向无旋

$$
\nabla \times w^{\parallel}(\boldsymbol{x}) = 0
$$

- 横向无源

$$
\nabla \cdot w^{\bot}(\boldsymbol{x}) = 0
$$

# 投影算符
傅立叶空间中，$\boldsymbol{k}$ 模的纵向（横向）投影算符分别为

$$
\begin{aligned}
P^{\parallel}(\boldsymbol{k}) &= \boldsymbol{\hat{k}}\otimes\boldsymbol{\hat{k}} \\
P^{\perp}(\boldsymbol{k}) &= 1 - \boldsymbol{\hat{k}}\otimes\boldsymbol{\hat{k}} \\
\end{aligned}
$$
或者分量表示
$$
\begin{aligned}
P^{\parallel}_{ij}(\boldsymbol{k}) &= \boldsymbol{\hat{k}}_i\boldsymbol{\hat{k}}_j \\
P^{\perp}_{ij}(\boldsymbol{k}) &= \delta_{ij} - \boldsymbol{\hat{k}}_i\boldsymbol{\hat{k}}_j \\
\end{aligned}
$$
