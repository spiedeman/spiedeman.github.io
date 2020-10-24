---
title: 逆拉普拉斯算符
mathjax: true
abbrlink: f03e62a1
date: 2020-06-07 10:48:49
category:
  - 物理
tags:
  - Physics
---

# 泊松方程及其解
拉普拉斯算符$\Delta$是$n$维欧几里得空间中的一个二阶微分算符，定义为标量函数$f$的梯度的散度。
$$
\begin{equation}
\label{eq:poisson-equation}
\Delta \varphi = \nabla^2 \varphi = \nabla\cdot\nabla \varphi = f
\end{equation}
$$
物理中通常称为**泊松方程**，一般利用**格林函数**法求解。拉普拉斯算符对应的格林函数满足方程
$$
\begin{equation}
\label{eq:green-function-for-laplacian-operator}
\nabla^2 G(\bm{r},\bm{r^\prime}) = \delta(\bm{r}-\bm{r}^\prime)
\end{equation}
$$
方程$\eqref{eq:green-function-for-laplacian-operator}$的解为
$$
\begin{equation}
G(\bm{r},\bm{r^\prime}) = - \frac{1}{4\pi} \frac{1}{\Abs{\bm{r}-\bm{r^\prime}}} 
\end{equation}
$$
因此**泊松方程**的特解为
$$
\begin{equation}
\label{eq:particular-solution-for-poisson-equation}
\begin{aligned}
\varphi_p(\bm{r}) &= \int G(\bm{r},\bm{r^\prime})f(\bm{r^\prime})d^3\bm{r^\prime} \\
&=-\frac{1}{4\pi}\int
\frac{f(\bm{r^\prime})}{\Abs{\bm{r}-\bm{r^\prime}}}d^3\bm{r^\prime}
\end{aligned}
\end{equation}
$$

若源项$f(\bm{r})$在全空间积分为有限值，则
$$
\begin{equation}
\begin{aligned}
\lim_{r\to\infty}\varphi_p(\bm{r})&=-\frac{1}{4\pi}\lim_{r\to\infty}
\int\frac{f(\bm{r})}{\Abs{\bm{r-r^\prime}}} d^3\bm{r^\prime} \\
&=-\frac{1}{4\pi}
\left(\lim_{r\to\infty}\frac{1}{r}\int
f(\bm{r^\prime})d^3\bm{r^\prime} + \bigo\lrp{\frac{1}{r^2}}
\right) \\
&=-\frac{1}{4\pi}
\lrp{\lim_{r\to\infty}\frac{q}{r}+\bigo\lrp{\frac{1}{r^2}}} \\
&=0
\end{aligned}
\end{equation}
$$
因此精确的表述为，当泊松方程$\eqref{eq:poisson-equation}$的边界条件为：$\varphi(\bm{r})$在无穷远处为零时，解为$\varphi_p(\bm{r})$。当指定非零边界条件时，完整的解为
$$
\begin{equation}
\varphi(\bm{r}) =\varphi_p(\bm{r})+\varphi_c(\bm{r})
\end{equation}
$$
其中$\varphi_c(\bm{r})$为任意**拉普拉斯方程**（无源泊松方程）的解
$$
\begin{equation}
\label{eq:solution-for-boundary-laplace-equation}
\nabla^2\varphi_c(\bm{r}) = 0
\end{equation}
$$

# 逆拉普拉斯算符
当源项$f(\bm{r})$在全空间积分收敛，即选取无穷远处为零的边界条件时，泊松方程的解为
$$
\begin{equation}
\varphi(\bm{r})=\varphi_p(\bm{r})
\end{equation}
$$
在上述前提条件下，定义逆拉普拉斯算符为$\Delta^{-1}$，将其作用于泊松方程$\eqref{eq:poisson-equation}$得到
$$
\begin{equation}
\Delta^{-1}\Delta\varphi(\bm{r})\equiv \varphi(\bm{r}) = \Delta^{-1}f(\bm{r})
\end{equation}
$$
根据$\eqref{eq:particular-solution-for-poisson-equation}$可得
$$
\begin{equation}
\boxed{
\Delta^{-1}f(\bm{r})
=-\frac{1}{4\pi}\int\frac{f(\bm{r^\prime})}{\Abs{\bm{r-r^\prime}}}d^3\bm{r^\prime}}
\end{equation}
$$
容易验证
$$
\begin{equation}
\Delta^{-1}\Delta f(\bm{r})=\Delta\Delta^{-1}f(\bm{r})=f(\bm{r})
\end{equation}
$$

