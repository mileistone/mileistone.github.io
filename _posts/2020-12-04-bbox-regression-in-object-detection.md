---
layout: post
comments: true
title: 目标检测框回归问题
excerpt: 基于x、y、w、h的回归和基于IoU的回归
categories: Work
date: 2020-12-04 22:00:00
---

目标检测模型训练的时候有两个任务，框分类（框里是什么）和框回归（框在哪），本文主要讲第二点。

框回归可以分为两大类，基于x，y，w，h的回归（比如[Faster R-CNN](https://arxiv.org/abs/1506.01497)、[YOLO](https://github.com/pjreddie/darknet)、[RetinaNet](https://arxiv.org/abs/1708.02002)里框回归的loss），基于IoU的回归（比如IoU loss、[GIoU loss](https://arxiv.org/abs/1902.09630)、[DIoU loss](https://arxiv.org/abs/1911.08287)、[CIoU loss](https://arxiv.org/abs/1911.08287)）。

##### 基于x，y，w，h的回归

基于x，y，w，h的回归，可以细分为x、y（GT框的中心）的回归和w、h的回归。

###### w、h的回归
Faster R-CNN、YOLO、RetinaNet的w、h回归方式大体相同。假设$$t_w^g$$、$$t_h^g$$为拟合目标，$$t_w^p$$、$$t_h^p$$为网络预测值，$$w_g$$为GT框的宽，$$w_a$$为样本对应anchor框的宽，$$h_g$$为GT框的宽，$$h_a$$为GT框对应的anchor框的高，$$L_nLoss$$为$$L1Loss$$、$$SmoothL1Loss$$、$$L2Loss$$等。

$$t_w^g=log(w_g/w_a)$$

$$t_h^g=log(h_g/h_a)$$

$$loss_w=L_nLoss(t_w^p, t_w^g)$$

$$loss_h=L_nLoss(t_h^p, t_h^g)$$

其中通过anchor的归一化，和取log，可以一定程度增加$$loss_w$$和$$loss_h$$对框scale的invariance。

###### x、y的回归

x、y的回归方式可以分为两类，一类以YOLO为代表，一类以Faster R-CNN和RetinaNet为代表。后者x、y的回归方式与它们对w、h的回归方式相同，不再赘述。

YOLO中x、y的回归方式比较奇特。假设$$t_x^g$$、$$t_x^g$$为拟合目标，$$t_y^p$$、$$t_y^p$$为网络预测值,$$w_{featmap}$$为对应head输出feature map的宽，$$h_{featmap}$$为对应head输出feature map的高。

$$x_g$$为GT框中心的x坐标，$$y_g$$为GT框中心的y坐标，$$x_{cc}$$为GT框匹配上的grid cell的x坐标，，$$y_{cc}$$为GT框匹配上的grid cell的y坐标，
x坐标的范围缩放到化到$$[0, w_{featmap}]$$，y坐标的范围缩放到到$$[0, h_{featmap}]$$。

$$t_x^g=x_g-x_{cc}$$

$$t_y^g=y_g-y_{cc}$$

$$loss_x = BCELoss(t_x^p, t_x^g)$$

$$loss_y = BCELoss(t_y^p, t_y^g)$$


###### 对scale进行reweight
关于x、y、w、h的回归，YOLO还会对不同scale的框回归loss进行reweight，减小大scale的框回归loss，增大小scale的框回归loss，Fatser R-CNN和RetinaNet没这么做。总体而言，YOLO里很多操作都是比较特立独行的，不过在论文里讲得很少，只有看作者的C代码实现才能发现。

##### 基于IoU的回归

IoU loss有两个所谓的优点，一个是“Given the choice between optimizing a metric itself vs. a surrogate loss function, the optimal choice is the metric itself”，另一个是IoU loss对框的scale具有invariance特性，大家觉得这个对于框回归而言非常必要。

IoU loss关注预测框与GT框的IoU，而其他基于IoU loss的变体，关注的点除了IoU 之外还有：

1、预测框与GT框并集占据预测框与GT框最小包络框的比例（越大越好）；

2、归一化（以预测框和GT框最小包络框的对角线为分母）的预测框中心与GT框中心距离（越小越好）；

3、预测框长宽比与GT框长宽比的相似程度（越大越好）。

GIoU loss关注了1，DIoU loss关注了2，CIoU loss关注了2和3。

GIoU loss缓解了IoU loss在预测框和GT框之间IoU为0，梯度为0的问题。实验中GIoU收敛比较慢，DIoU缓解了GIoU这个问题；CIoU基于DIoU，添加了一个关于长宽比的惩罚项。

##### 一些想法

1、the optimal choice is the metric itself?

将IoU作为loss是不是真的如论文中所说“Given the choice between optimizing a metric itself vs. a surrogate loss function, the optimal choice is the metric itself”。

这句话很对，但是IoU只是整体metric（比如mAP）中的一部分，这一个部分达成了“optimizing a metric itself”，问题是局部最优不一定能达到全局最优，这个问题导致IoU loss提出来之后，后续大家打了一个接一个的补丁（比如GIoU、DIoU、CIoU），甚至[PP-YOLO](https://arxiv.org/abs/2007.12099)发现把基于x、y、w、h的回归和基于IoU的回归结合起来效果更好。

事情并不如IoU提出来的时候想的那么美好。

相信后面还会有更多的补丁。一个问题在于无论是IoU也好，还是后面提出来的其他惩罚项也好，既缓解了一部分问题，也带来了新的问题；另一个问题是，整体地“optimizing a metric itself”这个命题听起来很美好，但是基本不可实现：想象很美好，现实很骨感。

后面我们大概率会从不同角度提出更多的惩罚项，这里会带来一个问题，当惩罚项越来越多的时候，如何平衡各个惩罚项loss，进而如何平衡框回归与框分类loss，里面会涉及到很多超参。

2、对框scale的invariance特性

框回归问题中，对框scale具有invariance是否一定是优点呢？我想不尽然，因为不同scale的框之间可能存在不平衡，在这种条件下，对框scale具有invariance可能不一定是最好的，我们可能需要做一些reweight。

3、anchor free

这里我们没提到anchor free的目标检测框回归计算方式，但是思路是类似的，基于上述的思路，可以很自然地想到anchor free目标检测器里框回归会如何设计。
