---
title: 矢量函数分解
mathjax: true
toc: true
category:
  - Math
tags:
abbrlink: 2388637373
date: 2018-10-08 16:34:27
---

结论：**纵向无旋，横向无源**

----------
任意给定一个矢量函数 $w(\overrightarrow{x})=(w_1(\overrightarrow{x}),w_2(\overrightarrow{x}),w_3(\overrightarrow{x}))$。在傅立叶空间中，$\overrightarrow{k}$模式的系数$w(\overrightarrow{k})=(w_1(\overrightarrow{k}),w_2(\overrightarrow{k}),w_3(\overrightarrow{k}))$
可以分解为动量$k$的平行和垂直方向。

$$
w = w^{//} + w^{\bot}
$$

且有 $w^{//} // \overrightarrow{k}$ 及 $w^{\bot}\cdot{\overrightarrow{k}} = 0$

回到实空间中，发现 $w(\overrightarrow{x})$ 可以分解为纵向和横向两部分，$w^{//}(\overrightarrow{x})$ 和 $w^{\bot}(\overrightarrow{x})$。分别有如下性质：

- 纵向无旋

$$
\nabla \times w^{//}(\overrightarrow{x}) = 0
$$

- 横向无源

$$
\nabla \cdot w^{\bot}(\overrightarrow{x}) = 0
$$

# 投影算符
傅立叶空间中，$k$ mode 的纵向（横向）投影算符分别为

$$
\begin{aligned}
P^{\parallel}(\vec{k}) &= \hat{k}\otimes\hat{k} \\
P^{\perp}(\vec{k}) &= 1 - \hat{k}\otimes\hat{k} \\
\end{aligned}
$$
或者分量表示
$$
\begin{aligned}
P^{\parallel}_{ij}(\vec{k}) &= \hat{k}_i\hat{k}_j \\
P^{\perp}_{ij}(\vec{k}) &= \delta_{ij} - \hat{k}_i\hat{k}_j \\
\end{aligned}
$$
