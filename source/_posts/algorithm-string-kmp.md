---
title: 字符串匹配之KMP算法
mathjax: true
toc: true
category:
  - 算法
tags:
  - 字符串匹配
abbrlink: b5ae7cab
date: 2019-08-18 11:20:11
---

> **问题**：给定长度为 $n$，$m$ 的**目标**字符串`s`和**模式**字符串`p`，判断`p`是否是`s`的子串。若是，
> 返回匹配位置；否则返回 $-1$。

#### 暴力算法
- 模式串从目标串的**第一个元素**开始匹配
- 匹配失败，则**目标串**右移一位重新判断是否匹配模式串

{% asset_img bf-1.gif 暴力算法最优情况 %}

------
最坏情况为 $O(mn)$。

{% asset_img bf-2.gif 暴力算法最坏情况 %}

------
#### KMP
通过观察我们可以提问：
> 是否遇到不匹配时，只能右移 **一位** 重新判断？

当模式串的第 $k$ 位失配时，前 $k-1$ 位是已经匹配的。那么可以问这样一个问题：
> 右移多少位之后匹配的指针不必往前移动，在当前位置的基础上继续往后开始匹配，直到匹配成功或者再次遇到失配位？

{% asset_img kmp-1.gif kmp算法示意图 %}

-----
首先提出一个概念：字符串前缀和后缀的**最大公共长度**。假设最大公共长度为 $j$，字符串长度为 $n$，那么右移 $n-j$ 位后，前 $j$ 位是已经匹配了的。

{% asset_img kmp-2.jpg 前后缀最大公共长度 %}

-----
**最大公共长度**中的**最大**表示为了匹配需要右移的**最少**位数。也许右移更多位也能匹配，但可能会漏掉解。

-----
#### Next数组
现在我们清楚了，为了使匹配指针不往前移，有效利用已经获得的匹配信息，我们需要对模式串进行预处理。直观地说就是我们需要知道当匹配到模式串的第几个元素失配时，应该右移多少位。这就是**Next数组**的含义。

在 $\text{Python}$ 中，数组下标从 $0$ 开始，以此为前提。假设下标为 $j$（第 $j+1$ 位）失配，则匹配长度为 $j$。又设最大公共长度为 $i$，则应右移 $j-i$ 位，使失配位为模式串第 $i+1$ 位，即下标为 $i$。故有 $\text{next}[j]=i$。

问题变为 **如何快速计算最大公共长度 $i$** ？

**Next数组**的算法步骤如下图所示，稍后再详细讨论其过程。

{% asset_img next-1.gif Next数组算法步骤示意图 %}

-----
假设已知模式串 $\text{P}$ 有 $\text{next}[q]=k$，说明前 $q$ 位字符串的最大公共长度为 $k$。即有 $\text{P}[0]=\text{P}[q-k],\cdots, \text{P}[k-1]=\text{P}[q-1]$。问 $\text{next}[q+1]=?$

{% asset_img next-2.png Next数组计算 %}

若 $\text{P}[q]=\text{P}[k]$，则$\text{next}[q+1]=\text{next}[q]+1=k+1$。若  $\text{P}[q]\neq \text{P}[k]$，那么 $\text{next}[q]$ 等于多少？


{% asset_img next-3.png Next数组计算 %}

假设 $\text{next}[q+1]=j+1$，则有 
$$\text{P}[0] = \text{P}[q-j],\cdots,\text{P}[j-1]=\text{P}[q-1],\text{P}[j]=\text{P}[q]
$$
又因为 $\text{next}[q]=k$，所以有 
$$
\text{P}[k-j]=\text{P}[q-j],\cdots,\text{P}[k-1]=\text{P}[q-1]
$$
由上面两式可知，
$$
\text{P}[0]=\text{P}[k-j],\cdots,\text{P}[j-1]=\text{P}[k-1]
$$

这说明 $j$ 最大可以取到 $\text{next}[k]$，也就是我们应该比较 $\text{P}[q]$ 和 $\text{P}[\text{next}[\text{next}[q]]]$。经过一点思考可知，$\text{P}[q]$ 将依次与序列 $\{\text{P}[\text{next}[q]],\text{P}[\text{next}[\text{next}[q]]],\cdots,\text{P}[\text{next}^{(n)}[q]] \}$ 中的元素进行比较，直到**相等**或$\text{next}^{(n)}[q]=0$。

#### Python 实现

```python
def Next(p):
    """
    返回模式串的Next（优化）数组
    """
    ans = [0 for _ in p]
    for i in range(1, len(p) - 1):
        k = ans[i]
        while k:
            if p[k] == p[i]:
                ans[i+1] = k + 1
            else:
               k = ans[k]
        if ans[i+1] == 0 and p[0] == p[i]:
            ans[i+1] = 1
    # 第二个for循环给出优化KMP算法的Next数组
    for i in range(2, len(p)):
        if ans[i] == i - 1:
            ans[i] = 0
    return ans
```

```python
def KMP(s, p):
    """
    返回模式串 p 在目标串 s 中第一次出现的下标，否则返回 -1
    """
    _next = Next(p)

    i, j = 0, 0
    ns, np = len(s), len(p)
    while i < ns and j < np:
        if s[i] == p[j]:
            i, j = i + 1, j + 1
        else:
            if j == 0:
                i = i + 1
            else:
                # 遇到失配时快速移动模式串，优于暴力算法的地方
                j = _next[j]
    if j == np:
        return i - j
    return -1
```
