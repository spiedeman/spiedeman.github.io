---
title: Mukhanov-Sasaki 方程
mathjax: true
category:
  - 物理
tags:
  - Physics
  - Cosmology
abbrlink: 579653d0
date: 2020-06-18 19:28:55
---
## 标量扰动
记录一下宇宙学中**规范扰动理论**中的**Mukhanov-Sasaki(MS)**方程。

$$
\begin{equation}
\label{eq:ms-equation}
u^{\pprime}-c_s^2\Delta u - \frac{\theta^{\pprime}}{\theta}u = 0.
\end{equation}
$$

以标量扰动以及完美流体为例，验证**MS方程**$\eqref{eq:ms-equation}$。

为了叙述方便，以下将**规范扰动**简称为**扰动**，在物理量上添加横线代表相对应的规范不变量，若不至于产生误解，则省略横线。


完美流体的能动张量为
$$
\begin{equation}
\label{eq:perfect-fluid-energy-mommentum-tensor}
T^{\alpha}_{\ \beta} = (\varepsilon + p)
u^{\alpha}u_{\beta}-p\delta^{\alpha}_{\ \beta}.
\end{equation}
$$
相应的一阶扰动为
$$
\begin{equation}
\label{eq:guage-invariant-perturbation-for-perfect-fluid}
\overline{\delta T}^{0}_0 = \overline{\delta\varepsilon},\quad
\overline{\delta T}^{0}_i =
\frac{1}{a}(\varepsilon_0+p_0)(\overline{\delta u}_{\parallel i}+\overline{\delta
u}_{\bot i}), \quad
\overline{\delta T}^i_j = -\overline{\delta p}\delta^i_j.
\end{equation}
$$
其中$\overline{\delta\varepsilon},\overline{\delta u}_{\parallel
i},\overline{\delta p}$对应标量扰动，而$\overline{\delta u}_{\bot
i}$有旋无源，贡献为矢量扰动。

因为$\delta T^i_{\ j}=0$，当$i\ne
j$。因而$(ij)$分量对应的标量扰动方程约化为
$$
\begin{equation}
(\Phi-\Psi)_{,ij} = 0 \qquad (i\ne j).
\end{equation}
$$
因而有$\Phi = \Psi$。故而标量扰动满足的规范方程组的形式具体如下
$$
\begin{align}
\Delta \Phi - 3\mathcal{H}(\Phi^{\prime}+\mathcal{H}\Phi) =4\pi
Ga^2\overline{\delta\varepsilon},
\label{eq:scalar-guage-invariant-perturbation-equations-00} \\
(a\Phi)_{,}^{\prime}=4\pi Ga^2(\varepsilon_0+p_0)\overline{\delta
u}_{\parallel i},
\label{eq:scalar-guage-invariant-perturbation-equations-0i} \\
\Phi^{\pprime}+3\mathcal{H}\Phi^{\prime}+(2\mathcal{H^\prime+H^2}\Phi) =
4\pi Ga^2\overline{\delta p}.
\label{eq:scalar-guage-invariant-perturbation-equations-ij}
\end{align}
$$
根据热力学可知压强为内能和熵的函数，$p=p(\varepsilon,
S)$。压强的涨落$\overline{\delta p}$可以表示为
$$
\begin{equation}
\label{eq:sound-speed}
\overline{\delta p}=c_s^2\overline{\delta\varepsilon}+\tau\delta S.
\end{equation}
$$
这里只考虑绝热扰动（$\delta S=0$），故$\overline{\delta p}=c_s^2\overline{\delta\varepsilon}$。
联合方程$\eqref{eq:scalar-guage-invariant-perturbation-equations-00}$和$\eqref{eq:scalar-guage-invariant-perturbation-equations-ij}$
得到引力势$\Phi$满足的动力学方程
$$
\begin{equation}
\label{eq:bardeen-equation}
\Phi^{\pprime}+3(1+c_s^2)\mathcal{H}\Phi^{\prime}-c_s^2\Delta\Phi+\lrp{2\mathcal{H^\prime}+(1+3
c_s^2)\mathcal{H^2}}\Phi = 0
\end{equation}
$$
方程$\eqref{eq:bardeen-equation}$称为**bardeen**方程，这个方程并不总是存在解析解。
不过有可能得到长波极限和短波极限下的渐进解。这可以通过引入新变量，改写方程形式消灭其中的摩擦项做到。

