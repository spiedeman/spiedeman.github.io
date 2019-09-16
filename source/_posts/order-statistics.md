---
title: 次序统计量
mathjax: true
tags:
  - 数学
  - 统计
abbrlink: 8a864e22
date: 2019-05-28 08:59:11
---
设 $X_1,\dots,X_n$ 独立同分布，概率密度函数与累积分布函数分别为
$f(x;\theta)$ 和 $F(x;\theta)$，$\theta$ 为分布族的参数。为了简化表达式，后文将省略参数 $\theta$，默认为已知常数。设 $X_{(1)}\le \dots\le
X_{(n)}$，为相应的次序统计量。$X_{(i)}$ 不对应某个确定的
$X_j$，因为每次试验，$x_1,\dots,x_n$ 的相对大小关系都不同。再加上对 $X\sim
f(x)$绝对连续型分布的假设，即 $P(X_{(i)}=X_{(j)})=0,i\ne
j;P(X_{(1)}\lt\dots\lt X_{(n)})=1$。下文将在前述基础上进行推导。

**积分恒等式**
$$
\sum^n_{l=i}{n\choose l}p^l(1-p)^{n-l}=i{n\choose i}\int^p_0dt\ 
t^{i-1}(1-t)^{n-i},\quad 0\le t\le p\le 1.
$$

**概率密度函数 $f_{(i)}(x)$**

$f_{(i)}(x)$ 表示 $X_{(i)}\in[x,x+dx]$
的精确到一阶的概率。这对应这样一种情况：有一个 $X$ 落在区间
$[x,x+dx]$，$i-1$ 个 $X$ 落在区间 $[-\infty,x]$，$n-i$ 个 $X$ 落在区间
$[x+dx,\infty]$。二阶及高阶对应的情形为，有两个及以上的 $X$ 落在区间 $[x,x+dx]$，计算 $f_{(i)}(x)$ 不需要考虑。因此有

$$
f_{(i)}(x)=n{n-1\choose i-1}[F(x)]^{i-1}f(x)[1-F(x)]^{n-i}
\tag{1}
$$

**累积分布函数$F_{(i)}(x)$**

$$
\begin{aligned}
    F_{(i)}(x)&=\int^x_{-\infty}dy\ f_{(i)}(y) \\
    &=n{n-1\choose i-1}\int^{F(x)}_0dF(y)\ F(y)^{i-1}[1-F(y)]^{n-i} \\
    &=n{n-1\choose
    i-1}\sum^n_{j=i}\frac{(n-i)!}{(n-j)!}\frac{(i-1)!}{j!}F(x)^j[1-F(x)]^{n-j}\\
    &=\sum^n_{j=i}{n\choose j}F(x)^j[1-F(x)]^{n-j}
\end{aligned}
\tag{2}
$$

这个结果也很好理解，$F_{(i)}(x)$ 表示概率 $\text{P}_\text{r}(X_{(i)}\le
x)$。$X_{(i)}\le x$ 表示至少有 $j\ge i$ 个 $X$ 落在区间
$[-\infty,x]$，对每个 $j$，共有 ${n\choose j}$ 个选择。

**概率密度函数 $f_{(i,j)}(x)$**

考虑 $X_{(i)},X_{(j)}$ 的联合概率密度函数 $f_{(i,j)}(x,y)$，不妨设 $i\lt
j$。类似 $f_{(i)}(x)$ 的推导，可以直接写出 $f_{(i,j)}(x,y)$
$$
f_{(i,j)}(x,y)=2{n\choose i-1}{n-i+1\choose n-j}{j-i+1\choose
2}F(x)^{i-1}[1-F(x)]^{n-j}[F(y)-F(x)]^{j-i-1}f(x)f(y)I\{x\lt y\}
\tag{3}
$$

此处
$$
I\{x\le y\}=
\begin{cases}
    1,& x\le y\\
    0,& x\gt y
\end{cases}
\tag{4}
$$

**累积分布函数 $F_{(i,j)}(x,y)$**

$$
\begin{aligned}
    F_{(i,j)}(x,y)&=\int^x_{-\infty}dx\int^y_{-\infty}dy\ f_{(i,j)}(x,y)\\
    &=2{n\choose i-1}{n-i+1\choose j-i+1}{n-j\choose2}
    \int^x_{-\infty}ds\int^y_{-\infty}dt\ 
    F(s)^{i-1}[F(t)-F(s)]^{j-i-1}[1-F(t)]^{n-i}f(s)f(t) I\{s\lt t\}\\
    &=2{n\choose i-1}{n-i+1\choose j-i+1}{n-j\choose2}
    \int^{F(x)}_0dF(s)\ 
    F(s)^{i-1}\int^{F(y)}_{F(s)}dF(t)[F(t)-F(s)]^{j-i-1}[1-F(t)]^{n-j}I\{s\lt
    y\}\\
    &=2{n\choose i-1}{n-i+1\choose j-i+1}{n-j\choose2}
    \int^{F(x)}_0dF(s)\ 
    F(s)^{i-1}\sum^{n-j}_{k=0}\frac{(n-j)!}{(n-j-k)!}\frac{(j-i-1)!}{(j-i+k)!}[F(y)-F(s)]^{j-i+k}[1-F(y)]^{n-j-k}I\{s\lt y\}\\
    &=i\sum^n_{k=j}{n\choose k}{k\choose i}[1-F(y)]^{n-k}
    \int^{F(x)}_0dF(s)\ F(s)^{i-1}[F(y)-F(s)]^{k-i}I\{x\lt
    y\}\\
    &=i\sum^n_{k=j}{n\choose k}{k\choose i}[1-F(y)]^{n-k}
    \sum^{k-i}_{l=0}\frac{(k-i)!}{(k-i-l)!}\frac{(i-1)!}{(i+l)!}F(x)^{i+l}[F(y)-F(x)]^{k-i-l}I\{x\lt
    y\}\\
    &=\sum^n_{k=j}\sum^k_{l=i}\frac{n!}{(n-k)!(k-l)!l!}F(x)^l[F(y)-F(x)]^{k-l}[1-F(y)]^{n-k} I\{x\lt y\}\\
    &=\sum^n_{k=j}{n\choose k}[1-F(y)]^{n-k}\sum^k_{l=i}{k\choose
    l}F(x)^l[F(y)-F(x)]^{k-l}I\{x\lt y\}
\end{aligned}
\tag{5}
$$

同样，这个结果也很好理解，$F_{(i,j)}(x,y)=\text{P}_\text{r}(X_{(i)}\le
x,X_{(j)}\le y)$。$X_{(j)}\le y$ 表示至少有 $k\ge j$ 个 $X$ 落在区间
$[-\infty,y]$，对每个 $k$，共有 ${n\choose k}$ 个选择。然后，$X_{(i)}\le
x$ 表示至少有 $i\le l\le k$ 个 $X$ 落在区间 $[-\infty, x]$，对每个
$l$，共有 ${k\choose l}$ 个选择。再考虑到需要满足 $x\lt y$，即得 $(5)$
中最后一个等式。


