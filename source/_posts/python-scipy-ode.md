---
title: Python-数值积分初值常微分方程组
category: 数值计算
tags:
  - Python
  - Scipy
  - 数值计算
abbrlink: 7f1dadbd
date: 2019-07-09 17:27:35
---

通常可以将常微分方程组的求解问题分成两类：
- 初值问题
- 边界问题

这里讨论**初值**常微分方程组的数值求解，用到的工具为`scipy.integrate`模块中的`solve_ivp`函数。接收的参数和返回值分别为

**参数**：
- `fun`: *函数*

    一阶常微分方程组的右端，`dy / dt = f(t, y)`。`t`为标量，`y`可以是一维数组，或二维数组（每一列表示一个函数`y`）。若参数`vectorized`为真，则`y`可以是二维数组，这一矢量化的实现是为了在刚性问题中允许通过有限差分更快地估计雅可比矩阵。
- `t_span`: *长度为2的浮点数元组*

    积分区间为`[t0, tf]`。
- `y0`: *一维数组*

    系统初值。若要在复数域上求解，则使`y0`的`data type`为复数。
（以下为常用可选参数）
- `method`: *方法名*

    积分方法有如下几种：
    - `RK45`（默认）：45阶龙格库塔法。
    - `RK23`：23阶龙格库塔法。
    - `Radau`：5阶隐式龙格库塔法。
    - `RK23`：23阶龙格库塔法。
    - `Radau`：5阶隐式龙格库塔法。
    - `BDF`：基于后向微分公式的隐式多步变阶法。
    - `LSODA`：Adams/BDF方法，带有刚性检测和转换功能。

    非刚性问题使用
    `RK45`或`RK23`，刚性问题使用`Radau`或`BDF`。也可以传递任意一个实现求解功能的`OdeSolver`的子类。

- `dense_output`: *布尔值*

    是否计算连续解，默认为`False`。

- `t_eval`: *一维数组或None*

    对解的值感兴趣的一组时刻。默认由solver决定。

- `events`: *函数或函数列表*

    追踪的事件。默认为None，表示没有事件需要被追踪。每一个返回值为float的形如`event(t,
    y)`的函数都可以作为一个追踪器。solver会使用root-finding算法找到`event(t,
    y) = 0`的时刻`t`。每个*事件*函数可能拥有下面的两个属性：
    > terminal: bool, optional
    >
    >       是否在事件发生时终止积分。无此属性等价于False。
    > direction: float, optional
    > 
    >       表示穿过零点的方向。direction为正，则事件只会以负到正的方式穿过零点时才会被触发，
    >       反之则direction为负。若为零，则已任意方式穿过零点都会被触发。无此属性等价于0。

- `vectorized`: *布尔值*
    函数是否以矢量化方式实现。

**返回值**
- `t`: *数组*

    `t_eval`对应的时刻。
- `y`: *数组*

    解在时刻`t`的值。
- `sol`: *OdeSolution* 或 *None*

    如果`dense_output`为False，则为None，否则为找到的解，作为OdeSolution的一个实例。
- `t_events`: *列表* 或 *None*

    包含一组数组，每个数组对应一个事件，包含所有该事件发生时的时刻。


例子可在[此处](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.solve_ivp.html#r179348322575-1)找到。
