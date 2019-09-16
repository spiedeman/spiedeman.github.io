---
title: 记快手笔试大题
mathjax: true
tags:
  - 面试
abbrlink: d82964a7
date: 2019-09-16 21:57:09
---

### 第一题
#### 题目描述
输入一个英文句子，词之间有 `1` 个或若干个空格，句子以英文标点 "." 结尾。
要求颠倒该句子中的词语顺序，并且词之间有且只有一个空格，结尾仍是
"."，结尾的 "." 与前一个单词之间无空格。

#### 代码
```python
#coding=utf-8
import sys
line = sys.stdin.readline().strip().split()
if line[-1] == '.':
    line = line[:-1]
else:
    line[-1] = line[-1][:-1]

print ' '.join(line[::-1]) + '.'
```

### 第二题
#### 题目描述
已知两个字符串`strA`和`strB`，求将`strA`转换成`strB`所需的最小编辑操作次数。许可的编辑操作包括将一个字符替换成另一个字符，插入一个字符，删除一个字符。

本题参考[LeetCode 第72题——编辑距离](https://leetcode-cn.com/problems/edit-distance/)

#### 代码
```python
#coding=utf-8
import sys
strA = sys.stdin.readline().strip()
strB = sys.stdin.readline().strip()
n1, n2 = len(strA), len(strB)
dp = [[0] * (n2 + 1) for _ in range(n1 + 1)]
for j in range(1, n2 + 1):
    dp[0][j] = dp[0][j-1] + 1
for i in range(1, n1 + 1):
    dp[i][0] = dp[i-1][0] + 1
for i in range(1, n1 + 1):
    for j in range(1, n2 + 1):
        if strA[i-1] == strB[j-1]:
            dp[i][j] = dp[i-1][j-1]
        else:
            dp[i][j] = 1 + min(dp[i][j-1], dp[i-1][j], dp[i-1][j-1])
print dp[-1][-1]
```

### 第三题
#### 题目描述
我们知道每一个大于 `1` 的数都一定是质数或者可以用质数的乘积来表示，如
`10=2*5`。现在请设计一个程序，对于给定的一个 `(1, N]` 之间的正整数（`N`
取值不超过 `10` 万），你需要统计 `(1, N]`
之间所有正整数的质数分解后，所有质数个数的总和。举例，输入数据
`6`，那么满足 `(1, 6])` 的整数为 `2, 3, 4, 5,
6`，各自进行质数分解后为：`2=>2, 3=>3, 4=>2*2, 5=>5,
6=>2*3`。对应的质数个数即 `1, 1, 2, 1, 2`。最后统计总数为 `7`。

本题是完全自己做出来的，复用了求质数的筛法。
> 关键点在 若 $n = a\times b$，则 $f(n) = f(a)+f(b)$。

#### 代码
```python
#coding=utf-8
import sys
def prime_sieve(n):
    """返回所有小于 n 的质数"""
    sieve = [True] * (n // 2)
    for i in range(3, int(n**0.5) + 1, 2):
        if sieve[i // 2]:
            sieve[i *i // 2::i] = [False] * ((n - i * i - 1) // (2 * i) + 1)
    return [2] + [2 * i + 1 for i in range(1, n // 2) if sieve[i]]

n = int(sys.stdin.readline().strip())

primes = prime_sieve(n + 1)
data = {p: 1 for p in primes}
def help(n):
    if n == 1:
        return 0
    if n in data:
        return data[n]
    i, n0 = 0, n
    ans = 0
    while not ans:
        c, p = 0, primes[i]
        while n % p == 0:
            c += 1
            n = n // p
        if c:
            ans = c + help(n)
        i += 1
    data[n0] = ans
    return ans

ans = 0
for m in range(2, n + 1):
    ans += help(m)
print ans
```

### 第四题
#### 题目描述
给定一个数独板的输入，确认当前的填法是否合法。
合法的输入需要满足一下三个条件：
1. 每一行的`9`个格子中是`1-9`的`9`个数字，且没有重复
2. 每一列的`9`个格子中是`1-9`的`9`个数字，且没有重复
3. `9`个`3*3`的小格子中是`1-9`的`9`个数字，且没有重复

空格用`X`表示


本题抄自己的代码[LeetCode 第36题——有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)
#### 代码
```python
#coding=utf-8
import sys
board = []
for _ in range(9):
    board.append(sys.stdin.readline().strip())

def help(board):
    board_t = zip(*board)
    for i in range(3):
        for j in range(3):
            if i == j:
                for k in range(3):
                    col = [n for n in board[3*i+k] if n!='X']
                    if len(col) != len(set(col)):
                        return False
                    row = [n for n in board_t[3*i+k] if n!='X']
                    if len(row) != len(set(row)):
                        return False
            block = set()
            for k in range(3):
                for s in board[3*i+k][3*j:3*j+3]:
                    if s != 'X':
                        if s in block:
                            return False
                        block.add(s)
    return True

print 'true' if help(board) else 'false'
```
