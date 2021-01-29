---
layout: post
comments: true
title: 时序动作识别研究思路探秘
excerpt: 横向、纵向看论文，获取看待问题的角度与解决方案，结合目标任务数据分布，提出新的想法
categories: Work
date: 2021-01-29 22:00:00
---

最近调研了时序动作识别，相关论文见[GitHub链接](https://github.com/mileistone/study_resources/blob/master/modeling/supervised_learning/3d/temporal_action_recognition/README.md)，最后抽象成如下树状结构：
```
- effectiveness
    - temporal scale
        - TSN
        - TRN[Relation Networks]
        - SlowFast[FPN]
        - TPN[FPN]
    - separated modeling of space and time
        - spatial feature representation
            - Conv
        - temporal feature representation
            - low level
                - optical flow
                - frame difference
            - high level
                - RNN
                - Conv
                - Non-local
    - correlated modeling of space and time
        - Conv
            - 3D-Conv
                - C3D, I3D[2D-Cnnv]
                - P3D, R(2+1)D[Separable-Conv]
                - CSN[Separable-Conv]
            - 3D-Conv mixed with 2D-Conv
                - S3D[SENet]
        - Non-local
        - Two-Stream
- efficiency
    - kernel factorization
        - space, time, channel
            - space, time 
                - P3D, R(2+1)D[Separable-Conv]
            - channel
                - CSN[Separable-Conv]
    - 2D-Conv enhancement
        - TSM, TIN[Shift]
        - S3D[SENet]
    - compound scaling
        - X3D[EffientDet]
```

中括号里的内容是一个简单注释，注明中括号前的时序动作识别方法借鉴的是哪些2D领域的思路。

时序动作识别的数据是视频（3D），比图像数据（2D）多了一个时间维度。

时序动作识别面对的问题跟2D领域的类似，大部分问题沿着2D已有的方法做一个拓展，就有一个不错的方案。

##### 效率

比如时序动作识别也有效率问题。

2D里面的一个解决方案是将space-channel进行分解（比如MobileNet），将2D拓展到时序动作识别，那就可以将space-time-channel进行分解，对应的方案就是P3D、R(2+1)D、CSN。

2D里面另一个解决方案是进行compound scaling（比如EfficientDet），也就是scaling的时候要同时考虑网络输入的空间尺寸、网络宽度、网络深度，这样得到的网络效率会更高，从2D拓展到时序动作识别，那就在scaling的时候同时考虑网络输入的空间尺寸、网络输入的时间尺寸、网络宽度、网络深度，对应的方案就是X3D。

##### 效果

比如时序动作识别也有效果问题。

2D里面一个比较重要的问题是空间的scale，时序动作识别里存在时间的scale问题。2D里面一个对应的方案是FPN，时序动作识别里按照类似的思路得到的方案是SlowFast和TPN。

2D里另一个方案是ASPP，我没看到时序动作识别领域内有类似论文，但是[Scale Matters: Temporal Scale Aggregation Network for Precise Action Localization in Untrimmed Videos](https://arxiv.org/abs/1908.00707)这篇文章里将ASPP应用到时序动作检测中，我想ASPP应该应用到时序动作识别中也会有收益。

##### 做研究的思路

上述的例子阐述了时序动作识别的研究思路不少来自于2D领域，只要我们对2D领域的根本问题理解足够深，以及对相应的解决方案足够熟，那么我们很容易针对时序动作识别提出新的问题以及解决方案。

其实不仅仅是时序动作识别，几乎所有的领域都可以从其他横向领域中发现相似点，然后获取看待问题的角度和解决思路。比如时序动作检测，非常多论文的思路是迁移自基于图像的目标检测。

刚刚说的是**横向看**，也可以**纵向看**，看看历史上，这个领域有哪些挑战以及解决思路，然后将之迁移到现在。比如深度学习时代的FPN，跟传统计算机视觉时代的图像金字塔异曲同工。

横向看、纵向看获取思路，然后多看目标任务的数据，**归纳总结该数据分布的特点**，结合该特点才能更好地将借鉴的思路迁移到目标任务中。
