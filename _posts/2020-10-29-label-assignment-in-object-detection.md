---
layout: post
comments: true
title: Label Assignment in Object Detection
excerpt: 目标检测label assignment方法的演进路径以及趋势
categories: Work
date: 2020-10-29 20:00:00
---

label assignment是目标检测中非常重要的问题，这里我们先调研了目标检测中主流模型label assignment的方式，然后对此做了一个简单总结。

label assignment就是要对目标检测中的anchor box或者anchor point打上label，是positive、negative还是ignore。这里面有两个挑战，一个挑战是negative非常多，容易导致样本不均衡问题；另一个挑战是判定标准只能经验性地设置，然后通过实验结果来验证，基本是一个trial and error的过程。

这里提到的论文见[github链接](https://github.com/mileistone/study_resources/blob/master/modeling/supervised_learning/2d/object_detection/label_assignment.md)。

###### 背后的逻辑
归纳和演绎是科学研究中运用得较为广泛的逻辑思维方法。马克思主义认识论认为，一切科学研究都必须运用到归纳和演绎的逻辑思维方法。人类认识活动，总是先接触到个别事物，而后推及一般，又从一般推及个别，如此循环往复，使认识不断深化。归纳就是从个别到一般，演绎则是从一般到个别。

论文调研相当于归纳，根据总结则可以进行演绎。就像门捷列夫在十九世纪归纳各种已知元素的性质而制作出元素周期表，并借此基本成功预测了当时尚未发现的、位于周期表空位中元素的相当一部分性质。

对于label assignment这个问题，我们调研和总结之后，就可以理清各个方法的异同与优劣，同时可以指明后续的研究方向。

###### Survey
- Faster RCNN/SSD/RetinaNet
    - grid cell中有多个anchor 
    - 通过anchor与GT框之间的IoU判定是positive、negative还是ignore
        - 比如RetinaNet里面，IoU低于0.4为negative，高于0.5为positive，其他为ignore
- YOLO
    - grid cell中有多个anchor 
    - YOLOv2 
        - 首先通过GT框的中心计算得到对应的grid cell
        - 然后计算GT框与该grid cell中anchor的IoU
            - 选择其中IoU最大的anchor做为positive
        - 计算GT与该grid cell预测proposal的IoU
            - IoU大于0.6的proposal对应的anchor为ignore
        - 其他anchor为negative
    - YOLOv3
        - 对于每个proposal
            - 计算其与所有GT框的IoU，如果最大的IoU大于0.7，则该proposal对应的anchor为ignore
        - 对于每个GT框
            - 计算其与所有anchor之间IoU（将x、y置为0，仅保留w、h的值），取IoU最大的anchor为positive（对应的grid cell，通过GT框中心计算得到）
            - 其他anchor为negative
- FCOS
    - grid cell中没有anchor
    - 对于每一层feature map，将其中每个grid cell对应回原图，对应回去之后如果落在某个GT框内，则该grid cell为positive，如果同时落在多个GT框内，选择面积小的GT框与该grid cell对应
    - 其余grid cell为negative
    - 没有ignore
- FreeAnchor
    - 每个gird cell中有多个anchor 
    - 通过anchor与GT框之间的IoU判定positive、negative、ignore
        - 低于0.6的anchor为negative
        - 对于每个GT框，根据IoU大小进行降序排序，选择top 50的anchor为候选positive
        - 计算候选positive anchor对应的loss（包含分类loss和框回归loss），loss最小的anchor即为positive（类似OHEM）
    - positive和negative以外的anchor为ignore
    - loss
        - 分类
            - 包含了proposal与GT的IoU反馈 
- Learning from Noisy Anchors for One-stage Object Detection
    - None
- AutoAssign
    - grid cell中没有anchor
    - 对于每一层feature map，将其中每个grid cell对应回原图，对应回去之后如果落在某个GT框内，则该grid cell既为positive，也为negative（类似label smoothing）；positive的权重通过attention的方式来学，negative的权重通过该grid对应的proposal与所有GT框之间最大的IoU确定，即IoU越大，negative权重越小，反之，negative权重越大
    - 其余grid cell为negative
    - 没有ignore
     
###### Summary    
- 有无模型的反馈
    - 无（即label assignment过程与模型前向无关）
        - GT与anchor bbox的IoU（Faster RCNN、SSD、RetinaNet）
        - 落在GT内的grid cell（YOLO、FCOS）
    - 有（即模型前向会参与label assignment过程）
        - 分类 
            - 针对negative
                - OHEM
            - 同时考虑positive和negative
                - focal loss
        - 框回归
            - 针对ignore
                - YOLO 
        - 分类和框回归
            - 针对positive
                - FSAF
                - FreeAnchor
                - Learning from Noisy Anchors for One-stage Object Detection
        - loss reweight
        - rescore
- hard or soft label assignment
    - hard label assignment
        - Faster RCNN、YOLO、SSD、RetinaNet、FCOS 
    - soft label assignment
        - AutoAssign
        - SAPD

可以看到，有模型反馈和soft label assignment是一个趋势。
