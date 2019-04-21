---
title: euler-lagrange-equation
mathjax: true
toc: true
category:
  - QFT
tags:
  - 最小作用量
  - 诺特定理
abbrlink: 4141491110
date: 2018-12-02 20:12:33
---

# 作用量
作用量 $S$ 是拉氏密度$\mathcal{L}$ 在四维时空中的积分，
$$
S = \frac{1}{c}\int d^4x \mathcal{L}(\phi,\partial_\mu\phi)
$$

# 场的变分
对场的变分的贡献共来自两个方面，1、坐标无穷小变换；2、场自身的无穷小变换。下面分别讨论这两种情况。

1、坐标无穷小变换
$$
x^{\mu}\rightarrow x'^{\mu}= x^{\mu} + \delta x^{\mu}
$$

通常谈论的场的变分是指坐标不变时的变分 $\bar{\delta}$，
$$
\bar{\delta}\phi(x) = \phi'(x) - \phi(x)
$$
根据标量场 $\phi(x)$ 在坐标变换下的性质 $\phi'(x')=\phi(x)$，容易得到
$$
\bar{\delta}\phi = -\delta x^{\mu}\partial_{\mu}\phi
$$

2、场自身的无穷小变换
在没有坐标变换的情况下，场 $\phi$ 产生了一个无穷小变换。此时坐标不变时的变分为
$$
\bar{\delta}\phi = \phi'(x) - \phi(x) = \delta\phi
$$
注意到这里用 $\delta$ 表示场自身的变分。通常情况下遇到的变分就是这种情况。

3、一般情况
如果同时考虑到**坐标变换**以及**场自身的变分**，则标量场 $\phi(x)$ 满足的性质需修改为
$$
\phi'(x') = \phi(x) + \delta\phi(x)
$$
故坐标不变时的变分为
$$
\begin{aligned}
\bar{\delta}\phi(x) &= \phi'(x) - \phi(x) \\
                &= \phi(x-\delta{x}) + \delta\phi(x-\delta{x}) - \phi(x) \\
                &\approx \delta\phi(x) - \delta{x^\mu}\partial_{\mu}\phi(x) \\
\end{aligned}
$$

根据上述讨论可知，场自身的变分一般情况下并非坐标不变时的变分 ($\bar{\delta} \neq \delta$)，
只有当不存在坐标变换的情况下两者才相等 ($\bar{\delta} = \delta$)。

## 拉氏密度的变分
一般情况下，作为标量场的拉氏密度的坐标不变时的变分为
$$
\begin{aligned}
\bar{\delta}\mathcal{L}(x) &= \mathcal{L}'(x) - \mathcal{L}(x) \\
                           &= \mathcal{L}(x-\delta x) -
                           \delta\mathcal{L}(x-\delta x) - \mathcal{L}(x) \\
                           &= \delta\mathcal{L}(x) -
                           \delta{x^\mu}\partial_{\mu}\mathcal{L} \\
                           &=
                           \frac{\partial\mathcal{L}}{\partial\phi}\bar{\delta}\phi
                           +
                           \frac{\partial\mathcal{L}}{\partial\partial_{\mu}\phi}\bar{\delta}\partial_{\mu}\phi
\end{aligned}
$$

<!-- 其中 $\delta\mathcal{L}(x)$ 可展开为 -->
<!-- $$ -->
<!-- \begin{align} -->
<!-- \delta\mathcal{L}(x) &= \frac{\partial\mathcal{L}}{\partial\phi}\delta\phi + -->
<!-- \frac{\partial\mathcal{L}}{\partial\partial_{\mu}\phi}\delta{\partial_{\mu}\phi} -->
<!-- \end{align} -->
<!-- $$ -->

## 作用量的变分
$$
\begin{aligned}
\delta S &= \int \delta(d^4 x)\mathcal{L} + \int d^4 x\, \delta{\mathcal{L}} \\
        &= \int d^4x \left(\bar{\delta}\mathcal{L}+\delta x^{\mu}\partial_{\mu}{\mathcal{L}} + \mathcal{L}\partial_{\mu}\delta{x^\mu}\right) \\
        &= \int d^4x \left(\bar{\delta}\mathcal{L} + \partial_{\mu}(\mathcal{L}\delta x^{\mu})\right) \\
        &= \int d^4x \left(\frac{\partial\mathcal{L}}{\partial\phi}\bar{\delta}\phi +
        \frac{\partial\mathcal{L}}{\partial\partial_{\mu}\phi}\bar{\delta}\partial_{\mu}\phi + \partial_{\mu}(\mathcal{L}\delta x^{\mu})\right) \\
        &= \int d^4x \left[\left(\frac{\partial\mathcal{L}}{\partial\phi} -
        \partial_{\mu}\frac{\partial\mathcal{L}}{\partial\partial_{\mu}\phi}\right)\bar{\delta}\phi +
        \partial_{\mu}\left(\frac{\partial\mathcal{L}}{\partial\partial_{\mu}\phi}\bar{\delta}\phi+\mathcal{L}\delta x^{\mu}\right) \right]
\end{aligned}
$$

## 拉格朗日方程
假设坐标不变，只考虑场量的变分，则有
$$
\begin{aligned}
& \bar{\delta}\phi = \delta\phi \\
& \delta x^{\mu} = 0
\end{aligned}
$$

由表面项在4维时空边界上为零及变分 $\bar{\delta}\phi$ 任意，作用量的变分取极值的条件给出拉格朗日方程
$$
\partial_{\mu}\frac{\partial\mathcal{L}}{\partial\partial_{\mu}\phi} -
\frac{\partial\mathcal{L}}{\partial\phi} = 0
$$

## 诺特定理
若场 $\phi(x)$
满足拉格朗日方程，则作用量的变分为某个表面项的4维积分。若场在变换（包括坐标变换和自身变换）前后均满足拉格朗日方程，且保持作用量不变。则
由
$$
\delta S = \frac{1}{c} \int d^4x \partial_{\mu}j^{\mu} = 0
$$
以及
$$
j^{\mu} = \frac{\partial\mathcal{L}}{\partial\partial_{\mu}\phi}\bar{\delta}\phi
+ \mathcal{L}\delta x^{\mu}
$$
得到一个对应该变换的守恒流 $j^{\mu}$。将 $j^{\mu}$ 改写为
$$
j^{\mu} = \frac{\partial\mathcal{L}}{\partial\partial_{\mu}\phi}\delta\phi -
\left(\frac{\partial\mathcal{L}}{\partial\partial_{\mu}\phi}\partial_{\nu} -
\mathcal{L}g^{\mu}_{\nu}\right)\delta x^{\nu}
$$

$\delta\phi$ 和 $\delta x^{\mu}$ 现在是保持拉格朗日方程不变的变换。