# 应用
利用逆拉普拉斯算符$\Delta^{-1}$，可以将矢量场$\bm{V}(\bm{r})$唯一分解为不相交的两部分
$$
\begin{equation}
\bm{V}=\bm{V}_{\parallel} + \bm{V}_{\bot},\qquad \text{其中}\ \nabla\cdot \bm{V}_{\bot}=\nabla\times
\bm{V}_{\parallel}=0
\end{equation}
$$
直接构造
$$
\begin{equation}
\label{eq:longitudinal}
\bm{V}_{\parallel}=\nabla\Delta^{-1}\nabla\cdot \bm{V}
\end{equation}
$$
观察到$\psi\equiv\Delta^{-1}\nabla\cdot
\bm{V}$是一个标量场，因此$\bm{V}\times \bm{V}_{\parallel}=\bm{V}\times\nabla\psi=0$。横向部分为
$$
\begin{equation}
\bm{V}_{\bot}=\bm{V}-\bm{V}_{\parallel}=\bm{V}-\nabla\Delta^{-1}\nabla\cdot \bm{V}
\end{equation}
$$
可以验证
$$
\begin{equation}
\nabla\cdot \bm{V}_{\bot}=\nabla\cdot\lrb{\bm{V}-\nabla\Delta^{-1}\nabla\cdot \bm{V}}
=\nabla\cdot \bm{V}-\Delta\Delta^{-1}\lrp{\nabla\cdot \bm{V}}
=\nabla\cdot \bm{V}-\nabla\cdot \bm{V}=0
\end{equation}
$$
与$\bm{V}_{\parallel}$一样，$\bm{V}_{\bot}$也存在类似的表达式
$$
\begin{equation}
\label{eq:transverse}
\bm{V}_{\bot}=-\nabla\times\Delta^{-1}\lrp{\nabla\times \bm{V}}
\end{equation}
$$
为了证明$\eqref{eq:transverse}$成立，首先引入两个等式，不过省略证明。
$$
\begin{aligned}
&\nabla\times\Delta^{-1}\lrp{\bm{A}}=\Delta^{-1}\lrp{\nabla\times \bm{A}}\\
&\nabla\Delta^{-1}\lrp{\psi}=\Delta^{-1}\lrp{\nabla\psi}
\end{aligned}
$$
于是
$$
\begin{aligned}
\bm{V}_{\bot}&=-\nabla\times\Delta^{-1}\lrp{\nabla\times\bm{V}}\\
&=-\Delta^{-1}\lrb{\nabla\times\lrp{\nabla\times\bm{V}}} \\
&=\Delta^{-1}\lrb{\Delta\bm{V}-\nabla\lrp{\nabla\cdot\bm{V}}}\\
&=\bm{V}-\Delta^{-1}\nabla\lrp{\nabla\cdot\bm{V}}\\
&=\bm{V}-\nabla\Delta^{-1}\lrp{\nabla\cdot\bm{V}} \\
&=\bm{V}-\bm{V}_{\parallel}
\end{aligned}
$$
以上说明了分解的存在性，接下来说明该分解具有唯一性，并且只需说明纵向部分具有唯一性即可。
对矢量$\bm{V}$取散度，得到关于标量场$\psi$的泊松方程
$$
\begin{equation}
\nabla\cdot \bm{V} = \nabla\cdot\bm{V_{\parallel}} = \Delta \psi
\end{equation}
$$
由于泊松方程具有唯一解，因而矢量分解也具有唯一性。
