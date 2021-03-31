---
layout: post
comments: true
title: GluonCV、OpenMMLab、Paddle系列算法框架的对比与思考
excerpt: 作为一个开发工程师或者算法工程师，想做出有影响力的工作，除了技术实力之外，适当地了解产品、运营、推广、渠道是非常必要的
categories: Work
date: 2021-03-30 22:00:00
---

- Detectron系列
    - **2018-01-23** Detectron[Caffe2](24.2k star)
    - **2019-10-11** Detectron2[PyTorch](15.6k star)
- GluonCV
    - **2018-02-26** 主流计算机视觉任务[MXNet](4.6k star)
- OpenMMLab算法框架系列[PyTorch]
    - **2018-09-02** MMDetection(14.1k star)
    - **2018-01-01** MMSkeleton(2.1k star)
    - **2020-03-28** MMEditing(1.9k star)
    - **2019-04-16** MMAction(1.6k star)
    - **2020-07-10** MMSegmentation(1.5k star)
    - **2020-07-28** MMTracking(1.3k star
- Paddle算法框架系列[PaddlePaddle]
    - **2020-05-10** PaddleOCR(11.4k star)
    - **2019-06-28** PaddleDetection(2.4k star)    
    - **2019-08-26** PaddleSeg(1.3k star)

---

##### Detectron2：Facebook学术成果展示台

Detectron2包含的主要是Facebook提出的或者跟Faccbook有密切关系的目标检测和实例分割模型，比如Faster RCNN、Mask RCNN、RetinaNet，主要是一个自家学术成功展示。

##### GluonCV：新手友好
GluonCV起步很早，基本包含了所有计算机视觉任务，tutorial做得比较完善，另外还通过《Dive into Deep Learning》进行推广。

某种程度上来说，GluonCV起到了计算机视觉启蒙的作用，降低了进入计算机视觉这个领域的门槛，对新手相当友好：如果你是一个对计算机视觉感兴趣的小白，你可以跟着《Dive into Deep Learning》了解深度学习，然后通过GluonCV的代码了解某个具体模型（比如YOLO、SSD等）的实现细节，最后GluonCV还告诉你实践中的一些经验，比如[Deep dive into SSD training: 3 tips to boost performance](https://cv.gluon.ai/build/examples_detection/train_ssd_advanced.html)、[Bag of Freebies for Training Object Detection Neural Networks](https://arxiv.org/abs/1902.04103)、[Bag of Tricks for Image Classification with Convolutional Neural Networks](https://arxiv.org/abs/1812.01187)。

但是由于GluonCV设计得不够模块化，不容易拓展，所以GluonCV对比较资深的用户吸引力不够大，我想这是GluonCV影响力难以再上一层楼的原因之一。

##### OpenMMLab：学术研究、打比赛

OpenMMLab现在也基本囊括了所有计算机视觉任务。OpenMMlab系列的开源项目，代码模块化、抽象做得比较好，容易拓展，对新手不太友好，但是对相对资深的从业者，无论是学术研究还是打比赛，比较友好。

OpenMMLab将MMDetection打造成了一个爆款，目标检测在所有计算机视觉任务中是重要性和难度结合得最好的任务。分类很重要，但是分类非常简单，实现起来难度不大；实例分割实现起来难度较大，但是却没那么重要。MMDetection紧跟学术前沿，基本所有目标检测模型都能在MMDetection中找到。

商汤的品牌背书，商汤公司的推广，再加上MMDetection这个爆款，整个OpenMMLab系列后面推出的开源项目都可以得到足够的流量和用户。

##### Paddle算法框架系列：面向工业应用

Paddle系列算法框架囊括了重要的计算机视觉任务。PaddlePaddle系列算法框架的优势是在工业应用上面做得比较好，对加速、部署支持得较好（服务器端、移动端、Python前端、C++前端、在线serving、TensorRT加速、模型压缩等等）。

比如PaddleDetection提供通用物体、人脸、汽车、行人的预训练模型。

PaddleOCR提供直接可以应用的OCR解决方案——从文本检测、方向矫正到文本识别整条pipeline帮你打通，还提供直接可用的中英文OCR预训练模型、OCR标注工具和数据合成工具，帮你整理好各种OCR的数据集。PaddleOCR这种一站式服务，大大降低了工业应用OCR的门槛，我想这应该是PaddleOCR能够吸引这么多人使用的重要原因。

另外百度在推广Paddle算法框架上面不遗余力，垂直媒体上隔三岔五可以看到PR文章。

##### 对比与总结

GluonCV、OpenMMlab和Paddle系列的目标都是希望做成一个计算机视觉算法框架，Detectron2表面是一个计算机视觉算法框架，其实是一个Facebook学术成功展示台，所有后面我们暂且把Detectron2略过。

总体看来，OpenMMLab和Paddle系列做得比较成功，一个面向学术研究，一个面向工业应用，有点像深度学习框架中的PyTorch和TensorFlow。GluonCV对整个行业做了很大的贡献，我自己也受惠于GluonCV，但是现在发展出现了一点颓势。

算法框架看起来是一个工程问题，其实是一个产品问题。算法框架本身是一个产品，一个产品的流程包括：需求分析、产品定位、产品规划、产品设计、产品运营。算法框架的工程实现只是整个流程中的一环。一个产品要想做好，流程中的每一环都要做得不错，然后其中某一环或者某几环做得特别亮眼。

OpenMMLab和Paddle系列的产品定位做得比较好；OpenMMLab的产品设计做得比较亮眼；Paddle系列的产品运营和推广做得很扎实。

作为一个开发工程师或者算法工程师，想做出有影响力的工作，除了技术实力之外，适当地了解产品、运营、推广、渠道是非常必要的。

##### 算法框架与深度学习框架的关系

之前我认为GluonCV发展出现颓势有一个重要原因是MXNet的市场不够大，但是Paddle系列的表现让我对这个想法产生了怀疑。

这两年深度学习框架高速发展的阶段已经过去了，各个深度学习框架越来越趋同，深度学习框架的效率和易用性区别越来越小，深度学习框架的切换成本也日渐降低，所以深度学习框架的市场占有率对算法框架的影响越来越小（除非某个深度学习框架这几年不思进取，做得特别烂）。相反，算法框架的繁荣会反过来影响深度学习框架的市场占有率。这对国产深度学习框架来说是一个好消息。

##### 对未来的展望

算法框架接下来的发展我觉得类似两三年前的深度学习框架，在学术研究领域占据优势的PyTorch开始更好地支持工业应用，在工业应用领域执牛耳的TensorFlow想补易用性的短板。算法框架接下来，我觉得也是类似，OpenMMLab可能会更好地支持工业应用，Paddle系列可能会增加在学术领域的影响力。与此同时，运营和推广也会日渐被重视。

##### 个人实践

这几年一方面我关注着各个算法框架的发展，另外一方面当自己有一些想法和思考的时候，也会进行一些实践。

2018年的时候，PyTorch开始异军突起，我2015年开始接触深度学习，用过Caffe、Theano、TensorFlow、MXNet，使用PyTorch的时候，我的直觉告诉我PyTorch应该就是未来。所以在2018年12月份的时候，我用PyTorch复现了YOLO——[OneStageDet](https://github.com/Tencent/ObjectDetection-OneStageDet)。

2019年MMDetection“小荷才露尖尖角”的时候，我感觉这种高度模块化和抽象的算法框架会是趋势，所以在2019年11月的时候，我们学习MMDetection这种方式开发了语义分割工具箱[vedaseg](https://github.com/Media-Smart/vedaseg)（比MMSegmentation早8个月），在2020年2月份开发了文字识别工具箱[vedastr](https://github.com/Media-Smart/vedastr)（比PaddleOCR早3个月），在2020年5月份开发了图像分类工具箱[vedacls](https://github.com/Media-Smart/vedacls)。

在平时做项目的过程中，我意识到当前的算法框架对工业应用支持得不够好，然后我们开发了[volkscv](https://github.com/Media-Smart/volkscv)来浏览和分析数据，[volksdep](https://github.com/Media-Smart/volksdep)来加速模型，[flexinfer](https://github.com/Media-Smart/flexinfer)作为Python前端的部署SDK、[cheetahinfer](https://github.com/Media-Smart/cheetahinfer)作为C++前端的部署SDK。因为MMDetection对部署不够友好，我们取出MMDetection单阶段目标检测器部分进行了重构，于是有了目标检测工具箱[vedadet](https://github.com/Media-Smart/vedadet)。

至此我们的开源有了一横一纵，横向上囊括最基础的计算机视觉任务目标检测、语义分割、图像分类、文字识别，纵向上包含从数据浏览与分析、模型训练、模型加速到模型部署。我们将这一系列工具链称之为[CVChain](https://zhuanlan.zhihu.com/p/271963670)。

从2017年开始，我一直在知乎上撰写技术博客，阐述自己对计算机视觉的一些理解，没想到受到了越来越多从业者的关注，某种程度我自己有了一些流量，我可以通过自己的账号来推广我们做的开源项目，加上有幸认识一些行业内的媒体朋友，他们也非常友好地帮我进行了广告。

但是即便如此，流量和背书还是比较欠缺，于是我们开始做第二件事：打比赛和提出创新的方法。前者就是我们的[TinaFace: Strong but Simple Baseline for Face Detection](https://arxiv.org/abs/2011.13183)，后者对应的是[CSTR: A Classification Perspective on Scene Text Recognition](https://arxiv.org/abs/2102.10884)。

整体来看，我们做开源的过程中，时间点都踩得比较好，对用户的需求也猜得比较准。做[OneStageDet](https://github.com/Tencent/ObjectDetection-OneStageDet)的时候，因为有腾讯这样的大公司背书，虽然公司没有去推广，最后关注量依然很大。后面的[CVChain](https://zhuanlan.zhihu.com/p/271963670)就彻底没有了大公司的背书，做起来就艰难了不少。与此同时，整个做开源的过程基本都是业余玩票，时间和精力有限，所以产品打磨得也不够完善。但是在整个过程中的确成长了不少，一方面对算法、工程有了更深的理解，另一方面能够站得更高，看到了技术之外的产品、运营、推广的重要性。
