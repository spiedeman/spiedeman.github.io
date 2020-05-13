---
title: 记小米一面算法题——人民币大写金额转换
mathjax: true
category: 算法
tags:
  - 面试
abbrlink: 8bd25443
date: 2019-09-09 22:01:15
---
30分钟小米算法工程师一面，由于没有项目经历面试官给了一道算法题，现撸代码。毫无意外，快速码代码向来不是我的强项。

特此记录失败史

### 题目描述
根据人民币大写金额规范，将浮点数转换为人民币读法字符串需要注意以下几点：
1. 数字中间有连续一个或以上个 0 时，只写一个**零**。如 ¥ 140,090.3，应写作人民币十四万零玖拾元叁角。
2. 角位非 0 时，**元**后面可以不加**零**。如 ¥ 1690.32，可以写作人民币壹仟陆佰玖拾元叁角[零]贰分。
3. 角位是 0，分位不是 0 时，大写金额**元**后面应写**零**。如 ¥ 325.04，应写为人民币叁佰贰拾元零肆分。
4. 角和分都是 0 时，**元**后面应写上**整**或**正**。如 ¥ 1234.00，应写为人民币壹仟贰佰叁拾肆元（整/正）。

**注意**：最小单位为**分**。

### 题解
中文数字读法以 4 个数字为一个组，辅以单位如**万、亿、兆、京**等。这里取到最高为**亿**。由于每组单位不同，一度不知如何处理。因为金额大小没有限制，所以数字位数可以很大，如果根据单位用`if...else...`语句，是行不通的。
后来发现，还是可以使用**递归**来解决问题。

至于**零**的处理，可以设定字符串替换规则，使得处理后满足规范。

### 代码
```python
ch_num = ['零', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖']
ch_num = dict(zip('0123456789', ch_num))

ch_unit = ['', '万', '亿']
ch_unit = dict(zip(range(3), ch_unit)) 

def convert_int(intPart):
    """ 转换整数部分
    intPart: str
    return: str
    """
    if len(intPart) <= 4:
        # 若长度小于 4， 则补足到 4 位。
        ans = [ch_num[c] for c in '0'*(4-len(intPart))+intPart]
        if ans[-1] == ch_num['0']:
            ans = '{}仟{}佰{}拾'.format(*ans[:-1])
        else:
            ans = '{}仟{}佰{}拾{}'.format(*ans)
        # 处理中间有 0 的情况
        ans = ans.replace('零仟', ch_num['0']) 
        ans = ans.replace('零佰', ch_num['0']) 
        ans = ans.replace('零拾', ch_num['0']) 
        # 处理中间连续多个 0，末尾有零的情况
        ans = re.sub('零+', ch_num['0'], ans)
        ans = re.sub('零+$', ch_unit[0], ans)
        # 若开头有 0 且整数部分长度小于 4，则丢弃开头的’零‘（只有 1 个）。
        return ans[4!=len(intPart):]
    else:
        # 金额数字长度可以写成 n = 4 * u + r
        u, r = len(intPart) // 4, len(intPart) % 4
        # 若 r = 0，则 u = u - 1，至少先处理开头 4 位。之后再看 u，
        # 若 u > 2，则先处理不包括最后 8 位的部分，否则就先只处理开头 4 位。
        i = r * (r != 0) + 4 * (max(u - 2 - (r == 0), 0) + (r == 0))
        # 于是分成两部分递归处理 intPart[:i] 和 intPart[i:]
        left, right = convert_int(intPart[:i]), convert_int(intPart[i:])
        # 若左部分返回空字符串 ''，则不加单位，即 j = 0 and ch_unit[j] = ''。
        # 又因为最大单位为“亿”，所以 0 <= j <= 2
        j = min(u - (r == 0), 2) if left else 0
        # 得到单位
        unit = ch_unit[j]
        # 返回最终结果
        return '{}{}{}'.format(left, unit, right)

def convert_decimal(decimalPart):
    """ 处理小数部分，容易多了
    decimalPart: str
    return: str
    """
    ans = [ch_num[c] for c in decimalPart + '0' * (2 - len(decimalPart))]
    ans = '{}角{}分'.format(*ans)
    ans = ans.replace('零角', ch_num['0'])
    ans = ans.replace('零分', ch_unit[0])
    ans = re.sub('零+$', ch_unit[0], ans)
    # 因为使用递归处理整数部分，所以整数部分单位“元”放在这里处理。
    return '元{}'.format(ans or '整')

def convert_RMB(money):
    """ 将浮点数转换为大写人民币规范读法
    money: str
    return: str
    """
    if '.' in money:
        intPart, decimalPart = money.split('.')
    else:
        intPart, decimalPart = money, ch_unit[0]
    return '人民币{}{}'.format(convert_int(intPart), convert_decimal(decimalPart))
```