$$
\begin{equation}
\begin{aligned}
u&\equiv \exp\lrp{\frac{3}{2}\int (1+c_s^2)\mathcal{H}d\eta}\Phi \\
&=\exp\lrp{-\frac{1}{2}\int \lrp{1+\frac{p_0^{\prime}}{\varepsilon_0^{\prime}}}
\frac{\varepsilon_0^{\prime}}{\varepsilon_0+p_0}d\eta}\Phi \\
&=\frac{\Phi}{(\varepsilon_0+p_0)^{1/2}}.
\end{aligned}
\end{equation}
$$
其中用到了$c_s^2=p_0^{\prime}/\varepsilon_0^{\prime}$，连续性方程 $\varepsilon_0^{\prime}=-3\mathcal{H}(\varepsilon_0+p_0)$
。以及
$$
\begin{equation}
\theta\equiv \frac{1}{a}\lrp{1+\frac{p_0}{\varepsilon_0}}^{-1/2}
=\frac{1}{a}\lrp{\frac{2}{3}\lrp{1-\frac{\mathcal{H^{\prime}}}{\mathcal{H^2}}}}^{-1/2}.
\end{equation}
$$
其中用到了背景方程
$$
\begin{equation}
\label{eq:background-equation}
\mathcal{H^2}=\frac{8\pi G}{3}a^2\varepsilon_0,\quad
\mathcal{H^2-H^\prime}=4\pi Ga^2(\varepsilon_0+p_0).
\end{equation}
$$
经过复杂的计算，**bardeen方程**$\eqref{eq:bardeen-equation}$可以重写为不含摩擦项的**MS方程**
$$
\begin{equation}
u^{\pprime}-c_s^2\Delta u-\frac{\theta^{\pprime}}{\theta} u = 0.
\end{equation}
$$

接下来，只进行验证**MS方程**和**bardeen方程**$\eqref{eq:bardeen-equation}$等价。

首先注意到
$$
\begin{equation}
\begin{aligned}
u^{\pprime}-\frac{\theta^{\pprime}}{\theta}u &=
\lrp{\frac{u}{\theta}}^{\pprime}\theta+2\lrp{\frac{u}{\theta}}^{\prime}\theta^{\prime} \\
&=\lrp{\lrp{\frac{u}{\theta}}^{\prime}\theta}^{\prime} +
\lrp{\frac{u}{\theta}}^{\prime}\theta^{\prime} \\
&= v^{\prime} + v\frac{\theta^{\prime}}{\theta}. \quad
\text{令}(v=\lrp{\frac{u}{\theta}}^{\prime}\theta).
\end{aligned}
\end{equation}
$$
故**MS方程**等价于
$$
\begin{equation}
c_s^2\Delta u=v^{\prime}+v\frac{\theta^{\prime}}{\theta}.
\end{equation}
$$
$v$和$v^{\prime}$的表达式分别为
$$
\begin{equation}
\begin{aligned}
&\begin{aligned}
v\equiv \lrp{\frac{u}{\theta}}^{\prime}\theta &=
\frac{1}{\lrp{\varepsilon_0+p_0}^{1/2}}\lrp{\Phi^{\prime}+\Phi\lrp{\mathcal{H}-\frac{1}{2}\frac{\varepsilon_0^{\prime}}{\varepsilon_0}}} \\
&= \frac{1}{\lrp{\varepsilon_0+p_0}^{1/2}} f.
\end{aligned} \\
&\begin{aligned}
v^{\prime}=\frac{1}{\lrp{\varepsilon_0+p_0}^{1/2}}\lrp{f^{\prime}-\frac{1}{2}
f\frac{\lrp{\varepsilon_0+p_0}^{\prime}}{\varepsilon_0+p_0}}.
\end{aligned}
\end{aligned}
\end{equation}
$$

