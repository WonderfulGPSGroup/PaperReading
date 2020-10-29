# DMM: Fast Map Matching for Cellular Data

## Tips
- 可以学习的是它将变成序列转为定长向量的思想
- 疑惑点：为什么没有现实地图数据？？？
## 主要内容
- 提出一个Encoder，将==高维数据转为低维向量==，类似压缩映射
- 提出一个基于RNN模型（不仅考虑前一个路段，还考虑了之前很多路段）
> 输入：一系列的蜂窝站\
> 输出：对应的路段


- 强化学习模型优化输出
- 12个蜂窝站==一秒
- 1700km




## Overview
- 通过**Location representer**，将所有蜂窝站序列--->向量序列
- 通过基于RNN的模型，由向量序列得出路线（Ground truth数据来自hmm模型）
- 通过**强化学习**优化输出

> - 主路优先
> - 同路径优先
> - 少转弯优先

## Location Representer

#### 原有问题

- 不同蜂窝站的涉及的路段不同
> 例如A涉及abc，B涉及qwe，那输出究竟怎么定义呢
- one-hot表示法很冗余，而且不能迁移到新的路段里运用
- gps表示蜂窝站位置的方法，将单元塔的表示法限制为二维GPS坐标，这实际上编码了单元塔之间的空间接近度。然而，很难从二维坐标序列中得到输入单元塔序列的高质量表示。【我也觉得很难懂，直接翻译的】
#### 解决方案
用DNN模型学习自动编码的方式（最基本的深度学习模型）
> 所有蜂窝站序列--->向量序列

## MapMatcher
包括三部分：编码器、解码器、嵌入组件 
#### 原有问题

- RNN输出是独立的，前后输出的路径可能不连续



#### 解码器（RNN）

- 输入：用Location Representer编码好的蜂窝站序列X
- 输出：一个向量c，就是最后一个单元的输出
- 每个基本单元：输出为隐藏状态ht，输入为ht-1和用Location Representer编码好的蜂窝站向量xt
#### 解码器（另一个RNN）

