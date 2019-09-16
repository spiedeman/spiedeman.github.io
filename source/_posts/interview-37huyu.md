---
title: 记三七互娱笔试大题
mathjax: true
tags:
  - 面试
abbrlink: a8055618
date: 2019-09-10 15:23:29
---

**三七互娱**共两道算法大题，总共就一个小时的笔试时间，总感觉时间不够用。最后一题确实因为时间不够没写完，好在思路有了用注释提了一下，不知道能不能酌情给分:joy:。

#### 第一题
**描述**：给一个数组，其中 $1 \le A[i]\le
n$。有些元素出现两次，有些出现一次，返回所有出现两次的元素。

**解答**：
```python
#!/usr/bin/env python
# coding=utf-8
def solution(array):
    count = {}
    for a in array:
        if a not in array:
            count[a] = 1
        else:
            count[a] += 1
    return [key for key in count if count[key] == 2]
```

#### 第二题
**描述**：小明有想设计一个随机算法来听歌，并且希望每首歌被选中的概率正比于它的豆瓣评分。如歌
A 和 B 的评分分别为 $8.5$ 和 $9.3$，则这两首歌被选中的概率的比为
$85:93$。现给出 $1000$ 首歌的豆瓣评分，请设计一个随机算法并实现它。

**解答**：
```python
#!/usr/bin/env python
# coding=utf-8
import random
def solution(songs, scores):
    n = len(songs)
    # 假设评分只有一位小数
    srange = [0]
    for i in range(n):
        srange.append(srange[-1] + int(scores[i] * 10))
    # 生成 [0, srange[-1]] 范围的均匀随机数 rand
    rand = random.uniform(0, 1)
    rand = int(rand * srange[-1])
    # 二分法查找 rand 落在 srange 的哪个区间中
    i, j = 0, n
    m = (i + j) // 2
    while i < j:
        if srange[m] <= rand < srange[m+1]:
            return songs[m]
        if srange[m] > rand:
            j = m
        else:
            i = m + 1
        m = (i + j) // 2
```