故**MS方程**等价于
$$
\begin{equation}
c_s^2\Delta u =
\frac{1}{\lrp{\varepsilon_0+p_0}^{1/2}}\lrp{f^{\prime}
+f\lrp{\frac{\theta^{\prime}}{\theta}-\frac{1}{2}
\frac{\lrp{\varepsilon_0+p_0}^{\prime}}{\varepsilon_0+p_0}}}.
\end{equation}
$$
为了后续计算方便，先计算几个表达式
$$
\begin{align}
&\frac{\theta^{\prime}}{\theta} = 
\frac{\mathcal{H}}{2}(1+3
c_s^2)+\frac{1}{2}\frac{\varepsilon_0^{\prime}}{\varepsilon_0}, \\
&\frac{\lrp{\varepsilon_0^{\prime}}}{\varepsilon_0+p_0}
=\frac{\lrp{1+\frac{p_0^{\prime}}{\varepsilon_0^{\prime}}}\varepsilon_0^{\prime}}{\varepsilon_0+p_0}
=-3\mathcal{H}(1+c_s^2), \\
&\frac{\varepsilon_0^{\prime}}{\varepsilon_0}
=-3\mathcal{H}\lrp{1+\frac{p_0}{\varepsilon_0}}
=-2\mathcal{H}\lrp{1-\frac{\mathcal{H}^{\prime}}{\mathcal{H}^2}}, \\
&\lrp{\frac{\varepsilon_0^{\prime}}{\varepsilon_0}}^{\prime} 
=\frac{\varepsilon_0^{\prime}}{\varepsilon_0}
\lrp{\mathcal{\frac{H^{\prime}}{H}}-3\mathcal{H}(1+c_s^2)-
\frac{\varepsilon_0^{\prime}}{\varepsilon_0}}.
\end{align}
$$

故**MS方程**等价于
$$
\begin{equation}
\begin{aligned}
c_s^2\Delta\Phi 
&= f^{\prime} + 
f\lrp{\frac{\theta^{\prime}}{\theta}-\frac{1}{2}
\frac{\lrp{\varepsilon_0+p_0}^{\prime}}{\varepsilon_0+p_0}} \\
&=
\Phi^{\pprime}+\Phi^{\prime}\lrp{\mathcal{H}-\frac{1}{2}\frac{\varepsilon_0^{\prime}}{\varepsilon_0}}
+\Phi\lrp{\mathcal{H}-\frac{1}{2}\frac{\varepsilon_0^{\prime}}{\varepsilon_0}}^{\prime} \\
&\
+\lrp{\Phi^{\prime}+\Phi\lrp{\mathcal{H}-\frac{1}{2}\frac{\varepsilon_0^{\prime}}{\varepsilon_0}}}
\lrp{\mathcal{H}\lrp{2+3
c_s^2}+\frac{1}{2}\frac{\varepsilon_0^{\prime}}{\varepsilon_0}} \\
&=\Phi^{\pprime} + 3\mathcal{H}(1+3 c_s^2)\Phi^{\prime} \\
&\
+\Phi\lrp{\mathcal{H^{\prime}}-\lrp{\frac{1}{2}
\frac{\varepsilon_0^{\prime}}{\varepsilon_0}}^{\prime}
+\lrp{\mathcal{H}-\frac{1}{2}\frac{\varepsilon_0^{\prime}}{\varepsilon_0}}
\lrp{\mathcal{H}\lrp{2+3
c_s^2}+\frac{1}{2}\frac{\varepsilon_0^{\prime}}{\varepsilon_0}}},
\end{aligned}
\end{equation}
$$

经过简单但复杂的计算，上式最后一行中的括号为
$$
\begin{equation}
2\mathcal{H^{\prime}}+\mathcal{H^2}\lrp{1+3 c_s^2},
\end{equation}
$$
故**MS方程**确实等价于**bardeen方程**。显然**MS方程**是**规范不变方程**，引入的变量
$u$和$\theta$均为**规范不变量**。

## 曲率扰动
**MS方程**能够被写成更紧凑的形式
$$
\begin{equation}
\lrb{\lrp{\frac{u}{\theta}}^{\prime}\theta^2}^{\prime} = c_s^2\theta\Delta
u
\end{equation}
$$
在**长波极限**下，散度项$\Delta
u$可以被忽略，故而我们找到了一个在超视界区域为常数的规范不变量
$$
\begin{equation}
\label{eq:curvature-perturbation}
\begin{aligned}
\zeta &\equiv \frac{2}{3}\lrp{\frac{u}{\theta}}^{\prime}\theta^2 \\
&= \frac{v}{z}
\end{aligned}
\end{equation}
$$
其中
$$
\begin{equation}
v=\lrp{\frac{u}{\theta}}^{\prime}\theta,\quad
z = \frac{1}{\theta}.
\end{equation}
$$
该变量即是文献中常见的标量曲率扰动$\mathcal{R}$，通常在暴胀过程中会计算$\mathcal{R}$的功率谱$P_{\mathcal{R}}(k)$。

