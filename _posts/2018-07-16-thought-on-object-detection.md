---
layout: post
comments: true
title: 关于目标检测的思考
excerpt: 基于anchor和不基于anchor，one stage与two stage，数据分布
categories: Work
date: 2018-07-16 22:00:00
---

##### 基于anchor和不基于anchor
除了yolov1和densebox之外的检测网络基本都是基于anchor的。

anchor相当于增加了模型的assumption，如果模型assumption跟数据集的分布一致，那么这个assumption会有利于框的回归，提高模型的准确性；但是如果这个assumption跟数据集分布不一致，它反而会伤害模型的准确性。所以基于anchor的模型，anchor的设置比较重要，anchor要与数据分布一致。

##### one stage与two stage
one stage主要有ssd和yolo系列，two stage有rcnn系列和基于ssd的refinedet。

虽然基于ssd的改进文章比yolo的多很多，给人一种ssd比yolo好的错觉，但是yolo各方面相比ssd并不差，ssd比yolo更受欢迎的原因我认为主要是ssd基于主流框架开发，便于改进；而yolo基于小众的darknet，darknet没多少人熟悉，大家也不愿意去熟悉这样的小众框架。

小众框架社区不成熟，遇到了问题不容易找到解决方案，而且学会了darknet只能玩yolo系列模型，学习成本太高，不值当，另外darknet只支持最简单的layer，稍微复杂的layer里面都没有，如果你想基于darknet搭一些当前最fancy的结构，可能会遇到困难。

one stage和two stage的区别。two stage相当于在one stage的后面再refine一下。ssd和v1之后的yolo跟faster rcnn系列框架里的rpn差不多。

one stage给人快但是准确性不太好的印象，two stage给人准确性高但是慢的印象。但是我认为假以时日，one stage和two stage会逐渐吸收彼此的长处，最后one stage可以做到和two stage一样准，two stage可以做到和one stage一样快。

如果以速度横轴，mAP为纵轴画点阵图，那么当前one stage模型主要分布在右下角，two stage模型主要分布在左上角；而以后的话，one stage模型在这个坐标系里每个点的附近一定会有一个two stage模型。

##### 数据分布
模型其实就是一系列assumption的集合。这些assumption是我们对数据集分布的假设。

模型不是孤立的，模型跟数据分布是成对出现的，这也就是我们常说“没有最好的模型，只有最合适的模型”背后的道理。

深度学习模型的超强拟合能力让我们开始遗忘assumption的重要性。在传统机器学习时代，大家非常看重assumption，因为传统机器学习时代，模型拟合能力没那么强，模型的assumption一旦跟数据分布不一致，效果会很差。而深度学习时代，模型拟合能力太强，大多数时候我们不考虑assumption是否与数据分布契合，也能训练出一个不错的模型出来。

在深度学习刚火，到处一片蓝海，大家四处跑马圈地的时候，不关注assumption，粗放式地把模型往复杂了怼，的确是一种行之有效的方式。但是现在模型方面逐渐到了瓶颈，继续往复杂了怼，效果提升有限，这个时候我们可能需要从精细化入手，比如回到传统机器学习重视assumption的思路上。

以分类为例，现实场景里类别不平衡是常见现象，所以我们经常需要想一些办法对付类别不平衡，比如对数量少的类别升采样，或者对数量少类别的loss加更大的权重等等。

这个过程其实就是在处理数据分布跟assumption不一致的问题，普通分类模型的assumption是各个类别的样本数量差不多，但是实际数据集里各个类别的样本数量却差距较大，这里出现了gap。

以检测为例，提出ohem的原因也是从assumption角度入手。ohem是解决物体框和背景框之间的平衡问题。那么更进一步，检测里面各个类别之间平衡吗？

从assumption角度思考的话，会发现天空突然广阔很多，还有很多事情可做。我相信接下来两年会有越来越多的工作会从assumption入手，更甚，传统机器学习的思路也会慢慢地融入到深度学习领域。慢慢地，改改layer就能发文章的时代会逐渐远去。潮水褪去之后，既对传统机器学习了然于胸，又对深度学习有insight，还对具体领域（例如cv、nlp等等）有着扎实基础的人才能衣着光鲜地站在沙滩上，眺望远方，欣赏波涛汹涌的海浪。
