---
layout: post
comments: true
title: 深度卷积神经网络中的降采样
excerpt: 对比深度卷积网络中各种降采样的方式
categories: Work
date: 2018-10-13 22:00:00
---

降采样指的是成比例缩小特征图宽和高的过程，比如从（W，H）变为（W/2，H/2）。深度卷积神经网络中降采样的方法主要有三种：

1、stride大于1的pooling

2、stride大于1的conv

3、stride大于1的reorg（在YOLOv2的论文里叫passthrough layer）

其中1和2在深度卷积神经网络中使用非常普遍，3比较小众，由Joseph Redmon在YOLOv2中首次提出。

1和2的对比在[Striving for Simplicity: The All Convolutional Net](https://arxiv.org/abs/1412.6806)中有详述，文末有这么一段总结：

>With modern methods of training convolutional neural networks very simple architectures may perform very well: a network using nothing but convolutions and subsampling matches or even slightly outperforms the state of the art on CIFAR-10 and CIFAR-100. A similar architecture shows competitive results on ImageNet.
>
>In particular, as opposed to previous observations, including explicit (max-)pooling operations in a network does not always improve performance of CNNs. This seems to be especially the case if the network is large enough for the dataset it is being trained on and can learn all necessary invariances just with convolutional layers.

大概意思就是，用stride=2的conv降采样的卷积神经网络效果与使用pooling降采样的卷积神经网络效果相当；卷积神经网络小的时候，使用pooling降采样效果可能更好，卷积神经网络大的时候，使用stride=2的conv降采样效果可能更好。

总体来说，pooling提供了一种非线性，这种非线性需要较深的conv叠加才能实现，因此当网络比较浅的时候，pooling有一定优势；但是当网络很深的时候，多层叠加的conv可以学到pooling所能提供的非线性，甚至能根据训练集学到比pooling更好的非线性，因此当网络比较深的时候，不使用pooling没多大关系，甚至更好。

pooling的非线性是固定的，不可学习的，这种非线性其实就是一种先验。

3中降采样的优势在于能够较好的保留低层次的信息。1和2的降采样方式，好处是抽取的特征具有更强的语义性，坏处是会丢失一些细节信息。而3这种降采样方式与1、2相反，它提取的特征语义性不强，但是能保留大量细节信息。所以当我们既需要降采样，又需要不丢失细节信息的时候，3是一个非常合适的选择。