前面验证**MS方程**的过程中从数学的角度看，构造出的四个规范不变量$u,v,\theta,z$
满足了如下两个方程
$$
\begin{equation}
\label{eq:middle-equations}
c_s^2\Delta u=z\lrp{\frac{v}{z}}^{\prime},\quad
v=\theta\lrp{\frac{u}{\theta}}^{\prime}.
\end{equation}
$$
满足上述方程的函数$u$，在消除辅助函数$v$后自然得到**MS方程**。观察$\zeta$的定义
式$\eqref{eq:curvature-perturbation}$后，更希望找到$v$满足的方程。从方程组$\eqref{eq:middle-equations}$出发可以得到
$$
\begin{equation}
v^{\pprime}-c_s^2\Delta v-\frac{z^{\pprime}}{z}v= 0
\end{equation}
$$
巧合地是$u$和$v$满足的方程形式相同。实际上$v$满足的方程才是文献中提到的**MS方程**，$u$满足的方程只是形式相同，本质还是**bardeen方程**。
在暴胀过程中，根据规范不变量的定义
$$
\begin{equation}
z=\frac{a\lrp{\varepsilon+p}^{1/2}}{H}=\frac{a\dot{\varphi}}{H}
=\frac{a\varphi^{\prime}}{\mathcal{H}}.
\end{equation}
$$
其中$\varphi$是暴胀场。到这一步时，曲率扰动的功率谱就就呼之欲出了
$$
\begin{equation}
\label{eq:curvature-perturbation-power-spectrum}
P_{\zeta}(k)=P_{\mathcal{R}}(k) = \frac{k^{3}}{2\pi^2}
\left\lvert \frac{v_k}{z}\right\rvert ^2
\end{equation}
$$
暴胀过程中，变换到傅立叶空间中，$v$的$k$模$v_k$满足贝塞尔方程
$$
\begin{equation}
\label{eq:bessel-equation}
v_k^{\pprime}+\lrp{k^2-\frac{\nu^2-1/4}{\eta^2}}v_k = 0,
\end{equation}
$$
式中$\nu=3/2+2\varepsilon_H-\eta_H$。考虑到平面波边值条件
$$
\begin{equation}
\label{eq:plane-wave-boundary-condition}
v_k(\eta)\rightarrow \frac{1}{2k}e^{-ik\eta},\quad k\rightarrow
\infty
\end{equation}
$$
贝塞尔方程$\eqref{eq:bessel-equation}$的解为
$$
\begin{equation}
\label{eq:solution-for-ms-equation-in-inflation}
v_k(\eta)
=\frac{\sqrt{\pi}}{2}e^{i(\nu+1/2)\pi/2}\sqrt{-\eta}H_{\nu}^{(1)}(-k\eta).
\end{equation}
$$
式中$H_{\nu}^{(1)}$为第一类汉克尔函数。对于超视界扰动
$$
\begin{align}
\label{eq:for-super-horizon}
H_{\nu}^{(1)}(x\ll 1) \sim \sqrt{\frac{2}{\pi}}e^{-i\pi/2}
2^{\nu-3/2}\frac{\Gamma(\nu)}{\Gamma(3/2)}x^{-\nu}, \\
v_k(\eta) = e^{i(\nu-1/2)\pi/2}2^{\nu-3/2}
\frac{\Gamma(\nu)}{\Gamma(3/2)}\frac{1}{\sqrt{2k}}(-k\eta)^{1/2-\nu}.
\end{align}
$$
故曲率扰动的功率谱为
$$
\begin{equation}
\begin{aligned}
P_{\mathcal{R}}(k) &= \frac{k^{3}}{2\pi^2}\left\lvert \frac{\nu_k}{z}\right\rvert ^2 \\
&= 2^{2\nu-3}\lrp{\frac{\Gamma(\nu)}{\Gamma(3/2)}}^2
\lrp{\frac{H}{\dot{\varphi}_0}}^2
\lrp{\frac{H}{2\pi}}^2
\lrp{\frac{k}{aH}}^{3-2\nu} \Bigg\lvert_{k=aH}.
\end{aligned}
\end{equation}
$$

