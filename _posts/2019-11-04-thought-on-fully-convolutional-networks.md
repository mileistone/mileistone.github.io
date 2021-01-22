---
layout: post
comments: true
title: 关于全卷积神经网络的思考
excerpt: 基于CNN的任务的核心是全卷积神经网络，全卷积神经网络输出的特征图类似昆虫的复眼，每个grid都是一只眼睛，每只眼睛的感受野相同，但是看到的内容不同
categories: Work
date: 2019-11-04 22:00:00
---

<div class="imgcap">
<img src="/assets/2019-11-04-thought-on-fully-convolutional-networks.png">
<div class="thecap">全卷积网络图解</div>
</div>

最近一个月先后想明白了目标检测和图像分类、语意分割和图像分类之间的联系。

通过论文《Single-Stage Multi-Person Pose Machines》和《PolarMask: Single Shot Instance Segmentation with Polar Representation》，进一步找到了图像分类、语意分割、图像分类、多人姿态估计和实例分割之间的共同点。

即这些任务对应的模型大部分是全卷积神经网络，例如单阶段目标检测、语意分割等等，即使不是全卷积神经网络的图像分类模型，只要将最后一层fc换成1x1的conv，也就转换为了全卷积神经网络。

所有的任务都可以统一为一个全卷积神经网络，该全卷积神经网络输出的特征图如同昆虫的复眼，每个grid为一只眼睛，每只眼睛所看到的东西不一样，但是每只眼睛的视野范围相同（即，每只眼睛的感受野大小相同），每只眼睛单独工作，互不影响。具体可见图1，图像输入到全卷积网络中，输出的特征图大小为4\*4，中间2\*2个眼睛，每个眼睛看到的是图像不同的部位。

然后每只眼睛会判断：1、它看到了什么物体（类别）；2、这个物体有什么特点（属性，可选项）。

以图像分类为例子，每只眼睛（因为使用了global average pooling，图像分类只有一只眼睛）会判断它看到了什么物体（类别）。

以语意分割为例子，每只眼睛会判断它看到了什么物体（类别）。

以目标检测为例子，每只眼睛会判断它看到了什么物体（类别），这个东西的x offset、y offset、w、h分别是多少（属性）。

以实例分割为例子，每个眼睛会判断它看到了什么物体，以该眼睛所在的地方为中心，该物体的36条极线分别有多长（属性）。

其他基于CNN的计算机视觉任务可依次类推。

总结一句话就是：基于CNN的任务的核心是全卷积神经网络，全卷积神经网络输出的特征图类似昆虫的复眼，每个grid都是一只眼睛，每只眼睛的感受野相同，但是看到的内容不同，每只眼睛独立判断它看到了什么东西，这个东西有什么属性。

根据这一点，我们能更好的理解业界为了解决为了解决某种计算机视觉任务而设计的模型，当面对业界还没有研究过的计算机视觉任务时，我们也能自己设计出模型。