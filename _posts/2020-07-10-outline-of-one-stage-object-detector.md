---
layout: post
comments: true
title: 单阶段目标检测器梳理
excerpt: 纲举目张
categories: Work
date: 2019-07-10 22:00:00
---

- 1、Training
  - 1.1、Data augmentation
    - 1.1.1、Multi-scale
    - 1.1.2、Random crop
    - 1.1.3、Color jitter
    - 1.1.4、Mixup
    - 1.1.5、Label smoothing
    - 1.1.6、Random erase
  - 1.2、Model
    - 1.2.1、Backbone
      - 1.2.1.1、ResNet
        - 1.2.1.2、MobileNet series
      - 1.2.2、Neck
        - 1.2.2.1、FPN
        - 1.2.2.2、PAFPN
        - 1.2.2.3、NASFPN
        - 1.2.2.4、BiFPN
      - 1.2.3、Head
        - 1.2.3.1、Shared between feature map lavel or not
      - 1.2.4、Misc
        - 1.2.4.1、Normalization
        - 1.2.4.1.1、Batch norm
        - 1.2.4.1.2、Group norm
        - 1.2.4.2、Weight initialization
          - 1.2.4.2.1、Uniform、xavier、gaussian、kaiming
          - 1.2.4.2.2、Special initialization like the way in focal loss
        - 1.2.4.3、Gradient clipping
    - 1.3、Positive/negative/ignore labels assignment
      - 1.3.1、Feature map level selection
      - 1.3.2、Point/bbox label assignment
    - 1.4、Loss
      - 1.4.1、Classification
        - 1.4.1.1、Cross entropy loss
        - 1.4.1.2、Binary cross entropy loss
      - 1.4.2、BBox Regression
        - 1.4.2.1、Distance based
        - 1.4.2.1.1、L2 loss
          - 1.4.2.1.2、SmoothL1 loss
          - 1.4.2.1.3、L1 loss
        - 1.4.2.2、IoU based
          - 1.4.2.2.1、GIoU loss
          - 1.4.2.2.2、DIoU loss
          - 1.4.2.2.3、CIoU loss
      - 1.4.3、Imbalance
        - 1.4.3.1、OHEM
        - 1.4.3.2、GHM
        - 1.4.3.3、Weighted loss
        - 1.4.3.4、Focal loss
      - 1.4.4、Misc
        - 1.4.4.1、Normalization
          - 1.4.4.1.1、Sample-wise
          - 1.4.4.1.2、Batch-wise
          - 1.4.4.1.3、None
    - 1.5、Optimizer
      - 1.5.1、SGD
      - 1.5.2、Adam
      - 1.5.3、RMSprop
    - 1.6、LR scheduler
      - 1.6.1、MultiStepLR
      - 1.6.2、CosineAnnealingLR
      - 1.6.3、CyclicLR
      - 1.6.4、Misc
        - 1.6.4.1、Warmup

---

- 2、Testing
    - 2.1、Test time augmention
      - 2.1.1、Multi-scale
      - 2.1.2、Flipping
    - 2.2、Model ensemble
      - 2.2.1、Bagging
        - 2.2.1.1、Averaging
        - 2.2.1.2、Majority voting
    - 2.3、Postprocessing
      - 2.3.1、NMS、soft-NMS
        - 2.3.1.1、Class aware
        - 2.3.1.2、Class agnostic
    - 2.4、Confidence scores interpreting
      - 2.4.1、Process each class separately or not
