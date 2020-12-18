---
layout: post
comments: true
title: AutoML的能与不能
excerpt: AutoML是提高算法工程师的效率工具，而不是抢算法工程师饭碗的恶魔
categories: Work
date: 2020-03-06 20:00:00
---

最近几年AutoML炙手可热，一时风头无两。各大公司都推出了自己的AutoML服务。

谷歌云的[Cloud AutoML](https:/cloud.google.com/automl/docs)：

- Natural Language
  - classification
  - entity extraction
  - sentiment analysis
- Tables
- Translation
- Video Intelligence
  - classification
  - object tracking
- Vision
  - classification
- object detection

Azure的[Automated machine learning](https://azure.microsoft.com/en-us/services/machine-learning/automatedml/)：

- Tables
- classification
- regression
- time-series forecasting

AWS的[Amazon SageMaker Autopilot](https://aws.amazon.com/cn/sagemaker/autopilot/)：

- Tables
- classification
- regression

其中功能丰富度最高的是谷歌云的AutoML、文档最详尽的也是谷歌云的AutoML。

同时微软、亚马逊还有AutoML的开源toolbox——NNI、AutoGluon，之前写了一篇回答对二者做了一个简单的介绍。

AutoML的兴起搞得算法从业人员人心惶惶，不可终日，似乎自己就是珍妮纺织机和蒸汽机发明前夕的纺织工人。情况果真如此吗？我觉得短期看来，AutoML是
提高算法工程师效率的工具，而不是抢算法工程师饭碗的恶魔。下面是我对AutoML在计算机视觉领域的看法。

### AutoML在计算机视觉领域的能
AutoML在计算机视觉领域主要有两大用途：自动调参（比如LR、weight decay等等）、NAS（在特定软/硬件环境下面降低时延或者提升效果）。无论是调参
还是在特定软/硬件环境下面降低时延或者提升效果，都是算法工程师日常工作。AutoML在一定程度上可以减少算法工程师的工作，提高算法工程师的效率，
提升算法工程师的幸福感。

### AutoML在计算机视觉领域的不能

#### 数据层面

自动调超参和NAS有一些前提：任务可学习；采集的训练集分布与测试集分布（或者部署时的数据分布）一致；标注的质量足够高。

公开数据集比如ImageNet、Pascal VOC、COCO、LVIS、Open Images已经满足上述前提，提出这些数据集的人替我们做了这些事情（大家可以看看这些公开
数据集的论文如何解决上述问题）。但是当我们接到一个新的真实业务需求的时候，我们”一穷二白“，我们有以下事情需要做：

1、我们得确定业务应该建模为什么任务，是分类、检索、目标检测、语意分割、实例分割、关键点检测还是兼而有之；

2、需要采集什么样的数据，使得数据分布包含了部署算法时的几乎所有情况；

3、制定标注标注，使得任务可学习，而且操作性足够强（即不能太主观，标注标准太主观会导致一千个标注员有一千种标注结果）；

4、建立标注质量控制方案，以保证标注质量。

上述前提都需要算法工程师来做，AutoML基本无能为力。同时，上述四点表面看起来很简单，其实并不简单，数据和模型是一个硬币的两面，只有对模型非常熟悉
的人才能做好上述四件事情，而做好了上述四件事情，后面的模型选型、调参就会容易很多。

数据层面的工作做得够扎实，即使一个很简单的模型，效果就会好到不可思议；相反数据层面的工作不重视，一心扑在模型上，再fancy的模型，效果也会大打折扣。

#### 模型调试层面
算法工程师经常会遇到模型结果不好的情况，这个时候就需要进行模型调试，调超参是模型调试的一部分。

AutoML自动调参是在设定的超参空间里以指定指标（比如accuracy）为目标搜索最优超参的过程。自动调参是带着镣铐（资源有限）跳舞（希望搜索得到尽可能好
的效果），搜索空间越大，全局最优在搜索空间中的可能性越大，搜索效率越低（在资源有限的情况下）。

为了在效率和效果之间达到折衷，AutoML往往会将搜索空间限制在一个比较小的空间之中，这个空间是AutoML系统的先验，如果实际情况与该先验一致，那么AutoML
效果会很好；但是如果实际情况与该先验不一致，那么AutoML效果会不太好。

另外，有一些超参的搜索空间不像LR和weight decay这样好定义，搜索空间定义不出来，自动调参只能望洋兴叹，只能靠算法工程师撸袖子干。

还有一些情况，算法工程师把预测得不好的结果打印在图上，立马就能猜出来原因何在，像老中医一般；但是如果用自动调参的方式，也许要搜索很久，效率不一定
比人高。

说到底自动调参的目的是为了降低成本，提高效率，只有自动调参成本比人低，或者效率比人高，那么企业才有动力用自动调参系统替换算法工程师。

#### NAS层面

NAS跟上述自动调参有一个类似的问题，带着镣铐（资源有限）跳舞（希望搜索到尽可能好的结果），为了在效率和效果之间取得折衷，NAS也会通过先验将搜索空间限
制在一个较小的空间之中。

如果实际情况（具体任务、数据分布等等）与NAS的先验一致，效果好；如果不一致，效果则差。

### 总结

AutoML是提高算法工程师的效率工具，而不是抢算法工程师饭碗的恶魔。自动化的背后是效率和效果的折衷，折衷是靠限制搜索空间实现的，搜索空间就是AutoML的先验，
天下没有免费的午餐。

没有专业知识，无法保证先验的正确性，也无法保证有好的结果；在算法工程师手里，AutoML能发挥更大的效用，算法工程师可以通过自己的专业知识，根据实际情况，
设置合适的搜索空间。

AutoML能否替换算法工程师的根本在于成本能否做得比算法工程师低，或者效率比算法工程师高。