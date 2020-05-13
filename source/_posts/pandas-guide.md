---
title: Pandas 简介
mathjax: false
toc: true
abbrlink: 1982572123
date: 2018-09-21 14:22:14
category:
  - Python
tags:
  - Python
  - Pandas
---

**学习目标**

- 大致了解*pandas*库的`DataFrame`和`Series`数据结构
- 存取和处理`DataFrame`和`Series`中的数据
- 将`CSV`数据导入`pandas`库的`DataFrame`
- 对`DataFrame`重建索引来随机打乱数据

[<u>*pandas*</u>](http://pandas.pydata.org) 是一种存列数据分析`API`。它是用于处理和分析输入数据的强大工具，很多机器学习框架都支持将*pandas*数据结构作为输入。本文只介绍它的核心概念。有关更完整的参考，请访问 [<u>*pandas*文档网站</u>](http://pandas.pydata.org/pandas-docs/stable/index.html)。


# 基本概念

导入*pandas* API 并输出相应的 API 版本：

```python
from __future__ import print_function

import pandas as pd
pd.__version__
```
```python
u'0.22.2'
```

*pandas*中主要的数据被实现为以下两类：

- **DataFrame**，可以想象成一个`Excel`表，包含多个行和已命名的列。
- **Series**，它是单一列。DataFrame 中包含一个或多个 Series，每个 Series
  均有一个名称。

数据框架是用于 数据操控的一种常用抽象实现形式。 [<u>Spark</u>](https://spark.apache.org)和 [<u>R</u>](https://www.r-project.org/about.html)中也有类似的实现。

创建 Series 的一种方法时构建 Series 对象。例如：

```python
pd.Series(['San Francisco', 'San Jose', 'Sacramento'])
```

DataFrame 和字典很像，Series 对应字典的 **键-值对**，Series 的名称为**键**，数据为**值**。例如像下面这样创建 DataFrame 对象：

```python
city_names = pd.Series(['San Francisco', 'San Jose', 'Sacramento'])
population = pd.Series([852469, 1015785, 485199])

pd.DataFrame({'City name': city_names, 'Population': population})
```

 但在大多数情况下，是将整个文件加载到 DataFrame 中。

 ```python
 data = pd.read_csv('path or url', *args, **kwargs)
 ```

# 访问数据

前面提到，DataFrame 和字典很像，还体现在访问数据的方式上。

```python
cities['City name']     # 返回名称为 City name 的 Series

cities[0:2]     # 返回所有 Series 的前两个值构建的 DataFrame
```

此外，*pandas*针对高级 [<u>索引和选择</u>](http://pandas.pydata.org/pandas-docs/stable/indexing.html)提供了机器丰富的 API。

# 操控数据

可以向 Series 应用 Python 的基本运算指令。例如：

```python
population / 1000.
```

[<u>Numpy</u>](http://www.numpy.org)是一种用于进行科学计算的常用工具包。*pandas
Series* 可用作大多数 Numpy 函数的参数：

```python
import numpy as np
np.log(population)
```

对于更复杂的单列转换，可以使用 Series.apply。像 Python [<u>映射函数</u>](https://docs.python.org/2/library/functions.html#map)一样，Series.apply 将以参数形式接受 [<u>lambda函数</u>](https://docs.python.org/2/tutorial/controlflow.html#lambda-expressions)，而该函数会应用于每个值。
下面的示例创建了一个指明 population 是否超过 100 万的新 Series：

```python
population.apply(lambda val: val > 1000000)

0    False
1     True
2    False
dtype: bool
```

DataFrame 的修改方式也非常简单。例如，以下代码向现有 DataFrame 添加了两个 Series：

```python
cities['Area square miles'] = pd.Series([46.87, 176.53, 97.92])
cities['Population density'] = cities['Population'] / cities['Area square miles']
cities
```


