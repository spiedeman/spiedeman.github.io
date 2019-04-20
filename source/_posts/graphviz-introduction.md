---
title: graphviz-introduction
mathjax: false
toc: true
abbrlink: 1414645917
date: 2019-02-16 16:46:44
category:
  - Graphviz
tags:
  - graphviz
---

这篇文章参考自[Drawing graphs with *dot*](http://www.graphviz.org/pdf/dotguide.pdf)

$dot$ 通过$DOT$语言来作图。该语言描述了三大对象：图(graphs)，节点(nodes)，边(edges)。同时支持有向图和无向图，并支持图的嵌套($subgraph$)。
默认采用有向图的布局方式，有另一个独立的布局工具，*neato*，用于无向图。

$dot$ 编译的方式是按顺序一行一行地处理 *.dot 文件中内容。所以各种对象的属性的有效性从从属性设置语句所在行开始直到下一次属性设置语句所在行之前。

完整的属性描述可以参考$Graphviz$官网的[该页面](http://www.graphviz.org/doc/info/attrs.html)。

## 要素
一幅图可以包含如下要素：

1. 注释
  - 双斜杠
2. 有向图 or 无向图
3. 节点之间的关系
  - 有向图：`a->b`，节点 a 指向节点 b
  - 无向图：`a--b`，节点 a 和节点 b连通
4. 定义节点属性
  - 形状，多边形`polygon`和`record`（不知道怎么翻译）
  - 颜色
  - 标签
  - 等等
5. 定义边的属性
  - 形状
  - 颜色
  - 文本
  - 等等
6. 定义结构(`record`)
  - 内部结构
  - 方向，水平或竖直排列
  - 标签


## 样例

```dot
digraph G{
    a -> b -> c;
    b -> d;
    a [shape=polygon,sides=5,peripheries=3,color=lightblue,style=filled];
    c [shape=polygon,sides=4,skew=.4,label="hello world"]
    d [shape=invtriangle];
    e [shape=polygon,sides=4,distortion=.7];
}
```
{% asset_img graph-with-polygonal-shapes.svg %}
<!-- ![](graph-with-polygonal-shapes.svg) -->

```dot
digraph structs{
node [shape=record];
    struct1 [shape=record,label="<f0> left|<f1> mid\ dle|<f2> right"];
    struct2 [shape=record,label="<f0> one|<f1> two"];
    struct3 [shape=record,label="hello\nworld | { b |{c|<here> d|e}| f}|g|h "];
    struct1 -> struct2;
    struct1 -> struct3;
}
```
{% asset_img records-with-nested-fields.svg %}

```dot
digraph G {
    node[shape=record,height=.1];   //定义node样式

    node0[label="<f0> |<f1> A|<f2> "]; //具体的一个node，含三个属性，第二个属性有名字
    node0[label="<f0> |<f1> B|<f2> "];
    node0[label="<f0> |<f1> C|<f2> "];
    node0[label="<f0> |<f1> D|<f2> "];
    node0[label="<f0> |<f1> E|<f2> "];
    node0[label="<f0> |<f1> F|<f2> "];
    node0[label="<f0> |<f1> H|<f2> "];
    node0[label="<f0> |<f1> I|<f2> "];
    node0[label="<f0> |<f1> J|<f2> "];
    node0[label="<f0> |<f1> K|<f2> "];

    "node0":f2 -> "node1": f1;  //node0的第三个属性指向node1的第二个属性
    "node1":f0 -> "node2": f1;
    "node1":f1 -> "node3": f2;
    "node3":f0 -> "node4": f0;
    "node3":f1 -> "node5": f1;
    "node3":f2 -> "node6": f2;
    "node6":f1 -> "node7": f1;
    "node7":f1 -> "node8": f0;
    "node2":f2 -> "node9": f1;
}
```
{% asset_img record-structure.svg %}
